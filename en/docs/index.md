
Shieldon is a Web Application Firewall (WAF) for PHP, with a beautiful and useful control panel that helps you easily manage the firewall rules and security settings.

- Website: [https://shieldon.io](https://shieldon.io/)
- GitHub repository:  [https://github.com/terrylinooo/shieldon](https://github.com/terrylinooo/shieldon)
- WordPress plugin: [https://wordpress.org/plugins/wp-shieldon/](https://wordpress.org/plugins/wp-shieldon/)

## Features

- SEO friendly, no impacts to SERP.
- Http-type DDOS mitigation.
- Anti-scraping.
- Limit the amount of online users.
- Cross-site scripting (XSS) protection.
- Interrupting vulnerability scanning.
- Eradicating brute force attacks.
- IP manager.
- Protecting pages via WWW-Authenticate.
- Detailed statistics and charts.
- Sending notifications to third-party services.
- A Web UI for management of iptables, the system firewall.

## Installation

Use PHP Composer:

```php
composer require shieldon/shieldon ^2
```

## Implementing

Here are the guides to integrating with the popular PHP frameworks.

- [Laravel](https://shieldon.io/en/guide/laravel.html)
- [Symfony](https://shieldon.io/en/guide/symfony.html)
- [CodeIgniter](https://shieldon.io/en/guide/codeigniter.html)
- [CakePHP](https://shieldon.io/en/guide/cakephp.html)
- [Yii](https://shieldon.io/en/guide/yii.html)
- [Zend](https://shieldon.io/en/guide/zend.html)
- [Slim](https://shieldon.io/en/guide/slim.html)
- [Fat-Free](https://shieldon.io/en/guide/fatfree.html)
- [Fuel](https://shieldon.io/en/guide/fuel.html)
- [PHPixie](https://shieldon.io/en/guide/phpixie.html)

## Firewall Panel

Shieldon provides a visualization UI called Firewall Panel. By using Shieldon Firewall, you can easily implement it on your web application.

![Firewall Panel](https://i.imgur.com/MELx6Vl.png)

Click [here](/demo/) to view demo.

- user: `demo`
- password: `demo`

## Screenshots

Only a few screenshots are listed below.


## Screenshots

Only a few screenshots are listed below.

### Firewall Panel

#### Captcha Stats

![Captcha Statistics](https://i.imgur.com/tjc8mW8.png)

#### Online Session Stats

You can see the real-time data here if `Online Session Limit` is enabled.

![Firewall Panel - Online Session Control](https://i.imgur.com/sfssPyj.png)

#### Rule Table

You can temporarily ban a user here.

![Firewall Panel - Rule Table](https://i.imgur.com/5Vg2brX.png)

#### Responsive

Shieldon's Firewall Panel is fully responsive, and you can manage it when you are not in front of your computer, using your mobile phone at any time.

![Responsive Firewall Panel](https://i.imgur.com/fUz9lZD.png)

### Dialog

#### Temporarily Ban a User

When the users or robots are trying to view many your web pages in a short period of time, they will temporarily get banned. Get unbanned by solving a Catpcha.

![Firewall Dialog 1](https://i.imgur.com/rlsEwSG.png)

#### Permanently Ban a User

When a user has been permanently banned.

![Firewall Dialog 2](https://i.imgur.com/Qy1sADw.png)

#### Online Session Control

![Firewall Dialog 3](https://i.imgur.com/cAOKIY8.png)

When a user has reached the online session limit.

### Notification

Provided by [Messenger](https://github.com/terrylinooo/messenger) library.

![Telegram](https://i.imgur.com/3lqamO7.png)

Send notification via Telegram API.

## Author

Shieldon library is brought to you by [Terry L.](https://terryl.in) from Taiwan.

## License

MIT
