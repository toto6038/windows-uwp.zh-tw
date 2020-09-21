---
Description: 許多企業都使用防火牆來封鎖不想要的流量。 本檔說明如何允許 WNS 流量通過防火牆。
title: 將 WNS 流量新增至防火牆允許清單
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/20/2019
ms.topic: article
keywords: windows 10、uwp、WNS、windows 通知服務、通知、windows、防火牆、疑難排解、IP、流量、企業、網路、IPv4、VIP、FQDN、公用 IP 位址
ms.localizationpriority: medium
ms.openlocfilehash: 4277b46728464630bf478b1f78008e92b4e3fe99
ms.sourcegitcommit: 41dbee78d827107c224a9136c26f90be4dfe12ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2020
ms.locfileid: "90845527"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>支援 WNS 流量的企業防火牆和 Proxy 設定

## <a name="background"></a>背景
許多企業都使用防火牆來封鎖不必要的網路流量和埠;可惜的是，這也會封鎖像是 Windows 通知服務通訊的重要事項。 這表示透過 WNS 傳送的所有通知都會在特定的網路設定下卸載。 為了避免這種情況，網路系統管理員可以將已核准的 WNS Fqdn 或 Vip 清單新增至其豁免清單，以允許 WNS 流量通過防火牆。 以下是有關如何以及要新增的內容，以及支援不同 proxy 類型的詳細資料。

## <a name="proxy-support"></a>Proxy 支援

> [!Note]
> Windows 上的 WNS 推播通知目前不支援所有 proxy。 為了獲得最佳結果，WNS 的連接必須是直接連線。

我們正在積極調查不同的網路設定、proxy 和防火牆。 我們很快就會更新此頁面，其中包含常見企業案例和 WNS 支援的更多詳細資料。


## <a name="what-information-should-be-added-to-the-allowlist"></a>應新增至允許清單的資訊
以下是一份清單，其中包含 Windows 通知服務所使用的 Fqdn、Vip 和 IP 位址範圍。 

> [!IMPORTANT]
> 我們強烈建議您允許依 FQDN 列出，因為這些不會變更。 如果您允許依 FQDN 列出清單，則不需要也允許 IP 位址範圍。

> [!IMPORTANT]
> IP 位址範圍會定期變更;基於這個原因，這些資訊不會包含在此頁面上。 如果您想要查看 IP 範圍的清單，您可以從下載中心下載檔案： [Windows 通知服務 (WNS) VIP 和 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44238)。 請定期回來查看，確定您有最新的資訊。 


### <a name="fqdns-vips-ips-and-ports"></a>Fqdn、Vip、Ip 和埠
無論您選擇的方法為何，您都必須允許透過 **埠 443**對列出的目的地進行網路流量。 下列 XML 檔中的每個元素都將在下表中說明 ([詞彙和標記法](#terms-and-notations)) 。 這份檔刻意省略 IP 範圍，建議您只使用 Fqdn，因為 Fqdn 會維持不變。 不過，您可以從下載中心下載包含完整清單的 XML 檔案： [Windows Notification Service (WNS) VIP 和 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44238)。 新的 Vip 或 IP 範圍將在 **上傳後的一周內生效**。

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
    <IdentityServiceDNS>
        <DNS FQDN="login.microsoftonline.com"/>
        <DNS FQDN="login.live.com"/>
    </IdentityServiceDNS>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>詞彙和標記法
以下是上述 XML 程式碼片段中使用的標記法和元素的說明。

| 詞彙 | 說明 |
|---|---|
| **小數點-十進位標記法 (亦即 64.4.28.0/26) ** | 點-十進位標記法是描述 IP 位址範圍的方式。 例如，64.4.28.0/26 表示64.4.28.0 的前26個位是常數，而最後6個位是變數。  在此情況下，IPv4 範圍為 64.4.28.0-64.4.28.63。 |
| **ClientDNS** | 這些是用戶端裝置的完整功能變數名稱 (FQDN) 篩選 (Windows 電腦、桌面) 從 WNS 接收通知。 這些必須透過防火牆允許，WNS 用戶端才能使用 WNS 功能。  建議您允許-依 Fqdn （而非 IP/VIP 範圍）列出，因為它們永遠不會變更。 |
| **ClientIPsIPv4** | 這些是用戶端裝置所存取之伺服器的 IPv4 位址， (Windows 電腦、桌面) 接收來自 WNS 的通知。 |
| **CloudServiceDNS** | 這些是您的雲端服務將用來傳送 notificatios 至 WNS 的 WNS 伺服器的完整功能變數名稱 (FQDN) 篩選。 這些必須透過防火牆允許，才能讓服務傳送 WNS 通知。  建議您允許-依 Fqdn （而非 IP/VIP 範圍）列出，因為它們永遠不會變更。|
| **CloudServiceIPs** | CloudServiceIPs 是用於雲端服務的伺服器 IPv4 位址，可將通知傳送至 WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft 推播通知服務 (MPNS) 公用 IP 範圍
如果您使用的是舊版通知服務，您將需要新增至允許清單的 IP 位址範圍可從下載中心取得： [Microsoft 推播通知服務 (MPNS) 公用 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44535)。


## <a name="related-topics"></a>相關主題

* [快速入門：傳送推播通知](/previous-versions/windows/apps/hh868252(v=win.10))
* [如何要求、建立以及儲存通知通道](/previous-versions/windows/apps/hh465412(v=win.10))
* [如何攔截執行應用程式的通知](/previous-versions/windows/apps/jj709907(v=win.10))
* [如何使用 Windows 推播通知服務 (WNS) 進行驗證](/previous-versions/windows/apps/hh465407(v=win.10))
* [推播通知服務要求和回應標頭](/previous-versions/windows/apps/hh465435(v=win.10))
* [推播通知的指導方針和檢查清單](./windows-push-notification-services--wns--overview.md)
 
