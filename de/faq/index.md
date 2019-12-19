# FAQ

If you have any question please post it on [stackoverflow](https://stackoverflow.com/), if you have found any bug, please post it to [issue forum](https://github.com/terrylinooo/shieldon/issues) of Shieldon's repository.


- [Can I limit session number for only a specific page?](#faq-1)
- [How do I block all IP addresses for a specific URL?](#faq-2)
- [What is the default login and password for Firewall Panel?](#faq-3)
- [Can Shieldon Firewall truly mitigate DDOS attack?](#faq-4)

## Frequently Asked Questions

<div id="faq-1"></div>

### Can I limit session number for only a specific page?

The answer is **YES**, but you have to turn off `Online Session Limit` in Firewall Panel, because its scope is global - it cannot be used on the same data pool.

After turning `Online Session Limit` off, you can use Shieldon's public API, for example as the following code:

```php
if (strpos($_SERVER['REQUEST_URI'], 'faq/online-session-limit.html') !== false) {
    $firewall->getShieldon()->limitSession(5, 300);
}
```

[This page](/en/faq/online-session-limit.html) is limiting the maximum user number of **5**, and each user has **300** seconds in viewing this page.

When entering this page and the user number has reached the limitation, the dialog that is similar to following screenshot will be showed to you. 

![](https://i.imgur.com/cAOKIY8.png)

<div id="faq-2"></div>

### How do I block all IP addresses for a specific URL?

I do not put that `block all` feature into Firewall Panel because of preventing some smart guys block all IP addresses of URL `/`. This is block all URLs including the entry of Firewall Panel. I suggest you use IP manager instead and set up a needed IP range.

If you want to block all IP addresses for a specific URL , the following code will help you.

```php
// Put this code before $firewall->run();
if (strpos($_SERVER['REQUEST_URI'], 'example/block-all.html') !== false) {
    $firewall->getShieldon()->getComponent('Ip')->denyAll();
}
```

[This is an example page](/en/faq/block-all.html) that blocks all IP addresses from the Internet.

When a user has been blocked, they will see the dialog just like the screenshot below.

![](https://i.imgur.com/Qy1sADw.png)

<div id="faq-3"></div>

### What is the default login and password for Firewall Panel?

The default login is `shieldon_user` and `password` is `shieldon_pass`. After logging in the Firewall Panel, the first thing you need to do is to change the login and password.

<div id="faq-4"></div>

### Can Shieldon Firewall truly mitigate DDOS attack?

For the small scale of HTTP-type DDOS attacks, my answer is **YES**, but the real situation depends on many factors such as the bandwidth, hardware level, system adjustment, code quality, and so on.

Let me take a simple example, assuming your website has an average page size of 3 MB, and the bandwidth of your server is 100 Mbps, your server can actually  handle megabyte-per-second is `12.5 MB/s`, If someone would like to attack your webiste maliciously, do you think how many attacking sources can block your server? Not to mention the heavy loading to the MySQL connection.

Shieldon Firewall interrupts the malicious connection, returning a CAPTCHA page size that it is less than 50 KB, stopping executing PHP scripts - no more MySQL connection - Shieldon Firewall saves memory and CPU usage when your website is under attack - but, it is just a way to mitigate HTTP-type DDOS attack, not a final solution.