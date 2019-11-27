# Rdns

## `Shieldon\Component\Rdns`

- *return* self

```php
$rdns = new \Shieldon\Component\Rdns();
$shieldon->setComponent($rdns);
```

## Strict Mode

- 訪客的 RDNS 記錄為空的話會被封鎖。
- IP 反解主機名稱 (RDNS) 和 IP 位址須互相吻合。

```
$rdns->setStrict(true);
```


