# Header

## `Shieldon\Firewall\Component\Header`

- **return** `self`

```php
$header = new \Shieldon\Firewall\Component\Header();
$shieldon->setComponent($header);
```

## Strict Mode

- Visitors without common header information will be blocked.

```php
$header->setStrict(true);
```

