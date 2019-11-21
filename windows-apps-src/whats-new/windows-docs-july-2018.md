---
title: 2018 年 7 月 Windows 文件的新增功能 - 開發 UWP 應用程式
description: 新功能、影片、範例及開發人員指引已加入 2018 年 7 月的 Windows 10 開發人員文件中。
keywords: 新增功能, 更新, 功能, 開發人員指引, Windows 10, 7 月
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c92de1901c9cc67a30f495e34289bac5d7d2b94
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258800"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>2018 年 7 月 Windows 開發人員文件的新增功能

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、開發人員指引、影片和範例已在 7 月份提供使用。

在 Windows 10 上[安裝工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="progressive-web-apps-on-windows"></a>Windows 上的漸進式 Web 應用程式

[漸進式 Web 應用程式 (PWA)](https://developer.microsoft.com/windows/pwa) 只是在支援平台和瀏覽器引擎上，使用類似原生應用程式功能 (例如從主螢幕安裝啟動、離線支援與推播通知) [漸進式增強](https://www.wikipedia.org/wiki/Progressive_enhancement)的 Web 應用程式。 在使用 Microsoft Edge (EdgeHTML) 引擎的 Windows 10上，PWA 享有額外的優點，即[作為 UWP 應用程式獨立於瀏覽器視窗以外執行](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)。

![作用中 PWA 的影像](images/progressive-web-apps.jpg)

請查看我們的 PWA 指南，以：

* [建置簡單的 Web 應用程序作為 PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started) \(英文\)
* [使用 Windows 執行階段增強您的 PWA](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features) \(英文\)
* [將您的 PWA 發佈到 Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store) \(英文\)

### <a name="notepad"></a>記事本

Windows 10 Insider Preview Build 17713 中已提供使用，[[記事本] 已更新許多新功能](https://blogs.windows.com/windowsexperience/2018/07/11/announcing-windows-10-insider-preview-build-17713/)。 [Windows 測試人員](https://insider.windows.com/)現在可以使用縮放、循環尋找/取代，以及 Unix/Linux (LF) 和 Mac (CR) 行尾結束符號的支援。 

## <a name="developer-guidance"></a>開發人員指引

### <a name="design-landing-page"></a>設計登陸頁面

請參閱[更新的設計登陸頁面](https://developer.microsoft.com/windows/apps/design)，以取得 UWP 設計區域以及有關 Fluent Design 最新新增項目的資訊概述。

### <a name="design-toolkits"></a>設計工具組

Adobe XD 和 Adobe Illustrator 工具組已更新了新功能。 這些設計工具組提供控制項與版面配置範本，可用於設計 UWP 應用程式。 [試用看看](../design/downloads/index.md)。

### <a name="webvr"></a>WebVR

我們在 [WebVR 文件](https://docs.microsoft.com/microsoft-edge/webvr/)中新增了幾個新主題：

* [什麼是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) \(英文\) 說明何謂 WebVR、為什麼您應該使用它，以及如何開始針對它進行開發。

* [漸進式 Web 應用程式中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas) \(英文\)：了解如何將 WebVR 新增至漸進式 Web 應用程式 (PWA)。

* [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview) \(英文\)：了解如何將 WebVR 新增至 Windows 10 應用程式中的 WebView 控制項。

* [WebVR 示範](https://docs.microsoft.com/microsoft-edge/webvr/demos) \(英文\)：請參閱一些使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式頭戴裝置的 WebVR 示範。

此外，我們對現有的頁面進行了一些更新：

* 現在，目錄更妥善地分成四個不同的最上層貯體：**基本概念**、**開發**、**資源**和**示範**。

* [WebVR 開發人員指南 (登陸頁面)](https://docs.microsoft.com/microsoft-edge/webvr/) \(英文\)：更新的外觀與風格，包含較大的影像和圖示與新的示範。

* [將 WebVR 與 Microsoft Edge 搭配使用](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge) \(英文\)：已更新，其中包含有關 Windows 10 2018 年 4 月更新的資訊。

## <a name="videos"></a>影片

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開發人員入門：在 Window 10 上建立和自訂表單

我們針對 Windows 開發人員的[入門文件](../get-started/index.md)現在提供了基本應用程式開發工作的實際操作經驗。 這段影片將向您逐步說明其中一個主題，並說明如何在您的應用程式中建立表單 UI 的基本概念。 [觀看影片](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看實際操作的程式碼，然後[自行查看主題](https://docs.microsoft.com/windows/uwp/get-started/construct-form-learning-track) \(部分機器翻譯\)。

### <a name="enhance-your-bot-with-project-personality-chat"></a>使用個人化聊天專案增強您的機器人

個人化聊天專案允許您為聊天機器人新增可自訂的角色。 藉由與 Microsoft Bot Framework SDK 整合，您可以新增閒聊功能，以更交談式的方式與客戶互動。 [觀看影片](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)以了解如何實作它，然後[試用互動式示範](https://www.microsoft.com/research/project/personality-chat/)以取得實際操作體驗。

### <a name="one-dev-question"></a>One Dev Question

在 One Dev Question 影片系列中，資深的 Microsoft 開發人員會談論一系列關於 Windows 開發、團隊文化和歷史的問題。 以下是我們所回答的最新問題！

Raymond Chen：

* [您為什麼向 Microsoft 進行申請？](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman：

* [為什麼我們不讓開發人員變更預設音訊裝置？](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [為什麼有這麼多非同步的 UWP 函式？](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>範例

### <a name="photo-editor-cwinrt"></a>Photo Editor C++/WinRT

Photo Editor 範例應用程式展示使用 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 語言投影進行開發。 該應用程式可讓您從**圖片**庫擷取相片，然後以關聯的相片效果編輯所選的影像。 [在此處複製或下載範例](https://github.com/Microsoft/Windows-appsample-photo-editor)。

![作用中範例的範例](images/photo-editor-banner.png)
