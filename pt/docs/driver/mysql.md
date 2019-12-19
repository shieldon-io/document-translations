# MySQL

## `Shieldon\Driver\MysqlDriver`

- *param* PDO $pdo
- *param* boolean $debug  [default: false]
- *return* self

You have to inject a PDO instance to Shieldon data driver.

```php
$mysqlDriverInstance = new \Shieldon\Driver\MysqlDriver($pdoInstance);
```

Example:

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

That's it.