# SQLite

## `Shieldon\Driver\SqliteDriver`

- *param* PDO $pdo
- *param* boolean $debug  [default: false]
- *return* self

You have to inject a PDO instance to Shieldon data driver.

```php
new \Shieldon\Driver\SqliteDriver($pdoInstance);
```

Example:

```php
$dbLocation = APPPATH . 'cache/shieldon.sqlite3';
$pdoInstance = new \PDO('sqlite:' . $dbLocation);
$shieldon->setDriver(new \Shieldon\Driver\SqliteDriver($pdoInstance));
```

## Note

Do not set $debug to true, overwise SqliteDriver will throw an error when data tables not exist.