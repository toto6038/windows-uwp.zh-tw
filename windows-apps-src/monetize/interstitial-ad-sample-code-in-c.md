---
author: Xansky
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: 了解如何使用 C# 啟動插播式廣告。
title: 使用 C# 的插播式廣告範例程式碼
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 廣告, 廣告, 插播式, c#, 範例程式碼
ms.localizationpriority: medium
ms.openlocfilehash: 195f13d3a51925925d320b87cd0142d14d449226
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5405387"
---
# <a name="interstitial-ad-sample-code-in-c"></a>使用 C\# 的插播式廣告範例程式碼 #  

本主題提供顯示插播式影片廣告的基本 C# 和 XAML 通用 Windows 平台 (UWP) 應用程式的完整範例程式碼。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插播式廣告](interstitial-ads.md)。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="code-example"></a>程式碼範例

本節顯示的內容為顯示插播式廣告的基本應用程式中的 MainPage.xaml 和 MainPage.xaml.cs 檔的內容。 若要使用這些範例，請將程式碼複製到 Visual Studio 中 Visual C# 的 **「空白應用程式 (通用 Windows)」** 專案中。

此範例應用程式使用兩個按鈕來要求然後啟動插播式廣告。 使用 Windows 開發人員中心的實際值取代 ```myAppId``` 和 ```myAdUnitId``` 欄位的值，再提交應用程式到 Microsoft Store。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要變更此範例以顯示插播式橫幅廣告，而不是插播式影片廣告，請將值 **AdType.Display** 傳遞到  [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一個參數，而不是 **AdType.Video**。 如需詳細資訊，請參閱[插播式廣告](interstitial-ads.md)。

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
 
