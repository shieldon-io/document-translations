# ReCaptcah

## `Shieldon\Captcha\Recaptcha`

- *param* array `$config`
- *return* void

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

務必使用 v3 版本的 `網站金鑰` 和 `密鑰`。如果您在這裡使用 v2 版的會沒有作用。

```php

$captchaConfig = [
    'key' => '6LfkOaUSAAAAAH-AETz3hRQ21K8kEKb2hDRSwz8',
    'secret' => '6LekOaUUAAAAAJdeZ7u-1j4hZC1rOqYZ9gtm0WQy',
    'version' => 'v3',
];

$captchaInstance = new \Shieldon\Captcha\Recaptcha($captchaConfig);
$shieldon->setCaptcha($captchaInstance);
```

### 語言

傳送 `lang`的值，您可以指定 UI 顯示的語言。

```php
$captchaConfig = [
    'key' => '6LfkOaUUAAAAAH-AlTz3hRQ25SK8kZKb2hDRSwz9',
    'secret' => '6LfkOaUUAAAAAJddZ6k-1j4hZC1rOqYZ9gLm0WQh',
    'lang' => 'zh-TW',
];
```