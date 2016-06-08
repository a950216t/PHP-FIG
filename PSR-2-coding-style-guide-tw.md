代碼風格規範
==================

本篇規範是 [PSR-1][] 基本代碼規範的繼承與增加。

本規範希望通過制定一系列規範化PHP代碼的規則，以減少在瀏覽不同作者的代碼時，因代碼風格的不同而造成不便。

當多名程序員在多個項目中合作時，就需要一個共同的編碼規範，
而本文中的風格規範源自於多個不同項目代碼風格的共同特性，
因此，本規範的價值在於我們都遵循這個編碼風格，而不是在於它本身。

關鍵詞 「必須」("MUST")、「一定不可/一定不能」("MUST NOT")、「需要」("REQUIRED")、
「將會」("SHALL")、「不會」("SHALL NOT")、「應該」("SHOULD")、「不該」("SHOULD NOT")、
「推薦」("RECOMMENDED")、「可以」("MAY")和」可選「("OPTIONAL")的詳細描述可參見 [RFC 2119][] 。

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/PizzaLiu/PHP-FIG/blob/master/PSR-0-cn.md
[PSR-1]: https://github.com/PizzaLiu/PHP-FIG/blob/master/PSR-1-basic-coding-standard-cn.md


1. 概覽
-----------

- 代碼**必須**遵循 [PSR-1][] 中的編碼規範 。

- 代碼**必須**使用4個空格符而不是 tab鍵 進行縮進。

- 每行的字符數**應該**軟性保持在80個之內， 理論上**一定不可**多於120個， 但**一定不能**有硬性限制。

- 每個 `namespace` 命名空間聲明語句和 `use` 聲明語句塊後面，**必須**插入一個空白行。

- 類的開始花括號(`{`)**必須**寫在函數聲明後自成一行，結束花括號(`}`)也**必須**寫在函數主體後自成一行。

- 方法的開始花括號(`{`)**必須**寫在函數聲明後自成一行，結束花括號(`}`)也**必須**寫在函數主體後自成一行。

- 類的屬性和方法**必須**增加訪問修飾符（`private`、`protected` 以及 `public`）， `abstract` 以及 `final` **必須**聲明在訪問修飾符之前，而 `static` **必須**聲明在訪問修飾符之後。
  
- 控制結構的關鍵字後**必須**要有一個空格符，而調用方法或函數時則**一定不能**有。

- 控制結構的開始花括號(`{`)**必須**寫在聲明的同一行，而結束花括號(`}`)**必須**寫在主體後自成一行。

- 控制結構的開始左括號後和結束右括號前，都**一定不能**有空格符。

### 1.1. 例子

以下例子程序簡單地展示了以上大部分規範：

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
```

2. 通則
----------

### 2.1 基本編碼準則

代碼**必須**符合 [PSR-1][] 中的所有規範。

### 2.2 檔案

所有PHP檔案**必須**使用`Unix LF (linefeed)`作為行的結束符。

所有PHP檔案**必須**以一個空白行作為結束。

純PHP代碼檔案**必須**省略最後的 `?>` 結束標籤。

### 2.3. 行

行的長度**一定不能**有硬性的約束。

軟性的長度約束**一定**要限制在120個字符以內，若超過此長度，帶代碼規範檢查的編輯器**一定**要發出警告，不過**一定不可**發出錯誤提示。

每行**不應該**多於80個字符，大於80字符的行**應該**折成多行。

非空行後**一定不能**有多餘的空格符。

空行**可以**使得閱讀代碼更加方便以及有助於代碼的分塊。

每行**一定不能**存在多於一條語句。

### 2.4. 縮進

代碼**必須**使用4個空格符的縮進，**一定不能**用 tab鍵 。

> 備註: 使用空格而不是tab鍵縮進的好處在於，
> 避免在比較代碼差異、打補丁、重閱代碼以及註釋時產生混淆。
> 並且，使用空格縮進，讓對齊變得更方便。

### 2.5. 關鍵字 以及 True/False/Null

PHP所有 [關鍵字][]**必須**全部小寫。

常量 `true` 、`false` 和 `null` 也**必須**全部小寫。

[關鍵字]: http://php.net/manual/en/reserved.keywords.php



3. namespace 以及 use 聲明
---------------------------------

`namespace` 聲明後 必須 插入一個空白行。

所有 `use` 必須 在 `namespace` 後聲明。

每條 `use` 聲明語句 必須 只有一個 `use` 關鍵詞。

`use` 聲明語句塊後 必須 要有一個空白行。

例如：

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...

```


