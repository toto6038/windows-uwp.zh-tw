---
author: mijacobs
Description: 許多企業會使用防火牆來封鎖不想要的流量。 此文件說明如何允許通過防火牆的 WNS 流量。
title: WNS 流量加入防火牆允許清單
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 uwp、 WNS，windows 通知服務、 通知、 windows、 防火牆、 疑難排解、 IP、 流量、 enterprise、 網路、 IPv4、 VIP、 FQDN、 公用 IP 位址
ms.localizationpriority: medium
ms.openlocfilehash: 466463bfc984707af4cb30618f9cbfa47d78703c
ms.sourcegitcommit: fd7d358aad3a5b7112f5a587bb6ea86805dc8a4d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976248"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>透過企業防火牆允許 Windows 通知流量

## <a name="background"></a>背景
許多企業會使用防火牆來封鎖不必要的網路流量;不幸的是，這也可以封鎖重要的事項，例如 Windows 通知服務通訊。 這表示透過 WNS 傳送的所有通知皆會予以都捨棄。 若要避免這個問題，網路系統管理員可以將已核准的 WNS 通道的清單，加入其豁免清單，以允許通過防火牆的 WNS 流量。 以下是更多詳細資料，而要新增的項目。 


## <a name="what-information-should-be-added-to-the-allowlist"></a>應是何種資訊新增至允許清單
以下是清單，其中包含的 Fqdn、 Vip 及與 IP 位址 Windows 通知服務所使用的範圍。 

> [!IMPORTANT]
> 我們強烈建議您允許清單中的 FQDN，因為這些不會變更。 如果您允許透過 FQDN 的清單，您不需要也允許的 IP 位址範圍。

> [!IMPORTANT]
> 會定期; 變更 IP 位址範圍基於這個原因，它們不會包含在此頁面上。 如果您想要查看的 IP 範圍清單，您可以從下載中心下載檔案：[Windows 通知服務 (WNS) VIP 和 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44238)。 請檢查後定期以確定您有最新的資訊。 


### <a name="fqdns-vips-and-ips"></a>Fqdn、 Vip 及和 Ip
它後面的表格中說明的每個下列的 XML 文件中的項目 (在[條款和標記法](#terms-and-notations)。 IP 範圍是故意維持此文件，建議您僅 Fqdn 作為 Fqdn 會保持不變。 不過，您可以下載包含從下載中心取得的完整清單的 XML 檔案：[Windows 通知服務 (WNS) VIP 和 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44238)。 新的 Vip 或 IP 範圍會被**有效的一週後會上傳**。

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

### <a name="terms-and-notations"></a>條款及標記法
以下是說明標記法和上述的 XML 程式碼片段中所使用的項目上。

| 詞彙 | 說明 |
|---|---|
| **小數點十進位表示法 (也就是 64.4.28.0/26)** | 小數點十進位表示法是一個方式來描述範圍的 IP 位址。 比方說，64.4.28.0/26 表示 64.4.28.0 前 26 個位元是常數，而最後的 6 位元是變數。  在此案例中，IPv4 範圍是 64.4.28.0-64.4.28.63。 |
| **ClientDNS** | 這些是用戶端裝置 （Windows 電腦、 桌上型電腦） 的 完整網域名稱 (FQDN) 篩選器從 WNS 中接收通知。 這些必須允許通過 WNS 用戶端，才能使用 WNS 功能中的防火牆。  建議加入允許清單的 Fqdn，而不是 IP/VIP 範圍中，因為這些永遠不會變更。 |
| **ClientIPsIPv4** | 這些是透過用戶端裝置 （Windows 電腦、 桌上型電腦） 存取伺服器的 IPv4 位址接收 WNS 的通知。 |
| **CloudServiceDNS** | 這些是您的雲端服務會向將通知傳送給 WNS 的 WNS 伺服器的 完整網域名稱 (FQDN) 篩選。 這些必須允許通過防火牆，為了讓服務以傳送 WNS 通知。  建議加入允許清單的 Fqdn，而不是 IP/VIP 範圍中，因為這些永遠不會變更。|
| **CloudServiceIPs** | CloudServiceIPs 會用於雲端服務，以傳送給 WNS 的通知伺服器的 IPv4 位址  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft 推播通知服務 (MPNS) 公用 IP 範圍
如果您使用舊版的 notification service，MPNS，您必須新增至允許清單的 IP 位址範圍可從下載中心取得：[Microsoft 推播通知服務 (MPNS) 的公用 IP 範圍](https://www.microsoft.com/download/details.aspx?id=44535)。


## <a name="related-topics"></a>相關主題

* [快速入門：傳送推播通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [如何要求、 建立和儲存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [如何攔截來執行應用程式的通知](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [如何驗證與 Windows 推播通知服務 (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [推播通知服務要求和回應標頭](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [指導方針和推播通知的檢查清單](https://msdn.microsoft.com/library/windows/apps/hh761462)
 
