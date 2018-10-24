---
author: c-don
title: 從 Azure 網頁伺服器安裝 UWP app
description: 本教學課程示範如何設定 Azure 網頁伺服器，確認您 Web 應用程式可以裝載應用程式套件，以及有效叫用和使用應用程式安裝程式。
ms.author: cdon
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages, Azure web server, 應用程式安裝程式, 側載, 相關集合, 選用套件, Azure 網頁伺服器
ms.localizationpriority: medium
ms.openlocfilehash: b98ca6316f733210dbdbc5201178b3a89a2b5982
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5441606"
---
# <a name="install-a-uwp-app-from-an-azure-web-app"></a>從 Azure Web 應用程式安裝 UWP app

應用程式安裝程式可讓開發人員和 IT 專業人員透過在他們自己的內容傳遞網路 (CDN) 上，散發 Windows 10 應用程式。 這對於不想要或不需要發佈其應用程式至 Microsoft Store ，但仍然想要利用 Windows 10 封裝與部署平台的企業而言，是很實用的。

本主題概述設定 Azure 網頁伺服器以裝載 UWP app 套件的步驟，以及如何使用應用程式安裝程式安裝應用程式套件。

本教學課程中，我們會透過在本機設定 IIS 伺服器，確認 Web 應用程式可以正確裝載應用程式套件，以及有效叫用和使用應用程式安裝程式 App。 我們也有教學課程教導在領域 (Azure 和 AWS) 中熱門雲端 Web 服務上適當裝載您的 Web 應用程式，以確保符合應用程式安裝程式 Web 安裝需求。 這個逐步教學課程不需要任何專業知識，並且很容易遵循。 

## <a name="setup"></a>設定

為成功遵循本教學課程，您需要：
 
1. Microsoft Azure 訂用帳戶 
2. UWP app 套件 - 您將散發的應用程式套件

選用：在 GitHub 上的[入門專案](https://github.com/AppInstaller/MySampleWebApp)。 如果您沒有要使用的應用程式套件或網頁，但仍然想要了解如何使用這項功能，這會很有幫助。

### <a name="step-1---get-an-azure-subscription"></a>步驟 1 - 取得 Azure 訂用帳戶
若要取得 Azure 訂用帳戶，請造訪 [Azure 帳戶頁面](https://azure.microsoft.com/free/)。 基於本教學課程的目的，您可以使用免費的會員資格。

### <a name="step-2---create-an-azure-web-app"></a>步驟 2 - 建立 Azure Web 應用程式 
在 Azure 入口網站頁面上，按一下 **\[+ 建立資源\]** 按鈕，然後選取 **\[Web 應用程式\]**

![建立](images/azure-create-app.png)

建立唯一的 **App 名稱**並讓剩餘的欄位使用預設值。 按一下 **\[建立\]** 以完成建立 Web 應用程式精靈。 

![建立 Web 應用程式](images/azure-create-app-2.png)

### <a name="step-3---hosting-the-app-package-and-the-web-page"></a>步驟 3 - 裝載應用程式套件和網頁 
一旦建立 Web 應用程式，您就可以從 Azure 入口網站上儀表板進行存取。 在此步驟中，我們將使用 Azure 入口網站的 GUI 建立簡單的網頁。

從儀表板選取新建立的 Web 應用程式之後，請使用搜尋欄位來尋找和開啟 **\[App Service 編輯器\]**。 

在編輯器中，有預設的 `hostingstart.html` 檔案。 以滑鼠右鍵按一下檔案總管面板的空白區域，然後選取 **\[上傳檔案\]** 以開始上傳您的應用程式套件。

> [!NOTE]
> 如果您沒有可使用的應用程式套件，您可以使用 GitHub 上屬於所提供[入門專案](https://github.com/AppInstaller/MySampleWebApp)存放庫一部分的應用程式套件。 簽署套件的憑證 (MySampleApp.cer) 也是在 GitHub 上的範例。 您必須在安裝應用程式之前安裝憑證到您的裝置。

![上傳](images/azure-upload-file.png)

以滑鼠右鍵按一下檔案總管面板的空白區域，然後選取 **\[新檔案\]** 以建立新的檔案。 為檔案命名：`default.html`。

如果您使用[入門專案](https://github.com/AppInstaller/MySampleWebApp)中提供的應用程式套件，複製下列 HTML 程式碼到新建立的網頁 `default.html`。 如果您使用您自己的應用程式套件，請修改 App 服務 URL (`source=` 之後的 URL)。 您可以在 Azure 入口網站中從您的應用程式的概觀頁面取得的 App 服務 URL。

```html
<html>
<head>
    <meta charset="utf-8" />
    <title> Install My Sample App</title>
</head>
<body>
    <a href="ms-appinstaller:?source=https://appinstaller-azure-demo.azurewebsites.net/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

### <a name="step-4---configure-the-web-app-for-app-package-mime-types"></a>步驟 4 - 設定應用程式套件 MIME 類型的 Web 應用程式

新增檔案到 Web 應用程式，其命名為：`Web.config`。 從 [檔案總管]開啟 `Web.config` 檔案，然後新增下列行。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
  </system.webServer>
</configuration>
```

### <a name="step-5---run-and-test"></a>步驟 5 - 執行和測試

若要啟動您建立的建立網頁，請使用步驟 3 的 URL 到瀏覽器，後面跟著 `/default.html`。 

![Edge](images/edge.png)

按一下 \[安裝我範例應用程式\] 來啟動應用程式安裝程式，並安裝您的應用程式套件。 

## <a name="troubleshooting-issues"></a>疑難排解問題

### <a name="app-installer-app-fails-to-install"></a>應用程式安裝程式 App 無法安裝 
如果裝置上未安裝簽署應用程式套件的憑證，應用程式安裝將會失敗。 若要修正這個問題，您必須在安裝應用程式之前安裝憑證。 如果您裝載公開散發的應用程式套件，我們建議您使用憑證授權單位的憑證簽署您的應用程式套件。 

![應用程式憑證](images/aws-app-cert.png)

### <a name="nothing-happens-when-you-click-the-link"></a>當您按下連結時沒有任何作用 
確定已安裝應用程式安裝程式 App。 移至 **\[設定\]** -> **\[應用程式與功能\]**，並在已安裝的 app 清單中尋找應用程式安裝程式。 

