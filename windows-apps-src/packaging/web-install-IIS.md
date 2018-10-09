---
author: laurenhughes
title: 從 IIS 伺服器上安裝 UWP 應用程式
description: 本教學課程示範如何設定 IIS 伺服器，確認您的 web 應用程式可以裝載應用程式套件和叫用並有效地使用應用程式安裝程式。
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，應用程式安裝程式，AppInstaller，側載，相關設定，選用套件，IIS 伺服器
ms.localizationpriority: medium
ms.openlocfilehash: 214ddd2b55bca1acecbab0a841cf2048335e7b3a
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2018
ms.locfileid: "4423855"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>從 IIS 伺服器上安裝 UWP 應用程式

本教學課程示範如何設定 IIS 伺服器，確認您的 web 應用程式可以裝載應用程式套件和叫用並有效地使用應用程式安裝程式。

應用程式安裝程式可讓開發人員和 IT 專業人員透過在他們自己的內容傳遞網路 (CDN) 上，散發 Windows 10 應用程式。 這對於不想要或不需要發佈其應用程式至 Microsoft Store ，但仍然想要利用 Windows 10 封裝與部署平台的企業而言，是很實用的。 

## <a name="setup"></a>設定

成功瀏覽與本教學課程，您將需要下列項目：

1. Visual Studio 2017  
2. Web 開發工具和 IIS 
3. UWP app 套件 - 您將散發的應用程式套件

選用：在 GitHub 上的[入門專案](https://github.com/AppInstaller/MySampleWebApp)。 如果您不需要應用程式套件可搭配使用，但仍然想要了解如何使用這項功能，這會很有幫助。

## <a name="step-1---install-iis-and-aspnet"></a>步驟 1-安裝 IIS 和 ASP.NET 

[Internet Information Services](https://www.iis.net/)是可以透過 [開始] 功能表中安裝的 Windows 功能。 在 **[開始] 功能表****開啟或關閉 Windows 功能**搜尋。

尋找並選取要安裝 IIS **Internet Information Services** 。

> [!NOTE]
> 您不需要選取 Internet Information Services 底下的所有核取方塊。 只是選取核取**Internet Information Services**時就已足夠。

您也必須安裝 ASP.NET 4.5 或更高。 若要安裝它，找出**Internet Information Services 全球資訊網]-> [服務]-> [應用程式開發功能**。 選取 ASP.NET 大於或等於 ASP.NET 4.5 的的版本。

![安裝 ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>步驟 2-安裝 Visual Studio 2017 和 Web 開發工具 

[安裝 Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio)如果您有尚未安裝它。 如果您已經有 Visual Studio 2017，請確定已安裝下列工作負載。 如果工作負載未出現於您的安裝，請遵循使用 Visual Studio 安裝程式 （從 [開始] 功能表中找到）。  

在安裝期間，選取 [ **ASP.NET 和 Web 開發**與您感興趣的任何其他工作負載。 

安裝完成之後，啟動 Visual Studio，並建立新的專案 (**檔案** -> **新的專案**)。

## <a name="step-3---build-a-web-app"></a>步驟 3-建置 Web 應用程式

啟動 Visual Studio 2017，**系統管理員**身分，並使用**空白**的專案範本建立新的**Visual C# Web 應用程式**專案。 

![新專案](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>步驟 4-使用我們的 Web 應用程式設定 IIS 

從 [方案總管] 中，在根專案上按一下滑鼠右鍵並選取 [**屬性**]。

在 web 應用程式內容中，選取 [ **Web** ] 索引標籤。在**伺服器**區段中，從下拉式功能表中選擇**本機 IIS** ，並按一下 [**建立虛擬目錄**。 

![web] 索引標籤](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>步驟 5-新增應用程式套件的 web 應用程式 

新增您要發佈到 web 應用程式的應用程式套件。 您可以使用的是 GitHub 上提供的[起始專案套件](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages)的一部分，如果您沒有可用的應用程式套件的應用程式套件。 簽署套件的憑證 (MySampleApp.cer) 也是在 GitHub 上的範例。 您必須有憑證安裝到您在之前安裝應用程式 (步驟 9) 的裝置。

在 「 入門專案 web 應用程式的新資料夾已新增到 web 應用程式呼叫`packages`，其中包含散佈應用程式套件。 若要在 Visual Studio 中建立資料夾，以滑鼠右鍵按一下 [方案總管] 中的根目錄中，選取 [**新增** -> **新的資料夾**，並命名為`packages`。 若要新增應用程式套件的資料夾，以滑鼠右鍵按一下`packages`資料夾，然後選取 [**新增]** -> **現有項目]** 並瀏覽至應用程式套件的位置。 

![新增套件](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>步驟 6-建立網頁

此範例 web 應用程式會使用簡單的 HTML。 您可以自由地建置您的 web 應用程式，視需要每個您的需求。 

在 [方案總管] 中的根專案上按一下滑鼠右鍵，選取 [**加入** -> **新項目**，並從 [ **Web** ] 區段中新增新的**HTML 頁面**。

一旦建立的 HTML 頁面時，以滑鼠右鍵按一下 [方案總管] 中的 HTML 頁面上，並選取**設定為起始頁**。  

按兩下 HTML 檔案，在程式碼編輯器視窗中開啟它。 在本教學課程中，將會使用只在必要的網頁來叫用應用程式安裝程式應用程式順利安裝的 Windows 10 應用程式中的元素。 

在您的網頁中包含下列 HTML 程式碼。 若要成功叫用應用程式安裝程式的索引鍵是使用應用程式安裝程式會向作業系統器註冊的自訂配置： `ms-appinstaller:?source=`。 下列程式碼範例，如需詳細資訊，請參閱。

> [!NOTE]
> 請確定指定自訂配置符合專案中的 Url VS 解決方案的 [web] 索引標籤之後的 URL 路徑。
 
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

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>步驟 7-設定應用程式套件 MIME 類型的 web 應用程式

從方案總管] 中開啟**Web.config**檔案，然後新增下列行內`<configuration>`項目。 

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

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>步驟 8-新增回送豁免的應用程式安裝程式

網路隔離，因為 UWP 應用程式，例如應用程式安裝程式會受到限制使用 IP 回送位址，例如http://localhost/。 使用本機 IIS 伺服器時，應用程式安裝程式必須將新增到回送豁免清單。 

若要這樣做，請開啟**命令提示字元中**，系統**管理員**的身分，並輸入下列: '' 命令列 CheckNetIsolation.exe LoopbackExempt--n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

您應該尋找`microsoft.desktopappinstaller_8wekyb3d8bbwe`清單中。

透過應用程式安裝程式的應用程式安裝本機驗證完成後，您可以移除您在此步驟中所加入回送豁免：

'' 命令列 CheckNetIsolation.exe LoopbackExempt-d-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
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
