---
title: 建立套件簽署的憑證
description: 使用 PowerShell 工具，建立和匯出應用程式套件簽署的憑證。
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: 963c73bb7667ced5bbe9e33fef0cac561fe1183a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591543"
---
# <a name="create-a-certificate-for-package-signing"></a>建立套件簽署的憑證


本文章將說明如何使用 PowerShell 工具，以建立和匯出應用程式套件簽署的憑證。 建議您使用適用於[封裝 UWP app](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps) 的 Visual Studio，但如果您未使用 Visual Studio 開發應用程式，仍然可以手動封裝可在市集上架的應用程式。

> [!IMPORTANT] 
> 如果您使用 Visual Studio 來開發 App，建議您使用 Visual Studio 精靈匯入憑證並簽署應用程式套件。 如需詳細資訊，請參閱[使用 Visual Studio 封裝 UWP app](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

## <a name="prerequisites"></a>必要條件

- **封裝或未封裝的應用程式**  
包含 AppxManifest.xml 檔案的應用程式。 在建立用來簽署最終應用程式套件的憑證時，您會需要參考資訊清單檔。 如需如何手動封裝應用程式的詳細資訊，請參閱[使用 MakeAppx.exe 工具建立應用程式套件](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。

- **公開金鑰基礎結構 (PKI) Cmdlet**  
您需要 PKI cmdlet 來建立和匯出您的簽署憑證。 如需詳細資訊，請參閱 [Public Key Infrastructure Cmdlets](https://docs.microsoft.com/powershell/module/pkiclient/)。

## <a name="create-a-self-signed-certificate"></a>建立自我簽署憑證

自我簽署的憑證很適合在您準備將應用程式發行至市集之前，測試您的應用程式。 請依照本節中所述的步驟來建立自我簽署憑證。

### <a name="determine-the-subject-of-your-packaged-app"></a>判斷您經過封裝之應用程式的主旨  

若要使用憑證來簽署您的應用程式套件，憑證中的「主旨」**必須**符合您應用程式資訊清單中的「發行者」區段。

舉例來說，應用程式之 AppxManifest.xml 檔案中的「身分識別」區段看起來像這樣︰
```
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

本案例中的「發行者」為 "CN=Contoso Software, O=Contoso Corporation, C=US"，而且您需要使用此項目來建立憑證。 

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>使用 **New-SelfSignedCertificate** 來建立憑證
使用 **New-SelfSignedCertificate** PowerShell cmdlet 來建立自我簽署憑證。 **New-SelfSignedCertificate** 有數個可供自訂的參數，但為符合本篇文章的主旨，我們會著重於建立使用 ***SignTool** 的簡單憑證。 如需詳細範例以及此 Cmdlet 的使用方法，請參閱 [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate)。

根據先前範例的 AppxManifest.xml 檔案，您應該使用下列語法來建立憑證。 在提升權限的 PowerShell 提示字元內︰
```
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName <Your Friendly Name> -CertStoreLocation "Cert:\LocalMachine\My"
```

執行此命令之後，憑證將會依照 "-CertStoreLocation" 參數新增至本機憑證存放區中。 命令的結果也會產生憑證的指紋。  

**注意**  
您可以使用下列命令在 PowerShell 視窗中檢視您的憑證︰
```
Set-Location Cert:\LocalMachine\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```
這會顯示出您本機存放區中的所有憑證。

## <a name="export-a-certificate"></a>匯出憑證 

若要將本機存放區的憑證匯出至個人資訊交換 (PFX) 檔案，請使用 **Export-PfxCertificate** Cmdlet。

在使用 **Export-PfxCertificate** 時，您必須建立並使用密碼，或使用 "-ProtectTo" 參數來指定能不需密碼即可存取檔案的使用者或群組。 請注意，如果您沒有使用 "-Password" 或 "-ProtectTo" 兩項參數中的其中一項，將會顯示錯誤訊息。

- **密碼使用方式**
```
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\LocalMachine\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

- **ProtectTo 使用量**
```
Export-PfxCertificate -cert Cert:\LocalMachine\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

在您建立和匯出憑證之後，就可以使用 **SignTool** 登入您的應用程式套件。 如需手動封裝過程的下一個步驟，請參閱[使用 SignTool 簽署應用程式套件](https://msdn.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool) (英文)。

## <a name="security-considerations"></a>安全性考量 
藉由將認證新增至[本機電腦憑證存放區](https://msdn.microsoft.com/windows/hardware/drivers/install/local-machine-and-current-user-certificate-stores) (英文)，您會對電腦上所有使用者的憑證信任造成影響。 建議您移除不再需要的憑證，以避免它們危害系統信任。
