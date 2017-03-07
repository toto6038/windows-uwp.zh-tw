---
author: mcleanbyron
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: "了解如何使用 JavaScript/HTML 啟動插播式廣告。"
title: "使用 JavaScript 的插播式廣告範例程式碼"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, 廣告, 插播式, javascript, 範例程式碼"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 91a54bdd2e41b3e7df0ee0aad32448ab9ed66ac0
ms.lasthandoff: 02/07/2017

---

# <a name="interstitial-ad-sample-code-in-javascript"></a>使用 JavaScript 的插播式廣告範例程式碼

本主題提供顯示插播式廣告的基本 JavaScript 和 HTML 通用 Windows 平台 (UWP) 應用程式的完整範例程式碼。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插播式廣告](interstitial-ads.md)。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="code-example"></a>程式碼範例

本節顯示的內容為顯示插播式廣告的基本應用程式中的 HTML 和 JavaScript 檔的內容。 若要使用這些範例，請將此程式碼複製到 Visual Studio 2015 中 JavaScript 的「WinJS 應用程式 (通用 Windows)」****專案中。

此範例應用程式使用兩個按鈕來要求然後啟動插播式廣告。 由 Visual Studio 產生的 main.js 和 index.html 檔案已經過修改並顯示於下方。 下方顯示的 script.js 檔包含範例中的大部分程式碼，您應將此檔案新增到專案中的 **js** 資料夾。

>**Windows 8.x 和 Windows Phone 8.1 的注意事項**&nbsp;&nbsp;若您的專案目標為 Windows 8.1 或 Windows Phone 8.1，則您專案中的預設 HTML 檔案會命名為 default.html 而非 index.html，且您專案中的預設 JavaScript 檔案會命名為 default.js 而非 main.js。

使用 Windows 開發人員中心的實際值取代 ```applicationId``` 和 ```adUnitId``` 變數的值，再提交應用程式到市集。如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md)。

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

<span/>
>**Windows 8.x 和 Windows Phone 8.1 的注意事項**&nbsp;&nbsp;如果您的專案目標為 Windows 8.1 或 Windows Phone 8.1，請以範例中的 ```<script src="/MSAdvertisingJS/ads/ad.js"></script>``` 取代 ```<script src="//Microsoft.Advertising.JavaScript/ad.js"></script>``` 行。

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 

