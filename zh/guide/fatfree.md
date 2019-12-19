# Fat-Free

不像其它的框架，Fat-Free 是極輕量的 PHP 框架。

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

假設您的程式碼應該是長這樣。

```php
<?php

require dirname(__DIR__) . '/vendor/autoload.php';

$f3 = \Base::instance();
$f3->route('GET /',
    function() {
        echo 'Hello, world!';
    }
);
$f3->run();

```

### 步驟

#### 1. 初始化 Shieldon 防火牆

在這一行之後：

```php
require dirname(__DIR__) . '/vendor/autoload.php';
```
加入以下程式碼

```php
// 運行 Shieldon 防火牆。
new \Shieldon\Integration\Bootstrapper();
```

請建立一個可寫入的目錄並命名為 `shieldon`，位於上一層目錄，Shieldon 防火牆在那兒存放資料。

#### 2. 為防火牆面板定義路由。

```php
// Shieldon 防火牆的進入點。
$f3->route('GET|POST /firewall/panel/', function() {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->entry();
    exit;
});
```

就是這樣囉。

現在，您可以連接上防火牆面板，透過網址：

```
https://for.example.com/firewall/panel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。
