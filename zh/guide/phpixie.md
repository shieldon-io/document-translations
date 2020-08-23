# PHPixie

PHPixie is a mirco framework. Its version 3 documentation is vague - missing lots of important article such as route setting - and I have no time to watch their video, hence this guide is just an idea about how to implement Shieldon Firewall by using the simplest way.

![Firewall in PHPixie Framework](https://shieldon.io/images/home/phpixie-framework-firewall.png)

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

### Steps

#### 1. Before initializing PHPixie.

In your `web/index.php`, after this line:

```php
require_once(__DIR__.'/../vendor/autoload.php');
```
Add the following code:

Example:
```php
// Implement Shieldon Firewall.
$shieldon = new \Shieldon\Firewall\Integration\Bootstrap();
$shieldon->run();
```

So, your `index.php` will look like this:

Example:
```php
<?php

require_once(__DIR__.'/../vendor/autoload.php');

// Implement Shieldon Firewall.
$shieldon = new \Shieldon\Firewall\Integration\Bootstrap();
$shieldon->run();

$framework = new Project\Framework();
$framework->registerDebugHandlers();
$framework->processHttpSapiRequest();
```

That's it.

You can access the Firewall Panel by `/firewall/panel/`, to see the page, go to this URL in your browser.

```bash
https://yourwebsite.com/firewall/panel/
```

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

Shieldon Firewall will start watching your website if it get enabled in `Deamon` setting section, make sure you have set up the settings correctly.