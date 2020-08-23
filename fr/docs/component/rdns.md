# Rdns

## `Shieldon\Firewall\Component\Rdns`

- **return** `self`

```php
$rdns = new \Shieldon\Firewall\Component\Rdns();
$shieldon->setComponent($rdns);
```

## Strict Mode

- Visitors with empty Rdns record will be blocked.
- IP resolved hostname (Rdns) and IP address must match.

```php
$rdns->setStrict(true);
```
