# Captcha

You can implement Captcha verification when users on your site are detected usual behavior and get temporaily banned. Users are asked for solving Captcha to get unbanned.

- [ReCaptcha](https://shieldon.io/en/docs/captcha/recaptcha.html)
- [Image](https://shieldon.io/en/docs/captcha/image.html)

You can use multiple Captcha modules at a time. More Captcha modules will be added in the feature.

![](https://i.imgur.com/rlsEwSG.png)

### Note

If you have implemented CSRF protection on whole site, be sure to pass your CSRF token into Captcha form.


```php
$shieldon->setCaptcha(new /Shieldon/Captcha/Csrf([
    'name' => 'my_csrf_name',
    'value' => 'my_csrf_hash',
]));
```
