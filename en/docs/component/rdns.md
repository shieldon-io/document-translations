# Rdns

## `Shieldon\Component\Rdns`

- *return* self

```php
$rdns = new \Shieldon\Component\Rdns();
$shieldon->setComponent($rdns);
```

## Strict Mode

- Visitors with empty Rdns record will be blocked.
- IP resolved hostname (Rdns) and IP address must match.

```
$rdns->setStrict(true);
```
