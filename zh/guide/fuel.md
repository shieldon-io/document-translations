# Fuel

FuelPHP 是一套簡單，富有彈性, 社群驅動的 PHP 網站框架。

![Fuel 框架防火牆](https://shieldon.io/images/home/fuel-framework-firewall.png)

## 安裝

使用 PHP Composer:

```php
composer require shieldon/shieldon
```

或者下載後引入 Shieldon 自動載入器。

```php
require 'Shieldon/autoload.php';
```

## 部署

### 步驟

#### 1. 在初始化核心之前。

在您的 `fuel/app/bootstrap.php` 中，下面這一行之後：

```php
require COREPATH.'bootstrap.php';
```
加入以下的程式碼：

```php
/*
|--------------------------------------------------------------------------
| 運行 Shieldon 防火牆
|--------------------------------------------------------------------------
|
| Shieldon 防火牆將開始監看所有進入您網站的 HTTP 請求。
|
*/

if (isset($_SERVER['REQUEST_URI'])) {

	// 注意這個目錄必須是可寫入。
	// 我們把它放在 `fuel/app/tmp` 目錄中。
    $firewallstorage = __DIR__ . '/tmp/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

#### 2. 為防火牆面板定義路由

現在，修改您的 `fuel/app/config/routes.php` 檔案且加入以下的程式碼。

```php
'firewall/panel' => function () {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->entry();
    exit;
}
```

就是這樣囉。

您可以由 `/firewall/panel` 連接防火牆面板，在瀏覽器上打上網址：

```
https://for.example.com/firewall/panel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。