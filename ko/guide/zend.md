# Zend

Zend framework officially provides two types of skeleton - Zend MVC and Zend Expressive.

No matter which skeleton you are using, this guide might give you some ideas on how to implement Shieldon Firewall, not sure which way is considered best practice to Zend, you can pick one you prefer.

![Firewall in Zend Framework](https://shieldon.io/images/home/zend-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon ^2
```

This will also install dependencies built for Shieldon:

- [shieldon/psr-http](https://github.com/terrylinooo/psr-http) The PSR-7, 15, 17 Implementation with full documented and well tested.
- [shieldon/event-dispatcher](https://github.com/terrylinooo/event-dispatcher) The simplest event dispatcher.
- [shieldon/web-security](https://github.com/terrylinooo/web-security) The collection of functions about web security.
- [shieldon/messenger](https://github.com/terrylinooo/messenger) The collection of modules of sending message to third-party API or service, such as Telegram, Line, RocketChat, Slack, SendGrid, MailGun and more...

## Implementing

### Zend Expressive

This is an example that shows you using a PSR-15 Middleware in Zend Expressive skeleton.

#### 1. Register a middleware.

There is a [integration class](https://github.com/terrylinooo/shieldon/blob/2.x/src/Firewall/Integration/ZendPsr15.php) ready for Zend Expressive.

In your `pipeline.php`, add this line:

Example:

```php
$app->pipe(\Shieldon\Firewall\Integration\ZendPsr15:class);
```

#### 2.  Define a handler.

Let's go to `App/src/Handler` directory and create a PHP file named `FirewallPanelHandler`.

Copy the text blew, paste them into that file.

Example:

```php
<?php

declare(strict_types=1);

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
        $panel = new \Shieldon\Firewall\Panel();
        $panel->entry();

        return new Response();
    }
}
```

#### 3.  Define a route for firewall panel.

In your `route.php`, add this line:

Example:
```php

// Begin - Shieldon Firewall

$app->route('/firewall/panel/', App\Handler\FirewallPanelHandler::class, ['GET', 'POST']);

foreach(\Shieldon\Firewall\Panel::getRoutes() as $route) {
    $app->route("/firewall/panel/$route/", App\Handler\FirewallPanelHandler::class, ['GET', 'POST']);
}

// End - Shieldon Firewall
```

That's it.

### Zend MVC

I am not sure how old version of Zend framework you are using, therefore I decide to get rid of middleware to make sure this guide will work with the most versions of Zend.

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

    // This directory must be writable.
    $storage = dirname($_SERVER['SCRIPT_FILENAME']) . '/../shieldon_firewall';

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

The next step is to create a controller for control panel.


#### 2.  Define a controller.

Let's create a controller and named it with `FirewallController`.

```php
<?php

namespace Application\Controller;

use Zend\Mvc\Controller\AbstractActionController;

class FirewallController extends AbstractActionController
{
    /**
     * The entry point of the Firewall Panel.
     */
    public function panelAction()
    {
        $panel = new \Shieldon\Firewall\Panel();
        $panel->entry();
    }
}
```

#### 3.  Define a route for firewall panel.

Open the `module.config.php`, the location is at:

```bash
module/Application/config/module.config.php
```

**(3-1)** Inside the array `['router']['routes']` add the code as below.

Example:

```php
'firewallpanel' => [
    'type' => Segment::class,
    'options' => [
        'route'    => '/firewall/panel[:slug]',
        'constraints' => [
            'slug' => '[a-zA-Z0-9\/]*',
        ],
        'defaults' => [
            'controller' => Controller\FirewallController::class,
            'action'     => 'panel',
        ],
    ],
],
```

**(3-2)** Inside the array `['controllers']['factories']` add the code as below.

```php
Controller\FirewallController::class => InvokableFactory::class,
```

That's it.


## Control Panel

You can access the Firewall Panel by `/firewall/panel/`, to see the page, go to this URL in your browser.

```bash
https://yourwebsite.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.