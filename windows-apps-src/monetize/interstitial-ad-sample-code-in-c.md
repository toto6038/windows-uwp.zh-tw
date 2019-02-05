---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: 了解如何使用 C# 啟動插播式廣告。
title: 使用 C# 的插播式廣告範例程式碼
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, 插播式, c#, 範例程式碼
ms.localizationpriority: medium
ms.openlocfilehash: 075d98d49ba7e878abc7e800af84984bdb93e3a2
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/04/2019
ms.locfileid: "9045084"
---
# <a name="interstitial-ad-sample-code-in-c"></a>使用 C\# 的插播式廣告範例程式碼 #  

本主題提供顯示插播式影片廣告的基本 C# 和 XAML 通用 Windows 平台 (UWP) 應用程式的完整範例程式碼。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插播式廣告](interstitial-ads.md)。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](https://aka.ms/githubads)。

## <a name="code-example"></a>程式碼範例

本節顯示的內容為顯示插播式廣告的基本應用程式中的 MainPage.xaml 和 MainPage.xaml.cs 檔的內容。 若要使用這些範例，請將程式碼複製到 Visual Studio 中 Visual C# 的 **「空白應用程式 (通用 Windows)」** 專案中。

此範例應用程式使用兩個按鈕來要求然後啟動插播式廣告。 取代的值```myAppId```和```myAdUnitId```欄位，提交您的應用程式到市集之前從合作夥伴中心的實際值。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要變更此範例以顯示插播式橫幅廣告，而不是插播式影片廣告，請將值 **AdType.Display** 傳遞到  [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一個參數，而不是 **AdType.Video**。 如需詳細資訊，請參閱[插播式廣告](interstitial-ads.md)。

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](https://aka.ms/githubads)
 
