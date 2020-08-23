# File

## `Shieldon\Firewall\Driver\FileDriver`

- **param** `string` $directory `-` Directory path.
- **return** `self`

Example:

```php
$kernel->setDriver(
    new \Shieldon\Firewall\Driver\FileDriver(BOOTSTRAP_DIR . '/../tmp/shieldon')
);
```

That's it.