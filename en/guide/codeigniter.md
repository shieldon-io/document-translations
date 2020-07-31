# CodeIgniter

CodeIgniter is a light-weight MVC framework. I talk the CodeIgniter 3 first because that Its version 4 has extreme differences from the early versions.

In this guide, I will share with you the tips for implementing Shieldon Firewall on your CodeIgniter application.

![Firewall in CodeIgniter Framework](https://shieldon.io/images/home/codeigniter-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon ^2
```

## Implementing

### CodeIgniter 3

CodeIgniter 3 has a core controller called `CI_Controller` that handles its MVC (Model-View-Controller) architectural pattern.

I highly recommend you create the MY_Controller in the `core` folder as the parent controller and then put the initial code into it.

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
    public function dashboard()
    {
		$panel = new \Shieldon\Firewall\Panel();
		$panel->entry('/example/dashboard/');
    }
}
```

Now, you can access the Firewall Panel via URL:

```plaintext
https://yoursite.com/example/dashboard/
```

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section.