# PHPixie

PHPixie is a mirco framework. It's version 3 documentation is vague - missing lots of important article such as route setting - and I have no time to watch their video (reading documents is 100x faster than waching a video, agree?), hence this guide is just an idea about how to implement Shieldon Firewall on your PHPixie application.

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

### Steps

#### 1. Before initializing PHPixie.

In your `web/index.php`, after this line:

```php
require_once(__DIR__.'/../vendor/autoload.php');
```
Add the following code:

```php
// Implement Shieldon Firewall.
new \Shieldon\Integration\Bootstrapper(
    $storage = '',
    $fpRequestURI = '/firewall/panel'
);
```

The first parameter is the directory where the Shieldon Firewall will generate its data and logs in. The second parameter is a URL that can allow you to access the firewall panel.

So, your `index.php` will look like this:

```php
<?php

require_once(__DIR__.'/../vendor/autoload.php');

// Implement Shieldon Firewall.
new \Shieldon\Integration\Bootstrapper(
    $storage = '',
    $fpRequestURI = '/firewall/panel'
);

$framework = new Project\Framework();
$framework->registerDebugHandlers();
$framework->processHttpSapiRequest();
```

That's it.

You can access the Firewall Panel by `/firewall/panel`, to see the page, go to this URL in your browser.

```bash
https://for.example.com/firewall/panel
```