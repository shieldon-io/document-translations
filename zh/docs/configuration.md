# 組態

在初始化 Shieldon 實例時，您可以調整設定值，或者在稍候使用 `setProperty` 或 `setProperties` API。

*初始化*
```php

$config = [
    'time_unit_quota' => [
        ['s' => 2, 'm' => 10, 'h' => 30, 'd' => 60]
    ]
];

$shieldon = new \Shieldon\Shieldon($config);
```

*setProperty*
```php
$shieldon->setProperty('time_unit_quota', [
    's' => 2, 'm' => 10, 'h' => 30, 'd' => 60
]);
```

*setProperties*
```php

$config = [
    'time_unit_quota' => [
        ['s' => 2, 'm' => 10, 'h' => 30, 'd' => 60]
    ]
];

$shieldon->setProperties($config);
```
## 預設值

| key | 類型 | value |
| --- | --- | --- |
| time_unit_quota | array |  `['s' => 2, 'm' => 10, 'h' => 30, 'd' => 60]` |
| time_reset_limit | integer | 3600 |
| interval_check_referer | integer | 5 |
| interval_check_session | integer | 30 |
| limit_unusual_behavior | array | `['cookie' => 5, 'session' => 5, 'referer' => 10]` |
| cookie_name | string | ssjd |
| cookie_domain | string | " |


### time_unit_quota

為您網站上的訪客設定限制及額度。

您也許可以查看您的 Google 統計，找出訪客瀏覽網頁數量的平均值，然後想一下在您的網站上，什麼情況要被視為　**不良行為**。

- array

| key | value | description |
| --- | --- | --- |
| s | integer | 每位 **訪客** 每 **秒** 的網頁瀏覽數 |
| m | integer | 每位 **訪客** 每 **分** 的網頁瀏覽數 |
| h | integer | 每位 **訪客** 每 **時** 的網頁瀏覽數 |
| d | integer | 每位 **訪客** 每 **天** 的網頁瀏覽數 |

#### Tips

- 如果您只要限制使用者在一天內檢視 `100 張網頁`，只要把　`s`, `m`, `h` 設一個很高的數字，且設　`d` 到 `100`　即可。
- 要記得，當使用者達到限制時將會被暫時地封鎖，他們能夠藉由解決驗證碼來解封，所以不要把數字設的太寬鬆，否則失去用這個套件的意思囉。

### time_reset_limit

在 `time_reset_limit` 設定的多少秒後重設過濾器的標記數字。

### interval_check_referer

當使用者第一次造訪您的網站，在瀏覽器直接輸入網址時，`HTTP_REFERER` 是空值，在經過　`interval_check_referer` 的秒數後，Shieldon 將開始檢查 `HTTP_REFERER` 的值。

*您可以忽略這個設定值*

### interval_check_session

當使用者第一次造訪您的網站，在經過 `interval_check_session` 的秒數後，Shieldon 將開始檢查 `SESSION` 的值。

### limit_unusual_behavior

針對在您網站上的訪客，設定被標記為不尋常行為的限制和額度。

- array

| key | value | description |
| --- | --- | --- |
| cookie | integer | 由 JavaScript 產生的 Cookie |
| session | integer | PHP Session |
| referer | integer | HTTP_REFERER |

###　其它

請忽略。
