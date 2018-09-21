---
author: QuinnRadich
title: 在 2018 年 7 月 Windows 文件的最新動向-開發 UWP app
description: 新功能、 影片、 範例及開發人員指引已新增至 2018 年 7 月的 Windows 10 開發人員文件。
keywords: 最新動向，更新，功能，開發人員指引，Windows 10 年 7 月
ms.author: quradic
ms.date: 7/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f41d25fd6757e5d3f80d00de341168de4f34e946
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "4119912"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>在 2018 年 7 月 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、 開發人員指引、 影片及範例已可供年 7 月。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="progressive-web-apps-on-windows"></a>在 Windows 上的漸進式 Web 應用程式

[漸進式 Web 應用程式 (Pwa)](https://developer.microsoft.com/windows/pwa)是只是[逐漸增強](https://wikipedia.org/wiki/Progressive_enhancement)上支援的平台和瀏覽器引擎，例如啟動從 homescreen 安裝、 離線支援和推播的原生應用程式類似的功能與 web 應用程式通知。 在 Windows 10 與 Microsoft Edge (EdgeHTML) 引擎，Pwa 享受正在執行的已新增的優點[考慮為 UWP 應用程式的瀏覽器視窗。](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Pwa 的動作中的影像](images/progressive-web-apps.jpg)

請查看我們 PWA 指南：

* [PWA 與組建的簡單 web 應用程式](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [增強您的 Windows 執行階段 PWA](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [將您 PWA 發佈到 Microsoft 網上商店](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>記事本

適用於 Windows 10 Insider Preview 組建 17713， [「 記事本 」 已更新為使用許多新的功能](http://aka.ms/ant-man)。 縮放、 迴繞尋找/取代和支援的 Unix/Linux (LF) 和 Mac (CR) 行尾目前已提供給[Windows 測試人員](https://insider.windows.com/)。 

## <a name="developer-guidance"></a>開發人員指引

### <a name="design-landing-page"></a>設計登陸頁面

請查看[更新登陸頁面的設計](https://developer.microsoft.com/windows/apps/design)的 UWP 設計區域，以及的 Fluent 設計的最新新增項目在快速概觀。

### <a name="design-toolkits"></a>設計工具組

Adobe XD 和 Adobe Illustrator 工具組已更新的新功能。 這些設計工具組提供控制項與版面配置範本可用於設計 UWP 應用程式。 [查看他們這裡。](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

我們已在[WebVR 文件](https://docs.microsoft.com/microsoft-edge/webvr/
)中新增新的幾個主題：

* [什麼是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) 說明 WebVR 的是，為什麼您應該使用它，以及如何開始使用它進行開發。

* [漸進式 Web 應用程式中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas)： 了解如何新增 WebVR 至漸進式 Web 應用程式 (PWA)。

* [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview)： 了解如何新增 WebVR 至 WebView 控制項，在 Windows 10 應用程式。

* [WebVR 示範](https://docs.microsoft.com/microsoft-edge/webvr/demos)： 請查看某些 WebVR 示範使用 Microsoft Edge 與 Windows Mixed Reality 沉浸式頭戴式裝置。

此外，我們已經有些更新現有的頁面：

* 目錄中現在更清楚地組織為四個不同的最上層值區：**基礎知識**、**開發**、**資源**，以及**示範**。

* [WebVR 開發人員指南 （登陸頁面）](https://docs.microsoft.com/microsoft-edge/webvr/)： 重新整理的外觀及操作方式，較大的影像和圖示與新的示範。

* [使用 WebVR 與 Microsoft Edge](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge)： 更新以包含有關 Windows 10 2018 年更新。

## <a name="videos"></a>影片

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開始使用適用於開發人員： 建立和自訂 Windows 10 上的表單

我們[要開始使用的文件](../get-started/index.md)給 Windows 開發人員現在提供基本的應用程式開發工作的實機操作體驗。 這段影片會逐步引導您透過其中一個這些主題中，並說明在您的應用程式中建立表單的 UI 的基本知識。 [觀看影片](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看動作，然後在程式碼[自行查看本主題。](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增強您使用專案特質聊天 Bot

特質聊天專案可讓您將可自訂的角色新增到您的聊天 bot。 整合至 Microsoft Bot 架構 SDK，您可以新增更多交談的方式與客戶互動的小型通話功能。 若要了解如何實作它，然後[嘗試互動示範](http://aka.ms/PersonalityChat)實機操作體驗，[觀看影片](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)。

### <a name="one-dev-question"></a>開發人員問題

在開發人員問題影片系列，longtime Microsoft 開發人員會討論一系列的 Windows 開發、 小組文化特性和歷程記錄的相關問題。 以下是我們已經回答的最新問題 ！

Raymond Chen:

* [為什麼您未套用至 Microsoft？](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [我們為什麼不讓開發人員變更的預設音訊裝置？](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [為何許多 UWP 函式 async？](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>範例

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + /winrt

Photo Editor 範例應用程式展示使用開發[C + + /winrt](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)語言投影。 應用程式可讓您從**圖片**媒體櫃，擷取相片，然後選擇以編輯影像相關聯的相片效果。 [複製或下載的範例。](https://github.com/Microsoft/Windows-appsample-photo-editor)

![舉例來說，使用中的範例](images/photo-editor-banner.png)
