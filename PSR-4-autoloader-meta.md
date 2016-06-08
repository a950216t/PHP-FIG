PSR-4 標準補充
===================

1. 概述
----------

本文意圖制定統一的 PHP 加載器的命名空間映射檔案系統路徑的規則，並且能與其他的 SPL 註冊自動加
載器並存。本文只作為補充，並不能替代 PSR-0。

2. 為什麼這樣做？
--------------

### 以前的 PSR-0

PSR-0 類命名和自動加載標準提出自 PHP 5.2 和之前版本被廣泛接受的 Horde/REAR 約定之下，該約
定傾向把所有的 PHP 類源碼放在單一的主目錄中，在類名中使用下劃線指示偽命名空間，像下面這樣:

    /path/to/src/
        VendorFoo/
            Bar/
                Baz.php     # VendorFoo_Bar_Baz
        VendorDib/
            Zim/
                Gir.php     # Vendor_Dib_Zim_Gir

隨著 PHP 5.3 發佈，合適的命名空間語法可以被使用，PSR-0 被引入允許舊式的 Horde/PEAR 下劃線
模式 *和* 使用新的命名空間表示法。為了緩解舊式命名到新式命名的過渡，下劃線仍然允許在類名中使用
，由此支援更寬使用範圍。

    /path/to/src/
        VendorFoo/
            Bar/
                Baz.php     # VendorFoo_Bar_Baz
        VendorDib/
            Zim/
                Gir.php     # VendorDib_Zim_Gir
        Irk_Operation/
            Impending_Doom/
                V1.php
                V2.php      # Irk_Operation\Impending_Doom\V2

這種結構在使用 PEAR 安裝器從 PEAR 包移動源檔案到單一的中央目錄的情況下，是非常明智的。

### Composer 的到來

Composer 包源不在複製到單一的全局位置，使用他們在他們安裝的位置並且不來回移動。這意味著，
Composer 沒有PEAR 作為 PHP 源的 "單一主目錄"。相反，有多個目錄，每個包在每個項目獨立的目錄
中。

為了迎合 PSR-0 的要求，這導致 Composer 的包看起來像這樣:

    vendor/
        vendor_name/
            package_name/
                src/
                    Vendor_Name/
                        Package_Name/
                            ClassName.php       # Vendor_Name\Package_Name\ClassName
                tests/
                    Vendor_Name/
                        Package_Name/
                            ClassNameTest.php   # Vendor_Name\Package_Name\ClassNameTest

"src" 和 "tests" 目錄必須包含 vendor 和包目錄名稱，這是遵從 PSR-0 的產物。

很多人發現這種結構比必要的更深和更多重複。本提案建議補充或替代 PSR 將會更有用，如這樣做我們的
包看起來像:

    vendor/
        vendor_name/
            package_name/
                src/
                    ClassName.php       # Vendor_Name\Package_Name\ClassName
                tests/
                    ClassNameTest.php   # Vendor_Name\Package_Name\ClassNameTest

這將需要實現最初被稱為 "面向-包的自動加載" (作為對比傳統的 "直接的類-到-檔案的自動加載")。

### 面向-包的自動加載

通過增加或修訂 PSR-0 實現面向-包的自動加載非常困難，因為 PSR-0 不允許修改類名路徑之間的任何
部分。這意味著實現面向-包的自動加載要比 PSR-0 複雜的多，但是，它將使得包變的清潔。

最初，以下規則是建議的:

1. 實現者必須使用兩個以上的命名空間層級: 一個 vendor 名，和該 vendor 內的包名。(這兩個頂級
名稱組合被簡稱為 vendor-package 或 vendor-package namespace。)

2. 實現者必須允許 vendor-package namespace 與完全限定類名的其餘部分之間的路徑中綴。

3. vendor-package namespace 可以映射到任意目錄。完全限定類名的其餘部分，必須映射命名空間名
稱到同名目錄，類名必須映射到 .php 結尾的同名檔案。

注意這意味著結束了在類名中下劃線作為目錄分隔符的做法。有人可能認為下劃線應該被遵從因為它們出現
在 PSR-0 規範當中，但是在該文檔中它們作為 PHP 5.2 或者更舊的版本的偽命名空間過渡的做法，所以
它們最好在這裡被刪除。


3. 範圍
--------

### 3.1 目標

- 保留實現者必須使用兩個以上的命名空間層級的 PSR-0 規則: 一個 vendor 名，和該 vendor 內的
  包名。

- 允許 vendor-package namespace 與完全限定類名的其餘部分的路徑中綴。

- 允許 vendor-package namespace 可以映射到任何目錄，也可能是多個目錄。

- 結束遵從類名中下劃線作為目錄分隔符的做法。

### 3.2 非目標

- 為非類資源提供通用的轉換規則


4. 方案
-------------

### 4.1 最佳方案

本方案保留了 PSR-0 關鍵特性，同時消除了更深層次的目錄結構。此外，指定了一些附加規則使得更明確
的互操作性實現。

儘管不涉及目錄映射，最終草案還是規定了自動加載器應該如何處理錯誤。具體來說，它禁止拋出異常或提
出錯誤，這有兩方面的原因。

