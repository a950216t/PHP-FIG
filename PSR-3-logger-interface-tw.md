日誌接口規範
================

本文制定了日誌類庫的通用接口規範。

本規範的主要目的，是為了讓日誌類庫以簡單通用的方式，通過接收一個 `Psr\Log\LoggerInterface` 對象，來記錄日誌內容。
框架以及CMS內容管理系統如有需要，**可以**對此接口進行增加，但需遵循本規範，
這才能保證在使用第三方的類庫檔案時，日誌接口仍能正常對接。

關鍵詞 「必須」("MUST")、「一定不可/一定不能」("MUST NOT")、「需要」("REQUIRED")、
「將會」("SHALL")、「不會」("SHALL NOT")、「應該」("SHOULD")、「不該」("SHOULD NOT")、
「推薦」("RECOMMENDED")、「可以」("MAY")和」可選「("OPTIONAL")的詳細描述可參見 [RFC 2119][] 。

本文中的 `實現者` 指的是實現了 `LoggerInterface` 接口的類庫或者框架，反過來講，他們就是 `LoggerInterface` 的 `使用者`。

[RFC 2119]: http://tools.ietf.org/html/rfc2119

1. 規範說明
-----------------

### 1.1 基本規範

- `LoggerInterface` 接口對外定義了八個方法，分別用來記錄 [RFC 5424][] 中定義的八個等級的日誌：debug、 info、 notice、 warning、 error、 critical、 alert 以及 emergency 。

- 第九個方法 —— `log`，其第一個參數為記錄的等級。可使用一個預先定義的等級常量作為參數來調用此方法，**必須**與直接調用以上八個方法具有相同的效果。如果傳入的等級常量參數沒有預先定義，則**必須**拋出 `Psr\Log\InvalidArgumentException` 類型的異常。在不確定的情況下，使用者**不該**使用未支援的等級常量來調用此方法。

[RFC 5424]: http://tools.ietf.org/html/rfc5424

### 1.2 記錄內容

- 以上每個方法都接受一個字符串類型或者是有 `__toString()` 方法的對象作為記錄內容參數，這樣，實現者就能把它當成字符串來處理，否則實現者**必須**自己把它轉換成字符串。

- 記錄內容參數**可以**攜帶佔位符，實現者**可以**根據上下文將其它替換成相應的值。

  其中佔位符**必須**與上下文數組中的鍵名保持一致。

  佔位符的名稱**必須**由一個左花括號 `{` 以及一個右括號 `}` 包含。但花括號與名稱之間**一定不能**有空格符。

  佔位符的名稱**應該**只由 `A-Z`、 `a-z`,`0-9`、下劃線 `_`、以及英文的句號 `.`組成，其它字符作為將來佔位符規範的保留。

  實現者**可以**通過對佔位符採用不同的轉義和轉換策略，來建立最終的日誌。
  而使用者在不知道上下文的前提下，**不該**提前轉義佔位符。

  以下是一個佔位符使用的例子：

  ```php
  /**
   * 用上下文內容替換記錄內容中的佔位符
   */
  function interpolate($message, array $context = array())
  {
      // 構建一個花括號包含的鍵名的替換數組
      $replace = array();
      foreach ($context as $key => $val) {
          $replace['{' . $key . '}'] = $val;
      }

      // 替換記錄內容中的佔位符，最後返回修改後的記錄內容。
      return strtr($message, $replace);
  }

  // 含有帶花括號佔位符的記錄內容。
  $message = "User {username} created";

  // 帶有替換內容的上下文數組，鍵名為佔位符名稱，鍵值為替換值。
  $context = array('username' => 'bolivar');

  // 輸出 "Username bolivar created"
  echo interpolate($message, $context);
  ```

### 1.3 上下文

- 每個記錄函數都接受一個上下文數組參數，用來裝載字符串類型無法表示的內容。它**可以**裝載任何內容，所以實現者**必須**確保能正確處理其裝載的內容，對於其裝載的數據，**一定不能** 拋出異常，或產生PHP出錯、警告或提醒內容（error、warning、notice）。

