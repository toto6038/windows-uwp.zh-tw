---
author: Xansky
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: 了解如何使用 Microsoft Advertising SDK，在適用於 Windows 10 的 UWP app 中加入插播式廣告。
title: 插播式廣告
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 廣告, 廣告控制項, 插播式
ms.localizationpriority: medium
ms.openlocfilehash: 547a582064262d18467df4868df17a08e73b279c
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2018
ms.locfileid: "4745535"
---
# <a name="interstitial-ads"></a>插播式廣告

本逐步解說示範如何在通用 Windows 平台 (UWP) App 和 Windows 10 遊戲中包含插播式廣告。 如需示範如何使用 C# 和 C++ 將插播式廣告新增到 JavaScript/HTML 應用程式及 XAML 應用程式的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>什麼是插播式廣告？

不同於標準橫幅廣告，侷限於 app 或遊戲 UI 的一部分，插播式廣告會在整個畫面上顯示。 它們在遊戲中經常以兩種基本的格式呈現。

* 以「付費牆」** 廣告呈現，使用者必須在特定間隔觀賞廣告。 例如在遊戲關卡之間：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 以 *「獎勵式」* 廣告呈現，使用者將會明確地尋求益處 (例如尋求完成關卡的提示或額外時間)，並透過應用程式的使用者介面初始化廣告。

我們提供兩種類型的插播式廣告，供您的應用程式和遊戲使用：：**插入式影片廣告**和**插播式橫幅廣告**。

> [!NOTE]
> 插播式廣告 API 不會處理任何使用者介面，除了在影片播放時。 當您考慮如何將插播式廣告整合到應用程式時，請參閱[插播式廣告最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)，取得應該執行和應該避免之事項的指導方針。

## <a name="prerequisites"></a>必要條件

* 使用 Visual Studio 2015 或更新版本的 Visual Studio 來安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。 如需安裝指示，請參閱[本文](install-the-microsoft-advertising-libraries.md)。

## <a name="integrate-an-interstitial-ad-into-your-app"></a>將插播式廣告整合至您的應用程式

若要在您的應用程式中顯示插播式廣告，請遵循專案類型的指示：

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX 互通性)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

本節提供 C# 範例，但同時對於 XAML/.NET 專案也支援 Visual Basic 和 C++。 如需完整的 C# 程式碼範例，請參閱[使用 C# 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-c.md)。

1. 在 Visual Studio 中，開啟您的專案。
    > [!NOTE]
    > 如果您正在使用現有的專案，請在專案中開啟 Package.appxmanifest 檔案，並確保選取 **\[網際網路 (用戶端)\]** 功能。 您的應用程式需要這項功能來接收測試廣告和即時廣告。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在您的專案中新增 Microsoft Advertising SDK 的參考：

    1. 在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。
    2.  在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。
    3.  在 **\[參考管理員\]** 中，按一下 [確定]。

