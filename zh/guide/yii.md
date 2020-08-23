# Yii 2

In this guide, I will give you some idea about how to implement Shieldon Firewall on your Yii application.

![Firewall in Yii Framework](https://shieldon.io/images/home/yii-framework-firewall.png)

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

### Yii 2

#### 1. Before initializing Kernal

In your `public/index.php`, before this line:

```php
require __DIR__ . '/../vendor/yiisoft/yii2/Yii.php';
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
| Running Shieldon Firewall before initializing Yii will avoid possible
| conflicts with Yii's built-in functions.
*/

if (isset($_SERVER['REQUEST_URI'])) {
    $storage = __DIR__ . '/../runtime/shieldon';

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

Create a controller named `FirewallPanelController`. 

The content would be the code below:

Example:

```php
<?php

namespace app\controllers;

use yii\web\Controller;

class FirewallController extends Controller
{
    public function beforeAction($action)
    {
        $this->enableCsrfValidation = false;

        return parent::beforeAction($action);
    }

    /**
     * The entry point of the Firewall Panel.
     *
     * @return string
     */
    public function actionPanel()
    {
        $panel = new \Shieldon\Firewall\Panel();
        $panel->entry();
    }
}

```

Make sure that `enablePrettyUrl` is true in your `config/web.php`

Example:

```php
'urlManager' => [
    'enablePrettyUrl' => true,
    'showScriptName' => false,
    'rules' => [
        'firewall/panel/' => 'firewall/panel',
        'firewall/panel/<slug:.*>' => 'firewall/panel',
    ],
],
```

That's it.

You can access the Firewall Panel by `/firewall-panel`, to see the page, go to this URL in your browser.

## Controll Panel

```bash
https://yourwebsite.com/firewall-panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.