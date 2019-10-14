# Symfony

Symfony is a set of reusable PHP components and a PHP framework to build web applications, APIs, microservices and web services.

This guide has been tested successfully in version `4.3`, I think it can be used older versions as well.

Symfony doesn't have a middleware concept, therefore you can create a parent controller to implement Shieldon Firewall just like the steps in our [CodeIgniter guide](https://shieldon.io/en/guide/codeigniter.html).

If you don't like to initialize Shieldon Firewall in a parent controller, here are the steps that called Bootstrap mode you can try.

## Installation

Use PHP Composer:

```php
composer require terrylinooo/shieldon
```

## Implementing

### Bootstrap

#### 1. Before initializing Kernal

In your `config/bootstrap.php`, after this line:

```php
require dirname(__DIR__).'/vendor/autoload.php';
```
Add the following code:

```php
/*
|--------------------------------------------------------------------------
| Run The Shieldon Firewall
|--------------------------------------------------------------------------
|
| Shieldon Firewall will watch all HTTP requests coming to your website.
| Running Shieldon Firewall before initializing Laravel will avoid possible
| conflicts with Laravel's built-in functions.
*/

if (isset($_SERVER['REQUEST_URI'])) {

    // Notice that this directory must be writable.
    $firewallstorage = __DIR__ . '/../storage/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

#### 2.  Define a Route for Firewall Panel.

Create a controller named `FirewallPanelController` by typing the following command.

```bash
php bin/console make:controller FirewallPanelController
```

Add several lines in the `FirewallPanelController` controller class:

```php
$firewall = \Shieldon\Container::get('firewall');
$controlPanel = new \Shieldon\FirewallPanel($firewall);
$controlPanel->entry();
exit;
```

If you have CSRF enabled, add these lines:

```php
$csrf = $this->container->get('security.csrf.token_manager');
$token = $csrf->refreshToken('key');
```

The full example will look like this:

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Routing\Annotation\Route;

class FirewallPanelController extends AbstractController
{
    /**
     * @Route("/firewall/panel", name="firewall_panel")
     */
    public function index()
    {
        $firewall = \Shieldon\Container::get('firewall');
        $controlPanel = new \Shieldon\FirewallPanel($firewall);

        // If your have `symfony/security-csrf` installed.
        $csrf = $this->container->get('security.csrf.token_manager');
        $token = $csrf->refreshToken('key');

        $controlPanel->csrf('_token', $token);
        $controlPanel->entry();
        exit;
    }
}
```

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/firewall/panel
```