# Drivers

- [MySQL](https://shieldon.io/en/docs/driver/mysql.html)
- [SQLite](https://shieldon.io/en/docs/driver/sqlite.html)
- [File](https://shieldon.io/en/docs/driver/file.html)
- [Redis](https://shieldon.io/en/docs/driver/redis.html)

## Interface

### get

```php
get(string $ip, string $type = 'log'): array
```

### getAll

```php
getAll(string $type = 'log'): array
```

### has
```php
has(string $ip, string $type = 'log'): bool
```

### save

```php
save(string $ip, array $data, string $type = 'log'): int 
```

### delete
```php
delete(string $ip, string $type = 'log'): bool
```

### rebuild

```php
rebuild(): bool
```