4. 類、屬性和方法
-----------------------------------

此處的「類」泛指所有的class類、接口以及traits可復用代碼塊。

### 4.1. 增加與繼承

關鍵詞 `extends` 和 `implements`**必須**寫在類名稱的同一行。

類的開始花括號**必須**獨佔一行，結束花括號也**必須**在類主體後獨佔一行。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

`implements` 的繼承列表也**可以**分紅多行，這樣的話，每個繼承接口名稱都**必須**分開獨立成行，包括第一個。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```

### 4.2. 屬性

每個屬性都**必須**增加訪問修飾符。

**一定不可**使用關鍵字 `var` 聲明一個屬性。

每條語句**一定不可**定義超過一個屬性。

**不要**使用下劃線作為前綴，來區分屬性是 protected 或 private。

以下是屬性聲明的一個範例：

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3. 方法

所有方法都**必須**增加訪問修飾符。

**不要**使用下劃線作為前綴，來區分方法是 protected 或 private。

方法名稱後**一定不能**有空格符，其開始花括號**必須**獨佔一行，結束花括號也**必須**在方法主體後單獨成一行。參數左括號後和右括號前**一定不能**有空格。

一個標準的方法聲明可參照以下範例，留意其括號、逗號、空格以及花括號的位置。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```    

### 4.4. 方法的參數

參數列表中，每個逗號後面**必須**要有一個空格，而逗號前面**一定不能**有空格。

有預設值的參數，**必須**放到參數列表的末尾。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

參數列表**可以**分列成多行，這樣，包括第一個參數在內的每個參數都**必須**單獨成行。

拆分紅多行的參數列表後，結束括號以及方法開始花括號 必須 寫在同一行，中間用一個空格分隔。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

### 4.5. `abstract` 、 `final` 、 以及 `static`

需要增加 `abstract` 或 `final` 聲明時， **必須**寫在訪問修飾符前，而 `static` 則**必須**寫在其後。

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

### 4.6. 方法及函數調用

方法及函數調用時，方法名或函數名與參數左括號之間**一定不能**有空格，參數右括號前也 **一定不能**有空格。每個逗號前**一定不能**有空格，但其後**必須**有一個空格。

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

參數**可以**分列成多行，此時包括第一個參數在內的每個參數都**必須**單獨成行。

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

5. 控制結構
---------------------

控制結構的基本規範如下：

- 控制結構關鍵詞後**必須**有一個空格。
- 左括號 `(` 後**一定不能**有空格。
- 右括號 `)` 前也**一定不**能有空格。
- 右括號 `)` 與開始花括號 `{` 間**一定**有一個空格。
- 結構體主體**一定**要有一次縮進。
- 結束花括號 `}` **一定**在結構體主體後單獨成行。

每個結構體的主體都**必須**被包含在成對的花括號之中，
這能讓結構體更加結構話，以及減少加入新行時，出錯的可能性。


### 5.1. `if` 、 `elseif` 和 `else`

標準的 `if` 結構如下代碼所示，留意 括號、空格以及花括號的位置，
注意 `else` 和 `elseif` 都與前面的結束花括號在同一行。

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

**應該**使用關鍵詞 `elseif` 代替所有 `else if` ，以使得所有的控制關鍵字都像是單獨的一個詞。 


### 5.2. `switch` 和 `case`

標準的 `switch` 結構如下代碼所示，留意括號、空格以及花括號的位置。
`case` 語句**必須**相對 `switch` 進行一次縮進，而 `break` 語句以及 `case` 內的其它語句都 必須 相對 `case` 進行一次縮進。
如果存在非空的 `case` 直穿語句，主體裡必須有類似 `// no break` 的註釋。

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```


### 5.3. `while` 和 `do while`

一個規範的 `while` 語句應該如下所示，注意其 括號、空格以及花括號的位置。

```php
<?php
while ($expr) {
    // structure body
}
```

標準的 `do while` 語句如下所示，同樣的，注意其 括號、空格以及花括號的位置。

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4. `for`

標準的 `for` 語句如下所示，注意其 括號、空格以及花括號的位置。

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`
    
