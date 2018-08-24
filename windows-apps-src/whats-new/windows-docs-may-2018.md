---
author: QuinnRadich
title: Windows 文件中的 5 2018年的新功能-開發 UWP 應用程式
description: 新功能、 影片及開發人員指南已新增至 Windows 10 開發人員文件的 5 2018年和 Microsoft 組建會議。
keywords: 新功能、 更新、 功能、 開發人員指南 Windows 10，5，組建
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2842749"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>5 2018 Windows 開發人員文件中新增功能

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、 開發人員指南、 影片與範例已可使用中[Microsoft 組建 2018年](https://www.microsoft.com/build)開發人員會議與重疊的年 5 月。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>在 Fluent 設計的影片

發展動畫 Fluent 設計系統中的使用者、 內建的時機、 簡化、 方向及 gravity 基礎上。 套用這些基礎 （英文） 可協助引導使用者通過您的應用程式，並連線它們其數位經驗反映出自然 world。 本文章中深入了解：

* [影片概觀 （英文）](../design/motion/index.md)已更新並反映這些基礎 （英文）。
* [動畫的作法](../design/motion/motion-in-practice.md)提供如何套用您的應用程式內的這些基礎 （英文） 的範例。
* [Directionality 和 gravity](../design/motion/directionality-and-gravity.md) solidifies 應用程式的使用者的精神模型。
* [計時及簡化](../design/motion/timing-and-easing.md)將真實度新增至您的應用程式的影片。

![在 [動作動畫](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent 設計更新

視覺更新及小幅變更至下列 Fluent 設計頁面進行：

* [對齊、 邊框距離設邊界](../design/layout/alignment-margin-padding.md)
* [色彩](../design/style/color.md)
* [命令基本知識](../design/basics/commanding-basics.md)
* [Fluent 的 Windows 應用程式的設計](../design/fluent-design-system/index.md)
* [應用程式的設計簡介](../design/basics/design-and-ui-intro.md)
* [瀏覽基本知識](../design/basics/navigation-basics.md)
* [回應式設計技術](../design/layout/responsive-design.md)
* [螢幕大小與中斷點](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [樣式概觀 （英文)](../design/style/index.md)
* [撰寫方式](../design/style/writing-style.md)

此外，我們已修正其內容的區域上的所有新資訊與下列頁面：

* [圖示](../design/style/icons.md)現在提供使用圖示以及將它們可點選的實用建議。
* [印刷樣式](../design/style/typography.md)合併資訊從類似的文章、 每個項目放在已更新的指導和圖例的單一位置。

![色彩調色盤圖像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio 中的應用程式安裝程式檔案

現在可以使用 Visual Studio 2017、 更新 15.7 建立應用程式安裝程式檔案。 [了解如何使用 Visual Studio 建立應用程式安裝程式檔案](../packaging/create-appinstallerfile-vs.md)及啟用自動更新您的應用程式。 如果您正在執行將問題，請參閱檢視一般問題與解決方案的[疑難排解應用程式安裝程式檔案的安裝問題](../packaging/troubleshoot-appinstaller-issues.md)。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge Windows Form 與 WPF 應用程式的網頁檢視控制項

使用網頁檢視控制先前 UWP 應用程式只提供 web 內容顯示桌面應用程式中。 此控制項會使用 Microsoft 邊緣轉譯引擎嵌入呈現豐富格式化 HTML 檢視內容從遠端網頁伺服器、 動態產生的程式碼或內容檔案。 尋找最新版本的網頁檢視控制[Windows 社群 Toolkit。](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

尋找其他控制項像在未來版本的 Windows 社群 Toolkit 的網頁檢視。 如需詳細資訊，請參閱[主機 UWP WPF 和 Windows Form 應用程式中的控制項。](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>視線 input 及互動

[追蹤使用者的注視、注意力，以及根據他們眼球的位置與移動存在。](../design/input/gaze-interactions.md) 此功能強大的新方式使用和互動 UWP 應用程式會為輔助技術特別實用。 視線輸入也會提供強大的遊戲 （包括目標擷取及追蹤） 與傳統輸入的裝置 （、 滑鼠、 鍵盤觸控） 不提供其他互動式案例的機會。

### <a name="msix-packaging-format"></a>MSIX 封裝格式

在 Microsoft 組建 2018年會議即已宣佈、 MSIX 是適用於所有包括 Win32 和 Windows Form、 WPF、 UWP 的 Windows 應用程式的新 containerization 封裝格式。 這個新的格式是繼承 UWP 更好的功能：

* 強大的安裝和更新。 
* 受管理與彈性功能系統的安全性模型。
* 支援 Microsoft 存放區、 企業管理及許多自訂通訊模型。

若要建立這些套件工具可在未來的版本的 Visual Studio 和 Windows sdk （英文）。

MSIX 封裝格式為我們支援工具與解決方案 MSIX 生態系統的協力廠商輕易開放原始格式。 若要深入了解 MSIX 封裝格式，請參閱[MSIX sdk （英文）](https://github.com/Microsoft/msix-packaging)。 

![MSIX 封裝映像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>具可執行程式碼的選用套件

在您的應用程式的選用套件現在可以包含可執行檔的 C# 程式碼。 [了解如何使用 Visual Studio 來設定選用的附加元件封裝以支援您的主應用程式套件。](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>頁面轉換

[頁面切換](../design/motion/page-transitions.md)瀏覽應用程式中的頁面之間的使用者。 它們協助使用者了解其位於的導覽階層中的位置，並提供意見反應頁面之間的關係。

### <a name="project-rome"></a>Project Rome

Project 羅馬小組具有徹底檢查其 iOS 及 Android 的 Sdk、 新增新功能，例如使用者活動和重整有多少跨不同 Sdk 提供一致的程式設計經驗其程式碼。 [所有新的 API 參考 （英文） 和用法的文件](https://docs.microsoft.com/windows/project-rome/)預期存留期間組建 2018年開發人員會議。

### <a name="sets"></a>設定

使用在 Windows 內部預覽組建組功能。 當使用 「 組 」 功能，您的應用程式會繪製到可能會與其他應用程式] 與擁有自己的索引標籤的標題列中每個應用程式共用的視窗。 [設計適用於設定](../design/shell/design-for-sets.md)如何最佳化您的應用程式提供最佳的體驗設定 UI 中具有指引。

## <a name="developer-guidance"></a>開發人員指引

### <a name="get-started"></a>開始

我們已 revitalized 我們先入門內容與新的學習追蹤。 這些新主題旨在提供新的 Windows 10 開發人員的資訊一些常見的工作時可能會想要完成。 它們不是教學課程並不提供手持逐步解說，但而指向現有的文件所在以及如何使用它取出。 取出[開始撰寫程式碼](../get-started/create-uwp-apps.md)改頭換面] 頁面上，或探索每個個別的學習追蹤：

* [建構表單](../get-started/construct-form-learning-track.md)
* [顯示清單中的客戶](../get-started/display-customers-in-list-learning-track.md)
* [載入和儲存設定](../get-started/settings-learning-track.md)
* [使用檔案](../get-started/fileio-learning-track.md)

![取得入門的圖像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>廣告績效報告

在開發人員中心儀表板[廣告效能報告](../publish/advertising-performance-report.md)現在提供 viewability 評量。 我們也會新增[最佳化的 ad 單位 viewability](../monetize/optimize-ad-unit-viewability.md)文章提供的最佳化的您 ads viewability 建議。

### <a name="targeted-push-notifications"></a>目標的推入通知

開發人員中心儀表板中的 [[通知](../publish/send-push-notifications-to-your-apps-customers.md)] 頁面上圖形和 world 地圖檢視的所有通知現在都提供其他分析資料。

## <a name="videos"></a>影片

### <a name="cwinrt"></a>C++/WinRT

C + + WinRT 是製作及取用 Windows Runtime api （英文） 的新方式。 已實作獨立的標頭檔，並可提供您頂級現代應用程式功能的存取權。 若要了解其運作方式，然後[閱讀開發人員文件](../cpp-and-winrt-apis/index.md)的詳細資訊，請[觀賞影片](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)。

### <a name="multi-instance-uwp-apps"></a>多執行個體 UWP app

Windows 現在可讓您使用自己的不同程序中的每個執行多個您 UWP 應用程式執行個體。 若要了解如何建立新的應用程式的詳細指引如何支援此功能，然後[閱讀開發人員文件](../launch-resume/multi-instance-uwp.md)和使用這項功能的理由，請[觀賞影片](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)。

## <a name="samples"></a>範例

### <a name="customer-database-tutorial"></a>客戶資料庫教學課程

此教學課程會建立基本的 UWP 應用程式管理的客戶、 清單並介紹概念與企業開發適合的作法。 它會引導您透過實作 UI 元素和新增作業對本機 SQLite 資料庫，並提供如果您想要進一步移到遠端的其餘資料庫連接的寬鬆指導。 [請參閱以下的教學課程](../enterprise/customer-database-tutorial.md)