# UserAgent

## `Shieldon\Component\UserAgent`

- *return* self

```php
$agent = new \Shieldon\Component\UserAgent();
$shieldon->setComponent($agent);
```

Default setting in blacklist.

| user-agent | description |
| --- | --- |
| domain | Domain name information crawler. |
| copyright | Copyright information crawler. |
| Ahrefs | Backlink crawler. |
| roger | Backlink crawler. (SEOMOZ) |
| moz | SEOMOZ crawler. |
| MJ12bot | Backlink crawler. (Majestic) |
| findlinks | Backlink crawler. (findlinks) |
| Semrush | Backlink crawler. (Semrush ) |
| archive | Wayback machine. |

## Strict Mode

- Visitors with empty user-agent information will be blocked.