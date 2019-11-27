# 自建 WAF

假如您想要建立自己的 WAF，藉由組裝 Shieldon 套件裡這些公開的 API，您就能夠建立一個類似 Shieldon 防火牆的 WAF。

這裡是一個例子讓您瞭解 Shieldon 的運作方式。接著您可以手動地部署 Shieldon 到您的網站應用程式上。

## 生命週期表

以下是一個是示意圖，描述 Shieldon 實例的生命週期。您不用全盤瞭解目前運作的一切，但是當您想要自定義組件或 CAPTCHA 模組或更多時，它將是一個有用的參考。

![生命週期表](https://i.imgur.com/9RLHFG1.png)

## 提示

### 1. 初始化 Shieldon 實例

```php
$shieldon = new \Shieldon\Shieldon();
```

### 2. 設定一個資料驅動器

在這個例子。我用 SQLite 作為資料驅動器。

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);
$shieldon->setDriver(new \Shieldon\Driver\SqliteDriver($pdoInstance));
```

### 3. 設定組件

Shieldon 組件是規則集合，用來永久地許可或者拒絕工作階段。在這個例子中，我們載入 TrustedBot 組件來許可受歡迎的搜尋引擎，機器人不在規則集中的話，將會進入檢查的處理過程。（接下來的組件及過濾器）。

```php
$shieldon->setComponent(new \Shieldon\Component\TrustedBot());
```

### 4. 設定頻道 *(非必要)*

您可以忽略掉這個設定，如果您只使用一個 Shieldon 在您的網站應用程式上。此設定是為了多實例用。

```
$shieldon->setChannel('web_project');
```

### 5. 限制線上工作階段的數字 *(非必要)*

只許可 10 個工作階段瀏覽目前的網頁。預設的過期時間是 300 秒。

```
$shieldon->limitSession(10, 300);
```

### 6. 載入驗證碼模組

設定一個驗證碼服務。舉例： Google reCAPTCHA

```
$shieldon->setCaptcha(new \Shieldon\Captcha\Recaptcha([
    'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
    'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
]));
```

### 7. 開始報護您的網站

```
$result = $shieldon->run();

if ($result !== $shieldon::RESPONSE_ALLOW) {
    if ($shieldon->captchaResponse()) {

        // 解封目前的工作階段
        $shieldon->unban();
    }
    // 輸出結果頁，HTTP 回應碼設為 200。
    $shieldon->output(200);
}
```

就是這樣囉。

## 防火牆面板

即使您沒有使用防火牆實例，但您仍可以使用防火牆面板來檢示數據和圖表。

試試以下的程式碼：

```
$shieldon = \Shieldon\Container::get('shieldon');

// 進入防火牆面板
$controlPanel = new \Shieldon\FirewallPanel($shieldon);
$controlPanel->entry();
```

用預設的帳號及密碼登入即可。