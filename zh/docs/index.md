
Shieldon 是一個專用於 PHP 的網站應用層防火牆 (WAF)，只要不到 10 分鐘，專家級的 PHP 開發者能用瞭解如果部暑 Shieldon 防火牆到他們的網站應用程式中。這個套件的目標是讓 PHP 社群更加安全，且極其容易使用。

- 網站: [https://shieldon.io](https://shieldon.io/)
- GitHub 專案庫:  [https://github.com/terrylinooo/shieldon](https://github.com/terrylinooo/shieldon)

## 特色

- 對於搜尋引擎最佳化友善。
- Http 型態的 DDOS 攻擊緩和。
- 防砍站。
- 線上工作階段控制。
- 跨站腳本 (XSS) 防護。
- 打斷弱點掃描。
- 清除暴力攻擊。
- IP 管理員。
- 經由 WWW-Authenticate 保護網頁。
- 詳細的統計表和曲線圖。
- 當特定的事件發生時送出通知。支援的模組：
    - Slack
    - Telegram
    - Line Notify
- 系統防火牆的網頁介面 - iptables 及 ip6tables.
- 更多的特色將持續開發...

## 安裝

使用 PHP Composer:

```php
composer require shieldon/shieldon
```

或者，下載或引入 Shieldon 自動載入器。

```php
require 'Shieldon/autoload.php';
```

## 部署

這裡是整合受歡迎的 PHP 框架的指南。

- [Laravel](https://shieldon.io/en/guide/laravel.html)
- [Symfony](https://shieldon.io/en/guide/symfony.html)
- [CodeIgniter](https://shieldon.io/en/guide/codeigniter.html)
- [CakePHP](https://shieldon.io/en/guide/cakephp.html)
- [Yii](https://shieldon.io/en/guide/yii.html)
- [Zend](https://shieldon.io/en/guide/zend.html)
- [Slim](https://shieldon.io/en/guide/slim.html)
- [Fat-Free](https://shieldon.io/en/guide/fatfree.html)
- [Fuel](https://shieldon.io/en/guide/fuel.html)
- [PHPixie](https://shieldon.io/en/guide/phpixie.html)

## 防火牆面板

Shieldon 提供一個虛擬化的使用者介面，稱為防火牆面板。藉由使用 Shieldon 防火牆，您可以容易地部署它到您的網站應用程式中。

![Firewall Panel](https://i.imgur.com/MELx6Vl.png)

Click [here](/demo/) to view demo.

- user: `demo`
- password: `demo`

## 截圖

只有一些截圖會列在下方。

### 防火牆面板

#### 驗證碼統計

![驗證碼統計](https://i.imgur.com/tjc8mW8.png)

#### 線上工作階段統計

如果 `線上工作階段控制` 有啟用的話，您可以在這裡看到即時的資料。

![](https://i.imgur.com/sfssPyj.png)

#### 規則表

您可以在這裡暫時封鎖使用者。

![](https://i.imgur.com/5Vg2brX.png)

#### 響應式

Shieldon 的防火牆面板是全響應式的，所以您可以使用手機管理，即時您不在電腦前。


![響應式的防火牆面板](https://i.imgur.com/fUz9lZD.png)

### 對話框

#### 暫時地封鎖用戶

當使用者或者機器人們正試著在短時間內檢閱很多您的網站頁面，他們會被暫時地封鎖。用解決驗證碼的方式來解除封鎖。

![](https://i.imgur.com/rlsEwSG.png)

#### 永久地封鎖使用者

當使用者被永久地封鎖。

![](https://i.imgur.com/Qy1sADw.png)


#### 線上工作階段控制

當使用者到了線上工作階段的限制。

![](https://i.imgur.com/U02w70x.png)

## 作者

Shieldon 套件是由來自台灣的開發者 [Terry L.](https://terryl.in) 進行開方維護。

## 授權

MIT
