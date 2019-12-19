# 問答集

如果您有任何問題請發表在 [stackoverflow](https://stackoverflow.com/)。
如果您發現任何臭蟲，請發表在 Shieldon 原始碼專案庫的[問題回報區](https://github.com/terrylinooo/shieldon/issues)。


- [我能否只限制一張網頁的線上工作階段控制？](#faq-1)
- [我如何針對特定網址阻擋所有的 IP 位址？](#faq-2)
- [預設的防火牆面板的帳號及密碼是什麼？](#faq-3)
- [Shieldon 防火牆真的能緩和 DDOS 攻擊嗎？](#faq-4)

## 常被問到的問題

<div id="faq-1"></div>

### 我能否只限制一張網頁的線上工作階段控制？

答案是 **可是的**，但是您必須在防火牆面板中關掉 `線上工作階段控制`，因為它的影響範圍是全域，無法被用在同一個資料池。

在關掉 `線上工作階段控制` 之後，您可以使用　Shieldon 的開放 API，就如同以下的程式碼：

```php
if (strpos($_SERVER['REQUEST_URI'], 'faq/online-session-limit.html') !== false) {
    $firewall->getShieldon()->limitSession(5, 300);
}
```

[這個網頁](/zh/faq/online-session-limit.html)正限制最大線上瀏覽人數為 **5**，而每位使用者有 **300** 秒的時間可以檢視這張網頁。

當進入這個網頁時而使用者人數已達到限制，類似以下截圖的對話框會展示給您。

![](https://i.imgur.com/cAOKIY8.png)

<div id="faq-2"></div>

### 我如何針對特定網址阻擋所有的 IP 位址？

我並沒有放 `阻擋全部` 功能到防火牆面板中，因為是為了預防有些聰明人阻擋了全部的 IP 位址到 `/`。這為連同防火牆面板的入口也一起封鎖。我建議您改用 IP 管理員並設定需要的 IP 區段。

如果您真的為了特定的網址阻擋全部的 IP 位址，以下的程式碼能幫到您。

```php
// 在這段程式碼放在 $firewall->run(); 之前。
if (strpos($_SERVER['REQUEST_URI'], 'example/block-all.html') !== false) {
    $firewall->getShieldon()->getComponent('Ip')->denyAll();
}
```

[這一個範例網頁](/en/faq/block-all.html)封鎖了網路上全部的 IP。

當使用者已經被封鎖，他們將會看到像是以下截圖的對話框。

![](https://i.imgur.com/Qy1sADw.png)

<div id="faq-3"></div>

### 預設的防火牆面板的帳號及密碼是什麼？

預設的登入帳號是 `shieldon_user` 而密碼是 `shieldon_pass`。在您登入防火牆面板之後，第一件該做的事情就是更改帳號及密碼。


<div id="faq-4"></div>

### Shieldon 防火牆真的能緩和 DDOS 攻擊嗎？

對於小規模的 HTTP 型態 DDOS 攻擊，我的答案是 **可以的**，但實際的情況取決於許多因素像是頻寬、硬體等級、系統調校、程式碼品質等等。

讓我舉個簡單的例子，假設您的網站頁面平均下載尺寸為 3MB，而您伺服器的頻寬為 100 Mbps，您的伺服器實際上能處理的頻寬為 `12.5 MB/s`，如果有人真要惡意攻擊您的網站，您想想有多少攻擊來源能夠阻斷您的伺服器？更不用說 MySQL 連線的高負載量。

Shieldon 阻斷惡意連線，傳回一個尺寸不到 50KB 驗證碼頁，中止後續執行的 PHP 程式，沒有更多的 MySQL 連線。Shieldon 防火牆在您糟遇攻擊時節省 CPU 和記憶體，但是呢，它只是一個方法來緩解 HTTP 類型的 DDOS 攻擊，並非最後的解決方案。