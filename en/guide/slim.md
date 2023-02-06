# Slim

Slim framework is one of my favorites. Since Slim is a micro framework, implementing Shieldon Firewall is easy as well. Without further ado, let's get started.

![Firewall in Slim Framework](https://shieldon.io/images/home/slim-framework-firewall.png)

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

### Slim 4

#### (1) Create a firewall middleware.

You can create a middleware by yourself or just use the [integration class](https://github.com/terrylinooo/shieldon/blob/2.x/src/Firewall/Integration/Slim4.php).

Example:

```php
class FirewallMiddleware
{
    /**
     * Example middleware invokable class
     *
     * @param ServerRequest  $request PSR-7 request
     * @param RequestHandler $handler PSR-15 request handler
     *
     * @return Response
     */
    public function __invoke(Request $request, RequestHandler $handler): Response
    {
        $response = $handler->handle($request);

        $firewall = new \Shieldon\Firewall\Firewall($request, $response);

        // The directory in where Shieldon Firewall will place its files.
        $firewall->configure(__DIR__ . '/../cache/shieldon_firewall');
        $response = $firewall->run();

        if ($response->getStatusCode() !== 200) {
            $httpResolver = new \Shieldon\Firewall\HttpResolver();
            $httpResolver($response);
        }

        return $response;
    }
}
```

#### (2) Add the firewall middleware in your application.

For example, if you are using Slim 4 framework, the code should look like this.

Example:

```php
$app->add(new FirewallMiddleware());
```

Or, if you perfer to use the integration class, here is the code.

Example:
```php
$app->add(new \Shieldon\Firewall\Integration\Slim4);
```

#### (3) Create a route for control panel.

For example, if you are using Slim 4 framework, the code should look like this. Then you can access the URL `https://yourwebsite.com/firewall/panel/` to login to control panel.

Example:
```php
$app->any('/firewall/panel[/{params:.*}]', function (Request $request, Response $response, $args) {

$firewall = new \Shieldon\Firewall\Firewall($request);

    // The directory in where Shieldon Firewall will place its files.
    // Must be the same as firewallMiddleware.
    $firewall->configure(__DIR__ . '/../cache/shieldon_firewall');

    // The base url for the control panel.
    $firewall->controlPanel('/firewall/panel/');

    $panel = new \Shieldon\Firewall\Panel();

    // Begin - Need to set up CSRF fields if you have enabled Slim-CSRF

    $csrf = new \Slim\Csrf\Guard();
    $nameKey = $csrf->getTokenNameKey();
    $valueKey = $csrf->getTokenValueKey();

    $csrfName = $request->getAttribute('csrf_name');
    $csrfVale = $request->getAttribute('csrf_value');

    $panel->csrf(
        [$nameKey => $csrfName],
        [$valueKey => $csrfVale]
    );

    // End - Slim-CSRF

    $panel->entry();
});
```

Note:

- The HTTP method `POST` and `GET` both should be applied to your website. 
- `POST` method is needed for solving CAPTCHA by users who were temporarily blocked.

That's it.


### Slim 3

#### (1) Add the firewall middleware in your application.

Shieldon has a integration class ready for this middleware, just use it by the following step.

Example:
```php
$app->add(new \Shieldon\Firewall\Integration\Slim3);
```

For example, if you are using Slim3 skeleton, the code in `middleware.php` will look like this:

Example:

```php
<?php

use Slim\App;

return function (App $app) {
    $app->add(new \Shieldon\Firewall\Integration\Slim3);
    $app->add(new \Slim\Csrf\Guard);
};
```


#### (2) Create a route for control panel.

For example, if you are using Slim 4 framework, the code should look like this. Then you can access the URL `https://yourwebsite.com/firewall/panel/` to login to control panel.

Example:

```php
$app->map(['GET', 'POST'], '/firewall/panel[/{params:.*}]', function (Request $request, Response $response, array $args) {

    $firewall = new \Shieldon\Firewall\Firewall($request);

    // The directory in where Shieldon Firewall will place its files.
    // Must be the same as firewallMiddleware.
    $firewall->configure(__DIR__ . '/../cache/shieldon_firewall');

    // The base url for the control panel.
    $firewall->controlPanel('/firewall/panel/');

    $panel = new \Shieldon\Firewall\Panel();

    // Begin - Need to set up CSRF fields if you have enabled Slim-CSRF

    $csrf = new \Slim\Csrf\Guard();
    $nameKey = $csrf->getTokenNameKey();
    $valueKey = $csrf->getTokenValueKey();

    $csrfName = $request->getAttribute('csrf_name');
    $csrfVale = $request->getAttribute('csrf_value');

    $panel->csrf(
        [$nameKey => $csrfName],
        [$valueKey => $csrfVale]
    );

    // End - Slim-CSRF

    $panel->entry();
});
```

That's it.

## Controll Panel

You can access the Firewall Panel by `/firewall/panel/`, to see the page, go to this URL in your browser.

```bash
https://yourwebsite.com/firewall/panel/
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.
