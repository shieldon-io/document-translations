# Getting Started

## Server Requirements

Before using Shieldon Firewall on your Web application you must meet the basic server requirements below:

- PHP >= 7.1.0
- [Ctype](https://www.php.net/book.ctype) PHP Extension
- [JSON](https://www.php.net/book.json) PHP Extension
- [PDO](https://www.php.net/book.pdo) PHP Extension *(Required only if you would like to use MySQL, SQLite driver.)*
- [Redis](https://github.com/phpredis/phpredis) PHP Extension *(Required only if you would like to use Redis driver.)*

## Installation

Use PHP Composer:

```shell
composer require shieldon/shieldon ^2
```

## Implementing

There are three ways you can choose to use Shieldon on your application.

- Implement Shieldon as a PSR-15 middleware.
- Implement Shieldon in the bootstrap stage of your application.
- Implement Shieldon in the parent controller extended by the other controllers.

---

### PSR-15 Middleware

#### `Example: Slim 4 framework`

In this example, I will give you some tips on how to implement Shieldon as a PSR-15 middleware.

I use Slim 4 framwork for demonstration. This way can be used on any framework supporting PSR-15 too, just with a bit modification.

#### (1) Create a firewall middleware.

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

```php
$app->add(new FirewallMiddleware());
```

#### (3) Create a route for control panel.

For example, if you are using Slim 4 framework, the code should look like this. Then you can access the URL `https://yourwebsite.com/firewall/panel` to login to control panel.

```php
$app->any('/firewall/panel[/{params:.*}]', function (Request $request, Response $response, $args) {
    $firewall = new \Shieldon\Firewall\Firewall($request, $response);

    // The directory in where Shieldon Firewall will place its files.
    // Must be the same as firewallMiddleware.
    $firewall->configure(__DIR__ . '/../cache/shieldon_firewall');

    $panel = new \Shieldon\Firewall\Panel();

    // The base url for the control panel.
    $panel->entry('/firewall/panel/');
});
```

Note:
- The HTTP method `POST` and `GET` both should be applied to your website. 
- `POST` method is needed for solving CAPTCHA by users who were temporarily blocked.

---

### Bootstrap Stage

#### `Example: Laravel 6 framework`

Initialize Shieldon in the bootstrap stage of your application, mostly in just right after composer autoloader has been included.

In this example, I use Laravel 6 for demonstration.

#### (1) Before Initializing the $app

In your `bootstrap/app.php`, after `<?php`, add the following code.

```php
/*
|--------------------------------------------------------------------------
| Run The Shieldon Firewall
|--------------------------------------------------------------------------
|
| Shieldon Firewall will watch all HTTP requests coming to your website.
| Running Shieldon Firewall before initializing Laravel will avoid possible
| conflicts with Laravel's built-in functions.
*/
if (isset($_SERVER['REQUEST_URI'])) {

    // This directory must be writable.
    // We put it in the `storage/shieldon_firewall` directory.
    $storage =  __DIR__ . '/../storage/shieldon_firewall';

    $firewall = new \Shieldon\Firewall\Firewall();
    $firewall->configure($storage);
    $response = $firewall->run();

    if ($response->getStatusCode() !== 200) {
        $httpResolver = new \Shieldon\Firewall\HttpResolver();
        $httpResolver($response);
    }
}
```

#### (2) Define a route for firewall panel.

```php
Route::any('/firewall/panel/{path?}', function() {

    $panel = new \Shieldon\Firewall\Panel();
    $panel->csrf(['_token' => csrf_token()]);

    // The entry point must be the same as the route defined.
    $panel->entry('/firewall/panel/');

})->where('path', '(.*)');
```

---

### Parent Controller

#### `Example: CodeIgniter 3 framework`

If you are using a MVC framework, implementing Shieldon in a parent controller is also a good idea. In this example, I use CodeIgniter 3 for demonstration.

#### 1. Create a parent controller.

Let's create a `MY_Controller.php` in the `core` folder.

```php
class MY_Controller extends CI_Controller
{
    public function __construct()
    {
        parent::__construct();
    }
}
```

#### 2.  Initialize Firewall instance

Put the initial code in the constructor so that any controller extends `MY_Controller` will have Shieldon Firewall initialized and `$this->firewall()` method ready.

```php
class MY_Controller extends CI_Controller
{
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

#### 3.  Define a controller for controll panel.

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
        $panel->entry('/firewall/panel/');
    }
}
```

#### Login

Finally, no matter which way you choose, entering `https://yoursite.com/firewall/panel/`, the login page is suppose to be shown on your screen.

![](https://i.imgur.com/GFKzNYh.png)

The default user and password is `shieldon_user` and `shieldon_pass`. The first thing to do is to change the login and password after you login to control panel.

:)