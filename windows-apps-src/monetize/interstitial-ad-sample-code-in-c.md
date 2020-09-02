---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: '瞭解如何使用 c # 和 XAML 在通用 Windows 平臺 (UWP) 應用程式中顯示和啟動插入式 ad。'
title: 使用 C# 的插播式廣告範例程式碼
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, 插播式, c#, 範例程式碼
ms.localizationpriority: medium
ms.openlocfilehash: 3d80e53fd8d122e01588ffcbd16f487e1526c08d
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363681"
---
# <a name="interstitial-ad-sample-code-in-c"></a>C 中的插入式 ad 範例程式碼\# #  

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

本主題提供顯示插播式影片廣告的基本 C# 和 XAML 通用 Windows 平台 (UWP) 應用程式的完整範例程式碼。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插播式廣告](interstitial-ads.md)。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)。

## <a name="code-example"></a>程式碼範例

本節顯示的內容為顯示插播式廣告的基本應用程式中的 MainPage.xaml 和 MainPage.xaml.cs 檔的內容。 若要使用這些範例，請將程式碼複製到 Visual Studio 中 Visual C# 的 **「空白應用程式 (通用 Windows)」** 專案中。

此範例應用程式使用兩個按鈕來要求然後啟動插播式廣告。 將和欄位的值取代為 ```myAppId``` ```myAdUnitId``` 合作夥伴中心的即時值，然後再將您的應用程式提交至商店。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要變更此範例以顯示插播式橫幅廣告，而不是插播式影片廣告，請將值 **AdType.Display** 傳遞到  [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一個參數，而不是 **AdType.Video**。 如需詳細資訊，請參閱[插播式廣告](interstitial-ads.md)。

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
:::code language="xml" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml" range="1-13":::

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="CompleteSample":::

 
## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
