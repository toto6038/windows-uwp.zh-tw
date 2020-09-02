---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: 瞭解如何使用以 JavaScript 和 HTML 撰寫的通用 Windows 平臺 (UWP) 應用程式來啟動插入式 ad。
title: 使用 JavaScript 的插播式廣告範例程式碼
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, 插播式, javascript, 範例程式碼
ms.localizationpriority: medium
ms.openlocfilehash: 07a61bfde1fc6a77f87617ee553408c71f79cbc3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363631"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>使用 JavaScript 的插播式廣告範例程式碼

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

本主題提供顯示插播式廣告的基本 JavaScript 和 HTML 通用 Windows 平台 (UWP) 應用程式的完整範例程式碼。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插播式廣告](interstitial-ads.md)。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)。

## <a name="code-example"></a>程式碼範例

本節顯示的內容為顯示插播式廣告的基本應用程式中的 HTML 和 JavaScript 檔的內容。 若要使用這些範例，請將此程式碼複製到 Visual Studio 中 JavaScript 的 **「WinJS 應用程式 (通用 Windows)」** 專案中。

此範例應用程式使用兩個按鈕來要求然後啟動插播式廣告。 由 Visual Studio 產生的 main.js 和 index.html 檔案已經過修改並顯示於下方。 下方顯示的 script.js 檔包含範例中的大部分程式碼，您應將此檔案新增到專案中的 **js** 資料夾。

將和變數的值取代為 ```applicationId``` ```adUnitId``` 合作夥伴中心的即時值，然後再將您的應用程式提交至商店。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要變更此範例以顯示插播式橫幅廣告，而不是插播式影片廣告，請將值 **InterstitialAdType.display** 傳遞到 [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一個參數，而不是 **InterstitialAdType.video**。 如需詳細資訊，請參閱[插播式廣告](interstitial-ads.md)。

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
:::code language="html" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/index.html" range="1-21":::

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="script":::

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/main.js" id="main":::

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 
