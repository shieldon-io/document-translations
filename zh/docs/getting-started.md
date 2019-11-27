# 開始使用

## 伺服器需求

在開始使用 Shieldon 防火牆到您的網站應用程式之前，您必須符合以下最低的伺服器需求：

- PHP >= 7.1.0
- [Ctype](https://www.php.net/book.ctype) PHP 擴充套件
- [JSON](https://www.php.net/book.json) PHP 擴充套件
- [PDO](https://www.php.net/book.pdo) PHP 擴充套件 *(只在您使用 MySQL, SQLite 驅動器的時候需要。)*
- [Redis](https://github.com/phpredis/phpredis) PHP 擴充套件 *(只在您使用 Redis 驅動器的時候需要。)*

## 安裝

使用　PHP Composer:
```shell
composer require shieldon/shieldon
```
或者，下載或引入 Shieldon 自動載入器。
```php
require 'Shieldon/autoload.php';
```

## 部署

### 人氣框架

這裡是整合這些人氣框架的指南。

- [Laravel](https://shieldon.io/en/guide/laravel.html)
- [Symfony](https://shieldon.io/en/guide/symfony.html)
- [CodeIgniter](https://shieldon.io/en/guide/codeigniter.html)
- [CakePHP](https://shieldon.io/en/guide/cakephp.html)
- [Yii](https://shieldon.io/en/guide/yii.html)
- [Zend](https://shieldon.io/en/guide/zend.html)
- [Slim](https://shieldon.io/en/guide/slim.html)
- [Fat-Free](https://shieldon.io/en/guide/fatfree.html)
- [Fuel](https://shieldon.io/en/guide/fuel.html)
- [PHPixie](https://shieldon.io/en/guide/phpixie.html)

### 其它框架

部署 Shieldon 到其它框架也很簡單唷。

```php
// 注意一下，這個資料夾必須是可寫入。
$writable = __DIR__ . '/../shieldon';

// 初始化 Shieldon 實例。
$firewall = new \Shieldon\Firewall($writable);
$firewall->run();
```

把這一段程式碼區塊放到您的專案的起始區塊位置。

起始區塊必須是 `index.php`<sub>(1)</sub>, `Middleware` or `Parent Controller`.

<sup>(1)</sup> 在大部份的框架，例如 Laravel, CodeIgniter, Slim, WordPress 及更多， index.php 是所有請求的進入點。

```php
// Get Firewall instance from Shieldon Container.
$firewall = \Shieldon\Container::get('firewall');

// Get into the Firewall Panel.
$controlPanel = new \Shieldon\FirewallPanel($firewall);
$controlPanel->entry();
```

即使有基本的登入保護，把程式碼放在只有您知道網址的 Controller 上。

