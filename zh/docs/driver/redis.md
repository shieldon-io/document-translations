# Redis

## `Shieldon\Firewall\Driver\RedisDriver`

- **param*** `Redis` $redis `-` The Redis instance.
- **return** void

Make sure you have installed PHP Redis extension and Redis server on your server. You should see something like the screenshot below in `php.ini`.
![](https://i.imgur.com/Ru74yN4.png)

Inject a Redis instance to Shieldon data driver.

```php
$redisDriver = new \Shieldon\Firewall\Driver\RedisDriver($redisInstance));
```

Example:

```php
$redisInstance = new \Redis();
$redisInstance->connect('127.0.0.1', 6379); 

$kernel->setDriver(
    new \Shieldon\Firewall\Driver\RedisDriver($redisInstance)
);
```

That's it.