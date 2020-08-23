# MySQL

## `Shieldon\Firewall\Driver\MysqlDriver`

- **param** `PDO` $pdo `-` The PDO instance.
- **param** `bool` $debug `false` Trun debug mode or not.
- **return** `self`

You have to inject a PDO instance to Shieldon data driver.

```php
$mysqlDriver = new \Shieldon\Firewall\Driver\MysqlDriver($pdoInstance);
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

$kernel->setDriver( new \Shieldon\Firewall\Driver\MysqlDriver($pdoInstance));
```

That's it.