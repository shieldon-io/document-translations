# ReCaptcah

## `Shieldon\Firewall\Captcha\Recaptcha`

- **param** `array` $config `-` The settings.
- **return** `void`

### V2

![](https://i.imgur.com/rlsEwSG.png)
```php

$captchaConfig = [
    'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
    'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
];

$captchaInstance = new \Shieldon\Captcha\Recaptcha($captchaConfig);
$shieldon->setCaptcha($captchaInstance);
```

### V3

![](https://i.imgur.com/UTcle2h.png)

Make sure you are using v3 `site key` and `secret key`. If you use v2 key here, it won't work.

```php

$captchaConfig = [
    'key' => '6LfkOaUSAAAAAH-AETz3hRQ21K8kEKb2hDRSwz8',
    'secret' => '6LekOaUUAAAAAJdeZ7u-1j4hZC1rOqYZ9gtm0WQy',
    'version' => 'v3',
];

$captchaInstance = new \Shieldon\Captcha\Recaptcha($captchaConfig);
$shieldon->setCaptcha($captchaInstance);
```

### Language

You can specific the language for the UI by passing `lang`.

```php
$captchaConfig = [
    'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
    'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
    'lang' => 'zh-TW',
];
```