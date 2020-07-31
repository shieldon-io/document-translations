# Slim

Slim framework is one of my favorites. Since Slim is a mirco framework, implementing Shieldon Firewall is easy as well. Without further ado, let's get started.

![Firewall in Slim Framework](https://shieldon.io/images/home/slim-framework-firewall.png)

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon ^2
```

## Implementing

### Slim 4


#### Create a Firewall Middleware

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

#### Add the Firewall Middleware in Your Application

For example, if you are using Slim 4 framework, the code should look like this.
```php
$app->add(new FirewallMiddleware());
```

#### Create a Route for Control Panel

For example, if you are using Slim 4 framework, the code should look like this. Then you can access the URL `https://yourwebsite.com/firewall/panel/` to login to control panel.

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

That's it.

You can access the Firewall Panel by `/firewall/panel/`, to see the page, go to this URL in your browser.

```bash
https://yourwebsite.com/firewall/panel/
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.