---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "了解如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 程式庫，在 Windows 10、Windows 8.1 或 Windows Phone 8.1 應用程式中包含插播式廣告。"
title: "插播式廣告"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: fae0fc57eca3477bf46a6f3ac43ec35781241a6e

---

# <a name="interstitial-ads"></a>插播式廣告

本逐步解說示範如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 程式庫，在 Windows 10、Windows 8.1 或 Windows Phone 8.1 應用程式中包含插播式廣告。

如需示範如何使用 C# 和 C++ 將插播式廣告新增到 JavaScript/HTML 應用程式及 XAML 應用程式的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>什麼是插播式廣告？

插播式廣告 (或「插頁廣告」**) 和橫幅廣告不同，會在應用程式的整個畫面上顯示。 它們在遊戲中經常以兩種基本的格式呈現。

* 以「付費牆」**廣告呈現，使用者必須在特定間隔觀賞廣告。 例如在遊戲關卡之間：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 以「獎勵式」**廣告呈現，使用者將會明確地尋求益處 (例如尋求完成關卡的提示或額外時間)，並透過應用程式的使用者介面初始化廣告影片。

    請務必注意，此 SDK 除了在播放影片期間，並不會處理任何使用者介面。 當您考慮如何將插播式廣告整合到應用程式時，請參閱[插播式廣告最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)，取得應該執行和應該避免之事項的指導方針。

## <a name="build-an-app-with-interstitial-ads"></a>建置具有插播式廣告的應用程式

若要在您的應用程式中顯示插播式廣告，請遵循專案類型的指示：

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX 互通性)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>必要條件

* UWP 應用程式︰請安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 與 Visual Studio 2015。
* Windows 8.1 或 Windows Phone 8.1 應用程式：請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 與 Visual Studio 2015 或 Visual Studio 2013。

<span id="interstitialadsxaml10"/>
###<a name="xamlnet"></a>XAML/.NET

本節提供 C# 範例，但同時對於 XAML/.NET 專案也支援 Visual Basic 和 C++。 如需完整的 C# 程式碼範例，請參閱[使用 C# 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-c.md)。

1. 在 Visual Studio 中，開啟您的專案。

2. 在 [參考管理員]**** 中，根據您的專案類型選取下列其中一項參考︰

  * 通用 Windows 平台 (UWP) 專案：展開 \[通用 Windows\]****，按一下 \[擴充功能\]****，然後選取 \[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]**** 旁邊的核取方塊。

  * Windows 8.1 專案：展開 \[Windows 8.1\]****，按一下 \[擴充功能\]****，然後選取 \[適用於 Windows 8.1 XAML 的 Ad Mediator SDK\]**** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

  * Windows Phone 8.1 專案：展開 \[Windows Phone 8.1\]****，按一下 \[擴充功能\]****，然後選取 \[適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK\]**** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

3.  在您的應用程式的適當程式碼檔案中 (例如在 MainPage.xaml.cs 中，或部分其他頁面的程式碼檔案)，新增下列命名空間參考。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  在您的應用程式的適當位置中 (例如，在 ```MainPage``` 中或部分其他頁面)，為您的插播式廣告宣告 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 物件和代表應用程式識別碼和單位識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位，以測試[測試模式值](test-mode-values.md) 中提供的值。 這些值僅用於測試；您必須在發佈應用程式之前以 Windows 開發人員中心的實際值取代這些值。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件的事件連結事件處理常式。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  在您需要起始廣告前約 30-60 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

6.  在您要顯示廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法顯示廣告。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  為 **InterstitialAd** 物件定義事件處理常式。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="interstitialadshtml10"/>
###<a name="htmljavascript"></a>HTML/JavaScript

以下指示假設您已在 Visual Studio 2015 中建立 JavaScript 的通用 Windows 專案，並且是針對特定的 CPU。 如需完整的程式碼範例，請參閱[使用 JavaScript 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-javascript.md)。

1. 在 Visual Studio 中，開啟您的專案。

2.  在 [參考管理員]**** 中，根據您的專案類型選取下列其中一項參考︰

  * 通用 Windows 平台 (UWP) 專案：展開 [通用 Windows]****，按一下 [擴充功能]****，然後選取 [適用於 JavaScript 的 Microsoft Advertising SDK (Version 10.0)]**** 旁邊的核取方塊。

  * Windows 8.1 專案：展開 [Windows 8.1]****，按一下 [擴充功能]****，然後選取 [適用於 Windows 8.1 Native 的 Microsoft Advertising SDK (JS)]**** 旁邊的核取方塊。

  * Windows 8.1 專案：展開 [Windows Phone 8.1]****，按一下 [擴充功能]****，然後選取 [Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)]**** 旁邊的核取方塊。