3.  在您的應用程式的適當程式碼檔案中 (例如在 MainPage.xaml.cs 中，或部分其他頁面的程式碼檔案)，新增下列命名空間參考。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  在您的應用程式的適當位置中 (例如，在 ```MainPage``` 中或部分其他頁面)，為您的插播式廣告宣告 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 物件和代表應用程式識別碼和單位識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位至插播式廣告的[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。

    > [!NOTE]
    > 每個 **InterstitialAd** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元識別碼*和*應用程式識別碼*。 在這些步驟中，您將指派測試廣告單元識別碼和應用程式識別碼值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到 Microsoft Store 之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件的事件連結事件處理常式。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  若要顯示*插播式影片廣告*：在您需要起始廣告前約 30-60 秒，使用 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType.Video**做為廣告類型。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

    若要顯示*插播式橫幅廣告*：在您需要起始廣告前約 5-8 秒，使用 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType.Display**做為廣告類型。

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  在您要顯示插播式影片廣告或插播式橫幅廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法顯示廣告。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  為 **InterstitialAd** 物件定義事件處理常式。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

以下指示假設您已在 Visual Studio 中建立 JavaScript 的通用 Windows 專案，並且是針對特定的 CPU。 如需完整的程式碼範例，請參閱[使用 JavaScript 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-javascript.md)。

1. 在 Visual Studio 中，開啟您的專案。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在您的專案中新增 Microsoft Advertising SDK 的參考：

    1. 在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。
    2.  在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[Microsoft Advertising SDK for JavaScript\]** (Version 10.0) 旁邊的核取方塊。
    3.  在 **\[參考管理員\]** 中，按一下 \[確定\]。

3.  在專案的 HTML 檔案的 **&lt;head&gt;** 區段中，在專案的 default.css 和 default.js JavaScript 參考後面新增 ad.js 的參考。

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  在您專案的 .js 檔案中，為您的插播式廣告宣告 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 物件和包含應用程式識別碼和單位識別碼的幾個欄位。 以下程式碼範例指派 `applicationId` 和 `adUnitId` 欄位至插播式廣告的[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。

    > [!NOTE]
    > 每個 **InterstitialAd** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元識別碼*和*應用程式識別碼*。 在這些步驟中，您將指派測試廣告單元識別碼和應用程式識別碼值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到 Microsoft Store 之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件連結事件處理常式。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. 若要顯示*插播式影片廣告*：在您需要起始廣告前約 30-60 秒，使用 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **InterstitialAdType.video** 做為廣告類型。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

    若要顯示*插播式橫幅廣告*：在您需要起始廣告前約 5-8 秒，使用 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **InterstitialAdType.display** 做為廣告類型。

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  在您要顯示廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法顯示廣告。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  為 **InterstitialAd** 物件定義事件處理常式。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (DirectX 互通性)

此範例假設您已在 Visual Studio 中建立 C++ **DirectX 和 XAML 應用程式 (通用 Windows)** 專案，並且是針對特定的 CPU 架構。
 
1. 在 Visual Studio 中，開啟您的專案。

3. 在您的專案中新增 Microsoft Advertising SDK 的參考：

    1. 在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。
    2.  在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。
    3.  在 **\[參考管理員\]** 中，按一下 \[確定\]。

2.  在應用程式適當的標頭檔 (例如，DirectXPage.xaml.h) 中，宣告 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 物件和相關的事件處理常式方法。  

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  在相同標頭檔中，為您的插播式廣告宣告代表應用程式識別碼和單位識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位至插播式廣告的[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。

    > [!NOTE]
    > 每個 **InterstitialAd** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元識別碼*和*應用程式識別碼*。 在這些步驟中，您將指派測試廣告單元識別碼和應用程式識別碼值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到 Microsoft Store 之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  在您要新增程式碼以顯示插播式廣告的 .cpp 檔中，新增下列命名空間參考。 下列範例假設您在應用程式中新增程式碼到 DirectXPage.xaml.cpp 檔案中。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **InterstitialAd** 物件並為物件的事件連結事件處理常式。 在下列範例中，```InterstitialAdSamplesCpp``` 是您專案的命名空間，依您程式碼的需要變更此名稱。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. 若要顯示*插播式影片廣告*：在您需要插播式廣告前約 30-60 秒，使用 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType::Video**做為廣告類型。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

    若要顯示*插播式橫幅廣告*：在您需要起始廣告前約 5-8 秒，使用 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法預先擷取廣告。 這可讓您有足夠的時間在其顯示前請求和準備廣告。 請務必指定 **AdType::Display**做為廣告類型。

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  在您要顯示廣告的程式碼中的某個點，確認 **InterstitialAd** 已就緒可供顯示，然後使用 [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法顯示廣告。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  為 **InterstitialAd** 物件定義事件處理常式。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. 建置並測試您的應用程式以確認它會顯示測試廣告。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>發行包含即時廣告的應用程式

1. 請確定您在應用程式中使用插播式廣告的方式遵循我們的[插播式廣告指南](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

2.  在開發人員中心儀表板中，移至[應用程式內廣告](../publish/in-app-ads.md)頁面，並且[建立廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。 針對廣告單元類型，請選擇 **\[插播式影片\]** 或 **\[插播式橫幅\]**，視顯示的插播式廣告類型而定。 記下廣告單元識別碼與應用程式識別碼。
    > [!NOTE]
    > 測試廣告單元和即時 UWP 廣告單元的應用程式識別碼值有不同的格式。 測試應用程式識別碼值為 GUID。 當您在儀表板中建立即時 UWP 廣告單元時，廣告單元的應用程式識別碼值一律符合您應用程式的 Microsoft Store 識別碼 (Microsoft Store 識別碼值範例類似於 9NBLGGH4R315)。

3. 您可以選擇為 **\[InterstitialAd\]** 啟用廣告流量分配，方法是在[ \[應用程式內廣告\]](../publish/in-app-ads.md) 頁面的 [\[流量分配設定\]](../publish/in-app-ads.md#mediation) 區段進行設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路如 Taboola 和 Smaato 的廣告，以及 Microsoft 應用程式促銷活動廣告。

4.  在您的程式碼中，將測試的廣告單元值，用在開發人員中心產生的實際值取代。

5.  使用 Windows 開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md) 到 Microsoft Store。

6.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>管理您應用程式中多個插播式廣告控制項的廣告單元

您可以在單一 App 中使用多個 **InterstitialAd** 控制項。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/in-app-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [插播式廣告指南](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [使用 C# 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-c.md)
* [使用 JavaScript 的插播式廣告範例程式碼](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [為您的 App 設定廣告單元](set-up-ad-units-in-your-app.md)
