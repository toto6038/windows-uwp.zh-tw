---
author: laurenhughes
title: 從網頁安裝 UWP app
description: 在本節中，我們將檢閱可讓使用者直接從網頁安裝應用程式所需採取的步驟。
ms.author: lahugh
ms.date: 11/16/2017
ms.topic: article
keywords: windows 10, uwp, 應用程式安裝程式, AppInstaller, 側載, 相關集合, 選用套件
ms.localizationpriority: medium
ms.openlocfilehash: 98a761bf04b56d13745f2505b8d0806fc4fdf3e1
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5921152"
---
# <a name="installing-uwp-apps-from-a-web-page"></a>從網頁安裝 UWP app

應用程式通常必須可在裝置本機上使用，然後才能應用程式安裝程式加以安裝。 就 Web 案例而言，這表示使用者必須從網頁伺服器下載應用程式套件，在此之後才可以使用應用程式安裝程式進行安裝。 這種作法沒有效率而且浪費磁碟空間，也就是為什麼應用程式安裝程式現在會有內建功能來簡化此程序的原因。

應用程式安裝程式可以直接從網頁伺服器安裝應用程式。 使用者按一下應用程式套件裝載網站連結時，會自動叫用應用程式安裝程式。 接著就會將使用者帶到應用程式安裝程式的應用程式資訊檢視，然後只要再按一下即可直接與應用程式銜接。 

直接應用程式安裝只適用於 Windows 10 Fall Creators Update 和更新版本。 舊版 Windows (回溯到 Windows 10 年度更新版) 將會由[舊版 Windows 10 的 Web 安裝體驗](#web-install-experience)來支援。 這個體驗無法像直接應用程式安裝一樣流暢，但現有的應用程式安裝程序提供了重大改善。
  
> [!NOTE]
> 必須有版本號碼大於 1.0.12271.0 的應用程式安裝程式才能支援此功能。

## <a name="protocol-activation-scheme"></a>通訊協定啟用配置
在此機制下，應用程式安裝程式會向作業系統器註冊以進行通訊協定啟動配置。 使用者按一下 Web 連結時，瀏覽器會與作業系統確認已註冊至該 Web 連結的應用程式。 如果配置符合應用程式安裝程式指定的通訊協定啟動配置，就會叫用應用程式安裝程式。 請務必注意，此機制與瀏覽器無關。 這對網站系統管理員很有幫助，例如，將此機制加入網頁時，不需要考慮網頁瀏覽器差異。 

### <a name="requirements-for-protocol-activation-scheme"></a>通訊協定啟動配置需求

1. 需要有支援位元組範圍要求 (HTTP/1.1) 的網頁伺服器
    - 支援 HTTP/1.1 通訊協定的伺服器應該都支援位元組範圍要求 
2. 網頁伺服器將會需要了解 Windows 10 應用程式套件的內容類型
    - 以下說明如何將新的內容類型宣告為[網頁設定檔](web-install-IIS.md#step-7---configure-the-web-app-for-app-package-mime-types)的一部分

### <a name="how-to-enable-this-on-a-webpage"></a>如何在網頁上啟用此支援 
要在網站上裝載應用程式套件的應用程式開發人員需要依照這個步驟進行：

在應用程式套件 URI 加上應用程式安裝程式在網頁參考這些套件時註冊到的啟動配置 `'ms-appinstaller:?source='` 做為首碼。 如需詳細資訊，請參閱 **MyApp 網頁**範例。 
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.appx"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.appxbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

## <a name="signing-the-app-package"></a>簽署應用程式套件
若要讓使用者安裝您的應用程式，您將需要使用受信任的憑證簽署應用程式套件。 您可以使用受信任的憑證授權單位發出的第三方付費憑證來簽署應用程式套件。 如果您使用第三方憑證，使用者的裝置必須處於側載或開發人員模式，才能安裝及執行您的應用程式。

如果您要部署 app 給企業中的員工，您可以使用企業發出的憑證來簽署應用程式。 請務必注意，企業憑證必須部署至應用程式將安裝的任何裝置。 如需部署企業應用程式的詳細資訊，請查看[企業應用程式管理](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management)。

## 舊版 Windows 10 上的 Web 安裝體驗<a name="web-install-experience"></a>

所有提供應用程式安裝程式的 Windows 10 版本 (從年度更新版開始) 都會支援從瀏覽器叫用應用程式安裝程式。 不過，不需要下載套件即可從網頁直接安裝的功能只適用於 Windows 10 Fall Creators Update。  

舊版 Windows 10 (提供應用程式安裝程式) 的使用者也可以透過應用程式安裝程式利用 UWP app 的 Web 安裝，但使用者體驗有所不同。 當這些使用者按一下 Web 連結時，應用程式安裝程式會提示要**下載**套件，而不是**安裝**。 下載之後，應用程式安裝程式會自動開始啟動下載的套件。 由於應用程式套件是從 Web 下載，這些檔案會通過 Microsoft SmartScreen 的安全性檢查。 一旦使用者提供權限允許繼續後，只要再按一下 **\[安裝\]**，應用程式就可供使用！

雖然這個流程不像Windows 10 Fall Creators Update 的直接安裝那樣順暢，使用者仍然可以快速與應用程式銜接。 此外，有了這個流程後，使用者就不必擔心應用程式套件檔案佔用磁碟機的空間。 應用程式安裝程式管理空間很有效率，方式為下載套件至其應用程式資料的資料夾，當不再需要這些套件時即加以清除。 

以下是 Windows 10 Fall Creator Update 版本應用程式安裝程式與舊版應用程式安裝程式之間的快速比較：

| 應用程式安裝程式的最新版本。 | 應用程式安裝程式的先前版本 |
|------------------------------|----------------------------------|
| 應用程式安裝程式在下載開始之前顯示應用程式資訊 | 瀏覽器提示使用者選擇下載  |
| 應用程式安裝程式執行下載 | 使用者必須手動起始應用程式套件的啟動 |
| 套件下載後，應用程式安裝程式自動啟動應用程式套件 | 使用者必須按一下 **\[安裝\]**，並手動啟動應用程式套件 |
| 應用程式安裝程式會負責處置下載的套件 | 使用者必須手動刪除下載的檔案 |

## <a name="web-install-experience-on-previous-versions-of-windows-10"></a>舊版 Windows 10 上的 Web 安裝體驗
在 Windows 10 Fall Creators Update 以前的版本中，應用程式安裝程式無法直接從網頁安裝應用程式。 在這些版本中，應用程式安裝程式只能安裝可在本機使用的應用程式套件。 應用程式安裝程式反而會下載套件並要求使用者按兩下已下載的套件進行安裝。


## <a name="microsoft-smartscreen-integration"></a>Microsoft SmartScreen 整合

Microsoft SmartScreen 始終都是透過應用程式安裝程式安裝應用程式之安裝程序的一部分。 SmartScreen 可確保使用者受到安全防護，避免可侵入使用者裝置的惡意內容威脅。 與應用程式安裝程式的最新更新搭配後，SmartScreen 整合變得更加順暢且強固，並在您安裝不明應用程式時提供警告，保護裝置不受危害。 
