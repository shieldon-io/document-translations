# Laravel

This guide helps you get through the confusion of implementing Shieldon Firewall on your Laravel application. These tips are not the only way to make it, but also gives you some ideas.

The following steps have been tested on Laravel 5 and 6.

![Firewall in Laravel Framework](https://shieldon.io/images/home/laravel-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon
```

This will also install dependencies built for Shieldon:

- [shieldon/psr-http](https://github.com/terrylinooo/psr-http) The PSR-7, 15, 17 Implementation with full documented and well tested.
- [shieldon/event-dispatcher](https://github.com/terrylinooo/event-dispatcher) The simplest event dispatcher.
- [shieldon/web-security](https://github.com/terrylinooo/web-security) The collection of functions about web security.
- [shieldon/messenger](https://github.com/terrylinooo/messenger) The collection of modules of sending message to third-party API or service, such as Telegram, Line, RocketChat, Slack, SendGrid, MailGun and more...



## Implementing

You can use Shieldon as a **middleware** or implement Shieldon on the  **bootstrap stage** of your web application.

### Bootstrap Stage

Initialize Shieldon in the bootstrap stage of your application, mostly in just right after Composer autoloader has been included. I personally prefer this way because that there are fewer steps and to avoid possible conflicts with Laravel's built-in functions.

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

    // The base url for the control panel.
    $firewall->controlPanel('/firewall/panel/');

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
    $panel->entry();

})->where('path', '(.*)');
```

If you adopt this way, Shieldon Firewall will run in Global scope. But no worry, you can set up the exclusion list for the URLs you want Shieldon Firewall ignore them.

### Middleware

You can define a middleware by yourself or use [intergration class](https://github.com/terrylinooo/shieldon/blob/2.x/src/Firewall/Integration/Laravel.php).

If you choose to use intergation class, skip step 1 and go to step 2-2.

#### 1.  Define a Middleware.

Define a middleware named `ShieldonFirewall`
```bash
php artisan make:middleware ShieldonFirewall
```
Add several lines in the `ShieldonFirewall` middleware class, the content will look like below.

```php
<?php

namespace App\Http\Middleware;

use Closure;

class ShieldonFirewall
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        $firewall = new \Shieldon\Firewall\Firewall();

        // The directory in where Shieldon Firewall will place its files.
        $storage = storage_path('shieldon_firewall');;

        $firewall->configure($storage);

        // Base URL for control panel.
        $firewall->controlPanel('/firewall/panel/');

        $firewall->getKernel()->setCaptcha(
            new Csrf([
                'name' => '_token',
                'value' => csrf_token(),
            ])
        );

        $response = $firewall->run();

        if ($response->getStatusCode() !== 200) {
            $httpResolver = new \Shieldon\Firewall\HttpResolver();
            $httpResolver($response);
        }

        return $next($request);
    }
}
```

#### 2.  Register a Middleware alias.

Modify `app/Http/Kernel.php` and add this line in `$routeMiddleware` property.

2-1
```php
'firewall' => \App\Http\Middleware\ShieldonFirewall::class,
```

If you use *intergation class*, the code will look like this:

2-2
```php
'firewall' => \Shieldon\Firewall\Integration\Laravel::class,
```

#### 3.  Defind a Route for Firewall Panel.

We need a controller to get into the controll panel of Shieldon firewall.

```php
Route::any('/firewall/panel/{path?}', function() {

    $panel = new \Shieldon\Firewall\Panel();
    $panel->csrf(['_token' => csrf_token()]);
    $panel->entry();

})->where('path', '(.*)');
```

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section.

#### 4.  Assign `firewall` middleware to a route.

Assign `firewall` middleware to any route you would like to protect. For example:

```php
Route::any('/', function () {
    return view('welcome');

})->middleware('firewall');
```

That's it.


## Control Panel

You can access the control panel by entering `/firewall/panel/`.

```bash
https://for.example.com//firewall/panel/
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.
