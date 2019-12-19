# Zend

Zend 框架有提供兩個官方初始架構 - Zend MVC and Zend Expressive。

先不管你用那一種初始架構，這個指南應該會給您一些關於如何部署 Shieldon 防火牆的點子。不確定那一種方法是最佳作法，您可以挑一個偏好的。

![Zend 框架防火牆](https://shieldon.io/images/home/zend-framework-firewall.png)

這些點子有：

- PSR-7 中介程式。 (適用於版本 Zend 3.1.0 以前)
- PSR-15 Middleware (適用於 Zend 3.1.0 之後)
- 引導程式

```php
\Shieldon\Integration\Zend\Psr7Middleware
\Shieldon\Integration\Zend\Psr15Middleware
\Shieldon\Integration\Bootstrapper
```

如果您的 Zend 應用程式有 CSRF 防護，確定要定義一個名為 `_shieldon_csrf` 的 CSRF token 給 Shieldon 準備好的中介程式。

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

### Zend Expressive

這是一個使用 PSR-15 規範的中介程式在 Zend Expressive 架構的例子。

#### 1. 定義一個中介程式

在您的 `pipeline.php` 中，加入這一行。

```php
$app->pipe(\Shieldon\Integration\Zend\Psr15Middleware::class);
```

#### 2. 訂義一個處理程式

我們進到 `App/src/Handler` 目錄並建立一個 PHP 檔案，命名為 `FirewallPanelHandler`。

複製下方的文字，把它們貼到那個檔案裡。

```php
<?php declare(strict_types=1);

namespace App\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Zend\Diactoros\Response;

/**
 * Firewall 面板處理程式
 * 如果您有啟用 CSRF，必須確認 CSRF token 被轉傳至控制面板裡。 
 */
class FirewallPanelHandler implements RequestHandlerInterface
{
    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        $firewall = \Shieldon\Container::get('firewall');
        $controlPanel = new \Shieldon\FirewallPanel($firewall);
        $controlPanel->entry();

        return new Response();
    }
}
```

#### 3. 為防火牆面板定義路由。

在您的 `route.php` 中，加入這一行：

```php
$app->route('/firewall/panel', App\Handler\FirewallPanelHandler::class, ['GET', 'POST'], 'panel');
```

### Zend MVC

因為我不確定您用多舊版本的 Zend 框架，因此我在這個例子中捨棄用中介程式的方法，才能確定這個指南在不管多舊版本的 Zend 都能正常運作。

#### 1. 開初始化核心之前

在您的 `public/index.php` 中的這一行之後：

```php
include __DIR__ . '/../vendor/autoload.php';
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

    $firewallstorage = __DIR__ . '/../data/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```
下一個步驟是為控制面板建立一個控制器。

#### 2.  定義一個控制器

我們來建立一個控制器並命明為 `FirewallPanelController`。這是 Shieldon 防火牆的控制面板的入口。

```php
<?php

namespace Application\Controller;

use Zend\Mvc\Controller\AbstractActionController;

class FirewallPanelController extends AbstractActionController
{
    /**
     * 這是我們的防火牆面板的入口。
     *
     * @return string
     */
    public function indexAction()
    {
       // 從 Shieldon 容器中取得防火牆的實例。
       $firewall = \Shieldon\Container::get('firewall');

       // 進入防火牆面板。
       $controlPanel = new \Shieldon\FirewallPanel($firewall);
       $controlPanel->entry();
       exit;
    }
}
```

#### 3. 為防火牆面板定義路由。

在您的 `module.config.php` 中，加進程式碼如下。

```php
'firewallpanel' => [
    'type' => Literal::class,
    'options' => [
        'route'    => '/firewall/panel',
        'defaults' => [
            'controller' => Controller\FirewallPanelController::class,
            'action'     => 'index',
        ],
    ],
],
```

就是這樣囉。

您可以由 `/firewall/panel` 連接防火牆面板，在瀏覽器上打上網址：

```bash
https://for.example.com/firewall/panel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。