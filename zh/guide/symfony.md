# Symfony

Symfony is a set of reusable PHP components and a PHP framework to build web applications, APIs, microservices and web services.

This guide has been tested successfully in version `4.3`, but I think it can be used older versions as well.

Symfony doesn't have a middleware concept, therefore you can create a parent controller to implement Shieldon Firewall just like the steps in our [CodeIgniter guide](https://shieldon.io/en/guide/codeigniter.html).

If you don't like to initialize Shieldon Firewall in a parent controller, here are the steps that called Bootstrap mode you can try.

![Firewall in Symfony Framework](https://shieldon.io/images/home/symfony-framework-firewall.png)

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

### Bootstrap

#### 1. Before initializing Kernel

In your `config/bootstrap.php`, after this line:

```php
require dirname(__DIR__).'/vendor/autoload.php';
```
Add the following code:

Example:
```php
/*
|--------------------------------------------------------------------------
| Run The Shieldon Firewall
|--------------------------------------------------------------------------
|
| Shieldon Firewall will watch all HTTP requests coming to your website.
*/
if (isset($_SERVER['REQUEST_URI'])) {

	// This directory must be writable.
    $storage = __DIR__ . '/../storage/shieldon';

    $firewall = new \Shieldon\Firewall\Firewall();
    $firewall->configure($storage);

    // The base url for the control panel.
    $firewall->controlPanel('/firewall/panel/');

    $response = $firewall->run();

    if ($response->getStatusCode() !== 200) {
        $httpResolver = new \Shieldon\Firewall\HttpResolver();
        $httpResolver($response);
    }
}
```

#### 2.  Define a Route for Firewall Panel.

Create a controller named `FirewallPanelController` by typing the following command.

Example:
```bash
php bin/console make:controller FirewallPanelController
```

Add several lines in the `FirewallPanelController` controller class:

Example:
```php
$panel = new \Shieldon\Firewall\Panel();
$panel->entry();
```

If you have CSRF enabled, add these lines:

Example:
```php
$csrf = $this->container->get('security.csrf.token_manager');
$token = $csrf->refreshToken('key');
```

The full example will look like this:

Example:
```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Routing\Annotation\Route;

class FirewallPanelController extends AbstractController
{
    /**
     * @Route("/firewall/panel/", name="firewall_panel")
     */
    public function panel()
    {
        $panel = new \Shieldon\Firewall\Panel();

        // If your have `symfony/security-csrf` installed.
        $csrf = $this->container->get('security.csrf.token_manager');
        $token = $csrf->refreshToken('key')->getValue();

        $panel->csrf(['_token' => $token]);
        $panel->entry();
        exit;
    }

    /**
     * @Route("/firewall/panel/{class}/{method}", name="firewall_panel_page")
     */
    public function page()
    {
        $this->panel();
    }
}
```

That's it.

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

## Control Panel

```bash
https://yourwebsite.com/firewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.

