# PHPixie

PHPixie 是一個微型框架，它的版本為 3 的文件很模糊，漏掉很多重要的文件例如路由設定，而且我不想看他們的影片。（閱讀文件比看影片快 100 倍，您也這樣認為吧？）因此這個指南只有一個關於如何部署 Shieldon 防火牆在您的 PHPixie 應用程式的點子。

![PHPixie 框架防火牆](https://shieldon.io/images/home/phpixie-framework-firewall.png)

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

#### 1. 在初始化 PHPixie 之前。

在您的 `web/index.php` 中，在這一行之後：

```php
require_once(__DIR__.'/../vendor/autoload.php');
```
加入以下的程式碼：

```php
// 部署 Shieldon 防火牆。
new \Shieldon\Integration\Bootstrapper(
    $storage = '',
    $fpRequestURI = '/firewall/panel'
);
```
第一個參數是目錄，Shieldon 防火牆將會產生它的資料和記錄檔在裡面。第二個參數是網址路徑，讓您可以連進防火牆面板。

所以，您的 `index.php` 將會長的像這樣：

```php
<?php

require_once(__DIR__.'/../vendor/autoload.php');

// 部署 Shieldon 防火牆。
new \Shieldon\Integration\Bootstrapper(
    $storage = '',
    $fpRequestURI = '/firewall/panel'
);

$framework = new Project\Framework();
$framework->registerDebugHandlers();
$framework->processHttpSapiRequest();
```

就是這樣囉。

您可以由 `/firewall/panel` 連接防火牆面板，在瀏覽器上打上網址：

```
https://for.example.com/firewall/panel
```

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。

如果在設定區塊中的 `守護進程` 有啟用的話，Shieldon 將會開始監看您的網站，請確定您已經把設定值都設定正確。