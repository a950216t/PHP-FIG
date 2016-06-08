# Autoloader

關鍵詞 「必須」("MUST")、「一定不可/一定不能」("MUST NOT")、「需要」("REQUIRED")、
「將會」("SHALL")、「不會」("SHALL NOT")、「應該」("SHOULD")、「不該」("SHOULD NOT")、
「推薦」("RECOMMENDED")、「可以」("MAY")和」可選「("OPTIONAL")的詳細描述可參見 [RFC 2119][http://tools.ietf.org/html/rfc2119] 。


## 1. 概述

本 PSR 是關於由檔案路徑 [自動載入][自動載入] 對應類的相關規範，
本規範是可互操作的，可以作為任一自動載入規範的補充，其中包括 [PSR-0][PSR-0]，此外，
本 PSR 還包括自動載入的類對應的檔案存放路徑規範。


## 2. 詳細說明

1. 此處的「類」泛指所有的class類、接口、traits可復用代碼塊以及其它類似結構。

2. 一個完整的類名需具有以下結構:

        \<命名空間>(\<子命名空間>)*\<類名>

    1. 完整的類名**必須**要有一個頂級命名空間，被稱為 "vendor namespace"；

    2. 完整的類名**可以**有一個或多個子命名空間；

    3. 完整的類名**必須**有一個最終的類名；

    4. 完整的類名中任意一部分中的下劃線都是沒有特殊含義的；

    5. 完整的類名**可以**由任意大小寫字母組成；

    6. 所有類名都**必須**是大小寫敏感的。

3. 當根據完整的類名載入相應的檔案……

    1. 完整的類名中，去掉最前面的命名空間分隔符，前面連續的一個或多個命名空間和子命名空間，作為「命名空間前綴」，其必須與至少一個「檔案基目錄」相對應；

    2. 緊接命名空間前綴後的子命名空間**必須**與相應的」檔案基目錄「相匹配，其中的命名空間分隔符將作為目錄分隔符。

    3. 末尾的類名**必須**與對應的以 `.php` 為後綴的檔案同名。

    4. 自動加載器（autoloader）的實現**一定不能**拋出異常、**一定不能**觸發任一級別的錯誤內容以及**不應該**有返回值。


## 3. 例子

下表展示了符合規範完整類名、命名空間前綴和檔案基目錄所對應的檔案路徑。

| 完整類名    | 命名空間前綴   | 檔案基目錄           | 檔案路徑
| ----------------------------- |--------------------|--------------------------|-------------------------------------------
| \Acme\Log\Writer\File_Writer  | Acme\Log\Writer    | ./acme-log-writer/lib/   | ./acme-log-writer/lib/File_Writer.php
| \Aura\Web\Response\Status     | Aura\Web           | /path/to/aura-web/src/   | /path/to/aura-web/src/Response/Status.php
| \Symfony\Core\Request         | Symfony\Core       | ./vendor/Symfony/Core/   | ./vendor/Symfony/Core/Request.php
| \Zend\Acl                     | Zend               | /usr/includes/Zend/      | /usr/includes/Zend/Acl.php

關於本規範的實現，可參閱 [相關實例][]   
注意：實例並**不**屬於規範的一部分，且隨時**會**有所變動。

[自動載入]: http://php.net/autoload
[PSR-0]: https://github.com/PizzaLiu/PHP-FIG/blob/master/PSR-0-cn.md
[相關實例]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader-examples.md
