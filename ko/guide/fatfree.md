# Fat-Free

Not like other frameworks, Fat-Free is an extremely light-weight PHP framework.

![Firewall in FatFree Framework](https://shieldon.io/images/home/fatfree-framework-firewall.png)

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

Assuming your code is supposed to be this.

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

### Steps

#### 1. Initialize Shieldon Firewall

After this line:

```php
require dirname(__DIR__) . '/vendor/autoload.php';
```
Add the following code:

```php
// Run The Shieldon Firewall
new \Shieldon\Integration\Bootstrapper();
```

Please create a wriable directory named it with `shieldon` at above directory, Shieldon Firewall stores data in that.


#### 2.  Define a Route for Firewall Panel.

```php
// The Shieldon Firewall's entry point.
$f3->route('GET|POST /firewall/panel/', function() {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->entry();
    exit;
});
```

That's it.

Now, you can access the Firewall Panel via URL:

```bash
https://for.example.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.

