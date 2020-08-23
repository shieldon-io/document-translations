# SQLite

## `Shieldon\Firewall\Driver\SqliteDriver`

- **param** `PDO` $pdo `-` The PDO instance.
- **param** `bool` $debug `false` Trun debug mode or not.
- **return** `self`

You have to inject a PDO instance to Shieldon data driver.

```php
$sqliteDriver = new \Shieldon\Firewall\Driver\SqliteDriver($pdoInstance);
```

Example:

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);

$kernel->setDriver(
    new \Shieldon\Firewall\Driver\SqliteDriver($pdoInstance)
);
```

## Note

Do not set ***$debug*** to *true*, overwise SqliteDriver will throw an error when data tables not exist.