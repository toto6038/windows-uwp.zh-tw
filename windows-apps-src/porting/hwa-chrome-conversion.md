---
author: seksenov
title: "託管的 Web 應用程式 - 將您的 Chrome 應用程式轉換為通用 Windows 平台 App"
description: "將您的 Chrome 應用程式或 Chrome Extension 轉換為適用於 Windows 市集的通用 Windows 平台 (UWP) 應用程式。"
kw: Package Chrome Extension for Windows Store tutorial, Port Chrome Extension to Windows 10, How to convert Chrome App to Windows, How to add Chrome Extension to Windows Store, hwa-cli, Hosted Web Apps Command Line Interface CLI Tool, Install Chrome Extension on Windows 10 Device, convert .crx to .AppX
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "適用於 Windows 的 Chrome 擴充功能, 適用於 Windows 的 Chrome 應用程式, hwa-cli, 將 .crx 轉換成 .AppX"
ms.assetid: 04f37333-48ba-441b-875e-246fbc3e1a4d
ms.openlocfilehash: b2168242d5464dbf41f12c777aa5672753a4ae6e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-your-existing-chrome-app-to-a-uwp-app"></a>將您現有的 Chrome App 轉換為 UWP app

您可輕鬆地將現有的 Chrome 託管應用程式，轉換為在通用 Windows 平台 (UWP) 上執行的 App。 您可使用兩種方式來轉換 Chrome 應用程式：

- 選項 #1：[ManifoldJS](http://manifoldjs.com/) 現支援使用 Chrome 資訊清單做為輸入形式。 

- 選項 #2：運用我們開發的 [CLI 工具](https://github.com/MicrosoftEdge/hwa-cli)，自您的現有 `.zip` 或 `.crx` 檔案產生 `.appx` 套件。

## <a name="convert-your-existing-chrome-app-using-the-command-line-interface"></a>使用命令列介面轉換您的現有 Chrome 應用程式

