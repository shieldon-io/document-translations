# Laravel

這個指南幫助您解決部署 Shieldon 防火牆在您的 Laravel 應用程式的疑問。這些指引並非唯一可以達成的方法，只是給您一些點子。

以下的步驟已經在 Laravel 5 及 6 中測試過。

![Laravel 框架防火牆](https://shieldon.io/images/home/laravel-framework-firewall.png)

## 安裝

使用 PHP Composer:

```php
composer require shieldon/shieldon
```

或者下載後引入 Shieldon 自動載入器。

```php
require 'Shieldon/autoload.php';
```

部署 Shieldon 防火牆在您的網站應用程式上是相當簡單的藉著使用 Shieldon 面板，部署 Shieldon 防火牆在您的網站應用程式上是相當簡單的。我強烈建議您選用這個方法。

## 部署

對於 Laravel 愛好者，您可以選擇 **中介程式** 或 **引導程式** 來部署 Shieldon 防火牆在您的網站應用程式上。我個人偏好引導程式。

### 中介程式

#### 1. 定義一個中介程式。

定義一個中介程式名為 `ShieldonFirewall`

```bash
php artisan make:middleware ShieldonFirewall
```
在 `ShieldonFirewall` 中介程式的類別中加入以下程式碼:

```php
$firewall = new \Shieldon\Firewall(storage_path('shieldon'));

// 轉傳 Laravel CSRF Token 到驗證碼表單。
$firewall->getShieldon()->setCaptcha(new \Shieldon\Captcha\Csrf([
    'name' => '_token',
    'value' => csrf_token(),
]));

$firewall->restful();
$firewall->run();
```

#### 2. 註冊一個中介程式別名。

修改 `app/Http/Kernel.php` 然後加入這一樣到 `$routeMiddleware` 屬性中。
```php
'firewall' => \App\Http\Middleware\ShieldonFirewall::class,
```

#### 3. 為防火牆面板定義路由。

我們需要一個控制器以進入 Shieldon 防火牆控制面板，所以呢...

```php
Route::any('/your/secret/place/', function() {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf('_token', csrf_token());
    $controlPanel->entry();
})->middleware('firewall');
```

Shieldon 將會開始監看您的網站，如果在設定區塊中的 `守護進程` 有啟用的話。

#### 4. 指派 `firewall` 中介程式到路由。

指派 `firewall` 中介程式到任何您想要保護的路由。例如：

```php
Route::get('/', function () {
    return view('welcome');
})->middleware('firewall');
```

### 引導程式

這就是我所說，我偏好的方法，因為比較少的步驟，且會避免可能發生和 Laravel 內建功能的衝突。

#### 1. 在初始化 $app 之前。

在您的 `bootstrap/app.php` 中，在`<?php`, 之後加入以下程式碼。

```php
/*
|--------------------------------------------------------------------------
| 運行 Shieldon 防火牆
|--------------------------------------------------------------------------
|
| Shieldon 防火牆將開始監看所有進入您網站的 HTTP 請求。
| 在初始化 Laravel 之前運行 Shieldon 防火牆會避免和 Laravel 內建功能的衝突。
| 
*/

if (isset($_SERVER['REQUEST_URI'])) {

    // 注意這個目錄必須可寫入。
    $firewallstorage = __DIR__ . '/../storage/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

#### 2. 為防火牆面板定義路由。

```php
Route::any('/your/secret/place/', function() {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf('_token', csrf_token());
    $controlPanel->entry();
});
```

如果您採用這個方式，Shieldon 防火牆將會在全域範圍運行，不過不用擔心，您可以針對想要 Shiedon 防火牆忽略的網址設定設定排除列表。

就是這樣囉。

現在，您可以連接上防火牆面板，透過網址：

```
https://for.example.com/your/secret/place/
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。
