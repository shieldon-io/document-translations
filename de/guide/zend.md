# Zend

Zend framework officially provides two types of skeleton - Zend MVC and Zend Expressive.

No matter which skeleton you are using, this guide might give you some ideas on how to implement Shieldon Firewall, not sure which way is considered best practice to Zend, you can pick one you prefer.

![Firewall in Zend Framework](https://shieldon.io/images/home/zend-framework-firewall.png)

These ideas are:

- PSR-7 Middleware. (Prior to Zend 3.1.0)
- PSR-15 Middleware (Starting in Zend 3.1.0)
- Bootstrapper.

```php
\Shieldon\Integration\Zend\Psr7Middleware
\Shieldon\Integration\Zend\Psr15Middleware
\Shieldon\Integration\Bootstrapper
```

If your Zend application has CSRF protected, be sure to define a `_shieldon_csrf` CSRF token for Shieldon ready Middlewares.

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon
```

## Implementing

### Zend Expressive

This is an example that shows you using a PSR-15 Middleware in Zend Expressive skeleton.

#### 1. Define a Middleware.

In your `pipeline.php`, add this line:

```php
$app->pipe(\Shieldon\Integration\Zend\Psr15Middleware::class);
```

#### 2.  Define a Handler.

Let's go to `App/src/Handler` directory and create a PHP file named `FirewallPanelHandler`.

Copy the text blew, paste them into that file.

```php
<?php declare(strict_types=1);

namespace App\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Zend\Diactoros\Response;

/**
 * Firewall Panel Handler
 * If you have CSRF enabled, make sure to pass the csrf token to the control panel.
 */
class FirewallPanelHandler implements RequestHandlerInterface
{
    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        $firewall = \Shieldon\Container::get('firewall');
        $controlPanel = new \Shieldon\FirewallPanel($firewall);
        $controlPanel->entry();

        return new Response();
    }
}
```

#### 3.  Define a Route for Firewall Panel.

In your `route.php`, add this line:

```php
$app->route('/firewall/panel', App\Handler\FirewallPanelHandler::class, ['GET', 'POST'], 'panel');
```

### Zend MVC

Because that I am not sure how old version of Zend framework you are using. Therefore I decide to get rid of Middleware to make sure this guide will work with the most versions of Zend.

#### 1. Before initializing Core

In your `public/index.php` under this line:

```php
include __DIR__ . '/../vendor/autoload.php';
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

    $firewallstorage = __DIR__ . '/../data/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

The next step is to create a controller for control panel.


#### 2.  Define a Controller.

Let's create a controller and named it with `FirewallPanelController`. Ths is the entry point of our Shieldon Firewall's controll panel.

```php
<?php

namespace Application\Controller;

use Zend\Mvc\Controller\AbstractActionController;

class FirewallPanelController extends AbstractActionController
{
    /**
     * The entry point of the Firewall Panel.
     *
     * @return string
     */
    public function indexAction()
    {
       // Get Firewall instance from Shieldon Container.
       $firewall = \Shieldon\Container::get('firewall');

       // Get into the Firewall Panel.
       $controlPanel = new \Shieldon\FirewallPanel($firewall);
       $controlPanel->entry();
       exit;
    }
}
```

#### 3.  Define a Route for Firewall Panel.

In your `module.config.php`, add the code as below.
```php
'firewallpanel' => [
    'type' => Literal::class,
    'options' => [
        'route'    => '/firewall/panel',
        'defaults' => [
            'controller' => Controller\FirewallPanelController::class,
            'action'     => 'index',
        ],
    ],
],
```

That's it.

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.