- 如需通過上下文參數傳入了一個 `Exception` 對象， **必須**以 `'exception'` 作為鍵名。
記錄異常內容是很普遍的，所以如果它能夠在記錄類庫的底層實現，就能夠讓實現者從異常內容中抽絲剝繭。
當然，實現者在使用它時，**必須**確保鍵名為 `'exception'` 的鍵值是否真的是一個 `Exception`，畢竟它**可以**裝載任何內容。

### 1.4 助手類和接口

- `Psr\Log\AbstractLogger` 類使得只需繼承它和實現其中的 `log` 方法，就能夠很輕易地實現 `LoggerInterface` 接口，而另外八個方法就能夠把記錄內容和上下文內容傳給它。

- 同樣地，使用  `Psr\Log\LoggerTrait`  也只需實現其中的 `log` 方法。不過，需要特別注意的是，在traits可復用代碼塊還不能實現接口前，還需要  `implement LoggerInterface`。

- 在沒有可用的日誌記錄器時， `Psr\Log\NullLogger` 接口**可以**為使用者提供一個備用的日誌「黑洞」。不過，當上下文的構建非常消耗資源時，帶條件檢查的日誌記錄或許是更好的辦法。

- `Psr\Log\LoggerAwareInterface` 接口僅包括一個
  `setLogger(LoggerInterface $logger)` 方法，框架可以使用它實現自動連接任意的日誌記錄實例。

- `Psr\Log\LoggerAwareTrait` trait可復用代碼塊可以在任何的類裡面使用，只需通過它提供的 `$this->logger`，就可以輕鬆地實現等同的接口。

- `Psr\Log\LogLevel` 類裝載了八個記錄等級常量。

2. 包
----------

上述的接口、類和相關的異常類，以及一系列的實現檢測檔案，都包含在 [psr/log](https://packagist.org/packages/psr/log) 檔案包中。

3. `Psr\Log\LoggerInterface`
----------------------------

```php
<?php

namespace Psr\Log;

/**
 * 日誌記錄實例
 *
 * 日誌內容變量 —— message， **必須**是一個字符串或是實現了  __toString() 方法的對象。
 *
 * 日誌內容變量中**可以**包含格式如 「{foo}」 (代表foo) 的佔位符，
 * 它將會由上下文數組中鍵名為 "foo" 的鍵值替代。
 *
 * 上下文數組可以攜帶任意的數據，唯一的限制是，當它攜帶的是一個 exception 對像時，它的鍵名 必須 是 "exception"。
 *
 * 明細可參閱： https://github.com/PizzaLiu/PHP-FIG/blob/master/PSR-3-logger-interface-cn.md
 */
interface LoggerInterface
{
    /**
     * 系統不可用
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function emergency($message, array $context = array());

    /**
     * **必須**立刻採取行動
     *
     * 例如：在整個網站都垮掉了、數據庫不可用了或者其他的情況下，**應該**發送一條警報短信把你叫醒。
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function alert($message, array $context = array());

    /**
     * 緊急情況
     *
     * 例如：程序組件不可用或者出現非預期的異常。
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function critical($message, array $context = array());

    /**
     * 運行時出現的錯誤，不需要立刻採取行動，但必須記錄下來以備檢測。
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function error($message, array $context = array());

    /**
     * 出現非錯誤性的異常。
     *
     * 例如：使用了被棄用的API、錯誤地使用了API或者非預想的不必要錯誤。
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function warning($message, array $context = array());

    /**
     * 一般性重要的事件。
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function notice($message, array $context = array());

    /**
     * 重要事件
     *
     * 例如：會員登入和SQL記錄。
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function info($message, array $context = array());

    /**
     * debug 明細
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function debug($message, array $context = array());

    /**
     * 任意等級的日誌記錄
     *
     * @param mixed $level
     * @param string $message
     * @param array $context
     * @return null
     */
    public function log($level, $message, array $context = array());
}
```

4. `Psr\Log\LoggerAwareInterface`
---------------------------------

```php
<?php

namespace Psr\Log;

/**
 * logger-aware 定義實例
 */
interface LoggerAwareInterface
{
    /**
     * 設定一個日誌記錄實例
     *
     * @param LoggerInterface $logger
     * @return null
     */
    public function setLogger(LoggerInterface $logger);
}
```

5. `Psr\Log\LogLevel`
---------------------

```php
<?php

namespace Psr\Log;

/**
 * 日誌等級常量定義
 */
class LogLevel
{
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
}
```
