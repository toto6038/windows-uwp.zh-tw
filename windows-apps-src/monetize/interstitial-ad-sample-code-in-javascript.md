---
author: Xansky
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: 了解如何使用 JavaScript/HTML 啟動插播式廣告。
title: 使用 JavaScript 的插播式廣告範例程式碼
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, 插播式, javascript, 範例程式碼
ms.localizationpriority: medium
ms.openlocfilehash: 42ff810808e0e7b8d83152f7d23e535c721d3405
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5558929"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>使用 JavaScript 的插播式廣告範例程式碼

本主題提供顯示插播式廣告的基本 JavaScript 和 HTML 通用 Windows 平台 (UWP) 應用程式的完整範例程式碼。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插播式廣告](interstitial-ads.md)。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="code-example"></a>程式碼範例

本節顯示的內容為顯示插播式廣告的基本應用程式中的 HTML 和 JavaScript 檔的內容。 若要使用這些範例，請將此程式碼複製到 Visual Studio 中 JavaScript 的 **「WinJS 應用程式 (通用 Windows)」** 專案中。

此範例應用程式使用兩個按鈕來要求然後啟動插播式廣告。 由 Visual Studio 產生的 main.js 和 index.html 檔案已經過修改並顯示於下方。 下方顯示的 script.js 檔包含範例中的大部分程式碼，您應將此檔案新增到專案中的 **js** 資料夾。

使用 Windows 開發人員中心的實際值取代 ```applicationId``` 和 ```adUnitId``` 變數的值，再提交應用程式到 Microsoft Store。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要變更此範例以顯示插播式橫幅廣告，而不是插播式影片廣告，請將值 **InterstitialAdType.display** 傳遞到 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一個參數，而不是 **InterstitialAdType.video**。 如需詳細資訊，請參閱[插播式廣告](interstitial-ads.md)。

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 
