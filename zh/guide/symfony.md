# Symfony

Symfony 是用來建造網站應用程式、API、微服務和網站服務的一組可重用的 PHP 組件和 PHP 框架。

這個指南已經成功在 `4.3` 版成功測試，但我認為它也可用在較舊的版本上。

Symfony 沒有中介程式的概念，因此您可以建立一個母控制器來部署 Shieldon 防火牆，就像我們的　[CodeIgniter 指南](https://shieldon.io/en/guide/codeigniter.html) 一樣。

如果您不喜歡初始化 Shieldon 防火牆在母控制器，這裡的步驟，為引導程式的模式，您可以試試看。

![Symfony 框架防火牆](https://shieldon.io/images/home/symfony-framework-firewall.png)

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

### 引導程式

#### 1. 開初始化核心之前

在您的 `config/bootstrap.php` 中，在這一行之後：

```php
require dirname(__DIR__).'/vendor/autoload.php';
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
    $firewallstorage = __DIR__ . '/../storage/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

#### 2. 為防火牆面板定義路由。

建立一個控制器叫作 `FirewallPanelController` 由以下命令產生：

```bash
php bin/console make:controller FirewallPanelController
```

在類別 `FirewallPanelController` 控制器中加入幾行：

```php
$firewall = \Shieldon\Container::get('firewall');
$controlPanel = new \Shieldon\FirewallPanel($firewall);
$controlPanel->entry();
exit;
```

如果您已經有啟用 CSRF 保護，加入這些行：

```php
$csrf = $this->container->get('security.csrf.token_manager');
$token = $csrf->refreshToken('key');
```

完整的範例程式碼會看起來像這樣：

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Routing\Annotation\Route;

class FirewallPanelController extends AbstractController
{
    /**
     * @Route("/firewall/panel", name="firewall_panel")
     */
    public function index()
    {
        $firewall = \Shieldon\Container::get('firewall');
        $controlPanel = new \Shieldon\FirewallPanel($firewall);

        // 如果您有安裝 `symfony/security-csrf` 的話。
        $csrf = $this->container->get('security.csrf.token_manager');
        $token = $csrf->refreshToken('key');

        $controlPanel->csrf('_token', $token);
        $controlPanel->entry();
        exit;
    }
}
```

就是這樣囉。

您可以由 `/firewall/panel` 連接防火牆面板，在瀏覽器上打上網址：

```bash
https://for.example.com/firewall/panel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。