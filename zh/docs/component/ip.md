# IP

## `Shieldon\Firewall\Component\Ip`

- **return** `self`

```php
$ip = new \Shieldon\Firewall\Component\Ip();
$shieldon->setComponent($ip);
```

### inRange(`$ip`, `$range`)

- **param** `string` $ip `-` IP to check in IPV4 and IPV6 format
- **param** `mixed` $range `-` IP/CIDR netmask
- **return** bool

```php
$result = $ip->inRange('123.22.33.44', '123.22.33.1/24');

// true
```
### setAllowedItems(`$ips`)

- **param** `array` $ips `-` A colloection of IP addresses.
- **return** `void`

```php
$ip = new \Shieldon\Firewall\Component\Ip();

$allowedIps = [
    '123.22.33.44',
    '88.22.33.55',
];

$ip->setAllowedItems($allowedIps);
$shieldon->setComponent($ip);
```

### setAllowedItem(`$ip`)

- **param** `string` $ip `-` Single Ip address
- **return** `void`

```php
$ip = new \Shieldon\Firewall\Component\Ip();
$ip->setAllowedItem('123.22.33.44');
$shieldon->setComponent($ip);
```

### getAllowedItems()

- **return** `array`

```php
$ip = new \Shieldon\Firewall\Component\Ip();
$list = $ip->getAllowedItems();

// ['123.22.33.44', '123.22.33.43', 'xxx.xxx.xxx.xxx']
```
### setDeniedItems(`$ips`)

- **param** `array` $ips `-` IP array.
- **return** `void`

```php
$ip = new \Shieldon\Firewall\Component\Ip();

$deniedIps = [
    '123.22.33.44',
    '88.22.33.55',
];

$ip->setDenieddList($deniedIps);
$shieldon->setComponent($ip);
```

### setDeniedItem(`$ip`)

- **param** `string` $ip `-` Single Ip address
- **return** `void`

```php
$ip = new \Shieldon\Firewall\Component\Ip();
$ip->setDeniedItem('123.22.33.44');
$shieldon->setComponent($ip);
```

### getDeniedItems()

- **return** `array`

```php
$ip = new \Shieldon\Firewall\Component\Ip();
$list = $ip->getDeniedItems();

// ['123.22.33.44', '123.22.33.43', 'xxx.xxx.xxx.xxx']
```

