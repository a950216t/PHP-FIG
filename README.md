PHP程式碼書寫規範（正體中文版）指南
====================================
本文件是PHP通用性框架小組（[PHP-FIG][] :PHP Framework Interoperability Group）制定的PHP程式碼書寫規範（[PSR][]:Proposing a Standards Recommendation）中譯版。

翻譯過程中參照了 [莫希爾(Mosil)手札][] 的繁體中文版，以及 [Corrie Zhao][] 組織翻譯的簡體中文版，
譯文中為了讓語句通順，便於理解，沒有對原文逐字翻譯，個別語句與原文原意可能略有偏差，希望告知指正。

- 2014/04/25    增加`PSR-2補充`檔案以及修改之前版本中的翻譯不當與錯誤。
- 2014/07/31    增加`PSR-4`。

[PHP-FIG]: https://github.com/php-fig/
[PSR]: https://github.com/php-fig/fig-standards
[莫希爾(Mosil)手札]: https://github.com/mosil/fig-standards
[Corrie Zhao]: https://github.com/hfcorriez/fig-standards

以下是原版的導讀：

---------------

PHP通用性框架小組
====================================

組建本小組的目的是，通過在各項目的代表之間討論他們共同的編碼規範，以制定一個協作標準。本規範的主要面向對象是本小組的各個組成成員，當然，同時也歡迎追蹤本規範的其它PHP社區採用本規範。


送出規範建議
------------------------------------

可以通過以下方式給本規範送出建議:

- fork [PSR代碼庫][]，建立並檢出一個分支，在 `proposed/` 下增加 規範建議，然後 push 分支到 Github，最後給我們發送一個 pull request；又或者

- 在 Github 下新建一個討論 ticket；又或者

- 在 [郵件列表][] 中送出建議。

[郵件列表]: http://groups.google.com/group/php-fig/
[PSR代碼庫]: https://github.com/php-fig/fig-standards

成為投票成員
---------------------

注意，你 **不需要** 成為投票成員才能在 [郵件列表][] 中發表言論。

想要成為投票成員，你必須發送一封郵件到 [郵件列表][] 中。

- 郵件主題格式如下: `Membership Request: {你的名字} ({參與的項目名稱})`

- 郵件內容應包括你的名字、你參與的項目名稱、項目的地址以及其它相關內容。
  
目前的成員會對你的加入請求進行投票。

請不要在一份申請中送出多個加入請求，每份申請只能送出一份請求。


目前的成員及其代表項目列表
--------------

1. Nate Abele: Lithium

1. Nils Adermann: phpBB

1. Brett Bieber: PEAR, PEAR2
    
1. Guilherme Blanco: Doctrine, Doctrine2, et al.

1. Jordi Boggiano: Composer, Packagist

1. Padraic Brady: Zend Framework

1. Karma Dordrak: Zikula

1. Paul Dragoonis: PPI, PPI2

1. William Durand: Propel, Propel 2

1. Don Gilbert: Joomla

1. Cal Evans: the community at large

1. Larry Garfield: Drupal

1. Ivan Habunek: Apache log4php

1. Paul M. Jones: Solar Framework, Aura Project

1. Karsten Dambekalns: TYPO3 Flow, TYPO3 Neos

1. Larry Masters: CakePHP, CakePHP 2

1. John Mertic: SugarCRM

1. Taylor Otwell: Laravel

1. Ryan Parman: Amazon Web Services SDK

1. Evert Pot: SabreDAV

1. Fabien Potencier: Symfony, Symfony2

1. Mike van Riel: phpDocumentor

1. Andre Romcke: eZ Publish

1. Phil Sturgeon: PyroCMS

1. Lukas Smith: Jackalope

1. Kris Wallsmith: Assetic, Buzz

1. David Zulke: Agavi