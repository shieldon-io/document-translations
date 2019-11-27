# SQLite

## `Shieldon\Driver\SqliteDriver`

- *param* PDO $pdo
- *param* boolean $debug  [default: false]
- *return* self

您需要注入一個 PDO 實例到 Shieldon 資料驅動器中。

```php
new \Shieldon\Driver\SqliteDriver($pdoInstance);
```

例：

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);
$shieldon->setDriver(new \Shieldon\Driver\SqliteDriver($pdoInstance));
```

## 提醒

請不要把 $debug 設為 true，不然 SqliteDriver 會在資料表不存在時丟出錯誤。
