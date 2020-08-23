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

### setStrict(`$bool`)

- **param** `bool` $bool `-` Set true to enble strict mode, false to disable it overwise.
- **return** `void`

```php
$component->setStrict(true);
```

---

## Denied Trait

- setDeniedItem
- setDeniedItems
- getDeniedItem
- getDeniedItems
- removeDeniedItem
- removeDeniedItems
- hasDeniedItem
- getDenyWithPrefix
- removeDenyWithPrefix
- isDenied

### setDeniedItem(`$value`, `$key`)

- **param** `string|array` $value `-` The value of the data.
- **param** `string` $key `-` The key of the data.
- **return** `void`

Add an item to the blacklist pool.

Example:

```php
$component->setDeniedItem($string);
```

### setDeniedItems(`$itemList`)

- **param** `array` $itemList `-` String list.
- **return** `void`

Add items to the blacklist pool.

Example:

```php
$component->setDeniedItems($stringList);
```

### getDeniedItem(`$key`)

- **param** `string` $key `-` The key of the data field.
- **return** `string|array`

Get an item from the blacklist pool.

Example:

```php
$item = $component->getDeniedItems('this_item');
```

### getDeniedItems()

- **return** `array`

Get the items from the blacklist pool.

```php
$list = $component->getDeniedItems();
```

### removeDeniedItem(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `void`

Remove a denied item if exists.

```php
$component->removeDeniedItem($string);
```

### removeDeniedItems()

- **return** `void`

Remove all denied items.

```php
$component->removeDeniedItems();
```

### hasDeniedItem(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `bool`

Check if a denied item exists.

Example:

```php
if ($component->hasDeniedItem('test')) {
    echo 'item exists';
} else {
    echo 'item does not exist';
}
```

### getDenyWithPrefix(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `array`

Check if denied items exist with the same prefix.

Example:

```php
$deniedList = $component->getDenyWithPrefix('test');
```

### removeDenyWithPrefix(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `void`

Remove denied items with the same prefix.

```php
$component->removeDenyWithPrefix('test');
```

### isDenied()

- **return** `bool`

This method should adjust in extended class if need.

```php

if ($component->isDenied()) {
    echo 'This user has been denied.';
}
```

---

## Allowed Trait

- setAllowedItem
- setAllowedItems
- getAllowedItem
- getAllowedItems
- removeAllowedItem
- removeAllowedItems
- hasAllowedItem
- getDenyWithPrefix
- removeDenyWithPrefix
- isAllowed

### setAllowedItem(`$value`, `$key`)

- **param** `string|array` $value `-` The value of the data.
- **param** `string` $key `-` The key of the data.
- **return** `void`

Add an item to the blacklist pool.

Example:

```php
$component->setAllowedItem($string);
```

### setAllowedItems(`$itemList`)

- **param** `array` $itemList `-` String list.
- **return** `void`

Add items to the blacklist pool.

Example:

```php
$component->setAllowedItems($stringList);
```

### getAllowedItem(`$key`)

- **param** `string` $key `-` The key of the data field.
- **return** `string|array`

Get an item from the blacklist pool.

Example:

```php
$item = $component->getAllowedItems('this_item');
```

### getAllowedItems()

- **return** `array`

Get the items from the blacklist pool.

```php
$list = $component->getAllowedItems();
```

### removeAllowedItem(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `void`

Remove a allowed item if exists.

```php
$component->removeAllowedItem($string);
```

### removeAllowedItems()

- **return** `void`

Remove all allowed items.

```php
$component->removeAllowedItems();
```

### hasAllowedItem(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `bool`

Check if a allowed item exists.

Example:

```php
if ($component->hasAllowedItem('test')) {
    echo 'item exists';
} else {
    echo 'item does not exist';
}
```

### getDenyWithPrefix(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `array`

Check if a allowed item exists with the same prefix.

Example:

```php
$allowedList = $component->getDenyWithPrefix('test');
```

### removeDenyWithPrefix(`$key`)

- **param** `string` $key `-` The key of the data.
- **return** `void`

Remove allowed items with the same prefix.

```php
$component->removeDenyWithPrefix('test');
```

### isAllowed()

- **return** `bool`

This method should adjust in extended class if need.

```php

if ($component->isAllowed()) {
    echo 'This user has been allowed.';
}
```