1. PHP 中自動加載器設計是可堆疊的，如果一個自動加載器不能加載，則其他的仍有機會繼續加載。若有
其中一個自動加載器發生錯誤此過程將不會進行下去。

2. `class_exists()` 和 `interface_exists()` 允許在正常自動加載之後找不到類或接口，若自
動加載器拋出異常將使得 `class_exists()` 不可用，從互操作性的角度來看這是無法接受的。自動加載
器在找不到類的情況下最好通過日誌記錄提供附加的調試內容，日誌可以使用 PSR-3 兼容日誌記錄或其他
什麼。

優點:

- 較淺的目錄結構

- 檔案位置更加固定

- 不再使用類名中下劃線作為目錄分隔符

- 更明確的互操作性實現

缺點:

- 不能像 PSR-0 僅僅通過類名就能確定它在檔案系統的具體位置 (這種 "類-到-檔案" 約定繼承自
  Horde/PEAR)。


### 4.2 備選方案: 仍然遵循 PSR-0 標準

仍然遵循 PSR-0 標準，這儘管合理，但只會留下較深的目錄結構。

優點:

- 不需要改變任何慣例或實現

缺點:

- 留下更深的目錄結構

- 留下遵從類名中使用下劃線作為目錄分隔符的做法


### 4.3 備選方案: 自動加載與轉換分離

Beau Simensen 和其他人建議，轉換邏輯可以從自動加載提案中分離，這樣其他提案可以引用轉換規則。
對它們的分離工作，日後會通過投票和討論進行，合併版本 (如: 轉換規則嵌入自動加載提案) 做為優先采
用項。

優點:

- 其他提案可以引用分離的轉換規則

缺點:

- 不符合投票和一些合作者的意願

### 4.4 備選方案: 使用更多的祈使句和敘述語言

第二輪投票之後，發起人聽到最多的投票是他們支援這個想法但不同意提案的措辭 (或解釋)，有段時間提案
投票擴大到更多敘述和一些更命令式的語言上，這樣的做法被少數直言不諱的參與者譴責。之後的一段時間
Beau Simensen 開始著眼與 PSR-0 的實驗性修訂，編者和發起人更青睞這種簡明的做法並沿用至目前審
議中的版本 (由貢獻最多的 Paul M. Jones 編寫)。

### PHP 5.3.3 之前版本的兼容

PHP 5.3.3 之前的版本不捨去前綴的命名空間，因此找出具體實例落在了具體的聲明上。捨去前綴的命名
空間會導致異常發生。


5. 人員
---------

### 5.1 編者

- Paul M. Jones, Solar/Aura

### 5.2 發起人

- Phil Sturgeon, PyroCMS (Coordinator)
- Larry Garfield, Drupal

### 5.3 貢獻者

- Andreas Hennings
- Bernhard Schussek
- Beau Simensen
- Donald Gilbert
- Mike van Riel
- Paul Dragoonis
- 和其他不計其數的貢獻者


6. 投票
--------

- **投票入口:** <https://groups.google.com/d/msg/php-fig/_LYBgfcEoFE/ZwFTvVTIl4AJ>

- **採納的投票:**

    - 第一次: <https://groups.google.com/forum/#!topic/php-fig/Ua46E344_Ls>,
      提出之前的新工作流; 因意外的提案修改中止

    - 第二次: <https://groups.google.com/forum/#!topic/php-fig/NWfyAeF7Psk>,
      取消發起人的決定權 <https://groups.google.com/forum/#!topic/php-fig/t4mW2TQF7iE>

    - 第三次: 待定


7. 相關連結
-----------------

- [Autoloader, round 4](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/lpmJcmkNYjM)
- [POLL: Autoloader: Split or Combined?](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/fGwA6XHlYhI)
- [PSR-X autoloader spec: Loopholes, ambiguities](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/kUbzJAbHxmg)
- [Autoloader: Combine Proposals?](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/422dFBGs1Yc)
- [Package-Oriented Autoloader, Round 2](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/Y4xc71Q3YEQ)
- [Autoloader: looking again at namespace](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/bnoiTxE8L28)
- [DISCUSSION: Package-Oriented Autoloader - vote against](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/SJTL1ec46II)
- [VOTE: Package-Oriented Autoloader](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/Ua46E344_Ls)
- [Proposal: Package-Oriented Autoloader](https://groups.google.com/forum/#!topicsearchin/php-fig/autoload/php-fig/qT7mEy0RIuI)
- [Towards a Package Oriented Autoloader](https://groups.google.com/forum/#!searchin/php-fig/package$20oriented$20autoloader/php-fig/JdR-g8ZxKa8/jJr80ard-ekJ)
- [List of Alternative PSR-4 Proposals](https://groups.google.com/forum/#!topic/php-fig/oXr-2TU1lQY)
- [Summary of [post-Acceptance Vote pull] PSR-4 discussions](https://groups.google.com/forum/#!searchin/php-fig/psr-4$20summary/php-fig/bSTwUX58NhE/YPcFgBjwvpEJ)
