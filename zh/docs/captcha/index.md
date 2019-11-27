# 驗證碼模組

當使用者在您的網站上被偵測到不尋常行為時被封鎖，您可以運用驗證碼檢驗方式。使用者被要求解決驗證碼來解除封鎖。

- [ReCaptcha](https://shieldon.io/en/docs/captcha/recaptcha.html)
- [Image](https://shieldon.io/en/docs/captcha/image.html)

您可以同時使用多個驗證碼模組。更多驗證碼模組在未來會被陸續加入。

![](https://i.imgur.com/rlsEwSG.png)

### 注意

如果您有部署了 CRSF 防護在全網站上，務必傳送您的 CSRF 權杖到驗證碼表單中。

```php
$shieldon->setCaptcha(new /Shieldon/Captcha/Csrf([
    'name' => 'my_csrf_name',
    'value' => 'my_csrf_hash',
]));
```
