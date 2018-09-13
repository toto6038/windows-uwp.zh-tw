---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 使用自訂 SSL 憑證佈建裝置入口網站
description: TBD
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2018
ms.locfileid: "3958926"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>使用自訂 SSL 憑證佈建裝置入口網站
在 Windows 10 Creators Update 中，Windows Device Portal 會新增裝置系統管理員來安裝自訂使用憑證的 HTTPS 通訊的方式。 

雖然您自己的電腦上，您可以執行此動作，這項功能主要適用於中都有現有的憑證基礎結構的企業。  

例如，公司可能會有憑證授權單位 (CA) 用來簽署內部網路網站提供透過 HTTPS 的憑證。 這項功能上面代表基礎結構。 

## <a name="overview"></a>概觀
根據預設，Device Portal 會產生自我簽署的根 CA，並接著會使用的登入的每個端點接聽的 SSL 憑證。 這包括`localhost`， `127.0.0.1`，以及`::1`(IPv6 localhost)。

也包含是裝置的主機名稱 (例如， `https://LivingRoomPC`) 和指派給裝置的每個連結本機 IP 位址 (最多 2 個 [IPv4，IPv6] 每個網路介面卡)。 您可以看到裝置連結本機 IP 位址來看看裝置入口網站中的網路功能工具。 他們將開始`10.`或`192.`的 IPv4，或`fe80:`ipv6。 

在預設安裝程式中，憑證警告可能會出現在您的瀏覽器中，因為未受信任的根 CA。 具體而言，裝置入口網站所提供的 SSL 憑證是由根瀏覽器或電腦不信任的 CA 簽署。 這可以透過建立新的受信任的根 CA 修正。

## <a name="create-a-root-ca"></a>建立根 CA

這應該只執行您的公司 （或家用版） 不會有一個憑證的基礎結構設定，並，才應該執行一次。 下列 PowerShell 指令碼會建立根 CA 稱為_WdpTestCA.cer_。 安裝此檔案的本機電腦的受信任的根憑證授權單位會導致您信任此根 CA 簽署的 SSL 憑證的裝置。 您可以 （且應該） 安裝此.cer 檔案，在每個您想要連線到 Windows 裝置入口網站的電腦上。  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

這建立之後，您可以使用_WdpTestCA.cer_檔案來簽署 SSL 憑證。 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>根 CA 建立 SSL 憑證

SSL 憑證有兩個重要功能： 保護您透過加密的連線，並確認您實際通訊與瀏覽器列中顯示的地址 (Bing.com，192.168.1.37，等) 並不是惡意的協力廠商。

下列 PowerShell 指令碼會建立的 SSL 憑證`localhost`端點。 裝置入口網站接聽每個端點需要它自己的憑證。您可以取代`$IssuedTo`適用於您裝置的引數中每個不同的端點使用指令碼： 主機名稱、 本機主機和 IP 位址。

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

如果您有多個裝置，您可以重複使用 localhost.pfx 檔案，但您仍然需要個別建立每個裝置的 IP 位址和主機名稱憑證。

產生套件組合的.pfx 檔案時，您將需要載入 Windows Device Portal。 

## <a name="provision-device-portal-with-the-certifications"></a>使用 certification(s) 的佈建裝置入口網站

針對每個.pfx 檔案，您已建立的裝置，您將需要從提升權限的命令提示字元執行下列命令。

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

請參閱下方，例如用法：
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

一旦您已安裝憑證，只需重新啟動服務讓變更生效：

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 位址會隨著時間改變。
許多網路使用 DHCP 來提供出 IP 位址，讓裝置永遠不會取得他們先前已有相同的 IP 位址。 如果您已建立的 IP 位址的憑證，在裝置上且該裝置的地址有所變更，Windows 裝置入口網站將會產生新的憑證使用現有的自我簽署的憑證，並將會停止使用您所建立。 這會導致憑證警告頁面，才會出現在您的瀏覽器一次。 基於這個原因，我們建議您連接到您的裝置透過其主機名稱，您可以設定裝置入口網站中。 這些將會保持不變，無論 IP 位址。