標準的 `foreach` 語句如下所示，注意其 括號、空格以及花括號的位置。

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try`, `catch`

標準的 `try catch` 語句如下所示，注意其 括號、空格以及花括號的位置。

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

6. 閉包
-----------

閉包聲明時，關鍵詞 `function` 後以及關鍵詞 `use` 的前後都**必須**要有一個空格。

開始花括號**必須**寫在聲明的同一行，結束花括號**必須**緊跟主體結束的下一行。


參數列表和變量列表的左括號後以及右括號前，**必須不能**有空格。

參數和變量列表中，逗號前**必須不能**有空格，而逗號後**必須**要有空格。

閉包中有預設值的參數**必須**放到列表的後面。


標準的閉包聲明語句如下所示，注意其 括號、逗號、空格以及花括號的位置。

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

參數列表以及變量列表**可以**分紅多行，這樣，包括第一個在內的每個參數或變量都**必須**單獨成行，而列表的右括號與閉包的開始花括號**必須**放在同一行。

以下幾個例子，包含了參數和變量列表被分紅多行的多情況。

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

注意，閉包被直接用作函數或方法調用的參數時，以上規則仍然適用。

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```


7. 總結
--------------
以上規範難免有疏忽，其中包括但不僅限於：

- 全局變量和常量的定義

- 函數的定義

- 操作符和賦值

- 行內對齊

- 註釋和文檔描述塊

- 類名的前綴及後綴

- 最佳實踐

本規範之後的修訂與增加將彌補以上不足。


附錄 A. 問卷調查
------------------

為了編寫本規範，小組制定了調查問卷，用來統計各成員項目的共同規範。
以下是此問卷調查的數據，在此供查閱。

### A.1. 問卷數據

    url,http://www.horde.org/apps/horde/docs/CODING_STANDARDS,http://pear.php.net/manual/en/standards.php,http://solarphp.com/manual/appendix-standards.style,http://framework.zend.com/manual/en/coding-standard.html,http://symfony.com/doc/2.0/contributing/code/standards.html,http://www.ppi.io/docs/coding-standards.html,https://github.com/ezsystems/ezp-next/wiki/codingstandards,http://book.cakephp.org/2.0/en/contributing/cakephp-coding-conventions.html,https://github.com/UnionOfRAD/lithium/wiki/Spec%3A-Coding,http://drupal.org/coding-standards,http://code.google.com/p/sabredav/,http://area51.phpbb.com/docs/31x/coding-guidelines.html,https://docs.google.com/a/zikula.org/document/edit?authkey=CPCU0Us&hgd=1&id=1fcqb93Sn-hR9c0mkN6m_tyWnmEvoswKBtSc0tKkZmJA,http://www.chisimba.com,n/a,https://github.com/Respect/project-info/blob/master/coding-standards-sample.php,n/a,Object Calisthenics for PHP,http://doc.nette.org/en/coding-standard,http://flow3.typo3.org,https://github.com/propelorm/Propel2/wiki/Coding-Standards,http://developer.joomla.org/coding-standards.html
    voting,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,no,no,no,?,yes,no,yes
    indent_type,4,4,4,4,4,tab,4,tab,tab,2,4,tab,4,4,4,4,4,4,tab,tab,4,tab
    line_length_limit_soft,75,75,75,75,no,85,120,120,80,80,80,no,100,80,80,?,?,120,80,120,no,150
    line_length_limit_hard,85,85,85,85,no,no,no,no,100,?,no,no,no,100,100,?,120,120,no,no,no,no
    class_names,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,lower_under,studly,lower,studly,studly,studly,studly,?,studly,studly,studly
    class_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,next,next,next,next,next,next,same,next,next
    constant_names,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper
    true_false_null,lower,lower,lower,lower,lower,lower,lower,lower,lower,upper,lower,lower,lower,upper,lower,lower,lower,lower,lower,upper,lower,lower
    method_names,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,lower_under,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel
    method_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,same,next,next,next,next,next,same,next,next
    control_brace_line,same,same,same,same,same,same,next,same,same,same,same,next,same,same,next,same,same,same,same,same,same,next
    control_space_after,yes,yes,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes
    always_use_control_braces,yes,yes,yes,yes,yes,yes,no,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes
    else_elseif_line,same,same,same,same,same,same,next,same,same,next,same,next,same,next,next,same,same,same,same,same,same,next
    case_break_indent_from_switch,0/1,0/1,0/1,1/2,1/2,1/2,1/2,1/1,1/1,1/2,1/2,1/1,1/2,1/2,1/2,1/2,1/2,1/2,0/1,1/1,1/2,1/2
    function_space_after,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no
    closing_php_tag_required,no,no,no,no,no,no,no,no,yes,no,no,no,no,yes,no,no,no,no,no,yes,no,no
    line_endings,LF,LF,LF,LF,LF,LF,LF,LF,?,LF,?,LF,LF,LF,LF,?,,LF,?,LF,LF,LF
    static_or_visibility_first,static,?,static,either,either,either,visibility,visibility,visibility,either,static,either,?,visibility,?,?,either,either,visibility,visibility,static,?
    control_space_parens,no,no,no,no,no,no,yes,no,no,no,no,no,no,yes,?,no,no,no,no,no,no,no
    blank_line_after_php,no,no,no,no,yes,no,no,no,no,yes,yes,no,no,yes,?,yes,yes,no,yes,no,yes,no
    class_method_control_brace,next/next/same,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/next,same/same/same,same/same/same,same/same/same,same/same/same,next/next/next,next/next/same,next/same/same,next/next/next,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/same,next/next/next

