# 組件

Shieldon 組件是控制器的集合，讓您能增加客製化的規格，在開始偵測使用者的行為之前許可或拒絕使用者。

- [TrustedBot](https://shieldon.io/en/docs/component/trustedbot.html)
- [Ip](https://shieldon.io/en/docs/component/ip.html)
- [UserAgent](https://shieldon.io/en/docs/component/useragent.html)
- [Header](https://shieldon.io/en/docs/component/header.html)
- [Rdns](https://shieldon.io/en/docs/component/rdns.html)

## `TrustedBot`

TrustedBot 組件能讓受歡迎的搜尋引擎來索引您的網站不受限制。請至少載入這個組件。

## `Ip`

Ip component 能讓您設定單一 IP 位址或者 IP 範圍在白名單或黑名單之中。

## `UserAgent`

UserAgent 組件預設把廣為人知的不良機器人為為黑名登。您可以在 UserAgent 的黑名單中增加您的名單。

## `Header`

Header 組件在嚴格模式時封鎖未帶有常見標題資訊的訪客。

## `Rdns`

Rdns 組件在嚴格模式時封鎖 IP 和反解域名不符合的訪客。

---

## API

### setStrict

- *param* boolean `$bool` `$bool` 設為 true 以啟用嚴格模式, false 反之亦然。
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

在許可清單和拒絕清單中移除項目 (如果有存在的話)

- *param* string `$string`
- *return* void

```php
$list = $component->removeItem($string);
```