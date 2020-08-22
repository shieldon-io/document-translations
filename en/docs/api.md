# Kernel APIs

## Kernel

```
Shieldon\Firewall\Kernel
```

- ban
- getCurrentUrl
- managedBy
- run
- setClosure
- exclude
- setExcludedUrls
- setLogger
- setProperty
- setProperties
- setStrict
- unban

### ban(`$ip`)

- **param** `string` $ip `-` Single IP address.
- **return** `void`

Ban an IP address.

Example:
```php
$kernel->ban('33.125.12.87');
```

### getCurrentUrl()

- **return** `string`

Return current URL. This method equals `$_SERVER['REQUEST_URI']`.

Example:
```php
echo $kernel->getCurrentUrl();
```

### managedBy(`$type`)

- **param** `string` $type `""` managed | config | demo
- **return** `void`

Tell Shieldon what type the Shieldon is managed by.
This setting only influences the Firewall Panel.

Example:
```php
$kernel->managedBy('demo');
```

### run()

- **return** `int`

Run the checking process.

Reponse code:

| constant | value | reason |
| --- | --- | --- |
| `RESPONSE_DENY` | 0 | Banned permanently. |
| `RESPONSE_ALLOW` | 1 | Passed. |
| `RESPONSE_TEMPORARILY_DENY` | 2 | Banned temporarily and can be unbaned by solving Captcha. |
| `RESPONSE_LIMIT` | 3 | Stopped due to reaching online session limit. |

Example:
```php
$result = $kernel->run();
```

### setClosure(`$key`, `$closure`)

- **param** `string` $key `-` The identification name of the closure.
- **param** `Closure` $closure `-` The closure function.
- **return** `void`

Set a closure function.

Example:

```php
$kernel->setClosure('www_authenticate', function() use ($authHandler, $authenticateList) {
    $authHandler->set($authenticateList);
    $authHandler->check();
});
```

### exclude(`$uriPath`)

- **param** `string` $uriPath `-` The path component of a URI.
- **return** `void`

Set a URL you want them excluded them from protection.

Example:

```php
$kernel->exclude('/firewall/panel');
```

### setExcludedUrls(`$urls`)

- **param** `array` $urls `-` The collection of URLs.
- **return** `void`

Set the URLs you want them excluded them from protection.

Example:
```php
$list = [
    '/example/1',
    '/wp-login.php',
];

$kernel->setExcludedUrls($list);
```

### setLogger(`$logger`)

- **param** `ActionLogger` $logger `-` Record action logs for users.
- **return** `void`

Set an action log logger.

Example:

```php
$kernel->setLogger(
    new \Shieldon\Firewall\Log\ActionLogger(
        BOOTSTRAP_DIR . '/../tmp/shieldon'
    )
);
```

### setProperty(`$key`, `$value`)

- **param** `string` $key `-` The key name for the property.
- **param** `mixed` $value `-` The value of the key.
- **return** `void`

Example:

```php
$kernel->setProperty('time_unit_quota', [
    's' => 4,
    'm' => 20, 
    'h' => 60, 
    'd' => 240,
]);
```

The explanation of settings you cand find [here](http://shieldon.io/en/docs/configuration.html).

### setProperties(`$settings`)

- **param** `array` $settings `-` The settings.
- **return** `void`

Example:

```php
$kernel->setProperties($settings);
```

### setStrict(`$bool`)

- **param** `bool` $bool `-` Set true to enble strict mode, false to disable it overwise.
- **return** `void`

Example:
```php
$kernel->setStrict(true);
```

### unban(`$ip`)

- **param** `string` $ip `-` An IP address.
- **return** `void`

Unban an IP address.

Example:

```php
$kernel->unban('33.33.33.33');
```

---

## Captcha Trait

```
Shieldon\Firewall\Kernel\CaptchaTrait
```

- setCaptcha
- captchaResponse
- disableCaptcha

### setCaptcha(`$instance`)

- **param** `CaptchaInterface` $instance `-` The Captcha instance.
- **return** `void`

Set a captcha. For deatiled usages, please see [Captcha](captcha/index.md) section.

```php
$kernel->setCaptcha(
    new \Shieldon\Firewall\Captcha\Recaptcha([
        'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
        'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
        'version' => 'v2',
        'lang' => 'en',
    ])
);
```

### captchaResponse()

- **return** `bool`

Return the result from Captchas.
`true`: Captcha is solved successfully, `false` overwise.

Example:

```php
$result = $this->captchaResponsse();
```

### disableCaptcha()

- **return** `void`

Disable all Captcha modules. This method is for unit testing purpose.

---

## Component Trait

```
Shieldon\Firewall\Kernel\ComponentTrait
```

- setComponent
- getComponent
- disableComponents

### setComponent(`$instance`)

- **param** `ComponentProvider` $instance `-` The component instance.
- **return** `void`

Set a commponent.

Example:
```php
$kernel->setComponent(
    new \Shieldon\Firewall\Component\UserAgent()
);
```

### getComponent()

- **param** `string` $name `-` The component's class name.
- **return** `void` | `ComponentProvider`

Get a component instance from component's container.

Example:

```php
$useragent = $kernel->getComponent('UserAgent');
```

### disableComponents()

- **return** `void`

Disable all components even they have been set up ready.

Example:

```php
$kernel->disableComponents();
```

---

## Driver Trait

```
Shieldon\Firewall\Kernel\DriverTrait
```

- setDriver
- setChannel
- disableDbBuilder

### setDriver(`$driver`)

- **param** `DriverProvider` $driver `-` Query data from the driver you choose.
- **return** `void`

Example:

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);

