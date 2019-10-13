# Getting Started

## Server Requirements

Before using Shieldon Firewall on your Web application you must meet the basic server requirements below:

- PHP >= 7.1.0
- [Ctype](https://www.php.net/book.ctype) PHP Extension
- [JSON](https://www.php.net/book.json) PHP Extension
- [PDO](https://www.php.net/book.pdo) PHP Extension *(Required only if you would like to use MySQL, SQLite driver.)*
- [Redis](https://github.com/phpredis/phpredis) PHP Extension *(Required only if you would like to use Redis driver.)*

## Installation

Use PHP Composer:
```shell
composer require terrylinooo/shieldon
```
Or, download it and include the Shieldon autoloader.
```php
require 'Shieldon/src/autoload.php';
```

## Implementing


### Popular Frameworks

Here are the guides of integrating with the popular PHP frameworks.

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

### Other Frameworks

Implementing Shieldon on other framework is as easy as well.

```php
// Notice that this directory must be writable.
$writable = __DIR__ . '/../shieldon';

// Initialize Fireall instane.
$firewall = new \Shieldon\Firewall($writable);
$firewall->run();
```
Place this code section in a beginning section of your project.
The beginning section might be the `index.php`<sub>(1)</sub>, `Middleware` or `Parent Controller`.

<sup>(1)</sup> index.php is the entry point for all requests entering your application in most frameworks such as Laravel, CodeIgniter, Slim, WordPress and more.


```php
// Get Firewall instance from Shieldon Container.
$firewall = \Shieldon\Container::get('firewall');

// Get into the Firewall Panel.
$controlPanel = new \Shieldon\FirewallPanel($firewall);
$controlPanel->entry();
```

Put the code on the Controller and the URL that only you know.
Although it has a basic login protection.
