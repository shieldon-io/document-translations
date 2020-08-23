# CodeIgniter

CodeIgniter is a light-weight MVC framework. I talk the CodeIgniter 3 first because that Its version 4 has extreme differences from the early versions.

In this guide, I will share with you the tips for implementing Shieldon Firewall on your CodeIgniter application.

![Firewall in CodeIgniter Framework](https://shieldon.io/images/home/codeigniter-framework-firewall.png)


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

- CodeIgniter 3
- CodeIgniter 4

### CodeIgniter 3

CodeIgniter 3 has a core controller called `CI_Controller` that handles its MVC (Model-View-Controller) architectural pattern.

I highly recommend you to a parent controller calle `MY_Controller` in the `core` folder, and then put the initial code into it.

#### 1.  MY_Controller

Let's create a MY_Controller.php in the `core` folder.

```php
class MY_Controller extends CI_Controller
{
    /**
     * Constructor.
     */
    public function __construct()
    {
        parent::__construct();
    }
}
```

#### 2.  Initialize Firewall instance

Put the initial code in the constructor so that any controller extends MY_Controller will have Shieldon Firewall initialized and `$this->firewall()` method ready.

```php
class MY_Controller extends CI_Controller
{
    /**
     * Constructor.
     */
    public function __construct()
    {
        parent::__construct();

        // Composer autoloader
        require_once APPPATH . '../vendor/autoload.php';

        // This directory must be writable.
        $storage = APPPATH . 'cache/shieldon_firewall';

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

    /**
     * Shieldon Firewall protection.
     */
    public function firewall()
    {
        $firewall = \Shieldon\Container::get('firewall');
        $firewall->run();
    }
}
```

*Reminder*

For the best security, both the system and application folders should be placed above web root so that they are not directly accessible via a browser.

If your application folder is in the same level with index.php, please move the `$storage` to a safe place. For example:

```php
$storage =  APPPATH . '../shieldon';
```

#### 3.  Define a controller for control panel.

We need a controller to get into Shieldon firewall controll panel, in this example, wedefine a controller named `Firewall`.

```php
class Firewall extends MY_Controller
{
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * This is the entry of our Firewall Panel.
     */
    public function panel()
    {
        $panel = new \Shieldon\Firewall\Panel();
        $panel->entry();
    }
}
```

Now, you can access the Firewall Panel via URL:

```plaintext
https://yoursite.com/firewall/panel/
```

### CodeIgniter 4

#### 1. Register a Filter.

In your `app/Config/Filter.php`, add the following code to the `$aliases` property.

```php
'firewall' => \Shieldon\Firewall\Intergration\CodeIgniter4::class,
```

And then, add the string *firewall* to the `$globals` property, `before` array.


```php
public $globals = [
    'before' => [
        'firewall'
    ],
];
```

#### 2.  Define a Controller for Firewall Panel.

```php
<?php 

namespace App\Controllers;

class Firewall extends BaseController
{
    public function panel()
    {
        $panel = new \Shieldon\Firewall\Panel();
        $panel->csrf([csrf_token() => csrf_hash()]);
        $panel->entry();
    }
}
```

That's it.

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

## Control Panel

```plaintext
https://yoursite.com/firewall/panel/
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.