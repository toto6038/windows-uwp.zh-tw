---
title: 2018 年 5 月 Windows 文件的新增功能 - 開發 UWP 應用程式
description: 新功能、影片及開發人員指引已加入 2018 年 5 月的 Windows 10 開發人員文件和 Microsoft Build 大會。
keywords: 新增功能, 更新, 功能, 開發人員指引, Windows 10, 5 月, build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d9864c59a8bb8569861e9c239710a09602ffdcba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174352"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>2018 年 5 月 Windows 開發人員文件的新增功能

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、開發人員指引、影片和範例已經在 5 月份與 [Microsoft Build 2018](https://www.microsoft.com/build/) 開發人員會議同步提供。

在 Windows 10 上[安裝工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank) 之後，就表示您已經準備好[建立新的通用 Windows 應用程式](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的應用程式程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="motion-in-fluent-design"></a>Fluent Design 中的動作

Fluent Design 動作系統的使用正在不斷發展，建立在時間、簡化、方向性和重力基本概念之上。 套用這些基本概念將有助於引導使用者使用您的應用程式，並通過反映自然世界將他們其與數位體驗連結。 深入了解下列文章：

* [動作概觀](../design/motion/index.md)已更新，以反映這些基本概念。
* [動作實踐](../design/motion/motion-in-practice.md)提供如何在您的應用程式內套用這些基本概念的範例。
* [方向性和重力](../design/motion/directionality-and-gravity.md)鞏固了使用者的應用程式心智模型。
* [計時和加/減速](../design/motion/timing-and-easing.md)為您應用程式中的動作加入了真實感。

![作用中的動作](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design 更新

以下 Fluent Design 頁面已進行視覺效果更新和次要變更：

* [對齊方式、邊框間距、邊界](../design/layout/alignment-margin-padding.md)
* [色彩](../design/style/color.md)
* [命令基本知識](../design/basics/commanding-basics.md)
* [適用於 Windows 應用程式的 Fluent Design](/windows/apps/fluent-design-system)
* [應用程式設計簡介](../design/basics/design-and-ui-intro.md)
* [瀏覽基本知識](../design/basics/navigation-basics.md)
* [回應式設計技術](../design/layout/responsive-design.md)
* [螢幕大小與中斷點](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [樣式概觀](../design/style/index.md)
* [撰寫樣式](../design/style/writing-style.md)

此外，我們已使用全新資訊重寫了以下頁面的內容區域：

* [圖示](../design/style/icons.md)現在提供了使用圖示並使其可點按的實用建議。
* [印刷樣式](../design/style/typography.md)整合來自類似文章的資訊，將所有內容放在同一個地方，並包含更新的指引與圖例。

![色彩調色盤映像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio 中的應用程式安裝程式檔案

現在可以使用 Visual Studio 2017 Update 15.7 或更新版本建立應用程式安裝程式檔案。 [了解如何使用 Visual Studio 建立應用程式安裝程式檔案](/windows/msix/app-installer/create-appinstallerfile-vs)並啟用應用程式的自動更新。 如果您遇到問題，請參閱[針對應用程式安裝程式檔案的安裝問題進行疑難排解](/windows/msix/app-installer/troubleshoot-appinstaller-issues)，以檢視常見的問題和解決方案。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Windows Forms 和 WPF 應用程式的 Edge WebView 控制項

使用 WebView 控制項在傳統型應用程式中顯示 Web 內容 (先前僅適用於 UWP 應用程式)。 此控制項會使用 Microsoft Edge 轉譯引擎來內嵌一個檢視，該檢視可轉譯來自遠端網頁伺服器的格式豐富 HTML 內容、動態產生的程式碼或內容檔案。 您可以在最新版的 [Windows 社群工具組](/windows/uwpcommunitytoolkit/)中找到 WebView 控制項

在未來的 Windows 社群工具組版本中尋找 WebView 等其他控制項。 如需詳細資訊，請參閱[在 WPF 和 Windows Form 應用程式中裝載 UWP 控制項](/windows/apps/desktop/modernize/xaml-islands)。

### <a name="gaze-input-and-interactions"></a>注視輸入與互動

[追蹤使用者的注視、注意力，以及根據他們眼球的位置與移動存在。](../design/input/gaze-interactions.md) 使用 UWP 應用程式並之互動的這種強大新方式，是特別有用的輔助技術。 注視輸入也針對遊戲 (包括目標取得與追蹤) 和無法使用傳統輸入裝置 (鍵盤、滑鼠、觸控) 的其他互動式案例，提供令人注目的機會。

### <a name="msix-packaging-format"></a>MSIX 封裝格式

Microsoft Build 2018 大會宣布，MSIX 是新的容器化套件格式，適用於包括 Win32、Windows Forms、WPF 和 UWP 的所有 Windows 應用程式。 這個新格式繼承了 UWP 的絕佳特性：

* 穩固的安裝和更新。 
* 具有彈性容量系統的受控安全性模型。
* 支援 Microsoft Store、企業管理和許多自訂散佈模型。

在未來的 Visual Studio 和 Windows SDK 版本中可取得用於建立這些套件的工具。

MSIX 封裝格式是開放原始碼格式，讓合作夥伴能使用其工具和解決方案更輕鬆地支援 MSIX 生態系統。 若要深入了解 MSIX 封裝格式，請參閱 [MSIX SDK](https://github.com/Microsoft/msix-packaging)。 

![MSIX 封裝影像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>具可執行程式碼的選用套件

您應用程式中的選擇性套件現在可以包含可執行檔 C# 程式碼。 [了解如何使用 Visual Studio 設定選擇性附加元件套件，以支援您的主要應用程式套件。](/windows/msix/package/optional-packages)

### <a name="page-transitions"></a>頁面轉換

[頁面轉換](../design/motion/page-transitions.md)可在應用程式中的頁面之間導覽使用者。 它們可以幫助使用者了解他們在導覽階層架構中的位置，並提供有關頁面之間關聯性的意見反應。

### <a name="project-rome"></a>Project Rome

Project Rome 小組已大幅調整其 iOS 和 Android SDK，新增使用者活動等新功能及重構大部分的程式碼，以提供橫跨不同 SDK 的一致程式設計體驗。 [所有新的 API 參考和操作說明文件](/windows/project-rome/)都會在 Build 2018 開發人員大會期間上線。

### <a name="sets"></a>Sets

Sets 功能適用於 Windows 測試人員預覽組建。 使用 Sets 功能時，您的應用程式會繪製成可能與其他應用程式共用的視窗，而每個應用程式在標題列中都有自己的索引標籤。 

## <a name="developer-guidance"></a>開發人員指引

### <a name="get-started"></a>開始使用

我們已使用新的學習途徑來重振我們的開始使用內容。 這些新主題旨在為新的 Windows 10 開發人員提供其想完成的一些常見工作的相關資訊。 這些主題不是教學課程，並不會提供隨手可得的逐步解說，但會指出現有文件的位置及其使用方式。 請查看全新面貌的[開始撰寫程式碼](../get-started/create-uwp-apps.md)頁面，或瀏覽每個個別的學習途徑：

* [建構表單](../get-started/construct-form-learning-track.md)
* [在清單中顯示客戶](../get-started/display-customers-in-list-learning-track.md)
* [儲存和載入設定](../get-started/settings-learning-track.md)
* [使用檔案](../get-started/fileio-learning-track.md)

![開始使用影像](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>廣告績效報告

合作夥伴中心的[廣告績效報告](../publish/advertising-performance-report.md)現在提供可見性計量。 我們也新增了[最佳化您的廣告單元的可見性](../monetize/optimize-ad-unit-viewability.md)一文，以提供最佳化廣告可見性的建議。

### <a name="targeted-push-notifications"></a>目標式推播通知

合作夥伴中心的[通知](../publish/send-push-notifications-to-your-apps-customers.md)頁面現在針對圖形和世界地圖檢視中的所有通知，提供額外的分析資料。

## <a name="videos"></a>視訊

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT 是撰寫與使用 Windows 執行階段 API 的新方法。 它僅在標頭檔案中實作，以及設計用來提供您現代化應用程式功能的第一級存取。 [觀看影片](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)以了解其運作方式，然後[閱讀開發人員文件](../cpp-and-winrt-apis/index.md)以取得詳細資訊。

### <a name="multi-instance-uwp-apps"></a>多執行個體的 UWP 應用程式

Windows 現在允許您執行 UWP 應用程式的多個執行個體，而每個執行個體在其自己的個別處理序中執行。 [觀看影片](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)以了解如何建立支援這項功能的新應用程式，然後[閱讀開發人員文件](../launch-resume/multi-instance-uwp.md)，以取得如何及為何使用這項功能的詳細指引。

## <a name="samples"></a>範例

### <a name="customer-database-tutorial"></a>客戶資料庫教學課程

本教學課程會建立一個基本 UWP 應用程式，以供管理客戶清單，以及介紹企業應用程式開發的實用概念和做法。 它會逐步引導您實作 UI 元素並新增本機 SQLite 資料庫的作業，以及提供連線到遠端 REST 資料庫的簡易指引 (如果您想要進一步了解)。 [請查看這裡的教學課程](../enterprise/customer-database-tutorial.md)