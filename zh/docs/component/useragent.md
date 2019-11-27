# UserAgent

## `Shieldon\Component\UserAgent`

- *return* self

```php
$agent = new \Shieldon\Component\UserAgent();
$shieldon->setComponent($agent);
```

在黑名單內的預設值。

| 使用者代理 | 說明 |
| --- | --- |
| domain | 網域名稱資訊爬蟲。 |
| copyright | 版權名稱資訊爬蟲。 |
| Ahrefs | 反向連結爬蟲 |
| roger | 反向連結爬蟲 (SEOMOZ) |
| moz | SEOMOZ crawler. |
| MJ12bot | 反向連結爬蟲 (Majestic) |
| findlinks | 反向連結爬蟲 (findlinks) |
| Semrush | 反向連結爬蟲 (Semrush ) |
| archive | 時光機器網頁儲存庫 |

## 嚴格模式

```
$agent->setStrict(true);
```

- 訪客的使用者代理資訊為空的話會被封鎖。