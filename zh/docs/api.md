# Core APIs

- setCaptcha
- setChannel
- setClosure
- setComponent
- setDriver
- setExcludedUrls
- setFilter
- setFilters
- setProperty
- setProperties
- setMessenger
- setIp
- getCurrentUrl
- managedBy
- outputJsSnippet
- captchaResponse
- setView
- createDatabase
- ban
- unban
- limitSession
- run

這些公開的 API 可以串成鏈，但 `SetDriver` 必須在第一位，而 `run` 必須在最後一個。

## 可鏈式

### setCaptcha

- *param* CaptchaInterface
- *since* 2.0.0
- *return* self

更多詳細用法，請參閱 [Captcha](captcha/index.md) 段落。

```php
$shieldon->setCaptcha(new \Shieldon\Captcha\Recaptcha([
    'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
    'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
    'version' => 'v2',
    'lang' => 'en',
]));
```

### setChannel

- *param* string `$channel` 頻道名稱
- *return* self

```php
$shieldon->setChannel('web_project');

// Start new shieldon each day.
$shieldon->setChannel('web_project_' . date('Ymd'));
```

### setClosure

- *param* string `$key`
- *param* Closure `$closure`
- *since* 3.0.0
- *return* self

設定一個封閉函式。

```php
$shieldon->setClosure('www_authenticate', function() use ($authHandler, $authenticateList) {
    $authHandler->set($authenticateList);
    $authHandler->check();
});
```

### setComponent

- *param* ComponentInterface
- *return* self

```php
$shieldon->setComponent(new \Shieldon\Component\Ip());
```

### setDriver

- *param* DriverProvider
- *return* self

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);
$shieldon->setDriver(new \Shieldon\Driver\SqliteDriver($pdoInstance));
```

### setExcludedUrls

- *param* array `$urls`
- *since* 3.0.0
- *return* self

設定要排除在保護之外的網址列表。

```php
$list = [
    '/example/1',
    '/wp-login.php',
];

$shieldon->setExcludedUrls($list);
```

### setFilter

- *param* string `$filterName` Sheidlon 過濾器的名稱
- *param* bool `$value` true|false
- *since* 3.0.0
- *return* self

```php
$shieldon->setFilter('session', false);
$shieldon->setFilter('cookie', false);
$shieldon->setFilter('referer', true);
$shieldon->setFilter('frequency', false); 
```

### setFilters

- *param* array `$settings` 過濾器設定
- *return* self

```php
$shieldon->setFilters([
    'session' => true,
    'cookie' => true,
    'referer' => true,
    'frequency' => true,
]);
```

預設的設定值：

| 鍵值| 類別 | 值 | 說明 |
| --- | --- | --- | --- |
| session | boolean | true | 檢查 PHP 工作階段 |
| cookie | boolean | **false** | 檢查由 JavaScript　產生的 Cookie |
| referer | boolean | true | 檢查 `HTTP_REFERER` |
| frequency | boolean | true | 檢查 `time_unit_quota` 設定 |

Cookie 過濾器預設是 false，因為您必須輸出 JavaScript 的程式碼片段到您的網站中。JavaScript 的程式碼片段由　`outputJsSnippet` 產生。

查看 `outputJsSnippet` 瞭解用法。

### setProperty

- *param* string `$key`
- *param* mixed `$value`
- *return* self

```php
$shieldon->setProperty('time_unit_quota', [
    's' => 4,
    'm' => 20, 
    'h' => 60, 
    'd' => 240
]);
```
預設的設定值：

| 鍵值 | 類別 | 值 |
| --- | --- | --- |
| time_unit_quota | array |  `['s' => 2, 'm' => 10, 'h' => 30, 'd' => 60]` |
| time_reset_limit | integer | 3600 |
| interval_check_referer | integer | 5 |
| interval_check_session | integer | 30 |
| limit_unusual_behavior | array | `['cookie' => 5, 'session' => 5, 'referer' => 10]` |
| cookie_name | string | "ssjd" |
| cookie_domain | string | " |
| cookie_value | string | "1" |

這些設定值的解釋請參考 [這裡](http://shieldon.io/zh/docs/configuration.html).

### setProperties

- *param* array `$settings`
- *return* self

```
$shieldon->setProperties($settings);
```

### setIp

- *param* string `$ip`
- *return* self

```php
// Here is an example, cature real vistor IP from CloudFlare.
$realIp = $_SERVER['HTTP_CF_CONNECTING_IP'];

