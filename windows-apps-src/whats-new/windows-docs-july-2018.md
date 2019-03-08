---
title: 在 Windows 文件中於 2018 年 7 月新消息-開發 UWP 應用程式
description: 新功能、 影片、 範例和開發人員指引已新增至 2018 年 7 月的 Windows 10 開發人員文件。
keywords: 新功能、 更新、 功能、 開發人員指導方針，Windows 10 年 7 月
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22a3a9614a4488791a36f81a3d4dedac572111b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596833"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>Windows 開發人員文件在 2018 年 7 月新消息

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列的功能概觀、 開發人員指南、 影片和範例已在 7 月份中，您可以使用。

在 Windows 10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="progressive-web-apps-on-windows"></a>在 Windows 上的漸進式 Web 應用程式

[漸進式 Web 應用程式 (Pwa)](https://developer.microsoft.com/windows/pwa)都只是 web 應用程式[漸進式增強](https://wikipedia.org/wiki/Progressive_enhancement)上支援的平台和瀏覽器引擎，例如啟動從 homescreen 安裝，離線的原生類似應用程式的功能支援，並推播通知。 使用 Microsoft Edge (EdgeHTML) 引擎的 Windows 10，Pwa 享有額外的執行的優點，[獨立於瀏覽器視窗中，為 UWP 應用程式。](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Pwa 的作用中映像](images/progressive-web-apps.jpg)

請參閱我們的 PWA 指南，以：

* [PWA 的建置簡單的 web 應用程式](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [增強您的 Windows 執行階段的 PWA](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [將您的 PWA 發佈到 Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>記事本

在 Windows 10 Insider Preview 組建 17713，可用[許多新功能已更新 「 記事本 」](https://aka.ms/ant-man)。 縮放、 循環尋找/取代，以及支援 Unix/Linux (LF) 和 Mac (CR) 行尾結束符號現在都可[Windows 測試人員](https://insider.windows.com/)。 

## <a name="developer-guidance"></a>開發人員指引

### <a name="design-landing-page"></a>設計登陸頁面

請參閱[更新登陸頁面的設計](https://developer.microsoft.com/windows/apps/design)一眼概略 UWP 設計區域和 Fluent 設計的最新新增項目上的資訊。

### <a name="design-toolkits"></a>設計工具組

Adobe XD 和 Adobe Illustrator 的工具組已更新的新功能。 這些設計工具組提供設計 UWP 應用程式的控制項和版面配置範本。 [查看這裡。](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

我們已新增數個新的主題，來[WebVR 文件](https://docs.microsoft.com/microsoft-edge/webvr/
):

* [什麼是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) 說明何謂 WebVR、 為什麼您應該使用它，以及如何開始開發它。

* [漸進式 Web 應用程式中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas):了解如何新增 WebVR 漸進式 Web App (PWA)。

* [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview):了解如何在 Windows 10 應用程式的 WebView 控制項中加入 WebVR。

* [WebVR 示範](https://docs.microsoft.com/microsoft-edge/webvr/demos):請參閱使用 Microsoft Edge 和 Windows Mixed Reality 沈浸式耳機某些 WebVR 示範。

此外，我們做了一些更新至現有的頁面：

* 目錄現在更被分成四個不同的最上層值區：**基本概念**，**開發**，**資源**，和**示範**。

* [WebVR 開發人員指南 （登陸頁面）](https://docs.microsoft.com/microsoft-edge/webvr/):外觀與風格，較大的影像和圖示與新的示範。

* [使用 Microsoft Edge WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge):已更新為包含資訊的 Windows 10 April 2018 Update。

## <a name="videos"></a>影片

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開始進行開發：建立和自訂 Windows 10 上的表單

我們[開始使用 docs](../get-started/index.md)的 Windows 開發人員現在提供實際操作經驗與基本應用程式開發工作。 這段影片將逐步引導您透過這些主題，其中，並說明如何在您的應用程式中建立表單 UI 的基本概念。 [觀看影片](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看程式碼的動作，然後[自行查看主題。](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增強您的機器人專案個性交談

專案的特質交談可讓您將可自訂的角色新增至您的聊天機器人。 藉由整合 Microsoft Bot Framework sdk，您可以新增更交談式的方法，來與客戶互動的小型明瞭功能。 [觀看影片](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)以了解如何實作它，然後[試用互動式示範](https://aka.ms/PersonalityChat)實際操作體驗。

### <a name="one-dev-question"></a>一個開發人員問題

在一個開發人員問題影片系列中，資深的 Microsoft 開發人員會討論一系列有關 Windows 開發、 team 文化特性和歷程記錄的問題。 以下是我們所回答的最新問題 ！

Raymond Chen:

* [為何您未套用至 Microsoft？](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [我們為什麼不讓開發人員變更預設音訊裝置？](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [為何會這麼多 UWP 函式的非同步？](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>範例

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + /cli WinRT

Photo Editor 範例應用程式會展示使用開發[C + + /cli WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)語言投影。 應用程式可讓您擷取相片**圖片**程式庫，然後編輯選定的映像相關聯的相片效果。 [複製或下載的範例。](https://github.com/Microsoft/Windows-appsample-photo-editor)

![作用中範例的範例](images/photo-editor-banner.png)
