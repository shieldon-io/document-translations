# Yii

在這個指南中，我會給您一些關於如何部署 Shieldon 防火牆在您的 Yii 應用程式的點子。

![Yii 框架防火牆](https://shieldon.io/images/home/yii-framework-firewall.png)

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

### Yii 2

#### 1. 開初始化核心之前

在您的 `config/bootstrap.php` 中，在這一行之後：

```php
require __DIR__ . '/../vendor/yiisoft/yii2/Yii.php';
```

加入以下的程式碼：

```php
/*
|--------------------------------------------------------------------------
| 運行 Shieldon 防火牆
|--------------------------------------------------------------------------
|
| Shieldon 防火牆將開始監看所有進入您網站的 HTTP 請求。
| 在初始化 Yii 之前運行 Shieldon 防火牆會避免和 Yii 內建功能的衝突。
| 
*/

if (isset($_SERVER['REQUEST_URI'])) {

    // 注意這個目錄必須可寫入。
    $firewallstorage = __DIR__ . '/../runtime/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

#### 2. 為防火牆面板定義路由。

建立一個控制器叫作 `FirewallPanelController`。

內容如下：

```php
<?php

namespace app\controllers;

use yii\web\Controller;

class FirewallPanelController extends Controller
{
    public function beforeAction($action)
    {
        $this->enableCsrfValidation = false;

        return parent::beforeAction($action);
    }

    /**
     * 這是我們的防火牆面板的入口。
     *
     * @return string
     */
    public function actionIndex()
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

確認 `enablePrettyUrl` 在您的 `config/web.php` 是啟用狀態。

```php
'urlManager' => [
    'enablePrettyUrl' => true,
    'showScriptName' => false,
    'rules' => [
    ],
],
```

您可以由 `/firewall-panel` 連接防火牆面板，在瀏覽器上打上網址：

```bash
https://for.example.com/firewall-panel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。