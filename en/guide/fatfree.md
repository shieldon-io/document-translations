# Fat-Free

Not like other frameworks, Fat-Free is an extremely light-weight PHP framework.

## Install

Use PHP Composer:

```php
composer require terrylinooo/shieldon
```

Or, download it and include the Shieldon autoloader.
```php
require 'Shieldon/src/autoload.php';
```

## Implement

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

Now, you can access the Firewall Panel via URL:

```bash
https://for.example.com/firewall/panel
```

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.