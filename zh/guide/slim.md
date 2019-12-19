# Slim

Slim 是我喜愛的框架其中之一，由於 Slim 是一個微型的框架，部署 Shieldon 防火牆也是很容易的。話不多說，我們開始使用吧。

![Slim 框架防火牆](https://shieldon.io/images/home/slim-framework-firewall.png)

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

### Slim 3

#### 中介程式

Shieldon 支援許多受歡迎的 PHP 框架，依循它們的設計模式，而 Slim 是其中之一，所以我們已經準備好一個可使用的 Slim 3 的中介程式。

```php
$app->add(new \Shieldon\Integration\Slim\Slim3Middleware);
```

提醒：如果您有部署 Slim-Csrf 中介程式，確定一下順序應該像這樣：

```php
$app->add(new \Shieldon\Integration\Slim\Slim3Middleware);
$app->add(new \Slim\Csrf\Guard);
```

提醒: Slim-Csrf 已經不再支援 Slim 3，如果您要用在 Slim 3，確定裝的是舊版本：

```bash
composer require slim/csrf:0.8.3
```

#### 路由

這個路由是防火牆面板的入口。

```php
$app->map(['GET', 'POST'], '/example/fiewall/panel', function (Request $request, Response $response, array $args) {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf([
        'csrf_name' => $request->getAttribute('csrf_name'),
        'csrf_value' => $request->getAttribute('csrf_value')
    ]);
    $controlPanel->entry();
});
```

### Slim 4

#### 中介程式

在第一個位置載入 Slim4Middleware。

```php
$app->add(new \Shieldon\Integration\Slim\Slim4Middleware());
```

因此,，您的 middleware.php 也許長的像是這樣：

```php
return function (App $app) {
    $app->add(new \Shieldon\Integration\Slim\Slim4Middleware());
    $app->add(SessionMiddleware::class);
};
```

#### 路由

這個路由是防火牆面板的入口。

```php
$app->map(['GET', 'POST'], '/example/fiewall/panel', function (Request $request, Response $response, array $args) {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf([
        'csrf_name' => $request->getAttribute('csrf_name'),
        'csrf_value' => $request->getAttribute('csrf_value')
    ]);
    $controlPanel->entry();

    return $response;
});
```

確認把您所有的路由改成支援 Post 模式以讓驗證碼表單可以正常運作，不然您會遇到這種錯誤。

```json
{
    statusCode: 405,
    error: {
        type: "NOT_ALLOWED",
        description: "Method not allowed."
    }
}
```

#### 引導程式

這是另一種方法可以避免改變支援的模式。它是位於 `Shieldon\Integration` 命名空間的 `Bootstrapper` 類別。

在 `public/index.php` 中，找這一行：

```php
require __DIR__ . '/../vendor/autoload.php';
```
取而代之：

```php
require __DIR__ . '/../vendor/autoload.php';

// 部署 Shieldon 防火牆。
new \Shieldon\Integration\Bootstrapper();
```

大致上是這樣囉。

*提醒*：

為了預防 `session_start` 的衝突，請在您的  `SessionMiddleware` 中加入安全的判斷式：

```php
if (session_status() === PHP_SESSION_NONE) {
    session_start();
}
```

就是這樣囉。

現在，您可以連接上防火牆面板，透過網址：

```
https://for.example.com/example/fiewall/panel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。