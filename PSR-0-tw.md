自動加載規範
====================

> **此規範已被棄用** - 本規範已於2014年10月21日被標記為棄用，目前最新的替代規範為 [PSR-4] 。

本文是為`自動加載器（autoloader）`實現通用自動加載，所需要遵循的編碼規範。

規範說明
---------

* 一個標準的 命名空間(namespace) 與 類(class) 名稱的定義必須符合以下結構：
`\<Vendor Name>\(<Namespace>\)*<Class Name>`；
* 其中`Vendor Name`為每個命名空間都必須要有的一個頂級命名空間名；
* 需要的話，每個命名空間下可以擁有多個子命名空間；
* 當根據完整的命名空間名從檔案系統中載入類檔案時，每個命名空間之間的分隔符都會被轉換成檔案夾路徑分隔符；
* 類名稱中的每個 `_` 字符也會被轉換成檔案夾路徑分隔符，而命名空間中的 `_` 字符則是無特殊含義的。
* 當從檔案系統中載入標準的命名空間或類時，都將增加 `.php` 為目標檔案後綴；
* `組織名稱(Vendor Name)`、`命名空間(Namespace)` 以及 `類的名稱(Class Name)` 可由任意大小寫字母組成。

範例
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

命名空間以及類名稱中的下劃線
-----------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`


以上是使用通用自動加載必須遵循的最低規範標準， 可通過以下的示例函數 SplClassLoader 載入 PHP 5.3 的類檔案，來驗證你所寫的命名空間以及類是否符合以上規範。

實例
----------------------

以下示例函數為本規範的一個簡單實現。

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

SplClassLoader 實例
-----------------------------

以下的 gist 是 一個 SplClassLoader 類檔案的實例，如果你遵循了以上規範，可以把它用來載入你的類檔案。 這也是目前 PHP 5.3 建議的類檔案載入方式。

* [http://gist.github.com/221634](http://gist.github.com/221634)