$kernel->setDriver(
    new \Shieldon\Firewall\Driver\SqliteDriver($pdoInstance)
);
```

### setChannel(`$channel`)

- **param** `string` $channel `-` Specify a channel.
- **return** `void`

Example:

```php
$kernel->setChannel('web_project');
```

### disableDbBuilder()

- **return** `void`

Disable building database automatically.


Example:
```php
$kernel->disableDbBuilder();
```

## Filter Trait

```
Shieldon\Firewall\Kernel\FilterTrait
```

- setFilters
- setFilter
- disableFilters

### setFilters(`$settings`)

- **param** `array` $settings `-` Filter settings.
- **return** `void`

```php
$kernel->setFilters([
    'session' => true,
    'cookie' => true,
    'referer' => true,
    'frequency' => true,
]);
```

### setFilter(`$filterName`, `$value`)

- **param** `string` $filterName `-` The filter's name.
- **param** `bool` $value `-` True for enabling the filter, overwise.
- **return** `void`

Example:

```php
$kernel->setFilter('session', false);
$kernel->setFilter('cookie', false);
$kernel->setFilter('referer', true);
$kernel->setFilter('frequency', false); 
```

Default settings:

| key | type | value | description |
| --- | --- | --- | --- |
| session | bool | true | Check session created by Shieldon session driver. |
| cookie | bool | **false** | Check cookie generated by JavaScript. |
| referer | bool | true | Check `HTTP_REFERER` |
| frequency | bool | true | Check `time_unit_quota` setting. |

The Cookie filter is `false` by default, because you have to output the JavaScript snippet to your web pages. The snippet created by `getJavascript()` will generate cookie by JavaScript.

Check out `getJavascript()` for usage.

### disableFilters()

- **return** `void`

Disable all filters even they have been set up ready.

```php
$kernel->disableFilters();
```

---

## IP Trait

```
Shieldon\Firewall\IpTrait
```

- setIp
- getIp
- setRdns
- getRdns

### setIp(`$ip`)

- **param** `string` $ip `-` An IP address.
- **return** `void`

Set an IP address.

Example:

```php
// Here is an example, cature real vistor IP from CloudFlare.
$realIp = $_SERVER['HTTP_CF_CONNECTING_IP'];

// If you use a CDN serive on your website, 
// make sure to cature the real vistor IP, overwise users will get banned.
$kernel->setIp($realIp);
```

### getIp()

- **return** `string`

Get current set IP.

Example:

```php
$ip = $kernel->getIp();
```

### setRdns(`$rdns`)

- **param** `string` $rdns `-` Reserve DNS record for that IP address.
- **return** `void`


Set a RDNS record for the check.

Example:

```php
$kernel->setRdns('localhost');
```

### getRdns()

- **return** `string`

Get IP resolved hostname.

```php
$rdns = $kernel->getRdns();
```

---

## Messenger Trait

```
Shieldon\Firewall\Kernel\MessengerTrait
```

- setMessenger

### setMessenger(`$instance`)

- **param** `MessengerInterface` $instance `-` The messenger instance.
- **return** `void`

Example:

```php
$apiKey = '981441296:AAGCcgv_NETMdWQCBTaMOk_yoMfax5EV7YQ';
$channel = '@your_channel';

$telegramMessenger = new \Messenger\Telegram($apiKey, $channel);

$kernel->setMessenger($telegramMessenger);
```

---

## Template Trait

```
Shieldon\Firewall\Kernel\TemplateTrait
```

- setDialog
- respond
- setTemplateDirectory
- getJavascript

### setDialog(`$settings`)

- **param** `array` $settings `-` The dialog UI settings.
- **return** `void`

Customize the dialog UI.

Example:

```php
$settings = [
    'lang'             => 'en',
    'background_image' => '',
    'bg_color'         => '#ffffff',
    'header_bg_color'  => '#212531',
    'header_color'     => '#ffffff',
    'shadow_opacity'   => '0.2',
];

$kernel->setDialog($settings);
```

### respond()

- **return** `ResponseInterface`

Respond the result.

Example:

```php
$response = $kernel->respond();
```

### setTemplateDirectory(`$directory`)

- **param** `array` $directory `-` The directory in where the template files are placed.
- **return** `void`

Set the frontend template directory. You can copy [those files](https://github.com/terrylinooo/shieldon/tree/2.x/templates/frontend) to a dictionary, and modify them for customizing the look of dialogs.

Example:

```php
$kernel->setTemplateDirectory(YOUR_DIRECTORY_PATH);
```

### getJavascript()

- **return** `string`

Print a JavaScript snippet in your webpages.

This snippet generate cookie on client's browser,then we check the cookie to identify the client is a rebot or not.

```php
// Output this variable in your page template.
$jsCode = $kernel->getJavascript();
```

## Session

```
Shieldon\Firewall\Kernel\SessionTrait
```

- limitSession
- getSessionCount

### limitSession(`$count`, `$period`, `$unique`)

- **param** `int` $count `1000` The amount of online users. If reached, users will be in queue.
- **param** `int` $period `300` The period of time allows users browsing.  (unit: second)
- **param** `bool` $unique `false` Allow only one session per IP address.
- **return** `void`

Limt online sessions.

Example:

```php
$kernel->limitSession(100, 300);
```
   
### getSessionCount()

- **return** `int`

Get online people count. If enable limitSession.

Example:

```php
$count = $kernel->getSessionCount();
```



