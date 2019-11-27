# TrustedBot

## `Shieldon\Component\TrustedBot`

- *return* self

```php
$robot = new \Shieldon\Component\TrustedBot();
$shieldon->setComponent($robot);
```

在白名單內的預設值。

| 名稱 | 反解主機名稱 |
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

### setStrict

- *param* boolean `$bool` 設為 true 以啟用嚴格模式, false 反之亦然。
- *return* void

```php
$robot->setStrict(true);
```

### isAllowed

- *return* bool

```php
$result = $robot->isAllowed();
```

### isDenied

(deprecated)

### isGoogle

- *return* bool

```php
$result = $robot->isGoogle();
```

### isYahoo
- *return* bool

```php
$result = $robot->isYahoo();
```

### isBing
- *return* bool

```php
$result = $robot->isBing();
```

### addItem

- *param* string `$userAgent` 部分的用使用者代理的字串。
- *param* string `$rdns` IP 反解的主機名稱。
- *return* void

```php
$robot->addItem('google', '.googlebot.com');
```

## 嚴格模式

```
$robot->setStrict(true);
```

- IP 反解主機名稱 (RDNS) 和 IP 位址須互相吻合。