# CodeIgniter

CodeIgniter is a light-weight MVC framework. I talk the CodeIgniter 3 first because that Its version 4 has extreme differences from the early versions.

In this guide, I will share with you the tips for implementing Shieldon Firewall on your CodeIgniter application.

## Install

Use PHP Composer:

```php
composer require terrylinooo/shieldon
```

Or, download it and include the Shieldon autoloader.
```php
require 'Shieldon/src/autoload.php';
```

## Implement

### CodeIgniter 3

CodeIgniter 3 has a super singleton instance called `CI_Controller` that handles its MVC (Model-View-Controller) architectural pattern.

I highly recommend you create the MY_Controller in the `core` folder as the Parent Controller and then put the initial code into it.

#### 1.  MY_Controller

Let's create a MY_Controller.php in the `core` folder.

```php
class Core_Controller extends CI_Controller
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
        $storage =  APPPATH . 'cache/shieldon';
        $firewall = new \Shieldon\Firewall($storage);
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

For the best security, both the system and any application folders should be placed above web root so that they are not directly accessible via a browser.

If your application folder is in the same level with index.php, please move the `$storage` to a safe place. For example:

```php
$storage =  APPPATH . '../shieldon';
```

#### 3.  Defind a Controller for Firewall Panel.

We need a controller to get into Shieldon firewall controll panel, in this example, we defind a controller named `Example`.

```php
class Example extends MY_Controller
{
    /**
     * Constructor.
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * This is the entry of our Firewall Panel.
     */
    public function ControllPanel()
    {
        // Get Firewall instance from Shieldon Container.
        $firewall = \Shieldon\Container::get('firewall');

        // Get into the Firewall Panel.
        $controlPanel = new \Shieldon\FirewallPanel($firewall);
        $controlPanel->entry();
    }
}
```

Now, you can access the Firewall Panel via URL:

```plaintext
http://yoursite.com/example/controllPanel/
```

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section.


### CodeIgniter 4

### 1. Register a Filter.

In your `app/Config/Filter.php`, add the following code to the `$aliases` property.

```php
'firewall' => \Shieldon\Integration\CodeIgniter\CI4Middleware::class,
```

And then, add the string *firewall* to the `$globals` property, `before` array.


```php
public $globals = [
    'before' => [
        'firewall'
    ],
];
```

### 2.  Defind a Controller for Firewall Panel.

```php
<?php 

namespace App\Controllers;

class FirewallPanel extends BaseController
{
    public function index()
    {
        // Get Firewall instance from Shieldon Container.
        $firewall = \Shieldon\Container::get('firewall');

        // Get into the Firewall Panel.
        $controlPanel = new \Shieldon\FirewallPanel($firewall);
        $controlPanel->csrf(csrf_token(), csrf_hash());
        $controlPanel->entry();
    }
}
```

That's it.

You can access the Firewall Panel by `/firewallPanel`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/firewallPanel
```
