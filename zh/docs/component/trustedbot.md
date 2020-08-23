# TrustedBot

## `Shieldon\Firewall\Component\TrustedBot`

- **return** `self`

```php
$robot = new \Shieldon\Firewall\Component\TrustedBot();
$shieldon->setComponent($robot);
```

Default setting in whitelist.

| name | Rdns |
| --- | --- |
| google | .googlebot.com |
| google | .google.com |
| live | .live.com |
| msn | .msn.com |
| ask | .ask.com |
| bing | .bing.com |
| inktomisearch | .inktomisearch.com |
| yahoo | .yahoo.com |
| yahoo | .yahoo.net |
| yandex | .yandex.com |
| yandex | .yandex.ru |
| w3 | .w3.org |

### setStrict(`$bool`)

- **param** bool `$bool` Set true to enble strict mode, false to disable it overwise.
- **return** `void`

Example:
```php
$robot->setStrict(true);
```

### isAllowed()

- **return** `bool`

Example:
```php
$result = $robot->isAllowed();
```

### isDenied()

(deprecated)

### isGoogle()

- **return** `bool`

Example:
```php
$result = $robot->isGoogle();
```

### isYahoo()
- **return** `bool`

Example:
```php
$result = $robot->isYahoo();
```

### isBing()
- **return** `bool`

Example:
```php
$result = $robot->isBing();
```

### addItem(`$userAgent`, `$rdns`)

- **param** `string` $userAgent `-` Part of user-agent string
- **param** `string` $rdns `-` IP resolved hostname.
- **return** `void`

Example:
```php
$robot->addItem('google', '.googlebot.com');
```

## Strict Mode

- IP resolved hostname and IP address must match.

Example:
```php
$robot->setStrict(true);
```

