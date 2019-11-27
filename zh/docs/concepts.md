# 運作概念

這是關於 Shielon 如何運作的基本概念。

![Shieldon 防火牆運作概念](https://i.imgur.com/pRbI7gg.png)


- 網路層防火牆，例如 CloudFlare。
- 系統層防火牆，例如 iptables 套件。
- 使用防火牆軟體在應用程式層，我們能夠部署 Shieldon 在您的應用程式的初始階段，大部份位於 Composer 自動載入器之後。
- Shieldon 分析您全部的 HTTP 及 HTTPS 連線請求。
- 只要 Shieldon 偵測到連線請求的奇怪行為，Shieldon 會暫時地封鎖它並跳出驗證碼讓它解除封鎖。
- 如果連線請求連續失敗很多次 (取決於您的設定值)，它會在目前的資料週期被永久的封鎖。
- 如果連線請求已經被永久封鎖，但它然一直連接您的網站，把它丟到系統層防火牆 - iptables.