// If you use a CDN serive on your website, 
// make sure to cature the real vistor IP, overwise users will get banned.
$shieldon->setIp($realIp);
```

### setMessenger

- *param* MessengerInterface `$instance`
- *return* self

```php
$apiKey = '981441296:AAGCcgv_NETMdWQCBTaMOk_yoMfax5EV7YQ';
$channel = '@your_channel';

$telegramMessenger = new \Messenger\Telegram($apiKey, $channel);

$shieldon->setMessenger($telegramMessenger);
```

### setView

- *param* string `$html` HTML 文字字串
- *param* string `$type` 頁面類別
- *return* self

```php
$htmlText = '<html>...bala...{{captcha}} ...bala...</html>';
$this->setView($htmlText, 'stop');
```

*$type*

| 類別  | 說明 |
| --- | --- | --- | --- |
| stop | 當使用者被暫時性封鎖時顯示的頁面。 | 
| limit | 當使用者因為線上人數限制，而停留顯示的頁面。 |
| deny | 當使用者在黑名單中，被拒絕的頁面。 | 

*樣版標籤*

- `{{captcha}}`: 給 **stop** 頁面用的驗證碼表單 (必要)
- `{{online_info}}`: 顯示於 **limit** 頁面的線上工作階段資訊。
- `{{lineup_info}}`: 顯示於　**limit** 排隊資訊 頁面。

### createDatabase

- *param* bool `$option` true or false [預設值: true]
- *return* self

```php
$this->createDatabase(false);
```


### ban

- *param* string `$ip` 單個 IP 位址
- *return* self

```php
$shieldon->ban('33.125.12.87');
```

### unban

- *param* string `$ip` 單個 IP 位址
- *return* self

```php
$shieldon->unban('33.125.12.87');

```

### limitSession

- *param* integer `$amount` 線上訪客的最大數量
- *param* integer `$period` 期間 （單位：秒）
- *return* self

```php
$shieldon->limitSession(500, 300);
```

## Non-chainable

### getCurrentUrl

- *since* 3.0.0
- *return* string

回傳目前的網址。
這個方法等同於系統變數 $_SERVER['REQUEST_URI']

```php
echo $shieldon->getCurrentUrl();
```

### managedBy

- *param* string `Type` managed|config|demo
- *return* void

告訴 Shieldon 被什麼形式管理。這個設定只影響防火牆面板。

```php
$list = [
    '/example/1',
    '/wp-login.php',
];

$shieldon->managedBy('demo');
```

### outputJsSnippet

- *return* string JavaScript 字串

如果 Cookie 過濾器有啟用的話，這個方法必須使用於輸出 JavaScript 程式碼片段，並放到前端網頁中。

```php
// Output this variable in your page template.
$jsCode = $shieldon->outputJsSnippet();
```

### captchaResponse

- *return* boolean

true: 驗證碼有被成功解決掉。
false: 反之亦然。

```php
$result = $this->captchaResponsse();
```

### run

- *return* integer

回應碼：

| 常數 | 值 | 原因 |
| --- | --- | --- |
|RESPONSE_DENY | 0 | 永久性封鎖。 |
|RESPONSE_ALLOW | 1 | 通過。 |
|RESPONSE_TEMPORARILY_DENY | 2 | 暫時性封鎖。 |
|RESPONSE_LIMIT | 3 | 被限制停留頁面，因為達到線上人數限制。 |
 
```php
$result = $shieldon->run();
```