---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "了解如何使用 Microsoft Advertising 程式庫，在 Windows 10、Windows 8.1 或 Windows Phone 8.1 應用程式中包含插播式廣告。"
title: "插播式廣告"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 廣告, 廣告控制項, 插播式"
ms.openlocfilehash: 16cb78d9419d54a1bd56fb855ca1fad6fedf665a
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2017
---
# <a name="interstitial-ads"></a>插播式廣告

本逐步解說示範如何在通用 Windows 平台 (UWP) App、Windows 10 遊戲，以及 Windows 8.1 或 Windows Phone 8.1 App 中包含插播式廣告。 如需示範如何使用 C# 和 C++ 將插播式廣告新增到 JavaScript/HTML 應用程式及 XAML 應用程式的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>什麼是插播式廣告？

不同於標準橫幅廣告，侷限於 app 或遊戲 UI 的一部分，插播式廣告會在整個畫面上顯示。 它們在遊戲中經常以兩種基本的格式呈現。

* 以「付費牆」**廣告呈現，使用者必須在特定間隔觀賞廣告。 例如在遊戲關卡之間：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 以*「獎勵式」*廣告呈現，使用者將會明確地尋求益處 (例如尋求完成關卡的提示或額外時間)，並透過應用程式的使用者介面初始化廣告。

我們提供兩種類型的插播式廣告，供您的 app 和遊戲使用：

* **插播式影片廣告**：這些適用於 Windows 10 通用 Windows 平台 (UWP) App，以及 Windows 8.1 或 Windows Phone 8.1 App。

* **插播式橫幅廣告**：這些只適用於 Windows 10 的 UWP app。

> [!NOTE]
> 插播式廣告 API 不會處理任何使用者介面，除了在視訊播放時。 當您考慮如何將插播式廣告整合到應用程式時，請參閱[插播式廣告最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)，取得應該執行和應該避免之事項的指導方針。

## <a name="build-an-app-with-interstitial-ads"></a>建置具有插播式廣告的應用程式

若要在您的應用程式中顯示插播式廣告，請遵循專案類型的指示：

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX 互通性)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>先決條件

* 對於 UWP 應用程式：請使用 Visual Studio 2015 或較新版本安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。

* 對於 Windows 8.1 或 Windows Phone 8.1 應用程式：請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 與 Visual Studio 2015 或 Visual Studio 2013。

<span id="interstitialadsxaml10"/>
### <a name="xamlnet"></a>XAML/.NET

本節提供 C# 範例，但同時對於 XAML/.NET 專案也支援 Visual Basic 和 C++。 如需完整的 C# 程式碼範例，請參閱[使用 C# 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-c.md)。

1. 在 Visual Studio 中，開啟您的專案。

2. 在 [參考管理員]**** 中，根據您的專案類型選取下列其中一項參考︰

  * 對於通用 Windows 平台 (UWP) 專案：展開 **\[通用 Windows\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。

  * Windows 8.1 專案：展開 **\[Windows 8.1\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 Windows 8.1 XAML 的 Ad Mediator SDK\]** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

  * 對於 Windows Phone 8.1 專案：展開 **\[Windows Phone 8.1\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK\]** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

3.  在您的應用程式的適當程式碼檔案中 (例如在 MainPage.xaml.cs 中，或部分其他頁面的程式碼檔案)，新增下列命名空間參考。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  在您的應用程式的適當位置中 (例如，在 ```MainPage``` 中或部分其他頁面)，為您的插播式廣告宣告 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 物件和代表應用程式識別碼和單位識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位至[測試模式值](test-mode-values.md)中提供的插播式影片廣告測試值。

  > [!NOTE]
  > 每個 **InterstitialAd** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元識別碼*和*應用程式識別碼*。 在這些步驟中，您將指派測試廣告單元識別碼和應用程式識別碼值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到市集之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件的事件連結事件處理常式。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  若要顯示*插播式影片廣告*：在您需要起始廣告前約 30-60 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType.Video**做為廣告類型。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

  若要顯示*插播式橫幅廣告* (僅限 UWP app)：在您需要廣告前約 5-8 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType.Display**做為廣告類型。

  ```csharp
  myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
  ```

6.  在您要顯示插播式影片廣告或插播式橫幅廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法顯示廣告。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  為 **InterstitialAd** 物件定義事件處理常式。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="interstitialadshtml10"/>
### <a name="htmljavascript"></a>HTML/JavaScript

以下指示假設您已在 Visual Studio 中建立 JavaScript 的通用 Windows 專案，並且是針對特定的 CPU。 如需完整的程式碼範例，請參閱[使用 JavaScript 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-javascript.md)。

1. 在 Visual Studio 中，開啟您的專案。

2.  在 [參考管理員]**** 中，根據您的專案類型選取下列其中一項參考︰

  * 對於通用 Windows 平台 (UWP) 專案：展開 [通用 Windows]****，按一下 [擴充功能]****，然後選取 [適用於 JavaScript 的 Microsoft Advertising SDK (Version 10.0)]**** 旁邊的核取方塊。

  * 對於 Windows 8.1 專案：展開 [Windows 8.1]****，按一下 [擴充功能]****，然後選取 [適用於 Windows 8.1 Native 的 Microsoft Advertising SDK (JS)]**** 旁邊的核取方塊。

  * Windows 8.1 專案：展開 [Windows Phone 8.1]****，按一下 [擴充功能]****，然後選取 [Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)]**** 旁邊的核取方塊。

