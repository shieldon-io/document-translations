# Laravel

This guide helps you get through the confusion of implementing Shieldon Firewall on your Laravel application. These tips are not the only way to make it, but also gives you some ideas.

The following steps have been tested on Laravel 5 and 6.

![Firewall in Laravel Framework](https://shieldon.io/images/home/laravel-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon
```

Or, download it and include the Shieldon autoloader.
```php
require 'Shieldon/autoload.php';
```

Implementing Shieldon Firewall on your Web Application is pretty easy by using Firewall Panel, and I highly recommend you choose this way.


## Implementing

For Laravel lovers, you can choose **Middleware** or **Bootstrap** to implement Shieldon Firewall on your Web application. I prefer Bootstrap personally.

### Middleware

#### 1.  Define a Middleware.

Define a middleware named `ShieldonFirewall`
```bash
php artisan make:middleware ShieldonFirewall
```
Add several lines in the `ShieldonFirewall` middleware class:

```php
$firewall = new \Shieldon\Firewall(storage_path('shieldon'));

// Pass Laravel CSRF Token to Captcha form.
$firewall->getShieldon()->setCaptcha(new \Shieldon\Captcha\Csrf([
    'name' => '_token',
    'value' => csrf_token(),
]));

$firewall->restful();
$firewall->run();
```

#### 2.  Register a Middleware alias.

Modify `app/Http/Kernel.php` and add this line in `$routeMiddleware` property.
```php
'firewall' => \App\Http\Middleware\ShieldonFirewall::class,
```

#### 3.  Defind a Route for Firewall Panel.

We need a controller to get into Shieldon firewall controll panel, so that..

```php
Route::any('/your/secret/place/', function() {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf('_token', csrf_token());
    $controlPanel->entry();
})->middleware('firewall');
```

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section.

#### 4.  Assign `firewall` middleware to a route.

Assign `firewall` middleware to any route you would like to protect. For example:

```php
Route::get('/', function () {
    return view('welcome');
})->middleware('firewall');
```

### Bootstrap

This is what I said the preferred way, because that less steps and it will avoid possible 
conflicts with Laravel's built-in functions.

#### 1.  Before Initializing $app
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

    // Notice that this directory must be writable.
    $firewallstorage = __DIR__ . '/../storage/shieldon';

    $firewall = new \Shieldon\Firewall($firewallstorage);
    $firewall->restful();
    $firewall->run();
}
```

#### 2.  Define a Route for Firewall Panel.

```php
Route::any('/your/secret/place/', function() {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf('_token', csrf_token());
    $controlPanel->entry();
});
```

If you adopt this way, Shieldon Firewall will run in Global scope. But no worry, you can set up the exclusion list for the URLs you want Shieldon Firewall ignore them.

That's it.

You can access the Firewall Panel by `/your/secret/place/`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/your/secret/place/
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.