1. 安裝 [NodeJS](https://nodejs.org/en/) 及其套件管理員 [npm](https://www.npmjs.com/)。 


2. 開啟命令提示字元視窗瀏覽至您選擇的目錄


3. 在命令列中輸入下列命令，以安裝 Hosted Web Apps (HWA) Command Line Interface (CLI)： `npm i -g hwa-cli`

4. 在命令列中輸入下列命令，以轉換您的 Chrome 套件 (`.crx` 和 `.zip` 為支援的套件格式)：`hwa convert path/to/chrome/app.crx` 或 `hwa convert path/to/chrome/app.zip`

    **將 `path/to/chrome/app` 取代為導向您 Chrome 應用程式的路徑資訊。*
    
5. 產生的 `.appx` 會出現在與您 Chrome 套件相同的資料夾。 現在您可隨時將 App 上傳至 Windows 市集。 

## <a name="uploading-your-app-to-the-windows-store"></a>將您的 App 上傳至 Windows 市集

若要上傳您的 App，請造訪位於 [Windows 開發人員中心](https://developer.microsoft.com/windows)的 [儀表板]。 按一下 [[建立新的應用程式](https://developer.microsoft.com/dashboard/Application/New)]，並保留您的 App 名稱。
![Windows 開發人員中心儀表板保留名稱](images/hwa-to-uwp/reserve_a_name.png)


瀏覽至 [提交] 區段中的 [套件] 頁面，以上傳您的 `AppX` 套件。

填寫 Windows 市集提示。

    During the conversion process, you will be prompted for an Identity Name, Publisher Identity, and Publisher Display Name. To retrieve these values, visit the Dashboard in the [Windows Dev Center](https://developer.microsoft.com/windows).
    - Click on "[Create a new app](https://developer.microsoft.com/dashboard/Application/New)" and reserve your app name.
![Windows 開發人員中心儀表板保留名稱](images/hwa-to-uwp/reserve_a_name.png)
    - 接著在 [應用程式管理] 區段底下，按一下左邊功能表中的 [應用程式身分識別]。
    ![Windows 開發人員中心儀表板應用程式身分識別](images/hwa-to-uwp/app_identity.png)
    - 您應該會看到三個值，提示您在頁面上列出： 
        1. 身分識別名稱： `Package/Identity/Name`
        2. 發行者身分識別： `Package/Identity/Publisher`
        3. 發行者顯示名稱： `Package/Properties/PublisherDisplayName`


## <a name="guide-for-migrating-your-hosted-web-app"></a>託管的 Web 應用程式移轉指南

針對 Windows 市集完成 Web 應用程式封裝作業後，您可進行自訂使 App 可跨越所有的 Windows 裝置順暢運作，包括電腦、平板電腦、手機、HoloLens、Surface Hub、Xbox 和 Raspberry Pi。

### <a name="application-content-uri-rules"></a>應用程式內容 URI 規則

[應用程式內容 URI 規則 (ACUR)](./hwa-access-features.md) 或內容 URI 會透過您應用程式套件資訊清單中的 URL 允許清單，定義託管 Web 應用程式的範圍。 為了控制遠端內容的雙向通訊，您必須定義要從此清單中包含和/或排除的 URL。 若使用者按一下未明確包含的 URL，Windows 會在預設瀏覽器中開啟目標路徑。 您亦可透過 ACUR 授與 URL [通用 Windows API](https://msdn.microsoft.com/library/windows/apps/br211377.aspx) 存取權。

規則至少必須包含您 App 的開始頁面。 此轉換工具會根據您的開始頁面及其網域，自動為您建立一組 ACUR。 不過，若有任何程式設計方式重新導向操作 (無論是伺服器或用戶端)，則必須將這些目的地新增至允許清單。

*注意︰ACUR 僅適用於頁面瀏覽。 影像、JavaScript 程式庫以及其他類似資產不會受到這些限制所影響。*

許多應用程式皆使用第三方網站執行登入流程，例如 Facebook 和 Google。 此轉換工具會根據最熱門的網站，自動為您建立一組 ACUR。 若該清單未包含您的驗證方法，且其為重新導向流程，則您必須新增其路徑做為 ACUR。 您也可以考慮使用 [Web 驗證代理人](./hwa-access-features.md)。

### <a name="flash"></a>Flash

Windows 10 App 不允許使用 Flash。 您必須確定您的 App 使用體驗不會因缺少 Flash 而受到影響。

若為廣告，則必須確定廣告提供者具有 HTML5 選項。 您可以查看 [Bing 廣告](https://bingads.microsoft.com/)和 [Microsoft Advertising 程式庫](../monetize/display-ads-in-your-app.md)。 

YouTube 影片現已[預設使用 HTML5`<video>`，](http://youtube-eng.blogspot.com/2015/01/youtube-now-defaults-to-html5_27.html)因此只要您使用[`<iframe>`內嵌方法](https://developers.google.com/youtube/iframe_api_reference)，其仍可正常運作。 若您的應用程式仍使用 Flash API，則您必須切換至先前述及的內嵌樣式。

### <a name="image-assets"></a>影像資產

Chrome 網路商店已[要求在您的應用程式套件中使用 128x128 的 App 圖示影像](https://developer.chrome.com/webstore/images)。 針對 Windows 10 App，您至少必須提供 44x44、50x50、150x150 和 600x350 應用程式圖示影像。 此轉換工具會根據 128x128 影像，自動為您建立這些影像。 為了提供更豐富、完美的 App 使用體驗，我們強烈建議建立您建立專屬的影像檔案。 這裡有一些關於[磚和圖示資產的指導方針](https://msdn.microsoft.com/library/windows/apps/mt412102.aspx)。

### <a name="capabilities"></a>功能

必須在套件資訊清單中[宣告](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations) App 功能，才能存取特定的 API 和資源。 此轉換工具會自動為您啟用三個常用的裝置功能：位置、麥克風和網路攝影機。 針對上述功能，系統仍會提示使用者授與存取權限。

*注意︰對於 App 宣告的所有功能使用者都會收到通知。 我們建議移除任何您的 App 不需要的功能。*

### <a name="file-downloads"></a>檔案下載

目前不支援傳統的檔案下載，如您在瀏覽器中所見。

### <a name="chrome-platform-apis"></a>Chrome 平台 API

Chrome 為 App 提供[特殊用途的 API](https://developer.chrome.com/apps/api_index)，其可做為背景指令碼執行。 不支援使用這些 API。 您可透過 [Windows 執行階段 API](https://msdn.microsoft.com/library/windows/apps/br211377.aspx)，找到更多的同等功能。

## <a name="related-topics"></a>相關主題

- [存取通用 Windows 平台 (UWP) 功能以增強您的 Web 應用程式](./hwa-access-features.md)
- [通用 Windows 平台 (UWP) App 指南](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [下載 Windows 市集應用程式的設計資產](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)
