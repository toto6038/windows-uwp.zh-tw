---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: "了解如何使用 C# 啟動插播式廣告。"
title: "使用 C 的插播式廣告範例程式碼#"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: c7554b94e67ce7f4b83a9ad4360819881d09f0fb

---

# <a name="interstitial-ad-sample-code-in-c"></a>使用 C 的插播式廣告範例程式碼\# #  

本主題提供顯示插播式廣告的基本 C# 和 XAML 通用 Windows 平台 (UWP) 應用程式的完整範例程式碼。 如需示範如何設定專案以使用此程式碼的逐步指示，請參閱[插播式廣告](interstitial-ads.md)。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="code-example"></a>程式碼範例

本節顯示的內容為顯示插播式廣告的基本應用程式中的 MainPage.xaml 和 MainPage.xaml.cs 檔的內容。 若要使用這些範例，請將程式碼複製到 Visual Studio 2015 中 Visual C# 的「空白應用程式 (通用 Windows)」****專案中。

此範例應用程式使用兩個按鈕來要求然後啟動插播式廣告。 使用 Windows 開發人員中心的實際值取代 ```myAppId``` 和 ```myAdUnitId``` 欄位的值，再提交應用程式到市集。 如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md)。

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
 



<!--HONumber=Dec16_HO2-->


