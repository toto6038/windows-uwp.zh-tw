---
author: mijacobs
Description: 許多企業都使用防火牆來封鎖不想要的流量。 本檔說明如何允許 WNS 流量通過防火牆。
title: 將 WNS 流量新增至防火牆允許清單
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
keywords: windows 10, uwp, WNS, windows 通知服務, 通知, windows, 防火牆, 疑難排解, IP, 流量, 企業, 網路, IPv4, VIP, FQDN, 公用 IP 位址
ms.localizationpriority: medium
ms.openlocfilehash: 1f8a72eec46971fa27a4bd0dec112430f2eb3535
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867303"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>透過企業防火牆允許 Windows 通知流量

## <a name="background"></a>背景
許多企業都使用防火牆來封鎖不必要的網路流量;可惜的是, 這也會封鎖 Windows 通知服務通訊之類的重要事項。 這表示所有透過 WNS 傳送的通知都將遭到捨棄。 若要避免這種情況, 網路系統管理員可以將已核准的 WNS 通道清單新增至其豁免清單, 以允許 WNS 流量通過防火牆。 以下是有關如何和要新增之內容的詳細資訊。 

> [!Note] 
從 6/24/2019, Windows 用戶端**不**支援 PROXY, WNS 的連接必須是直接連線。

## <a name="what-information-should-be-added-to-the-allowlist"></a>應該將哪些資訊新增至允許清單
以下清單包含 Windows 通知服務所使用的 Fqdn、Vip 和 IP 位址範圍。 

> [!IMPORTANT]
> 我們強烈建議您允許依 FQDN 列出, 因為這些不會變更。 如果您允許依 FQDN 列出, 則不需要同時允許 IP 位址範圍。

> [!IMPORTANT]
> IP 位址範圍會定期變更;因此, 這些不包含在此頁面上。 如果您想要查看 IP 範圍的清單, 您可以從下載中心下載檔案:[Windows 通知服務 (WNS) VIP 和 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44238)。 請定期回來查看, 確定您有最新的資訊。 


### <a name="fqdns-vips-and-ips"></a>Fqdn、Vip 和 Ip
下列 XML 檔中的每個元素都會在其後面的表格中說明 (以[和標記法為依據](#terms-and-notations))。 這些 IP 範圍被刻意排除在本檔中, 以建議您只使用 Fqdn, 因為 Fqdn 將維持不變。 不過, 您可以從下載中心下載包含完整清單的 XML 檔案:[Windows 通知服務 (WNS) VIP 和 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44238)。 新的 Vip 或 IP 範圍將在**上傳後的一周內生效**。

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>詞彙和標記法
以下是上述 XML 程式碼片段中所使用之標記法和元素的說明。

| 詞彙 | 說明 |
|---|---|
| **點-十進位標記法 (例如 64.4.28.0/26)** | 點十進位標記法是描述 IP 位址範圍的方式。 例如, 64.4.28.0/26 表示64.4.28.0 的前26位是固定的, 而最後6個位則是可變的。  在此情況下, IPv4 範圍是 64.4.28.0-64.4.28.63。 |
| **ClientDNS** | 這些是從 WNS 接收通知之用戶端裝置 (Windows 電腦、桌面) 的完整功能變數名稱 (FQDN) 篩選器。 這些必須通過防火牆, 才能讓 WNS 用戶端使用 WNS 功能。  建議您依 Fqdn 而不是 IP/VIP 範圍來允許清單, 因為這些永遠不會變更。 |
| **ClientIPsIPv4** | 這些是用戶端裝置 (Windows 電腦、桌面) 所存取伺服器的 IPv4 位址, 會接收來自 WNS 的通知。 |
| **CloudServiceDNS** | 這些是您的雲端服務將會與之通訊的完整功能變數名稱 (FQDN) 篩選器, 以傳送通知到 WNS。 必須透過防火牆允許這些功能, 服務才能傳送 WNS 通知。  建議您依 Fqdn 而不是 IP/VIP 範圍來允許清單, 因為這些永遠不會變更。|
| **CloudServiceIPs** | CloudServiceIPs 是用於雲端服務的伺服器 IPv4 位址, 可將通知傳送至 WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft 推播通知服務 (MPNS) 公用 IP 範圍
如果您使用的是舊版通知服務 MPNS, 您將需要新增至允許清單的 IP 位址範圍可從下載中心取得:[Microsoft 推播通知服務 (MPNS) 公用 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44535)。


## <a name="related-topics"></a>相關主題

* [快速入門：傳送推播通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [如何要求、建立和儲存通知通道](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [如何攔截執行中應用程式的通知](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [如何使用 Windows 推播通知服務 (WNS) 進行驗證](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [推播通知服務要求和回應標頭](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [推播通知的方針和檢查清單](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
