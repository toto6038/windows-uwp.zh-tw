---
author: QuinnRadich
title: 在 2018 年 Windows 文件的最新動向-開發 UWP app
description: 新功能、 影片及開發人員指引已新增至 2018 年的 Windows 10 開發人員文件和 Microsoft Build 會議。
keywords: 新動向，更新，功能，開發人員指引，Windows 10 年，組建
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "4129295"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>在 2018 年 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、 開發人員指引、 影片及範例已可供可能與[Microsoft 組建 2018年](https://www.microsoft.com/build)開發人員會議一致的月份中。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>在 Fluent 設計的動作

在 Fluent 設計系統中動作的使用者已發展，內建的時間、 加/減速、 方向以及重力基礎上。 套用這些基礎可協助引導您的應用程式中，使用者和由反映自然界將它們連接與及其數位體驗。 了解更多此文章中：

* [動作概觀](../design/motion/index.md)已經更新以反映這些基本概念。
* [實務影片](../design/motion/motion-in-practice.md)提供如何套用這些基礎在您的應用程式內的範例。
* [方向性和重力](../design/motion/directionality-and-gravity.md)強化使用者的心理模式您的應用程式。
* [計時和加/減速](../design/motion/timing-and-easing.md)加入您的應用程式中的動作擬真度。

![作用中動作](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design 更新

下列的 Fluent Design 頁面已經做視覺更新及細微的變更：

* [對齊方式、 邊框間距，邊界](../design/layout/alignment-margin-padding.md)
* [色彩](../design/style/color.md)
* [命令基本知識](../design/basics/commanding-basics.md)
* [Windows 應用程式的 fluent 設計](../design/fluent-design-system/index.md)
* [應用程式設計簡介](../design/basics/design-and-ui-intro.md)
* [瀏覽基本知識](../design/basics/navigation-basics.md)
* [回應式設計技術](../design/layout/responsive-design.md)
* [螢幕大小與中斷點](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [樣式概觀](../design/style/index.md)
* [撰寫樣式](../design/style/writing-style.md)

此外，我們已經重寫下列頁面以其內容區域上的所有新資訊：

* [圖示](../design/style/icons.md)現在會提供實際使用的圖示，並讓他們成為可點選的建議。
* [印刷樣式](../design/style/typography.md)合併資訊從類似的文章，將所有項目放在單一位置的已更新的指導方針與圖例。

![色彩調色盤映像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>在 Visual Studio 中的應用程式安裝程式檔案

現在可以使用 Visual Studio 2017 更新 15.7 開始建立應用程式安裝程式檔案。 [了解如何使用 Visual Studio 建立應用程式安裝程式檔案](../packaging/create-appinstallerfile-vs.md)，並啟用自動更新您的應用程式。 如果您遇到問題，請參閱[疑難排解安裝問題的應用程式安裝程式檔案](../packaging/troubleshoot-appinstaller-issues.md)來檢視常見問題和解決方案。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge WebView 控制項，適用於 Windows Form 及 WPF 應用程式

藉由使用 WebView 控制項，先前僅適用於 UWP 應用程式，傳統型應用程式中顯示網頁內容。 這個控制項使用 Microsoft Edge 轉譯引擎，以內嵌檢視呈現豐富格式 HTML 內容從遠端 web 伺服器、 動態產生的程式碼或內容檔案。 最新版本中找到，WebView 控制項會[Windows 社群工具組。](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

尋找像是 WebView 未來版本的 Windows 社群工具組的其他控制項。 如需詳細資訊，請參閱[主機 UWP 控制項 WPF 及 Windows Forms 應用程式中。](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>注視輸入和互動

[追蹤使用者的注視、注意力，以及根據他們眼球的位置與移動存在。](../design/input/gaze-interactions.md) 此強大的新方法，使用和您的 UWP app 的互動是特別有用的輔助技術。 注視輸入也提供吸引人的機會用於電腦遊戲 （包括目標擷取和追蹤） 和其他互動式案例，其中傳統輸入的裝置 （鍵盤、 滑鼠、 觸控） 就無法使用。

### <a name="msix-packaging-format"></a>MSIX 封裝格式

在 Microsoft 組建 2018年會議已宣布，MSIX 是適用於所有 Windows 應用程式，包括 Win32、 Windows Forms、 WPF 和 UWP 的新邁向套件格式。 這個新的格式會繼承自 UWP 的絕佳的功能：

* 健全的安裝和更新。 
* 管理安全性模型，以彈性的功能的系統。
* 在 Microsoft Store、 企業版管理，以及許多自訂散發模型的支援。

若要建立這些套件的工具會在未來版本的 Visual Studio 和 Windows SDK 中提供。

MSIX 封裝格式是以方便我們的合作夥伴，以支援使用他們的工具和解決方案 MSIX 生態系統的開放原始碼格式。 若要深入了解 MSIX 封裝格式，請參閱[MSIX SDK](https://github.com/Microsoft/msix-packaging)。 

![MSIX 封裝映像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>具可執行程式碼的選用套件

在您的應用程式中的選用套件現在可以包含可執行檔的 C# 程式碼。 [了解如何使用 Visual Studio 設定選用的附加元件的套件，以支援您的主應用程式套件。](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>頁面轉換

使用者在應用程式中的頁面之間瀏覽的[頁面轉換](../design/motion/page-transitions.md)。 它們能協助使用者了解它們的瀏覽階層中的位置，並提供有關在頁面之間關聯性的意見反應。

### <a name="project-rome"></a>Project Rome

Project Rome 團隊有徹底檢查，其 iOS 和 Android 的 Sdk，新增新的功能，例如使用者活動和重構大部分的程式碼來跨不同的 Sdk 提供一致的程式設計體驗。 [所有新的 API 參考和如何使用文件](https://docs.microsoft.com/windows/project-rome/)將上線期間的組建 2018年開發人員會議。

### <a name="sets"></a>設定

在 Windows 測試人員預覽版中使用 「 集合 」 功能。 當使用 「 集合 」 功能，您的應用程式是繪製到可能會與其他應用程式，具有自己的索引標籤，在標題列中每個應用程式使用共用的視窗。 [針對集合的設計](../design/shell/design-for-sets.md)對如何最佳化您的應用程式，以提供最佳的體驗設定 UI 中的指導方針。

## <a name="developer-guidance"></a>開發人員指引

### <a name="get-started"></a>入門

我們已經 revitalized 我們取得啟動具有新的學習追蹤的內容。 這些新主題旨在提供新的 Windows 10 開發人員可能會想要完成一些常見的工作資訊。 它們不教學課程和未提供手持的逐步解說中，但改為為止現有的文件存在於的位置，以及如何使用它。 請查看改頭換面[開始撰寫程式碼](../get-started/create-uwp-apps.md)的頁面上，或瀏覽每個個別的學習追蹤：

* [建構表單](../get-started/construct-form-learning-track.md)
* [在清單中顯示客戶](../get-started/display-customers-in-list-learning-track.md)
* [儲存和載入設定](../get-started/settings-learning-track.md)
* [使用檔案](../get-started/fileio-learning-track.md)

![取得已啟動的映像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>廣告績效報告

在開發人員中心儀表板中的[廣告績效報告](../publish/advertising-performance-report.md)現在提供可見性計量。 我們也會新增[最佳化您的廣告單元的可見性](../monetize/optimize-ad-unit-viewability.md)文章來提供最佳化您的廣告的可見性。

### <a name="targeted-push-notifications"></a>目標式推播通知

在開發人員中心儀表板的 [[通知](../publish/send-push-notifications-to-your-apps-customers.md)] 頁面現在提供其他分析資料，為您在圖表和世界地圖檢視中的所有通知。

## <a name="videos"></a>影片

### <a name="cwinrt"></a>C++/WinRT

C + + /winrt 是以新的方式撰寫和使用 Windows 執行階段 Api。 它已在標頭檔案中實作唯一的以及設計用來提供您現代化應用程式功能的第一級存取。 若要了解它的運作方式，然後[閱讀開發人員文件](../cpp-and-winrt-apis/index.md)如需詳細資訊，[請觀看影片](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)。

### <a name="multi-instance-uwp-apps"></a>多執行個體 UWP app

Windows 現在可讓您使用自己的個別處理程序中的每個執行您的 UWP 應用程式的多重執行個體。 若要了解如何建立新的應用程式更多的指導方針，如何針對支援此功能，然後[閱讀開發人員文件](../launch-resume/multi-instance-uwp.md)，以及為什麼要使用此功能，[請觀看影片](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)。

## <a name="samples"></a>範例

### <a name="customer-database-tutorial"></a>客戶資料庫教學課程

本教學課程建立基本的 UWP 應用程式，用於管理的客戶清單，並引進了概念和企業開發有用的作法。 它會引導您完成實作 UI 元素以及新增對本機 SQLite 資料庫，作業，並提供連線到遠端的其餘部分資料庫，如果您想要進一步的鬆散指導方針。 [請參閱以下的教學課程](../enterprise/customer-database-tutorial.md)