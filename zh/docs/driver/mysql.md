# MySQL

## `Shieldon\Driver\MysqlDriver`

- *param* PDO $pdo
- *param* boolean $debug  [預設值: false]
- *return* self

您需要注入一個 PDO 實例到 Shieldon 資料驅動器中。

```php
$mysqlDriverInstance = new \Shieldon\Driver\MysqlDriver($pdoInstance);
```

例：

```php
$db = [
    'host' => '127.0.0.1',
    'dbname' => 'testdb',
    'user' => 'root',
    'pass' => 'sdfaa422kadhd3',
    'charset' => 'utf8',
];

$pdoInstance = new \PDO(
    'mysql:host=' . $db['host'] . ';dbname=' . $db['dbname'] . ';charset=' . $db['charset'],
    $db['user'],
    $db['pass']
);

$shieldon->setDriver( new \Shieldon\Driver\MysqlDriver($pdoInstance));
```

大概是這樣囉。