# Redis

## `Shieldon\Driver\RedisDriver`

- *param* Redis $redis
- *return* self

Make sure you have installed PHP Redis extension and Redis server on your server. You should see something like the screenshot below in `php.ini`.
![](https://i.imgur.com/Ru74yN4.png)

Inject a Redis instance to Shieldon data driver.

```php
$redisDriverInstance = new \Shieldon\Driver\RedisDriver($redisInstance));
```

例：

```php
$redisInstance = new \Redis();
$redisInstance->connect('127.0.0.1', 6379); 

$shieldon->setDriver(new \Shieldon\Driver\RedisDriver($redisInstance));
```

大概是這樣囉。