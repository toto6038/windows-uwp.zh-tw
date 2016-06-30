---
author: Mtoepke
title: "常見問題集"
description: "Xbox 上 UWP 的常見問題集。"
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: 34e186049039d5a8366f34e985ad7250ef664f00

---

# 常見問題集

項目未以您預期的方式運作？ 請閱讀這一頁的常見問題集。 也請參閱[已知問題](known-issues.md)主題和[開發通用 Windows 應用程式](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)論壇。 

### 我的遊戲和應用程式為何無法運作？

如果您的遊戲和應用程式無法運作，或您無法存取存放區或 Live 服務，則可能是以開發人員模式執行。 如果您選取 \[首頁\]，並在畫面右邊看到大型 \[開發首頁\] 磚，而不是一般的 Gold/Live 內容，則是正在以開發人員模式執行。 如果您想要玩遊戲，可以開啟 \[開發首頁\]，然後使用 \[離開開發人員模式\] 按鈕切換回 \[零售模式\]。

### 為什麼我無法使用 Visual Studio 連線到我的 Xbox One？

請從確認您是以開發人員模式執行開始，而不是零售模式。 處於零售模式時，您無法連線到您的 Xbox One。 按下 \[首頁\] 按鈕，並尋找畫面右側的 \[開發首頁\] 磚，即可進行這項檢查。 如果此磚不存在，而是看到 Gold/Live 內容，則是處於零售模式。 您需要執行「啟用開發人員模式」應用程式，才能切換至開發人員模式。

> **注意**
            &nbsp;&nbsp;您必須在已有使用者已登入的情況下才能部署 app。

