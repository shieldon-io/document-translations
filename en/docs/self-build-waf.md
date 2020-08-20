# Self-build WAF

If you would like to build your own Web Application Firewall, WAF, by combining the public APIs of Shieldon library, you can create one resembling the Shieldon Firewall.

Here is an example to let you know how Shieldon works and then you can manually implement Shieldon on your web application.

## Lifecycle Diagram

Below is a diagram for the Shieldon instance lifecycle. You don’t need to fully understand everything going on right now, but as you want to customize your own components or CAPTCHA modules or more, it will be a useful reference.

![Lifecycle Diagram](https://i.imgur.com/9RLHFG1.png)

## Tips

### 1. Initialize Shieldon instance.

```php
$kernel = new \Shieldon\Firewall\Kernel();
```

### 2. Set up a data driver.

In this example, I use SQLite as the data driver.

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);

$kernel->setDriver(
    new \Shieldon\Firewall\Driver\SqliteDriver($pdoInstance)
);
```

### 3. Set up the components.

Shieldon components are rule sets to allow or deny session permanently.

In this example, we load the TrustedBot component to allow popular search engines to prevent them bots go into the checking process - next components and filters.

```php
$kernel->setComponent(
    new \Shieldon\Firewall\Component\TrustedBot()
);
```

### 4. Set up a channel. *(not required)*

You can ignore this setting if you only use one Shieldon kernel instance on your web application. The channel is just the prefix of the name of the data tables.

```php
$kernel->setChannel('web_project');
```

### 5. Limit the online session number. *(not required)*

Only allow 10 sessions to view current page. The default expire time is 300 seconds.

```php
$kernel->limitSession(10, 300);
```

### 6. Load the Captcha modules.

Set a Captcha servie. For example: Google recaptcha.

```php
$kernel->setCaptcha(
    new \Shieldon\Firewall\Captcha\Recaptcha([
        'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
        'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
    ])
);
```

### 7. Start protecting your website

```php
$result = $kernel->run();

if ($result !== $kernel::RESPONSE_ALLOW) {
    if ($kernel->captchaResponse()) {
        // Unban current session.
        $kernel->unban();
    }

    $kernel->respond();
}
```

That's it.
