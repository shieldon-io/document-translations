# 檔案

## `Shieldon\Driver\FileDriver`

- *param* string $directory 目錄的路徑。
- *return* self

例：

```php
$shieldon->setDriver(
    new \Shieldon\Driver\FileDriver(BOOTSTRAP_DIR . '/../tmp/shieldon')
);
```

大概是這樣囉。