如需詳細資訊，請參閱此頁面後面的[修正部署失敗](frequently-asked-questions.md#fixing-deployment-failures)。

### 如何切換零售模式與開發人員模式?

請依照[啟用 Xbox One 開發人員模式](devkit-activation.md)指示進行，來深入了解這些狀態。

### 如何知道我是處於零售模式還是開發人員模式?

請依照[啟用 Xbox One 開發人員模式](devkit-activation.md)指示進行，來深入了解這些狀態。 

按下 \[首頁\] 按鈕，並查看畫面右側，即可進行這項檢查。 如果您是處於開發人員模式，則會在右側看到 \[開發首頁\] 磚。 如果您是處於零售模式，則會看到一般的 Gold/Live 內容。

### 如果我啟用開發人員模式，我的遊戲和應用程式仍然可以運作嗎？

是，您可以從開發人員模式切換至零售模式 (您可以在此玩遊戲)。 如需詳細資訊，請參閱[啟用 Xbox One 開發人員模式](devkit-activation.md)。 

<!-- > **CAUTION**&nbsp;&nbsp;The Xbox Developer Preview System Update includes experimental and early pre-release software. 
This means that some popular games and apps will not work as expected and you may experience occasional crashes and data loss. -->

### 我是否將遺失我的遊戲和應用程式或儲存的變更？

如果您決定離開開發人員預覽計畫，可能需要執行原廠重設，這將會清除您主機上的所有內容。 如果發生這種情況，您必須重新安裝所有遊戲和應用程式。 只要您在執行它們時在線上，您儲存的遊戲全部都會儲存在 Live 帳戶雲端設定檔上，因此將不會遺失。

### 如何離開開發人員預覽？

如需如何離開開發人員預覽的詳細資料，請參閱[停用 Xbox One 開發人員模式](devkit-deactivation.md)主題。

### 我賣掉 Xbox One，並讓它處於開發人員模式。 如何停用開發人員模式？

如果您不再需要存取 Xbox One，可以在 Windows 開發人員中心停用它。 如需詳細資料，請參閱[停用 Xbox One 開發人員模式](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center)主題的**使用 Windows 開發人員中心停用您的主機**一節。

### 我使用 Windows 開發人員中心離開開發人員預覽，但仍然處於開發人員模式。 我該怎麼做？

啟動 \[開發首頁\]，然後選取 \[離開開發人員模式\] 按鈕。 這會將您的主機重新啟動為零售模式。 

### 我是否可以發佈我的應用程式？

透過今年稍後的開發人員中心，可以發佈應用程式。 在零售 Xbox One 上建立和測試的 UWP 應用程式將會進行 Windows 目前所處理的相同擷取、檢閱和發佈程序，但需要一些額外的檢閱才能符合目前的 Xbox One 標準。

### 我是否可以發佈我的遊戲？

您可以在開發人員模式使用 UWP 和 Xbox One，以在 Xbox One 上建置並測試您的遊戲。 若要發佈 UWP 遊戲，您必須登錄 [ID@XBOX](http://www.xbox.com/en-us/Developers/id)。 
[ID@XBOX](http://www.xbox.com/en-us/Developers/id) 為開發人員提供其遊戲的 Xbox Live API (包括 遊戲分數與成就) 的完整存取權，以及利用裝置、雲端儲存以及 Xbox One 上的所有 Xbox Live 功能之間的多玩家功能。 
[ID@XBOX](http://www.xbox.com/en-us/Developers/id) 也可以存取遊戲的 Xbox One 開發套件，而遊戲可能需要存取最大部分的 Xbox One 硬體。

### 標準遊戲引擎是否可以運作？

請參閱此預覽版本的 \[已知問題\] 頁面。

### Xbox One 上的 UWP 遊戲可以使用哪些功能和系統資源？ 

如需詳細資訊，請參閱[適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源](system-resource-allocation.md)。

### 如果我建立 DirectX 12 UWP 遊戲，是否將以開發人員模式在 Xbox One 上執行該遊戲？

如需詳細資訊，請參閱[適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源](system-resource-allocation.md)。

### 在 Xbox 上是否可以使用整個 UWP API 表面？

請參閱此預覽版本的 \[已知問題\] 頁面。

### 修正部署失敗

如果您無法從 Visual Studio 部署您的應用程式，這些步驟可協助您修正問題。 如果您遇到困難，請在論壇上要求協助。

> **注意**
            &nbsp;&nbsp;您必須在已有使用者已登入的情況下才能部署 app。 如果您收到 0x87e10008 錯誤訊息，請確定已經有使用者登入，然後再試一次。

如果 Visual Studio 無法連線到 Xbox One：

1. 請確定您處於開發人員模式 (先前已在此頁面討論過)。
2. 請確定您已經正確地設定開發電腦。 您是否已依照[開始使用 Xbox One 上的 UWP 應用程式開發](getting-started.md)中的*所有*指示進行？ 

3. 如果您還沒有這麼做，請閱讀[開發環境設定](development-environment-setup.md)主題和 [Xbox One 工具簡介](introduction-to-xbox-tools.md)主題。

4. 請確定您可以從開發電腦 "ping" 到主控台 IP 位址。
> **注意**
            &nbsp;&nbsp;為了取得最佳的部署效能，建議您使用有線連線方式連線主機。

5. 請確定您是使用 \[偵錯\] 索引標籤的 \[驗證\] 下拉式清單中的 \[通用 (未加密的通訊協定)\]。 如需詳細資料，請參閱[開發環境設定](development-environment-setup.md)。

<!--6. Make sure you are not hitting a PIN pairing issue; see "Visual Studio/Xbox PIN pairing failures" in the [Known Issues](known-issues.md) topic.-->

<!--
If Visual Studio can connect, but deployment is failing (for example you get this error message: "DEP0700 : Registration of the app failed.(0x80073cf9)"):

1. Make sure that your app is not installed by uninstalling it from the Collections app in the Xbox One shell. 

> **Note**&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

2. If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode, and then switch back to Developer Mode. 
This will clear Dev Storage.

3. If your issues persist, follow the steps above and then use **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.
-->

### 如果我要使用 HTML/JavaScript 建置應用程式，要如何啟用遊戲台瀏覽？

TVHelpers 是一組 JavaScript 和 XAML/C# 範例和程式庫，可協助您以 JavaScript 和 C# 建置絕佳的 Xbox One 與電視體驗。 TVJS 是可協助您針對 Xbox One 建置優質 UWP app 的程式庫。 TVJS 包含自動控制器瀏覽、多媒體播放、搜尋等支援。 您可以使用 TVJS 搭配託管的 Web 應用程式，就如同搭配具備 Windows 執行階段 API 完整存取權之封裝的 Web 應用程式一樣簡單。

如需詳細資訊，請參閱 [TVHelpers](https://github.com/Microsoft/TVHelpers) 專案和專案 [wiki](https://github.com/Microsoft/TVHelpers/wiki)。

## 另請參閱
- [Xbox One 上的 UWP 開發人員預覽的已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Jun16_HO4-->


