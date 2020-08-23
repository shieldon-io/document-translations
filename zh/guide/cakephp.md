# CakePHP

CakePHP is an open-source web framework, following the MVC approach, which is one of the most popular frameworks in the PHP community.

This guide has been tested successfully in version `3.8`, I think it can be used older versions as well.

![Firewall in CakePHP Framework](https://shieldon.io/images/home/cakephp-framework-firewall.png)

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

### CakePHP 3

Step 1 and step 2 are applied to the same file located at `/config/route.php`.

#### 1. Register a Middleware.

A middleware for CakePHP [here](https://github.com/terrylinooo/shieldon/blob/2.x/src/Firewall/Integration/CakePhp.php) is ready for you. Just register it on your application.

Example:

```php
/**
 * Apply Shieldon Firewall tp the current route scope.
 */
$routes->registerMiddleware(
    'firewall',
    new \Shieldon\Firewall\Integration\CakePhp()
);

$routes->applyMiddleware('firewall');
```

#### 2. Define a Route for Firewall Panel.

Example:
```php
/**
 * Define the route for the firewall panel.
 */
$routes->connect('/firewall/panel/', [
    'controller' => 'FirewallPanel',
    'action' => 'entry'
]);
```


#### 3. Create a Controller for Firewall Panel.

Create a controller named `FirewallPanelController` and then add the following code into it.

Example:
```php
$panel = new \Shieldon\Firewall\Panel();
$panel->entry();
exit;
```

If you have CSRF enabled, add these lines:

Example:
```php
$panel->csrf(
    '_csrfToken',
    $this->request->getParam('_csrfToken')
);
```

The full example will look like this:

Example:
```php
<?php

namespace App\Controller;

class FirewallPanelController extends AppController
{
    /**
     * This is the entry of our Firewall Panel.
     */
    public function entry()
    {
        // Get into the Firewall Panel.
        $panel = new \Shieldon\Firewall\Panel();

        $panel->csrf([
            '_csrfToken' => $this->request->getParam('_csrfToken')
        ]);

        $panel->entry();
        exit;
    }
}
```

That's it.

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

## Controll Panel.

```bash
https://for.example.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.