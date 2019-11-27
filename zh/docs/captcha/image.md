# ImageCaptcha

## `Shieldon\Captcha\ImageCaptcha`

- *param* array `$config`
- *return* void

![](https://i.imgur.com/tJVTMsb.png)

```php
$config = [
    'word_length' => 6,
];

$captchaInstance = new \Shieldon\Captcha\ImageCaptcha($config);
$shieldon->setCaptcha($captchaInstance);
```

## 設定值

預設的設定值:

| 鍵值 | 類別 | 值 |
| --- | --- | --- |
| img_width | integer | 250 |
| img_height | integer | 50 |
| word_length | integer | 8 |
| font_spacing | integer | 10 |
| pool | string | 0123456789abcdefghijklmno<br />pqrstuvwxyzABCDEFGHIJKL<br />MNOPQRSTUVWXYZ |
| colors | array | 見 `顏色` 設定如下。 |

顏色的設定值：

| 鍵值 | 類別 | 值 |
| --- | --- | --- |
| background | array |  [255, 255, 255] |
| border | integer | [153, 200, 255] |
| text | integer | [51, 153, 255] |
| grid | integer | [153, 200, 255] |

例：

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