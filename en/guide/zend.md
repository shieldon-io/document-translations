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

#### 1. Define a Middleware.

```php
class FirewallMiddleware
{
    /**
     * The absolute path of the storage where stores Shieldon generated data.
     *
     * @var string
     */
    protected $storage = '';

    /**
     * Constructor.
     *
     * @param string $storage See property `storage` explanation.
     */
    public function __construct($storage = '')
    {
        // shieldon folder is placed above wwwroot for best security, this folder must be writable.
        $this->storage = dirname($_SERVER['SCRIPT_FILENAME']) . '/../data';

        if ('' !== $storage) {
            $this->storage = $storage;
        }
    }

    /**
     * Shieldon middleware invokable class.
     *
     * @param ServerRequest  $request PSR-7 request
     * @param RequestHandler $delegat PSR-15 request handler
     *
     * @return Response
     */
    public function process(Request $request, RequestHandler $handler): Response
    {
        $firewall = new \Shieldon\Firewall\Firewall($request, $response);
        $firewall->configure($this->storage);

        $firewall->getShieldon()->setCaptcha(
            new \Shieldon\Captcha\Csrf([
                'name' => '_shieldon_csrf',
                'value' => $request->getAttribute('_shieldon_csrf'),
            ])
        );

        $response = $firewall->run();

        if ($response->getStatusCode() !== 200) {
            $httpResolver = new \Shieldon\Firewall\HttpResolver();
            $httpResolver($response);
        }

        return $handler->handle($request);
    }
}
```

In your `pipeline.php`, add this line:

```php
$app->pipe(\FirewallMiddleware:class);
```

#### 2.  Defind a Handler.

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
        $panel = new \Shieldon\Firewall\Panel();
        // The base url for the control panel.
        $panel->entry('/firewall/panel');

        return new Response();
    }
}
```

#### 3.  Defind a Route for Firewall Panel.

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
	// This directory must be writable.
    $storage = __DIR__ . '/../data/shieldon';

    $firewall = new \Shieldon\Firewall\Firewall();
    $firewall->configure($storage);
    $response = $firewall->run();

    if ($response->getStatusCode() !== 200) {
        $httpResolver = new \Shieldon\Firewall\HttpResolver();
        $httpResolver($response);
    }
}
```

The next step is to create a controller for control panel.


#### 2.  Defind a Controller.

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
        $panel = new \Shieldon\Firewall\Panel();
        // The entry point must be the same as the route defined.
        $panel->entry('/firewall/panel');
        exit;
    }
}
```

#### 3.  Defind a Route for Firewall Panel.

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
https://yourwebsite.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.