HTTP消息接口
=====================

本文檔描述了在[RFC 7230][]和[RFC 7231][]中被描述來代表HTTP消息通用接口，以及在[RFC 3986][]中規定的URIs語法。

[RFC 7230]:http://tools.ietf.org/html/rfc7230
[RFC 7231]:http://tools.ietf.org/html/rfc7231
[RFC 3986]:http://tools.ietf.org/html/rfc3986
[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt

HTTP消息是Web開發的基礎。Web瀏覽器和HTTP客戶端之間，例如使用cURL建立一個發送到Web服務器的HTTP請求，Web服務器會返回一個HTTP響應。服務器端代碼收到一個HTTP請求消息，並返回一個HTTP響應消息。

HTTP消息通常對與最終的消費者會員來說是抽像的，但作為開發者，我們通常需要知道它們是如何構建以及如何訪問和操縱它們，以執行我們的任務，是否可能被建立為到HTTP API的請求或處理傳入請求。

每一個HTTP請求消息有一個具體形式：
```php
POST /path HTTP/1.1
Host: example.com

foo=bar&baz=bat
```
一個請求的第一行是「請求行」，按照順序包含了HTTP請求方法，請求的目標地址（通常是一個絕對URI或者服務器上的路徑）以及HTTP協議的版本。接著是一個或多個HTTP頭，一個空行，以及消息主體。

HTTP響應內容具有類似的結構：
```php
HTTP/1.1 200 OK
Content-Type: text/plain

This is the response body
```
第一行是「狀態行」，按照順序，依次包含了HTTP協議的版本，HTTP狀態碼，以及一個「原因分析」，也就是一個對人類友好可讀的狀態碼的描述。像請求消息一樣，隨後一個或多個HTTP頭，一個空行，以及消息主體。

本文檔中描述的接口都是圍繞HTTP消息的抽像和構成它們的元素。

關鍵詞 「必須」("MUST")、「一定不可/一定不能」("MUST NOT")、「需要」("REQUIRED")、
「將會」("SHALL")、「不會」("SHALL NOT")、「應該」("SHOULD")、「不該」("SHOULD NOT")、
「推薦」("RECOMMENDED")、「可以」("MAY")和」可選「("OPTIONAL")的詳細描述可參見 [RFC 2119][] 。

### 參考
- [RFC 2119][]
- [RFC 3986][]
- [RFC 7230][]
- [RFC 7231][]

[RFC 2119]:http://tools.ietf.org/html/rfc2119
[RFC 3986]:http://tools.ietf.org/html/rfc3986
[RFC 7230]:http://tools.ietf.org/html/rfc7230
[RFC 7231]:http://tools.ietf.org/html/rfc7231

1. 規範
--------

### 1.1. 消息

HTTP消息是從客戶機到服務器的請求或從服務器到客戶端的響應。本規範分別為其定義了對於HTTP消息的接口 `Psr\Http\Message\RequestInterface` 和 `Psr\Http\Message\ResponseInterface` 。

`Psr\Http\Message\RequestInterface` 和     `Psr\Http\Message\ResponseInterface` 都繼承自 `Psr\Http\Message\MessageInterface` 。而 `Psr\Http\Message\MessageInterface` **可以** 被直接實現，實現者 **應該** 實現 `Psr\Http\Message\RequestInterface` 和     `Psr\Http\Message\ResponseInterface` 。

從這裡開始，之後的描述中命名空間 `Psr\Http\Message` 在提到這個接口的時候將會被省略。

### 1.2. HTTP頭

#### 不區分大小寫的頭字段名

HTTP消息包括大小寫不敏感的頭字段名。頭是從實現了 `MessageInterface ` 接口的類中以不區分大小寫的方式取得的。例如，取得 `foo` 頭與取得 `FoO` 頭的返回結果是相同的。同樣，設定 `Foo` 頭將覆寫之前的設定的 `foo` 頭的值。

```php
$message = $message->withHeader('foo', 'bar');

echo $message->getHeaderLine('foo');
// Outputs: bar

echo $message->getHeaderLine('FOO');
// Outputs: bar

$message = $message->withHeader('fOO', 'baz');
echo $message->getHeaderLine('foo');
// Outputs: baz
```
儘管頭能被不區分大小寫地取得，但原有的大小寫規範 **必須** 被保留，尤其是使用 `getHeaders()` 函數來取得頭的時候。

不符合要求的HTTP應用程序可能依賴於一定的大小寫規範，所以對於一個會員能夠在建立一個請求或響應時控制HTTP報頭的大小寫的情況下是非常有用的。

#### 帶有多個值的報頭

為了能夠容納具有多個值且依然能夠方便地以字符串形式傳輸的報頭， `MessageInterface` 接口的實例能夠以數組或字符串的形式來取得報頭。使用 `getHeaderLine()` 方法來取得特定名稱的報頭的值，其形式為不區分大小寫並用逗號連接的字符串，包含了所有的報頭值。使用 `getHeader()` 來取得特定名稱的所有報頭值，其以數組形式返回結果，且不區分大小寫。












