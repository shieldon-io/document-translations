# CodeIgniter

CodeIgniter 是一套輕量的 MVC 框架。第 3 版和第 4 版在架構上有很大差異，所以先提 CodeIgniter 3。

在這個指南中，我將和您分享提示關於部署 Sheildon 防火牆在您的 CodeIgniter 應用程式。

![CodeIgniter 框架防火牆](https://shieldon.io/images/home/codeigniter-framework-firewall.png)

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

### CodeIgniter 3

CodeIgniter 3 使用一種超級單例實例稱為 `CI_Controller` 來處理它的 MVC (Model-View-Controller) 架構模式。

我強烈建議您在 `core` 資料夾中建立一個 MY_Controller 做為母控制器然後放進初始化的程式碼。

#### 1. MY_Controller

我們在 `core` 資料夾中建立一個 MY_Controller.php 。

```php
class Core_Controller extends CI_Controller
{
    /**
     * 建構子。
     */
    public function __construct()
    {
        parent::__construct();
    }
}
```

#### 2. 初始化防火牆實例

放進初始化的程式碼到建構子中，則任何繼承 MY_Controller 的控制器將會有被初始化後的 Shieldon 防火牆以及 `$this->firewall()` 可使用。

```php
class MY_Controller extends CI_Controller
{
    /**
     * 建構子。
     */
    public function __construct()
    {
        parent::__construct();

        // Composer 自動載入器。
        require_once APPPATH . '../vendor/autoload.php';

        // 此目錄必須可寫入。
        $storage =  APPPATH . 'cache/shieldon';
        $firewall = new \Shieldon\Firewall($storage);
    }

    /**
     * 使用此方法的網頁受到防火牆保護。
     */
    public function firewall()
    {
        $firewall = \Shieldon\Container::get('firewall');
        $firewall->run();
    }
}
```

*提醒*

為了安全性起見，system 和 application 資料夾都應該放在網站根目錄的上一層，則他們無法直接經由瀏覽器讀取。

如果你的 application 目錄放在和 index.php 同一層，請將 `$storage` 移往安全的位置，例如：

```php
$storage =  APPPATH . '../shieldon';
```

#### 3. 為防火牆面板定義一個控制器

我們需要一個控制器以進入 Shieldon 防火牆控制面板，在這個例子中，我們定義一個名為 `Example` 的控制器。

```php
class Example extends MY_Controller
{
    /**
     * 建構子。
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * 這是我們的防火牆面板的入口。
     */
    public function ControllPanel()
    {
        // 從 Shieldon 容器中取得防火牆的實例。
        $firewall = \Shieldon\Container::get('firewall');

        // 進入防火牆面板。
        $controlPanel = new \Shieldon\FirewallPanel($firewall);
        $controlPanel->entry();
    }
}
```

現在，您可以連接上防火牆面板，透過網址：

```plaintext
http://yoursite.com/example/controllPanel/
```

Shieldon 將會開始監看您的網站，如果在設定區塊中的 `守護進程` 有啟用的話。

### CodeIgniter 4

### 1. 註冊一個過濾器。

在您的 `app/Config/Filter.php` 檔案中，加入以下程式碼到 `$aliases` 屬性。

```php
'firewall' => \Shieldon\Integration\CodeIgniter\CI4Middleware::class,
```

然後，加入字串 *firewall* 到 `$globals` 屬性的 `before` 陣列中。


```php
public $globals = [
    'before' => [
        'firewall'
    ],
];
```

### 2. 為防火牆面板定義一個控制器

```php
<?php 

namespace App\Controllers;

class FirewallPanel extends BaseController
{
    public function index()
    {
        // 從 Shieldon 容器中取得防火牆的實例。
        $firewall = \Shieldon\Container::get('firewall');

        // 進入防火牆面板。
        $controlPanel = new \Shieldon\FirewallPanel($firewall);
        $controlPanel->csrf(csrf_token(), csrf_hash());
        $controlPanel->entry();
    }
}
```

就是這樣囉。

您可以由 `/firewallPanel` 連接防火牆面板，在瀏覽器上打上網址：

```
https://for.example.com/firewallPanel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。