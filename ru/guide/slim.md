# Slim

Slim framework is one of my favorites. Since Slim is a mirco framework, implementing Shieldon Firewall is easy as well. Without further ado, let's get started.

![Firewall in Slim Framework](https://shieldon.io/images/home/slim-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon
```

Or, download it and include the Shieldon autoloader.
```php
require 'Shieldon/autoload.php';
```

## Implementing

### Slim 3

#### Middleware

Shieldon supports most popular PHP frameworks, following its design pattern, Slim is one of them, so that there is a Middleware for Slim 3 already.

```php
$app->add(new \Shieldon\Integration\Slim\Slim3Middleware);
```

Reminer: If you have Slim-Csrf middleware implemented, make sure the order should look like this:

```php
$app->add(new \Shieldon\Integration\Slim\Slim3Middleware);
$app->add(new \Slim\Csrf\Guard);
```

Notice: Slim-Csrf is no longer support Slim 3, if you would like to use it on Slim 3, be sure to install the older vision.

```bash
composer require slim/csrf:0.8.3
```

#### Route

This route is the entry of Firewall Panel.

```php
$app->map(['GET', 'POST'], '/example/fiewall/panel', function (Request $request, Response $response, array $args) {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf([
        'csrf_name' => $request->getAttribute('csrf_name'),
        'csrf_value' => $request->getAttribute('csrf_value')
    ]);
    $controlPanel->entry();
});
```

### Slim 4

#### Middleware

Load Slim4Middleware at the first place.

```php
$app->add(new \Shieldon\Integration\Slim\Slim4Middleware());
```

So, your middleware.php probably looks like this:

```php
return function (App $app) {
    $app->add(new \Shieldon\Integration\Slim\Slim4Middleware());
    $app->add(SessionMiddleware::class);
};
```

#### Route

This route is the entry of Firewall Panel.

```php
$app->map(['GET', 'POST'], '/example/fiewall/panel', function (Request $request, Response $response, array $args) {
    $firewall = \Shieldon\Container::get('firewall');
    $controlPanel = new \Shieldon\FirewallPanel($firewall);
    $controlPanel->csrf([
        'csrf_name' => $request->getAttribute('csrf_name'),
        'csrf_value' => $request->getAttribute('csrf_value')
    ]);
    $controlPanel->entry();

    return $response;
});
```

Make sure to change all your routes to support Post method for making Captcha form worked, otherwise you will face this error.

```json
{
    statusCode: 405,
    error: {
        type: "NOT_ALLOWED",
        description: "Method not allowed."
    }
}
```

#### Bootstrapper

There is an another way to avoid changing support method. It is `Bootstrapper` located at `Shieldon\Integration` namespace.

In the `public/index.php`, find this line:
```php
require __DIR__ . '/../vendor/autoload.php';
```
Replace it with:

```php
require __DIR__ . '/../vendor/autoload.php';

// Implement Shieldon Firewall.
new \Shieldon\Integration\Bootstrapper();
```

That's it.

*Reminder*:

To prevent `session_start` conflicts, please start session safely in your `SessionMiddleware`

```php
if (session_status() === PHP_SESSION_NONE) {
    session_start();
}
```

That's it.

You can access the Firewall Panel by `/example/fiewall/panel`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/example/fiewall/panel
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.