3.  在專案的 HTML 檔案的 **&lt;head&gt;** 區段中，在專案的 default.css 和 default.js JavaScript 參考後面新增 ad.js 的參考。 在 UWP 專案中，新增下列參考。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  在 Windows 8.1 或 Windows Phone 8.1 專案中，新增下列參考。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  在您專案的 .js 檔案中，為您的插播式廣告宣告 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 物件和包含應用程式識別碼和單位識別碼的幾個欄位。 以下程式碼範例指派 `applicationId` 和 `adUnitId` 欄位，以測試[測試模式值](test-mode-values.md) 中提供的值。 這些值僅用於測試；您必須在發佈應用程式之前以 Windows 開發人員中心的實際值取代這些值。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件連結事件處理常式。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. 在您需要起始廣告前約 30-60 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

6.  在您要顯示廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法顯示廣告。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  為 **InterstitialAd** 物件定義事件處理常式。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="interstitialadsdirectx10"/>
###<a name="c-directx-interop"></a>C++ (DirectX 互通性)

此範例假設您已在 Visual Studio 2015 中建立 C++ **DirectX 和 XAML 應用程式 (通用 Windows)** 專案，並且是針對特定的 CPU 架構。
 
1. 在 Visual Studio 中，開啟您的專案。

1.  在 [參考管理員]**** 中，根據您的專案類型選取下列其中一項參考︰

  * 通用 Windows 平台 (UWP) 專案：展開 \[通用 Windows\]****，按一下 \[擴充功能\]****，然後選取 \[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]**** 旁邊的核取方塊。

  * Windows 8.1 專案：展開 \[Windows 8.1\]****，按一下 \[擴充功能\]****，然後選取 \[適用於 Windows 8.1 XAML 的 Ad Mediator SDK\]**** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

  * Windows Phone 8.1 專案：展開 \[Windows Phone 8.1\]****，按一下 \[擴充功能\]****，然後選取 \[適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK\]**** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

2.  在應用程式適當的標頭檔 (例如，DirectXPage.xaml.h) 中，宣告 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 物件和相關的事件處理常式方法。  

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  在相同標頭檔中，為您的插播式廣告宣告代表應用程式識別碼和單位識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位，以測試[測試模式值](test-mode-values.md) 中提供的值。 這些值僅用於測試；您必須在發佈應用程式之前以 Windows 開發人員中心的實際值取代這些值。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  在您要新增程式碼以顯示插播式廣告的 .cpp 檔中，新增下列命名空間參考。 下列範例假設您在應用程式中新增程式碼到 DirectXPage.xaml.cpp 檔案中。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件的事件連結事件處理常式。 在下列範例中，```InterstitialAdSamplesCpp``` 是您專案的命名空間，依您程式碼的需要變更此名稱。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. 在您需要起始廣告前約 30-60 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

7.  在您要顯示廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法顯示廣告。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  為 **InterstitialAd** 物件定義事件處理常式。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. 建置並測試您的應用程式以確認它會顯示測試廣告。

<span/>
### <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 開發人員中心發行包含即時廣告的應用程式

1.  在開發人員中心儀表板中，移至應用程式的 [營利]**** &gt; [利用廣告營利]**** 頁面，並[建立廣告單元](../publish/monetize-with-ads.md)。 針對廣告單位類型，指定 \[插播式影片\]****。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值，用在開發人員中心產生的實際值取代。

3.  使用 Windows 開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md) 到市集。

4.  在「開發人員中心」儀表板上檢閱您的[廣告績效報告](../publish/advertising-performance-report.md)。

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>插播式廣告最佳做法與原則


如需有關如何有效地使用插播式廣告及必需遵守之原則的詳細資訊，請參閱[插播式廣告最佳做法與原則](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>移除參考錯誤：以特定 CPU 平台為目標 (XAML 和 HTML)


使用 Microsoft Advertising 程式庫時，您在專案中將無法以 \[任何 CPU\]**** 為目標。 如果專案的目標是 \[任何 CPU\]****，當您將參照新增到 Microsoft Advertising 程式庫之後，便可能會在專案中看到警告。 如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 如需詳細資訊，請參閱[已知問題](known-issues-for-the-advertising-libraries.md)。

## <a name="related-topics"></a>相關主題


* [使用 C 的插播式廣告範例程式碼#](interstitial-ad-sample-code-in-c.md)
* [使用 JavaScript 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的廣告範例](http://aka.ms/githubads)

 

 



<!--HONumber=Dec16_HO2-->


