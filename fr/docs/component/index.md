# Components

Shieldon components are are sets of controller that allow you to add more custom rules to allow or deny before detecting user's behavior.

- [TrustedBot](https://shieldon.io/en/docs/component/trustedbot.html)
- [Ip](https://shieldon.io/en/docs/component/ip.html)
- [UserAgent](https://shieldon.io/en/docs/component/useragent.html)
- [Header](https://shieldon.io/en/docs/component/header.html)
- [Rdns](https://shieldon.io/en/docs/component/rdns.html)

## `TrustedBot`

TrustedBot component allows popular search engines to crawl your site without limit. please load this commponent at least .

## `Ip`

Ip component allows you to set single IPs or IP ranges in the whitelist or the blacklist.

## `UserAgent`

UserAgent component blocks well-known bad bots by default. You can add your list in UserAgent's blacklist.

## `Header`

Header component blocks vistors without common header information in strict mode, 

## `Rdns`

Rdns component blocks vistors without Rdns recond or Rdns not match to IP address in strict mode.

---

## API

### setStrict

- *param* boolean `$bool` Set true to enble strict mode, false to disable it overwise.
- *return* void

```php
$component->setStrict(true);
```

### setDeniedList

- *param* array `$stringList`
- *return* void

```php
$component->setDeniedList($stringList);
```

### setDeniedItem

- *param* string `$string`
- *return* void

```php
$component->setDeniedItem($string);
```

### getDeniedList

- *return* array

```php
$list = $component->getDeniedList();
```

### removeItem

Remove item from denied list and allowed list (if exists)

- *param* string `$string`
- *return* void

```php
$list = $component->removeItem($string);
```