### A.2. 問卷說明

`indent_type`:
縮進類型. `tab` = "使用 tab 鍵一次", `2` or `4` = "空格的數量"

`line_length_limit_soft`:
每行字符數量的「軟」限制. `?` = 不可辯或無作答, `no` 表示無限制.

`line_length_limit_hard`:
每行字符數量的「硬」限制. `?` = 不可辯或無作答, `no` 表示無限制.

`class_names`:
類名稱的命名. `lower` = 只允許小寫字母, `lower_under` = 下滑線分隔的小寫字母, `studly` = StudlyCase 的駝峰風格.

`class_brace_line`:
類的開始花括號是與 class 關鍵字在同一行或是在其的下一行？

`constant_names`:
類的常量如何命名? `upper` = 下劃線分隔的大寫字母.

`true_false_null`:
關鍵字 `true`、`false` 以及 `null` 是全部小寫 `lower` 還是全部大寫 `upper`?

`method_names`:
方法名稱如何命名? `camel` = `camelCase`, `lower_under` = 下劃線分隔的小寫字母.

`method_brace_line`:
方法的開始花括號是與方法名在同一行還是在其的下一行？

`control_brace_line`:
控制結構的開始花括號是與聲明在同一行還是在其的下一行？

`control_space_after`:
控制結構關鍵詞後是否有空格？

`always_use_control_braces`:
控制結構體是否都要被包含在花括號內？

`else_elseif_line`:
`else` 或 `elseif` 與前面的結束花括號在同一行還是在其的下一行？

`case_break_indent_from_switch`:
`switch` 語句中的 `case` 和 `break` 需要相對 `switch` 縮進多少次？

`function_space_after`:
函數調用語句中，函數名稱與變量列表的左括號間是否有空格？

`closing_php_tag_required`:
純 PHP 代碼的檔案，是否需要 `?>` 結束標籤？

`line_endings`:
選擇哪種類型的行結束符？

`static_or_visibility_first`:
聲明一個靜態方法時，`static` 是寫訪問修飾符前還是後？

`control_space_parens`:
控制結構裡，左括號後以及右括號前是否有空格？`yes` = `if ( $expr )`, `no` = `if ($expr)`.

`blank_line_after_php`:
PHP 開始標籤後，是否需要一個空行？

`class_method_control_brace`:
開始花括號在類、方法和控制結構的位置統計。

### A.3. 問卷統計結果

    indent_type:
        tab: 7
        2: 1
        4: 14
    line_length_limit_soft:
        ?: 2
        no: 3
        75: 4
        80: 6
        85: 1
        100: 1
        120: 4
        150: 1
    line_length_limit_hard:
        ?: 2
        no: 11
        85: 4
        100: 3
        120: 2
    class_names:
        ?: 1
        lower: 1
        lower_under: 1
        studly: 19
    class_brace_line:
        next: 16
        same: 6
    constant_names:
        upper: 22
    true_false_null:
        lower: 19
        upper: 3
    method_names:
        camel: 21
        lower_under: 1
    method_brace_line:
        next: 15
        same: 7
    control_brace_line:
        next: 4
        same: 18
    control_space_after:
        no: 2
        yes: 20
    always_use_control_braces:
        no: 3
        yes: 19
    else_elseif_line:
        next: 6
        same: 16
    case_break_indent_from_switch:
        0/1: 4
        1/1: 4
        1/2: 14
    function_space_after:
        no: 22
    closing_php_tag_required:
        no: 19
        yes: 3
    line_endings:
        ?: 5
        LF: 17
    static_or_visibility_first:
        ?: 5
        either: 7
        static: 4
        visibility: 6
    control_space_parens:
        ?: 1
        no: 19
        yes: 2
    blank_line_after_php:
        ?: 1
        no: 13
        yes: 8
    class_method_control_brace:
        next/next/next: 4
        next/next/same: 11
        next/same/same: 1
        same/same/same: 6
