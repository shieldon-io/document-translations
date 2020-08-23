# Fuel

FuelPHP is a simple, flexible, community driven PHP web framework.

![Firewall in Fuel Framework](https://shieldon.io/images/home/fuel-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon ^2
```

## Implementing

### Steps

#### 1. Before initializing Core

In your `fuel/app/bootstrap.php`, after this line:

```php
require COREPATH.'bootstrap.php';
```
Add the following code:

Example:

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

    // This directory must be writable.
    // We put it in the `fuel/app/tmp` directory.
    $storage = __DIR__ . '/tmp/shieldon_firewall';

    $firewall = new \Shieldon\Firewall\Firewall();
    $firewall->configure($storage);
    $firewall->controlPanel('/firewall/panel');

    $response = $firewall->run();

    if ($response->getStatusCode() !== 200) {
        $httpResolver = new \Shieldon\Firewall\HttpResolver();
        $httpResolver($response);
    }
}
```

Please make sure that `$storage` directory is existed and writable.

#### 2.  Define a Route for Firewall Panel.

Now, modify your `fuel/app/config/routes.php` and add the following code.

Example:

```php
'firewall/panel(:everything)' => function () {
    $panel = new \Shieldon\Firewall\Panel();
    $panel->entry();
},
```

The full example might look like this.

Example:

```php
return array(
    '_root_'  => 'welcome/index',  // The default route
    '_404_'   => 'welcome/404',    // The main 404 route
    
    'hello(/:name)?' => array('welcome/hello', 'name' => 'hello'),

    'firewall/panel(:everything)' => function () {
        $panel = new \Shieldon\Firewall\Panel();
        $panel->entry();
    },
);
```

That's it.

## Control Panel

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

```bash
https://yoursite.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.
