# Concepts

This is basic concepts about how Shieldon works.

![Shieldon Firewall Concepts](https://i.imgur.com/pRbI7gg.png)


- The network-layer firewall such as CloudFlare.
- The system-layer firewall such as iptables module.
- To use firewall software in the Web application layer, we are capable of implementing Shieldon in a very early stage of your APP, mostly just after Composer autoloader.
- Shieldon analyzes all your HTTP and HTTPS requests.
- Once Shieldon has detected strange behaviors of a request, Shieldon will temporarily ban them and prompt them CAPTCHA for them to unban.
- If a request fails in a row many times (depends on your setting), they will be permanently banned in current data circle.
- If a request has been permanently banned, but they still access your page, drop them in System-layer firewall - iptables.