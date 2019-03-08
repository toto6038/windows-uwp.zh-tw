---
title: 從 IIS 伺服器上安裝 UWP 應用程式
description: 本教學課程會示範如何設定 IIS 伺服器，請確認您的 web 應用程式可以裝載應用程式套件，並叫用，有效地使用應用程式安裝程式。
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10、 uwp、 應用程式安裝程式，AppInstaller，側載，相關的設定 （選擇性） 封裝，IIS 伺服器
ms.localizationpriority: medium
ms.openlocfilehash: 6a4512229a29a7adc59d6b61edd596eaeb56a5a8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623563"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>從 IIS 伺服器上安裝 UWP 應用程式

本教學課程會示範如何設定 IIS 伺服器，請確認您的 web 應用程式可以裝載應用程式套件，並叫用，有效地使用應用程式安裝程式。

應用程式安裝程式可讓開發人員和 IT 專業人員透過在他們自己的內容傳遞網路 (CDN) 上，散發 Windows 10 應用程式。 這對於不想要或不需要發佈其應用程式至 Microsoft Store ，但仍然想要利用 Windows 10 封裝與部署平台的企業而言，是很實用的。 

## <a name="setup"></a>設定

若要成功瀏覽本教學課程中，您需要下列項目：

1. Visual Studio 2017  
2. Web 開發工具和 IIS 
3. UWP app 套件 - 您將散發的應用程式套件

選擇性：[入門專案](https://github.com/AppInstaller/MySampleWebApp)GitHub 上。 如果您沒有要使用的應用程式套件，但仍想要了解如何使用這項功能，這是很有幫助。

## <a name="step-1---install-iis-and-aspnet"></a>步驟 1-安裝 IIS 和 ASP.NET 

[Internet Information Services](https://www.iis.net/)是一種 Windows 功能，您可以透過 [開始] 功能表安裝。 在  **開始 功能表**搜尋**開啟 Windows 功能開啟或關閉**。

尋找並選取**Internet Information Services**來安裝 IIS。

> [!NOTE]
> 您不需要選取 網際網路資訊服務下的所有核取方塊。 當您核取時選取的項目**Internet Information Services**就已足夠。

您也必須安裝 ASP.NET 4.5 或更新版本。 若要安裝，請找出**Internet Information Services]-> [全球資訊網服務]-> [應用程式開發功能**。 選取 大於或等於 ASP.NET 4.5 的 ASP.NET 版本。

![安裝 ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>步驟 2-安裝 Visual Studio 2017 和 Web 開發工具 

[安裝 Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio)如果您有尚未加以安裝。 如果您已經有 Visual Studio 2017，請確定已安裝下列的工作負載。 如果工作負載不存在於您的安裝上，遵循使用 Visual Studio 安裝程式 （從 [開始] 功能表找到）。  

在安裝期間，選取**ASP.NET 和 Web 開發**及其他您感興趣的工作負載。 

安裝完成之後，啟動 Visual Studio，並建立新的專案 (**檔案** -> **新專案**)。

## <a name="step-3---build-a-web-app"></a>步驟 3-建立 Web 應用程式

啟動做為 Visual Studio 2017**系統管理員**並建立新**視覺化C#Web 應用程式**專案**空白**專案範本。 

![新專案](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>步驟 4-使用我們的 Web 應用程式設定 IIS 

從 [方案總管] 中，請在根專案上按一下滑鼠右鍵，然後選取**屬性**。

在 web 應用程式內容中，選取**Web** ] 索引標籤。在 [**伺服器**區段中，選擇**本機 IIS**從下拉功能表，然後按一下**建立虛擬目錄**。 

![web 索引標籤](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>步驟 5-將應用程式套件新增至 web 應用程式 

新增要發佈到 web 應用程式的應用程式套件。 您可以使用屬於所提供的應用程式封裝[入門專案套件](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages)GitHub，如果您沒有可用的應用程式套件上。 簽署套件的憑證 (MySampleApp.cer) 也是在 GitHub 上的範例。 您必須安裝到您的裝置，才能安裝應用程式 (步驟 9) 的憑證。

入門專案的 web 應用程式，在新的資料夾加入到 web 應用程式呼叫`packages`，其中包含應用程式套件散發。 若要在 Visual Studio 中建立資料夾，以滑鼠右鍵按一下方案總管中，選取根目錄**新增** -> **新資料夾**並將它命名`packages`。 若要將應用程式套件新增至資料夾，以滑鼠右鍵按一下`packages`資料夾，然後選取**新增** -> **現有項目...** 並瀏覽至應用程式封裝的位置。 

![新增套件](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>步驟 6-建立 Web 網頁

此範例 web 應用程式會使用簡單的 HTML。 您可自行建置您的 web 應用程式，視需要根據您的需要。 

在 [方案總管] 中選取的根專案上按一下滑鼠右鍵**新增** -> **新項目**，再加入新**HTML 網頁**從**Web**一節。

一旦建立 HTML 網頁，以滑鼠右鍵按一下方案總管 中的 HTML 網頁，然後選取**設定為起始頁**。  

按兩下 HTML 檔案，在程式碼編輯器視窗中開啟它。 在本教學課程中，將使用中的元素需要叫用應用程式安裝程式應用程式已成功安裝的 Windows 10 應用程式的網頁中。 

在網頁中包含下列 HTML 程式碼。 若要順利叫用應用程式安裝程式的金鑰是使用應用程式安裝程式會向作業系統的自訂配置： `ms-appinstaller:?source=`。 請參閱下列程式碼範例，如需詳細資訊。

> [!NOTE]
> 請確認指定的自訂配置符合您在 VS 方案中的 [web] 索引標籤中的專案 Url 之後的 URL 路徑。
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>步驟 7： 設定 web 應用程式的應用程式封裝 MIME 類型

開啟**Web.config**檔案從 [方案總管]，然後新增下列幾行內`<configuration>`項目。 

```xml
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
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>步驟 8-新增應用程式安裝程式的回送豁免

因為網路隔離，而 UWP 應用程式，例如應用程式安裝程式會受限於使用 IP 回送位址，例如 http://localhost/。 使用本機 IIS 伺服器時，應用程式安裝程式必須新增到回送豁免清單。 

若要這樣做，請開啟**命令提示字元**作為**系統管理員**並輸入下列命令: '' 命令列 CheckNetIsolation.exe LoopbackExempt-a-n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

您應該會發現`microsoft.desktopappinstaller_8wekyb3d8bbwe`清單中。

透過應用程式安裝程式的應用程式安裝的本機驗證完成後，您可以移除您在此步驟中新增回送豁免：

```Command Line CheckNetIsolation.exe LoopbackExempt -d -n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![run](images/run.png)

A web page will open in your browser:

![web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