3.  在專案的 HTML 檔案的 **&lt;head&gt;** 區段中，在專案的 default.css 和 default.js JavaScript 參考後面新增 ad.js 的參考。 在 UWP 專案中，新增下列參考。

  ``` HTML
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  在 Windows 8.1 或 Windows Phone 8.1 專案中，新增下列參考。

  ``` HTML
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  在您專案的 .js 檔案中，為您的插播式廣告宣告 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 物件和包含應用程式識別碼和單位識別碼的幾個欄位。 以下程式碼範例指派 `applicationId` 和 `adUnitId` 欄位至[測試模式值](test-mode-values.md)中提供的插播式影片廣告測試值。

  > [!NOTE]
  > 每個 **InterstitialAd** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元識別碼*和*應用程式識別碼*。 在這些步驟中，您將指派測試廣告單元識別碼和應用程式識別碼值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到市集之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件連結事件處理常式。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. 若要顯示*插播式影片廣告*：在您需要起始廣告前約 30-60 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **InterstitialAdType.video** 做為廣告類型。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

  若要顯示*插播式橫幅廣告* (僅限 UWP app)：在您需要廣告前約 5-8 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **InterstitialAdType.display** 做為廣告類型。

  ```js
  if (interstitialAd) {
      interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
  }
  ```

6.  在您要顯示廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法顯示廣告。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  為 **InterstitialAd** 物件定義事件處理常式。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="interstitialadsdirectx10"/>
### <a name="c-directx-interop"></a>C++ (DirectX 互通性)

此範例假設您已在 Visual Studio 中建立 C++ **DirectX 和 XAML 應用程式 (通用 Windows)** 專案，並且是針對特定的 CPU 架構。
 
1. 在 Visual Studio 中，開啟您的專案。

1.  在 [參考管理員]**** 中，根據您的專案類型選取下列其中一項參考︰

  * 對於通用 Windows 平台 (UWP) 專案：展開 **\[通用 Windows\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。

  * Windows 8.1 專案：展開 **\[Windows 8.1\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 Windows 8.1 XAML 的 Ad Mediator SDK\]** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

  * 對於 Windows Phone 8.1 專案：展開 **\[Windows Phone 8.1\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK\]** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

2.  在應用程式適當的標頭檔 (例如，DirectXPage.xaml.h) 中，宣告 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 物件和相關的事件處理常式方法。  

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  在相同標頭檔中，為您的插播式廣告宣告代表應用程式識別碼和單位識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位至[測試模式值](test-mode-values.md)中提供的插播式影片廣告測試值。

  > [!NOTE]
  > 每個 **InterstitialAd** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元識別碼*和*應用程式識別碼*。 在這些步驟中，您將指派測試廣告單元識別碼和應用程式識別碼值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到市集之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  在您要新增程式碼以顯示插播式廣告的 .cpp 檔中，新增下列命名空間參考。 下列範例假設您在應用程式中新增程式碼到 DirectXPage.xaml.cpp 檔案中。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件的事件連結事件處理常式。 在下列範例中，```InterstitialAdSamplesCpp``` 是您專案的命名空間，依您程式碼的需要變更此名稱。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. 若要顯示*插播式影片廣告*：在您需要插播式廣告前約 30-60 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType::Video**做為廣告類型。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

  若要顯示*插播式橫幅廣告* (僅限 UWP app)：在您需要廣告前約 5-8 秒，使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType::Display**做為廣告類型。

  ```cpp
  m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
  ```

7.  在您要顯示廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法顯示廣告。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  為 **InterstitialAd** 物件定義事件處理常式。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. 建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 開發人員中心發行包含即時廣告的應用程式

1.  在開發人員中心儀表板中，移至應用程式的 [\[利用廣告獲利\]](../publish/monetize-with-ads.md) 頁面，並[建立廣告單元](../monetize/set-up-ad-units-in-your-app.md)。 針對廣告單元類型，請選擇 **\[插播式影片\]** 或 **\[插播式橫幅\]**，視顯示的插播式廣告類型而定。 記下廣告單元識別碼與應用程式識別碼。

2. 如果您的應用程式是適用於 Windows 10 的 UWP 應用程式，您可以選擇為應用程式中的 **InterstitialAd** 啟用廣告流量分配，方法是透過 [\[利用廣告獲利\]](../publish/monetize-with-ads.md#mediation) 頁面中的 [\[廣告流量分配\]](../publish/monetize-with-ads.md) 設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路如 Taboola 和 Smaato 的廣告，以及 Microsoft 應用程式促銷活動廣告。

3.  在您的程式碼中，將測試的廣告單元值，用在開發人員中心產生的實際值取代。

4.  使用 Windows 開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md) 到市集。

5.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。

<span id="manage" />
## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>管理您應用程式中多個插播式廣告控制項的廣告單元

您可以在單一 App 中使用多個 **InterstitialAd** 控制項。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/monetize-with-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>插播式廣告最佳做法與原則


如需有關如何有效地使用插播式廣告及必需遵守之原則的詳細資訊，請參閱[插播式廣告最佳做法與原則](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>移除參考錯誤：以特定 CPU 平台為目標 (XAML 和 HTML)


使用 Microsoft Advertising 程式庫時，您在專案中將無法以 \[任何 CPU\]**** 為目標。 如果專案的目標是 **\[任何 CPU\]**，當您將參照新增到 Microsoft Advertising 程式庫之後，便可能會在專案中看到警告。 如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 如需詳細資訊，請參閱[已知問題](known-issues-for-the-advertising-libraries.md)。

## <a name="related-topics"></a>相關主題


* [使用 C# 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-c.md)
* [使用 JavaScript 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [為您的 App 設定廣告單元](../monetize/set-up-ad-units-in-your-app.md)
