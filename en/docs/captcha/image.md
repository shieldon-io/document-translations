# ImageCaptcha

## `Shieldon\Firewall\Captcha\ImageCaptcha`

- **param** `array` $config `-` The settings.
- **return** `void`

![](https://i.imgur.com/tJVTMsb.png)

```php
$config = [
    'word_length' => 6,
];

$captchaInstance = new \Shieldon\Captcha\ImageCaptcha($config);
$shieldon->setCaptcha($captchaInstance);
```

## Settings

Default settings:

| key | type | value |
| --- | --- | --- |
| img_width | int | 250 |
| img_height | int | 50 |
| word_length | int | 8 |
| font_spacing | int | 10 |
| pool | string | 0123456789abcdefghijklmno<br />pqrstuvwxyzABCDEFGHIJKL<br />MNOPQRSTUVWXYZ |
| colors | array | see `Color` settings below. |

Color settings:

| key | type | value |
| --- | --- | --- |
| background | array |  [255, 255, 255] |
| border | array | [153, 200, 255] |
| text | array | [51, 153, 255] |
| grid | array | [153, 200, 255] |

Example:

```php
$defaults = [
    'img_width' => 250,
    'img_height' => 50,
    'word_length' => 8,
    'font_spacing' => 10,
    'pool' => '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ',
    'colors' => [
        'background' => [255, 255, 255],
        'border' => [153, 200, 255],
        'text' => [51, 153, 255],
        'grid' => [153, 200, 255]
    ]
];
```