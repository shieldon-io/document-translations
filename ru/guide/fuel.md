# Fuel

FuelPHP is a simple, flexible, community driven PHP web framework.

![Firewall in Fuel Framework](https://shieldon.io/images/home/fuel-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon
```

Or, download it and include the Shieldon autoloader.
```php
require 'Shieldon/autoload.php';
```

## Implementing

### Steps

#### 1. Before initializing Core

In your `fuel/app/bootstrap.php`, after this line:

```php
require COREPATH.'bootstrap.php';
```
Add the following code:

```php
/*
|--------------------------------------------------------------------------
| Run The Shieldon Firewall
|--------------------------------------------------------------------------
|
| Shieldon Firewall will watch all HTTP requests coming to your website.
|
*/

if (isset($_SERVER['REQUEST_URI'])) {

	// Notice that this directory must be writable.
	// We put it in the `fuel/app/tmp` directory.
    $firewallstorage = __DIR__ . '/tmp/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

#### 2.  Define a Route for Firewall Panel.

Now, modify your `fuel/app/config/routes.php` and add the following code.

```php
'firewall/panel' => function () {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->entry();
    exit;
}
```

That's it.

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.
