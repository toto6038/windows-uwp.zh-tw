---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 使用自訂的 SSL 憑證佈建裝置入口網站
description: TBD
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2834487"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>使用自訂的 SSL 憑證佈建裝置入口網站
在 Windows 10 建立者 Update Windows 裝置入口網站新增裝置的系統管理員可以使用自訂憑證安裝中的 HTTPS 通訊的一種方式。 

雖然您可以在您自己的電腦上達成此目的，此功能多半適合已經備妥的現有憑證基礎結構的企業版。  

例如，公司可能必須使用 HTTPS 上所服務的內部網路網站的憑證簽署憑證授權單位 (CA)。 這項功能上面代表基礎結構。 

## <a name="overview"></a>概觀
根據預設，裝置入口網站會產生自我簽署的根 CA，並再使用的登入所接聽的每個端點的 SSL 憑證。 這包括`localhost`、 `127.0.0.1`，及`::1`(IPv6 localhost)。

也包括裝置的主機名稱 (例如`https://LivingRoomPC`) 和裝置指派每個連結本機 IP 位址 (最多兩個 [IPv4，IPv6] 每個網路介面卡)。 您可以查看 \\servername\share\spbr 裝置入口網站中的網路工具看到裝置的連結本機 IP 位址。 將開頭`10.`或`192.`的 IPv4，或`fe80:`對於 IPv6。 

預設安裝程式中的憑證警告可能會出現在瀏覽器因為不受信任的根 CA。 尤其是根瀏覽器或電腦不信任的 CA 簽署提供裝置入口網站的 [SSL 憑證標示。 這可以透過建立新受信任的根 CA 固定。

## <a name="create-a-root-ca"></a>建立根 CA

這只應如果您的公司 （或首頁） 都不會有設定憑證基礎結構，而且應僅一次完成。 下列 PowerShell 指令碼會建立根 CA 呼叫_WdpTestCA.cer_。 本機電腦的信任的根憑證授權單位來安裝此檔案將會造成您此根 CA 所簽署的 SSL 憑證建立信任關係的裝置。 您可以 （而且也應該） 安裝在每一個您想要連線至 Windows 裝置入口網站的電腦上這個.cer 檔案。  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

這會建立之後，您可以使用_WdpTestCA.cer_檔案來登入 SSL 憑證。 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>建立根 CA 的 SSL 憑證

SSL 憑證有兩個重要功能： 保護透過加密連線並確認您實際通訊的瀏覽器列中顯示的地址 (Bing.com、 192.168.1.37、 等) 並不惡意協力廠商。

下列 PowerShell 指令碼會建立 SSL 憑證的`localhost`端點。 裝置入口網站會接聽在每個端點需要自己的憑證。您可以運用取代`$IssuedTo`裝置與每個不同的端點指令碼中的引數： 主機名稱、 localhost 和 IP 位址。

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

如果您有多個裝置，您可以重複使用 localhost.pfx 檔案，但您仍需要另外建立的每個裝置的 IP 位址及主機名稱憑證。

當產生.pfx 檔案的組合時，您必須載入 Windows 裝置入口網站。 

## <a name="provision-device-portal-with-the-certifications"></a>使用 certification(s) 的佈建裝置入口網站

每個.pfx 檔案已建立的裝置，您需要從提升權限的命令提示字元執行下列命令。

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

請參閱下方例如流量：
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

一旦您已安裝的憑證，只需重新啟動服務讓變更生效：

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 位址可以隨著時間變更。
許多網路使用 DHCP 能夠出 IP 位址，因此裝置不一定要取得其先前有相同的 IP 位址。 如果您已在裝置上建立的 IP 位址的憑證和裝置的地址已變更，Windows 裝置入口網站會產生新的憑證使用現有的自我簽署的憑證，並它將會停止使用您所建立的一個。 這會使 cert 警告] 頁面上一次顯示在瀏覽器中。 原因，建議您連線至您的裝置透過其 hostname 書籤裝置入口網站中的設定。 這些會保持不變不論 IP 位址。
