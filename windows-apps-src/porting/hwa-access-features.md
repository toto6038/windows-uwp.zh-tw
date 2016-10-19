---
author: seksenov
title: "託管的 Web 應用程式 - 存取通用 Windows 平台 (UWP) 功能與執行階段 API"
description: "存取通用 Windows 平台 (UWP) 原生功能與 Windows 10 執行階段 API，包括 Cortona 語音命令、動態磚、安全性專屬 ACUR、OpenID 和 OAuth，皆是來自遠端 JavaScript。"
kw: Hosted Web Apps, Accessing Windows 10 features from remote JavaScript, Building a Win10 Web Application, Windows JavaScript Apps, Microsoft Web Apps, HTML5 app for PC, ACUR URI Rules for Windows App, Call Live Tiles with web app, Use Cortana with web app, Access Cortana from website, msapplication-cortanavcd
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: fb74bfc40750941860dae0a8f811fde4a614e403

---

# 存取通用 Windows 平台 (UWP) 功能

您的 Web 應用程式可完整存取通用 Windows 平台 (UWP)、在 Windows 裝置上啟用原生功能、[享受 Windows 安全性](#keep-your-app-secure-setting-application-content-uri-rules-acurs)的各種優勢、直接從伺服器上託管的指令碼[呼叫 Windows 執行階段 API](#call-windows-runtime-apis)、運用 [Cortana 整合](#integrate-cortana-voice-commands)，以及使用[線上驗證提供者](#web-authentication-broker)。 此外亦支援[混合式應用程式](#create-hybrid-apps-packaged-web-apps-vs-hosted-web-apps)，因為您可將要從託管指令碼呼叫的本機程式碼包含在內，以及管理 App 在遠端和本機頁面之間的瀏覽。

## 保護 App 安全 – 設定應用程式內容 URI 規則 (ACUR)

您可透過 ACUR (另稱 URL 允許清單)，授與遠端 URL 權利直接從遠端 HTML、CSS 和 JavaScript 存取通用 Windows API。 系統已設定 Windows 作業系統層級的權利原則界限，允許您網頁伺服器上託管的程式碼直接呼叫平台 API。 當您在應用程式內容 URI 規則 (ACUR) 中放置一組構成託管 Web 應用程式的 URL，即可在應用程式套件資訊清單中定義這些界限。 您的規則應包含 App 的開始頁面，以及您想要納為 App 頁面的任何其他頁面。 您也可以選擇排除特定的 URL。

有幾種方法可以在您的規則中指定 URL 比對：

- 確切主機名稱
- 含子網域的 URI 可以包含或排除的主機名稱。
- 確切 URI
- 可包含查詢屬性的確切 URI
- 包含規則的部分路徑和用來指示特定副檔名的萬用字元。
- 排除規則的相對路徑

如果您的使用者瀏覽至您的規則中未包含的 URL，Windows 就會在瀏覽器中開啟目標 URL。

以下是 ACUR 的幾個範例。

```HTML
<Application
Id="App"
StartPage="http://contoso.com/home">
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="https://contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="include" Match="https://*.contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="exclude" Match="https://contoso.com/excludethispage.aspx" />
</uap:ApplicationContentUriRules>
```

## 呼叫 Windows 執行階段 API

若將 URL 定義於 App 的界限 (ACUR) 範圍內，則其可使用「WindowsRuntimeAccess」屬性呼叫具有 JavaScript 的 Windows 執行階段 API。 系統在主控處理程序中載入具適當存取權的 URL 時，Windows 命名空間將會插入並顯示於指令碼引擎。 這讓通用 Windows API 可供 App 的指令碼直接呼叫。 身為開發人員，您僅需針對要呼叫的 Windows API 進行功能偵測，若可供使用就會進一步啟動平台功能。

若要啟用此功能，您必須使用下列其中一個值指定 ACUR 中的 `(WindowsRuntimeAccess="<<level>>")` 屬性：

- **all**：遠端 JavaScript 程式碼可存取所有的 UWP API 以及任何本機封裝元件。
- **allowForWeb**︰遠端 JavaScript 程式碼僅可存取自訂封裝程式碼。 本機存取自訂 C++/C# 元件。
- **none**︰預設值。 指定的 URL 無平台存取權。

以下是規則類型範例：

```HTML
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="http://contoso.com/" WindowsRuntimeAccess="all"  />
</uap:ApplicationContentUriRules>
```

這可以讓正在 http://contoso.com/ 執行的指令碼，存取 Windows 執行階段命名空間與套件中的自訂封裝元件。 對於快顯通知，請參閱 GitHub 上的 [Windows.UI.Notifications.js](https://gist.github.com/Gr8Gatsby/3d471150e5b317eb1813#file-windows-ui-notifications-js) 範例。

以下是關於如何實作動態磚並透過遠端 JavaScript 進行更新的範例︰

```Javascript
function updateTile(message, imgUrl, imgAlt) {
    // Namespace: Windows.UI.Notifications

    if (typeof Windows !== 'undefined'&&
            typeof Windows.UI !== 'undefined' &&
            typeof Windows.UI.Notifications !== 'undefined') {  
        var notifications = Windows.UI.Notifications,
        tile = notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
        tileContent = notifications.TileUpdateManager.getTemplateContent(tile),
        tileText = tileContent.getElementsByTagName('text'),
        tileImage = tileContent.getElementsByTagName('image');  
        tileText[0].appendChild(tileContent.createTextNode(message || 'Demo Message'));
        tileImage[0].setAttribute('src', imgUrl || 'https://unsplash.it/150/150/?random');
        tileImage[0].setAttribute('alt', imgAlt || 'Random demo image');    
        var tileNotification = new notifications.TileNotification(tileContent);
        var currentTime = new Date();
        tileNotification.expirationTime = new Date(currentTime.getTime() + 600 * 1000);
        notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileNotification);
    } else {
        //Alternative behavior

    }
}
```

此程式碼會產生外觀如下的磚：

![Windows 10 呼叫動態磚](images/hwa-to-uwp/hwa_livetile.png)

透過任何您最熟悉的環境或技術呼叫 Windows 執行階段 API，而您可讓伺服器功能資源在呼叫 Windows 功能之前先加以偵測。 若平台功能無法使用，且是在另一部主機上執行 Web 應用程式，則您可為使用者提供支援瀏覽器運作的標準預設使用體驗。

## 整合 Cortana 語音命令

您可以透過在您的 HTML 頁面中指定語音命令定義 (VCD) 檔案來整合 Cortana。 VCD 檔案是一個可將命令對應至特定片語的 XML 檔案。 例如，使用者可以點選 [開始] 按鈕並說出「Contoso Books，顯示暢銷商品」來啟動「Contoso Books」App 並瀏覽至「暢銷商品」頁面。

您在新增列出 VCD 檔案位置的 `<meta>` 元素標籤時，Windows 會自動下載及註冊語音命令定義檔。

以下範例是在託管的 Web 應用程式的 HTML 頁面中使用該標籤：

```HTML
<meta name="msapplication-cortanavcd" content="http:// contoso.com/vcd.xml"/>
```

如需 Cortana 整合與 VCD 的詳細資訊，請參閱 Cortana 互動與語音命令定義 (VCD) 元素和屬性 v1.2。

## 建立混合式應用程式 – 封裝的 Web 應用程式與託管的 Web 應用程式

您有一些建立 UWP App 的選項。 您可以將 App 設計為從 Windows 市集下載，然後完全託管在本機用戶端，這通常稱為「封裝的 Web 應用程式」****。 這樣可讓您在任何相容的平台上離線執行您的 App。 或者，App 可以是一種在遠端 Web 伺服器上執行，完全託管的 Web 應用程式，這通常稱為「託管的 Web 應用程式」****。 但是也有第三種選項：App 可以部分裝載在本機用戶端上，部分裝載在遠端 Web 伺服器上。 我們稱這第三種選項為「混合式應用程式」****，其通常會使用 **WebView** 元件讓遠端內容看起來像本機內容。 混合式應用程式可以在本機應用程式用戶端內包含以套件方式執行的 HTML5、CSS 和 Javascript 程式碼，並且保留與遠端內容互動的能力。

## Web 驗證代理人

如果您有採用網際網路驗證與授權通訊協定 (例如 OpenID 或 OAuth) 的線上識別提供者，您就可以使用 Web 驗證代理人來處理使用者的登入流程。 您可以在您應用程式之 HTML 頁面上的 `<meta>` 標籤中，指定開始與結束 URL。 Windows 會偵測這些 URL 並將其傳遞給 Web 驗證代理人，以完成登入流程。 開始 URI 包含傳送驗證要求時所用的線上提供者位址以及其他所需的資訊 (例如應用程式識別碼)、驗證完成後使用者前往的重新導向 URI，以及預期的回應類型。 您可以向提供者詢問所需的參數。 以下是使用 `<meta>` 標籤的範例：

```HTML
<meta name="ms-webauth-uris" content="https://<providerstartpoint>?client_id=<clientid>&response_type=token, https://<appendpoint>"/>
```

如需詳細指導方針，請參閱[線上提供者的 Web 驗證代理人考量](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx)。

## 應用程式功能宣告

若您的 App 需要以程式設計方式存取使用者資源 (例如圖片) 或裝置 (例如相機或麥克風)，則您必須宣告適當的功能。 有三種應用程式功能宣告類別： 

- 適用於大部分通用 App 案例的[一般用途功能](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#General-use_capabilities)。 
- 可讓您的應用程式存取周邊和內部裝置的[裝置功能](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Device_capabilities)。 
- 需要特殊公司帳戶以送出到「市集」來使用它們的[特殊用途功能](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Special_and_restricted_capabilities)。 

如需有關公司帳戶的詳細資訊，請參閱[帳戶類型、位置和費用](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx)。

> [!NOTE]
> 請務必了解，當客戶從 Windows 市集取得您的應用程式時，會得知該應用程式宣告的所有功能。 因此，請勿使用您 App 不需要的功能。

您可藉由在 App 的[套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中宣告功能，以要求存取權。 您可以使用 Microsoft Visual Studio 中的[資訊清單設計工具](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx#Configure)來宣告一般功能，也可選擇以手動方式新增 - 請參閱[如何在套件資訊清單中指定功能](https://msdn.microsoft.com/library/windows/apps/br211477.aspx)。

部分功能提供 App 存取敏感資源的權限。 這些資源會視為敏感資源是因為其可以存取使用者的個人資料，或使用者必須付費才能使用。 受設定應用程式管理的隱私權設定，可讓使用者動態控制敏感資源的存取權。 因此，您的 App 不會假設敏感資源可隨時使用這點非常重要。 如需存取敏感資源的詳細資訊，請參閱[隱私權感知 App 的指導方針](https://msdn.microsoft.com/library/windows/apps/hh768223.aspx)。

## manifoldjs 和應用程式資訊清單

可將您的網站轉變成 UWP app 的輕鬆方式，即是使用**應用程式資訊清單**和 **manifoldjs**。 應用程式資訊清單是包含 App 相關中繼資料的 xml 檔案。 其會指定如 App 的名稱、資源的連結、顯示模式、URL 和其他資料，描述如何部署和執行 App。 manifoldjs 能夠讓此程序變得非常簡單，即使在不支援 Web 應用程式的系統上亦同。 請至 [manifoldjs.com](http://www.manifoldjs.com/) 了解其運作方式的詳細資訊。 您也可以檢視此 [Windows 10 Web 應用程式簡報](http://channel9.msdn.com/Events/WebPlatformSummit/2015/Hosted-web-apps-and-web-platform-innovations?wt.mc_id=relatedsession)當中的 manifoldjs 示範。

## 相關主題
- [Windows 執行階段 API：JavaScript 程式碼範例](http://rjs.azurewebsites.net/)
- [Codepen：用於呼叫 Windows 執行階段 API 的沙箱](http://codepen.io/seksenov/pen/wBbVyb/)
- [Cortana 互動](https://msdn.microsoft.com/library/windows/apps/dn974231.aspx)
- [語音命令定義 (VCD) 元素和屬性 v1.2](https://msdn.microsoft.com/library/windows/apps/dn954977.aspx)
- [線上提供者的 Web 驗證代理人考量](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx)
- [應用程式功能宣告](https://msdn.microsoft.com/ibrary/windows/apps/hh464936.aspx)


<!--HONumber=Aug16_HO3-->


