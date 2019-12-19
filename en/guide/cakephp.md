# CakePHP

CakePHP is an open-source web framework, following the MVC approach, which is one of the most popular frameworks in the PHP community.

This guide has been tested successfully in version `3.8`, I think it can be used older versions as well.

![Firewall in CakePHP Framework](https://shieldon.io/images/home/cakephp-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon
```

## Implementing

### CakePHP 3

Step 1 and step 2 are applied to the same file located at `/config/route.php`.

#### 1. Register a Middleware.

As always, a middleware is ready for you.

```php
/**
 * Apply Shieldon Firewall tp the current route scope.
 */
$routes->registerMiddleware(
    'firewall',
    new \Shieldon\Integration\CakePhp\CakePhpMiddleware()
);

$routes->applyMiddleware('firewall');
```

#### 2. Define a Route for Firewall Panel.

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

```php
$firewall = \Shieldon\Container::get('firewall');
$controlPanel = new \Shieldon\FirewallPanel($firewall);
$controlPanel->entry();
exit;
```

If you have CSRF enabled, add these lines:

```php
$controlPanel->csrf(
    '_csrfToken',
    $this->request->getParam('_csrfToken')
);
```

The full example will look like this:

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
        // Get Firewall instance from Shieldon Container.
        $firewall = \Shieldon\Container::get('firewall');

        // Get into the Firewall Panel.
        $controlPanel = new \Shieldon\FirewallPanel($firewall);
        $controlPanel->csrf(
            '_csrfToken',
            $this->request->getParam('_csrfToken')
        );

        $controlPanel->entry();
        exit;
    }
}
```

That's it.

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.