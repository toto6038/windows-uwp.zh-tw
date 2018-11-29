---
title: 2017 年 7 月 Windows 文件的最新動向 - 開發 UWP app
description: 新功能、範例及開發人員指引已加入 2017 年 7 月的 Windows 10 開發人員文件中
keywords: 最新動向, 更新, 功能, 開發人員指引, Windows 10
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dd00d1aba0e6a58cd2128c90b2c7f714d3f8f802
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8198912"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>2017 年 7 月 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 最近已有下列功能概觀、開發人員指引和程式碼範例可以使用，包含提供給 Windows 開發人員的全新及更新資訊。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/your-first-app.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="fluent-design"></a>Fluent 設計

SDK 預覽組建中有新的效果提供給 [Windows 測試人員](https://insider.windows.com/)，這些效果使用深度、透視和動作來協助使用者專注於重要的 UI 項目。

[壓克力材質](../design/style/acrylic.md)是一種建立透明紋理的筆刷。 

![在淺色佈景主題中的壓克力](../design/style/images/Acrylic_DarkTheme_Base.png)

[視差效果](../design/motion/parallax.md)可為您的應用程式增加立體深度及透視效果。

![一個帶有背景影像和清單的視差範例](../design/style/images/_Parallax_v2.gif)

[顯色](../design/style/reveal.md)會顯目提示您應用程式中的重要項目。 

![顯色視覺效果](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>UI 控制項

SDK 預覽組建中有新的控制項提供給 [Windows 測試人員](https://insider.windows.com/)，這些控制項可以更輕鬆地用來快速建置外觀出色的 UI。

[色彩選擇器控制項](../design/controls-and-patterns/color-picker.md)可讓使用者瀏覽及選取色彩。  

![預設色彩選擇器](../design/controls-and-patterns/images/color-picker-default.png)

[導覽檢視控制項](../design/controls-and-patterns/navigationview.md)可讓您將頂層導覽功能輕鬆的加入到您的應用程式之中。

![NavigationView 區塊](../design/controls-and-patterns/images/navview_sections.png)

[個人圖片控制項](../design/controls-and-patterns/person-picture.md)會為個人顯示圖片影像。

![個人圖片控制項](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

[評分控制項](../design/controls-and-patterns/rating.md)可讓使用者輕鬆的檢視及設定評分，以反映其對內容和服務的滿意程度。

![評分控制項的範例](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>設計工具組

[UWP app 的設計工具組和資源](../design/downloads/index.md)已藉由新增 Sketch 及 Adobe XD 工具組進行擴充。 先前既有的工具組也已更新並修訂，為您的 UWP app 提供更強固的控制項與版面配置範本。

### <a name="dashboard-monetization-and-store-services"></a>儀表板、創造營收和市集服務

現已提供下列新功能：

* Microsoft Advertising SDK 現已可讓您在應用程式中顯示[原生廣告](../monetize/native-ads.md)。 原生廣告是以元件為基礎的廣告格式，其中每一項廣告創意 (例如標題、影像、描述和喚起行動文字) 都會當做個別元素傳送到您的應用程式。 原生廣告則目前僅供加入試驗計劃的開發人員使用，但我們很快就要將這項功能提供給所有的開發人員。

* [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) 現在提供一個方法，您可用來[下載 CAB 檔案以取得 App 中的錯誤](../monetize/download-the-cab-file-for-an-error-in-your-app.md)。

* [針對性優惠](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)讓您以特定客戶區隔為目標，透過有吸引力的個人化內容提高吸引力、留住客戶並創造營收。 

* App 的市集清單現在可以包含[預告片](../publish/app-screenshots-and-images.md#trailers)。

* 新的價格與可用性選項可讓您[排程價格變更](../publish/set-and-schedule-app-pricing.md)和[設定精確的發行日期](..//publish/configure-precise-release-scheduling.md)。

* 您可以[匯入和匯出市集清單](../publish/import-and-export-store-listings.md)來加快更新，尤其是在您的清單有許多語言版本時。

### <a name="my-people"></a>朋友圈

SDK 預覽組建中有即將推出的朋友圈功能提供給 [Windows 測試人員](https://insider.windows.com/)，這項功能可讓使用者將連絡人直接從應用程式中釘選到他們的工作列。 [了解如何將朋友圈支援加入您的應用程式。](../contacts-and-calendar/my-people-support.md)

![朋友圈連絡人面板](images/my-people.png)

[朋友圈分享](../contacts-and-calendar/my-people-sharing.md)可讓使用者透過您的應用程式直接從工作列分享檔案。

![朋友圈分享](images/my-people-sharing.png)

[朋友圈通知](../contacts-and-calendar/my-people-support.md)是一種新的快顯通知，使用者可以將此通知傳送給他們釘選的連絡人。

![朋友圈通知](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>釘選到工作列

SDK 預覽組建中有新的 TaskbarManager 類別提供給 [Windows 測試人員](https://insider.windows.com/)，這個類別可讓您要求使用者[將您的應用程式釘選到工作列](../design/shell/pin-to-taskbar.md)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="media-playback"></a>媒體播放

媒體播放文章[使用 MediaPlayer 播放音訊和視訊](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)中加入了新章節。 [使用 MediaPlayer 播放球面視訊](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)小節告訴您如何播放球面編碼視訊，包括針對支援的格式調整視野及觀看方向。 [以畫面伺服器模式使用 MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) 告訴您如何從使用 [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 播放的媒體將畫面複製到 Direct3D 表面。 這會啟用像使用像素著色器套用即時效果這樣的案例。 範例程式碼示範使用 Win2D 快速實作視訊播放模糊效果。

### <a name="media-capture"></a>媒體擷取

已更新[使用 MediaFrameReader 處理媒體畫面](../audio-video-camera/process-media-frames-with-mediaframereader.md)文章來說明如何使用新的 [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 類別，這個類別可讓您從多個媒體來源取得與時間相互關聯的畫面。 如果您需要處理不同來源的畫面，例如景深相機和彩色攝影機，而您需要確定每個來源的畫面的擷取時間彼此接近，這就會很實用。 如需詳細資訊，請參閱[使用 MultiSourceMediaFrameReader 從多個來源取得與時間相互關聯的畫面](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources)。

### <a name="scoped-search"></a>限定範圍的搜尋

「UWP」範圍已新增至 docs.microsoft.com 上的 [UWP 概念](../get-started/universal-application-platform-guide.md)和 [API 參考](https://docs.microsoft.com/en-us/uwp/api/)文件。 除非停用這個範圍，否則從這些區域中進行的搜尋只會傳回 UWP 文件。

![限定範圍的搜尋](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>針對 Windows 10 S 測試您的 Windows 應用程式

測試您的 Windows 應用程式，以確定此應用程式會在執行 Windows S 的裝置上正常運作。使用[這份新指南](../porting/desktop-to-uwp-test-windows-s.md)了解做法。 

## <a name="samples"></a>範例

### <a name="annotated-audio-app-sample"></a>Annotated audio (標註語音) 應用程式範例

[一個示範語音、筆跡及 OneDrive 資料漫遊案例的迷你 app 範例](https://github.com/Microsoft/Windows-appsample-annotated-audio)。 這個範例程式碼會在允許同步擷取筆跡註解時錄製音訊，讓您稍後可以回憶做筆記時所討論的內容。

![標註語音範例的螢幕擷取畫面](images/Playback.png)  

### <a name="shopping-app-sample"></a>Shopping Cart (購物車) 範例

[一個展示使用者可以購買 Emoji 之基本購物體驗的迷你 app 範例](https://github.com/Microsoft/Windows-appsample-shopping)。 此 app 如何使用[付款要求 API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) 來實作結帳體驗。

![購物應用程式範例的螢幕擷取畫面](images/shoppingcart.png)  

## <a name="videos"></a>影片

### <a name="accessibility"></a>協助工具

將協助工具建置在您的應用程式中，將應用程式推廣至更廣大的客群。 [觀看影片](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility)，然後深入了解[開發無障礙應用程式](https://developer.microsoft.com/en-us/windows/accessible-apps)。

### <a name="payments-request-api"></a>付款要求 API

付款要求 API 可協助客戶和賣方順暢地完成線上結帳流程。 [觀看影片](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API)，然後探索[付款要求文件](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API)。

### <a name="windows-10-iot-core"></a>Windows 10 IoT 核心版

您可以透過 Windows 10 IoT 核心版和通用 Windows 平台，使用真實視覺及元件連線快速設計原型並建置專案，例如這個「寵物辨識門」。 [觀看影片](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core)，然後深入了解如何[開始使用 Windows 10 IoT 核心版](https://developer.microsoft.com/en-us/windows/iot)。