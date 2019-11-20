# Self-build WAF

If you would like to build your own WAF, by combining the public APIs of Shieldon library, you are able to create one like Shieldon Firewall.

Here is an example to let you know how Shieldon works and then you can manually implement Shieldon on your Web Application.

## Lifecycle Diagram

Below is a diagram for the Shieldon instance lifecycle. You don’t need to fully understand everything going on right now, but as you want to customize your own components or CAPTCHA modules or more, it will be a useful reference.

![Shieldon Lifecycle](https://i.imgur.com/BPAtwOY.png)

## Tips

### 1. Initialize Shieldon instance.

```php
$shieldon = new \Shieldon\Shieldon();
```

### 2. Set up a data driver.

In this example, I use SQLite as the data driver.

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);
$shieldon->setDriver(new \Shieldon\Driver\SqliteDriver($pdoInstance));
```

### 3. Set up the components.

Shieldon components are rule sets to allow or deny session permanently. In this example, we load the TrustedBot component to allow popular search engines, bots not in the rule set will go into the checking process (next components and filters).

```php
$shieldon->setComponent(new \Shieldon\Component\TrustedBot());
```

### 4. Set up a channel. *(not required)*

You can ignore this setting if you only use one Shieldon on your web application. This is for multiple instances.

```
$shieldon->setChannel('web_project');
```

### 5. Limit the online session number. *(not required)*

Only allow 10 sessions to view current page. The default expire time is 300 seconds.

```
$shieldon->limitSession(10, 300);
```

### 6. Load the Captcha modules.

Set a Captcha servie. For example: Google recaptcha.

```
$shieldon->setCaptcha(new \Shieldon\Captcha\Recaptcha([
    'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
    'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
]));
```

### 7. Start protecting your website

```
$result = $shieldon->run();

if ($result !== $shieldon::RESPONSE_ALLOW) {
    if ($shieldon->captchaResponse()) {

        // Unban current session.
        $shieldon->unban();
    }
    // Output the result page with HTTP status code 200.
    $shieldon->output(200);
}
```

That's it.

## Firewall Panel

Althogh you are not using Firewall instance, but you can still use Firewall Panel to view the statistics and charts.

Try the code below:

```
$shieldon = \Shieldon\Container::get('shieldon');

// Get into the Firewall Panel.
$controlPanel = new \Shieldon\FirewallPanel($shieldon);
$controlPanel->entry();
```

Use the default user and password to login.