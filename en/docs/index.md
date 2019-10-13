
Shieldon, a PHP library that provides anti-scraping and online session control for your web application. As if you are using a shield on your web applicaion to fight against bad-behavior bots, crawlers or vulnerability scanning and so on.

Since 3.0.0, Shieldon starts providing a Firewall Instance and it's visualization UI called Firewall Panel. By using Shieldon Firewall, you can easy implement it on your Web application.

However, if you would like to build your own WAF, combining those public APIs you are able to create one like Shieldon Firewall. Here are detailed documentations that can help you.

## Screenshots

### Temporarily Ban a User

When the users or robots are trying to view many your web pages in a short period of time, they will temporarily get banned. Get unbanned by solving a Catpcha.

![](https://i.imgur.com/rlsEwSG.png)


### Permanently Ban a User

When an user has been permanently banned.

![](https://i.imgur.com/Qy1sADw.png)


### Online Session Control

When an user has reached the online session limit. You can set the online session limit by using `limitSession` API.
![](https://i.imgur.com/U02w70x.png)

## License

MIT

## Author

Shieldon library is brought to you by [Terry L.](https://terryl.in) from Taiwan.
