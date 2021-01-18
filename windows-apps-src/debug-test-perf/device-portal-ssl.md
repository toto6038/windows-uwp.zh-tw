---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 使用自訂 SSL 憑證佈建 Windows 裝置入口網站
description: 了解如何使用自訂憑證佈建 Windows 裝置入口網站以供 HTTPS 通訊使用。
ms.date: 01/08/2021
ms.topic: article
keywords: windows 10, uwp, 裝置入口網站
ms.localizationpriority: medium
ms.custom:
- 19H1
- contperf-fy21q3
ms.openlocfilehash: d1d26edbd2d36cda0d5eb33a700534b55e0c36d0
ms.sourcegitcommit: 49df4993ac6d2e66bfa629555e1831ed99bac93a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/10/2021
ms.locfileid: "98058509"
---
# <a name="provision-windows-device-portal-with-a-custom-ssl-certificate"></a>使用自訂 SSL 憑證佈建 Windows 裝置入口網站

在 Windows 10 Creators Update 中，Windows 裝置入口網站 (WDP) 新增了一種方法，可讓裝置管理員安裝自訂憑證，以供 HTTPS 通訊使用。

雖然您可以在自己的電腦上執行此動作，但這項功能主要適用於現有憑證基礎結構已經準備就緒的企業。  

例如，公司可能設有憑證授權單位 (CA)，用於簽署透過 HTTPS 提供服務的內部網站憑證。 這項功能是建立在該基礎結構之上。

## <a name="overview"></a>概觀

根據預設，WDP 會產生自我簽署的根 CA，然後使用此 CA 簽署其接聽的每個端點的 SSL 憑證。 包括了 `localhost`、`127.0.0.1` 和 `::1` (IPv6 localhost)。

此外，還包括裝置的主機名稱 (例如，`https://LivingRoomPC`)，以及每個指派給裝置的連結-本機 IP 位址 (每個網路介面卡最多兩個 [IPv4, IPv6])。
您可以在 WDP 中的 [網路功能] 工具中尋找，以查看裝置的連結-本機 IP 位址。 這些位址會以 `10.` 或 `192.` (適用於 IPv4) 或 `fe80:` (適用於 IPv6) 開頭。

在預設設定中，瀏覽器可能會因為根 CA 未受信任而出現憑證警告。 具體而言，WDP 提供的 SSL 憑證是由瀏覽器或電腦不信任的根 CA 所簽署。 您可以建立新的受信任根 CA 來修正此問題。

## <a name="create-a-root-ca"></a>建立根 CA

只有在您的公司 (或家庭) 沒有設定憑證基礎結構時，才要這樣做，而且只能做一次。 下列 PowerShell 指令碼會建立稱為 _WdpTestCA.cer_ 的根 CA。 將此檔案安裝到本機電腦的 [信任的根憑證授權單位]，您的裝置即會信任此根 CA 所簽署的 SSL 憑證。 您可以 (而且也應該) 將這個 .cer 檔案安裝在每一台要連線到 WDP 的電腦。  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

建立完畢後，就可以使用 _WdpTestCA.cer_ 檔案來簽署 SSL 憑證。

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>使用根 CA 建立 SSL 憑證

SSL 憑證有兩個重要功能：透過加密保護您的連線安全，以及確認您進行通訊的對象確實是瀏覽器列顯示的位址 (Bing.com、192.168.1.37 等)，而非惡意的第三方。

下列 PowerShell 指令碼會建立 `localhost` 端點的 SSL 憑證。 WDP 接聽的每個端點都必須有自己的憑證。您可以將指令碼中的 `$IssuedTo` 引數取代為您裝置的各個不同端點：主機名稱、localhost 和 IP 位址。

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

如果您有多個裝置，則可以重複使用 localhost.pfx 檔案，但您仍然需要分別為每部裝置建立 IP 位址和主機名稱憑證。

產生 .pfx 檔案的套件後，則需要將這些檔案載入 WDP。

## <a name="provision-windows-device-portal-with-the-certifications"></a>使用憑證佈建 Windows 裝置入口網站

您必須從提升權限的命令提示字元，對您為裝置建立的每個 .pfx 檔案執行下列命令。

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

關於範例用法，請參閱以下內容：

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

安裝憑證之後，只需重新啟動服務讓變更生效即可：

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 位址可能會在一段時間後變更。
許多網路會使用 DHCP 提供 IP 位址，因此裝置不一定會取得相同的 IP 位址。 如果您為裝置的 IP 位址建立了憑證，但裝置的位址已變更時，WDP 將會使用現有的自我簽署憑證產生新的憑證，並停止使用您所建立的憑證。 這會造成您的瀏覽器再次顯示憑證警告頁面。 因此，建議您透過裝置的主機名稱 (您可在 Windows 裝置入口網站中設定此名稱) 連線到裝置。 無論 IP 位址如何變化，主機名稱都會保持不變。
