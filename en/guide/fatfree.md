# Fat-Free

Not like other frameworks, Fat-Free is an extremely light-weight PHP framework.

![Firewall in FatFree Framework](https://shieldon.io/images/home/fatfree-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon ^2
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
// Prevent error when running in CLI environment.
if (isset($_SERVER['REQUEST_URI'])) {

    // This directory must be writable.
    $storage = dirname($_SERVER['SCRIPT_FILENAME']) . '/../shieldon_firewall';

    $firewall = new \Shieldon\Firewall\Firewall();
    $firewall->configure($storage);
    $response = $firewall->run();

    if ($response->getStatusCode() !== 200) {
        $httpResolver = new \Shieldon\Firewall\HttpResolver();
        $httpResolver($response);
    }
}
```

Please create a wriable directory named it with `shieldon` at above directory, Shieldon Firewall stores data in that.


#### 2.  Define a Route for Firewall Panel.

```php
$f3->route('GET|POST /firewall/panel/', function() {
    $panel = new \Shieldon\Firewall\Panel();

    // The entry point must be the same as the route defined.
    $panel->entry('/firewall/panel/');
});
```

That's it.

Now, you can access the Firewall Panel via URL:

```bash
https://for.example.com/firewall/panel/
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.
