---
title: 在 Windows 文件中在 2018 年最新消息-開發 UWP 應用程式
description: 新功能、 影片和開發人員指引已新增至 2018 年的 Windows 10 開發人員文件和 Microsoft Build 會議。
keywords: 最新消息、 更新功能，開發人員指導方針，Windows 10，5，組建
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598783"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>在 Windows 開發人員文件在 2018 年最新消息

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能的概觀、 開發人員指南、 影片和範例可供與一致的 5 月份[Microsoft Build 2018](https://www.microsoft.com/build)開發人員會議。

在 Windows 10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>Fluent 設計的影片

Fluent Design System 中的動作的使用者不斷進化，基礎的執行時間、 簡化、 方向性和重力基本概念。 套用這些基本概念可協助引導您的應用程式，使用者並讓他們與他們的數位體驗，透過反映自然的世界。 了解本文中的其他資訊：

* [影片概觀](../design/motion/index.md)已更新以反映這些基本概念。
* [影片中練習](../design/motion/motion-in-practice.md)提供有關如何套用這些應用程式內的基本概念的範例。
* [方向和重力](../design/motion/directionality-and-gravity.md)solidifies 使用者的心智模型，您的應用程式。
* [計時，以及減輕](../design/motion/timing-and-easing.md)加入您的應用程式中的動畫的真實性。

![作用中的動作](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent 設計更新

視覺效果更新和次要的變更進行了下列 Fluent 設計頁面：

* [對齊方式，與邊框距離、 邊界](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [命令基本知識](../design/basics/commanding-basics.md)
* [Windows 應用程式的 Fluent 設計](../design/fluent-design-system/index.md)
* [應用程式設計的簡介](../design/basics/design-and-ui-intro.md)
* [瀏覽基本知識](../design/basics/navigation-basics.md)
* [回應式設計的技術](../design/layout/responsive-design.md)
* [螢幕大小與中斷點](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [樣式概觀](../design/style/index.md)
* [撰寫樣式](../design/style/writing-style.md)

此外，我們已重寫其內容區域的全新有關的下列頁面：

* [圖示](../design/style/icons.md)現在提供使用圖示，並且讓它們可點按的實用建議。
* [印刷樣式](../design/style/typography.md)將類似的文章，將所有內容放在同一個地方更新的指引與圖例資訊合併。

![色彩調色盤映像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>在 Visual Studio 中的應用程式安裝程式檔案

現在可以使用 Visual Studio 2017 時，更新 15.7 建立應用程式安裝程式檔案。 [了解如何使用 Visual Studio 來建立應用程式安裝程式檔案](../packaging/create-appinstallerfile-vs.md)並啟用自動更新您的應用程式。 如果您執行到的問題，請參閱[疑難排解安裝問題的應用程式安裝程式檔案](../packaging/troubleshoot-appinstaller-issues.md)檢視常見的問題和解決方案。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>邊緣 Windows Form 和 WPF 應用程式的 WebView 的控制項

使用 WebView 的 UWP 應用程式先前只提供控制項，在桌面應用程式中顯示 web 內容。 此控制項會使用 Microsoft Edge 轉譯引擎，內嵌轉譯豐富格式化 HTML 的檢視內容從遠端網頁伺服器、 以動態方式產生的程式碼或內容檔案。 尋找最新版的 WebView 控制項[Windows 社群工具組。](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

尋找其他控制項，例如 WebView 未來版本中的 Windows 社群工具組。 如需詳細資訊，請參閱[主機 UWP 控制項在 WPF 和 Windows Forms 應用程式。](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>視線輸入和互動

[追蹤使用者的視線、 注意及根據位置和移動的眼睛的目前狀態。](../design/input/gaze-interactions.md) 這個功能強大的新方式來使用，並與您的 UWP 應用程式互動是做為輔助技術特別有用。 視線輸入也提供吸引人的遊戲 （包括目標取得與追蹤） 和其他傳統的輸入的裝置 （鍵盤、 滑鼠、 觸控） 沒有可用的互動案例的機會。

### <a name="msix-packaging-format"></a>MSIX 封裝格式

Microsoft Build 2018 大會宣布，MSIX 是新的容器化封裝格式套用至所有包括 Win32，Windows Form、 WPF 和 UWP 的 Windows 應用程式。 這個新的格式會繼承 UWP 中最棒的功能：

* 穩固的安裝和更新。 
* 管理安全性模型與彈性功能的系統。
* 支援 Microsoft Store、 企業管理和許多自訂分佈的模型。

若要建立這些套件的工具可在未來的 Visual Studio 和 Windows SDK 版本。

MSIX 封裝的格式是開放原始碼格式，方便我們的合作夥伴，以支援其工具和解決方案 MSIX 生態系統。 若要深入了解 MSIX 封裝格式，請參閱[MSIX SDK](https://github.com/Microsoft/msix-packaging)。 

![MSIX 封裝映像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>具可執行程式碼的選用套件

在您的應用程式中的選擇性套件現在可包含可執行檔C#程式碼。 [了解如何使用 Visual Studio 設定選擇性的附加元件封裝，以支援您的主應用程式套件。](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>頁面轉換

[頁面轉換](../design/motion/page-transitions.md)瀏覽應用程式中的頁面之間的使用者。 它們幫助使用者了解它們的導覽階層架構中的位置，並提供意見反應頁面之間的關聯性。

### <a name="project-rome"></a>Project Rome

Project Rome 小組已經大幅度調整，其 iOS 和 Android 的 Sdk，加入新的功能，例如使用者活動和重構大部分跨不同的 Sdk 提供一致的程式設計體驗，其程式碼。 [所有新的 API 參考和 how-to docs](https://docs.microsoft.com/windows/project-rome/)就會上線期間 Build 2018 開發人員會議。

### <a name="sets"></a>設定

提供 Windows Insider preview 組建設定功能。 使用設定功能時，會將您的應用程式繪製至可能具有自己的索引標籤的標題列中每個應用程式具有的其他應用程式，共用的視窗。 

## <a name="developer-guidance"></a>開發人員指引

### <a name="get-started"></a>立即開始

我們已 revitalized 我們開始入門內容與新的學習途徑。 這些新主題的目標是新的 Windows 10 開發人員提供一些常見的工作中的相關資訊可能會想完成。 它們不教學課程並不會提供掌上型逐步解說中，但是而有現有的文件的位置，以及如何使用它將焦點。 請參閱上全新面貌[開始撰寫程式碼](../get-started/create-uwp-apps.md)頁面上，或瀏覽每個個別的學習追蹤：

* [建構表單](../get-started/construct-form-learning-track.md)
* [顯示在清單中的客戶](../get-started/display-customers-in-list-learning-track.md)
* [儲存及載入設定](../get-started/settings-learning-track.md)
* [使用的檔案](../get-started/fileio-learning-track.md)

![取得開始使用的映像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>廣告績效報告

[廣告效能報告](../publish/advertising-performance-report.md)在合作夥伴中心現在提供 viewability 度量資訊。 我們也新增了[最佳化您的 ad 單元 viewability](../monetize/optimize-ad-unit-viewability.md)文章，以提供最佳化的程式廣告 viewability 的建議事項。

### <a name="targeted-push-notifications"></a>目標式推播通知

[通知](../publish/send-push-notifications-to-your-apps-customers.md)頁面在合作夥伴中心現在會提供額外的分析資料圖形和世界地圖檢視中所有的通知。

## <a name="videos"></a>影片

### <a name="cwinrt"></a>C++/WinRT

C + + /cli WinRT 是撰寫和使用 Windows 執行階段 Api 的新方式。 它的標頭檔中實作唯一，而且專門提供現代化的應用程式功能的第一級存取。 [觀看影片](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)若要了解其運作方式，然後[閱讀開發人員文件](../cpp-and-winrt-apis/index.md)如需詳細資訊。

### <a name="multi-instance-uwp-apps"></a>多執行個體 UWP app

Windows 現在可讓您使用在自己個別的處理序中執行的 UWP 應用程式的多個執行個體。 [觀看影片](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)以了解如何建立新的應用程式支援這項功能，然後[閱讀開發人員文件](../launch-resume/multi-instance-uwp.md)詳細指引如何和為什麼使用這項功能。

## <a name="samples"></a>範例

### <a name="customer-database-tutorial"></a>客戶資料庫教學課程

本教學課程中建立基本的 UWP 應用程式來管理客戶的清單，並介紹概念和做法適用於企業應用程式開發。 它會逐步引導您實作的 UI 項目，並新增對本機 SQLite 資料庫中，執行的作業，並提供鬆散連接到遠端的其餘部分資料庫，如果您想要更進一步的指引。 [請參閱這裡的教學課程](../enterprise/customer-database-tutorial.md)