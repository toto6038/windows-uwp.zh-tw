---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "了解如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 程式庫，在 Windows 10、Windows 8.1 或 Windows Phone 8.1 App 中包含插入式廣告。"
title: "插入式廣告"
translationtype: Human Translation
ms.sourcegitcommit: 8574695fe12042e44831227f81e1f6ea45e9c0da
ms.openlocfilehash: fdc9bddafc7b80f66bb160183a6c416a8573883a

---

# 插入式廣告




本逐步解說示範如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 程式庫，在 Windows 10、Windows 8.1 或 Windows Phone 8.1 App 中包含插入式廣告。

如需示範如何使用 C# 和 C++ 將插入式廣告新增到 JavaScript/HTML App 及 XAML App 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>
## 什麼是插入式廣告？

插入式廣告 (或「插頁廣告」) 和橫幅廣告不同，會在 App 的整個畫面上顯示。 它們在遊戲中經常以兩種基本的格式呈現。

* 以「付費牆」廣告呈現，使用者必須在特定間隔觀賞廣告。 例如在遊戲關卡之間：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 以「獎勵式」廣告呈現，使用者將會明確地尋求益處 (例如尋求完成關卡的提示或額外時間)，並透過 App 的使用者介面初始化廣告影片。

    請務必注意，此 SDK 除了在播放影片期間，並不會處理任何使用者介面。 請參閱[插入式廣告最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)，以在您考慮如何將插入式廣告整合到 App 時，取得應該執行和應該避免之事項的指導方針。

## 建置具有插入式廣告的 App


### 先決條件

* 對於 UWP app︰請安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 與 Visual Studio 2015。
* 對於 Windows 8.1 或 Windows Phone 8.1 App：請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 與 Visual Studio 2015 或 Visual Studio 2013。

### 程式碼開發

* [XAML/.NET App 的步驟](#interstitialadsxaml10)
* [HTML/JavaScript 的步驟](#interstitialadshtml10)
* [C++ (DirectX Interop) 的步驟](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### 插入式廣告 (XAML/.NET)

> **注意** 本節將提供 C# 範例，但同時也支援 Visual Basic 和 C++。
 
1. 在 Visual Studio 中，開啟您的專案。
2. 在 \[參考管理員\] 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 \[通用 Windows\]，按一下 \[擴充功能\]，然後選取 \[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\] 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 \[Windows 8.1\]，按一下 \[擴充功能\]，然後選取 \[適用於 Windows 8.1 XAML 的 Ad Mediator SDK\] 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

    -   對於 Windows Phone 8.1 專案：展開 \[Windows Phone 8.1\]，按一下 \[擴充功能\]，然後選取 \[適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK\] 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

3.  在 App 程式碼中包含下列命名空間參照。

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  宣告您的 `MyAppId` 和 `MyAdUnitId` 屬性。

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **注意** 在提交 App 之前，請以實際值取代測試值。

5.  具現化 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)，連結所有事件處理常式，然後要求廣告。

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  在程式碼中要顯示廣告的位置，請確定廣告已就緒，然後顯示它。

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  定義並撰寫事件的程式碼。

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  將 `MyAppId` 屬性指派給在[測試模式值 (英文)](test-mode-values.md) 中提供的測試值。 此值僅用於測試。您在發佈 App 之前將會以實際值取代它。

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  將 `MyAdUnitId` 屬性指派給在[測試模式值 (英文)](test-mode-values.md) 中提供的測試值。 此值僅用於測試。您在發佈 App 之前將會以實際值取代它。

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  建置並測試您的 App 以確認它會顯示測試廣告。

<span id="interstitialadshtml10"/>
### 插入式廣告 (HTML/JavaScript)

此範例假設您已在 Visual Studio 2015 中建立 JavaScript 的通用 App 專案，並且是針對特定的 CPU。

1. 在 Visual Studio 中，開啟您的專案。
2.  在 \[參考管理員\] 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 \[通用 Windows\]，按一下 \[擴充功能\]，然後選取 \[適用於 JavaScript 的 Microsoft Advertising SDK (Version 10.0)\] 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 \[Windows 8.1\]，按一下 \[擴充功能\]，然後選取 \[適用於 Windows 8.1 Native 的 Microsoft Advertising SDK (JS)\] 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 \[Windows Phone 8.1\]，按一下 \[擴充功能\]，然後選取 \[Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)\] 旁邊的核取方塊。

3.  在 HTML 中包含下列指令碼參照。

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  宣告您的 `myAppId` 和 `myAdUnitId` 屬性。

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  具現化 **InterstitialAd**，連結所有事件處理常式，然後要求廣告。

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  在程式碼中要顯示廣告的位置，請確定廣告已就緒，然後顯示它。

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  定義並撰寫事件的程式碼。

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  將 `MyAppId` 屬性指派給在[測試模式值 (英文)](test-mode-values.md) 中提供的測試值。 此值僅用於測試。您在發佈 App 之前將會以實際值取代它。

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  將 `MyAdUnitId` 屬性指派給在[測試模式值 (英文)](test-mode-values.md) 中提供的測試值。 此值僅用於測試。您在發佈 App 之前將會以實際值取代它。

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  建置並測試您的 App 以確認它會顯示測試廣告。

<span id="interstitialadsdirectx10"/>
### 插入式廣告 (C++ 和 DirectX 搭配 XAML interop)

此範例假設您已在 Visual Studio 2015 中建立 XAML 的通用 App 專案，並且是針對特定的 CPU 架構。

> **重要** 此程式碼是以 C++ 撰寫，以符合 DirectX 要求。

 
1. 在 Visual Studio 中，開啟您的專案。
1.  在 \[參考管理員\] 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 \[通用 Windows\]，按一下 \[擴充功能\]，然後選取 \[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\] 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 \[Windows 8.1\]，按一下 \[擴充功能\]，然後選取 \[適用於 Windows 8.1 XAML 的 Ad Mediator SDK\] 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

    -   對於 Windows Phone 8.1 專案：展開 \[Windows Phone 8.1\]，按一下 \[擴充功能\]，然後選取 \[適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK\] 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

2.  在 App 適當的標頭檔中，宣告插入式廣告物件和相關屬性/方法。

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  宣告您的 `AppId` 和 `AdUnitId` 屬性。

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  在 .cpp 檔案中新增命名空間參照。

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  具現化 **InterstitialAd**，連結所有事件處理常式，然後要求廣告。

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  在程式碼中要顯示廣告的位置，請確定廣告已就緒，然後顯示它。

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  定義並撰寫事件的程式碼。

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  將 `AppId` 屬性指派給在[測試模式值 (英文)](test-mode-values.md) 中提供的測試值。 此值僅用於測試。您在發佈 App 之前將會以實際值取代它。

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  將 `AdUnitId` 屬性指派給在[測試模式值 (英文)](test-mode-values.md) 中提供的測試值。 此值僅用於測試。您在發佈 App 之前將會以實際值取代它。

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. 建置並測試您的 App 以確認它會顯示測試廣告。

### 使用 Windows 開發人員中心發行包含即時廣告的 App

1.  在開發人員中心儀表板中，移至 App 的 \[營利\]&gt;\[利用廣告營利\] 頁面，並[建立獨立的 Microsoft Advertising 單位](../publish/monetize-with-ads.md)。 針對廣告單位類型，指定 \[插入式影片\]。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值，用在開發人員中心產生的實際值取代。

3.  使用 Windows 開發人員中心儀表板[提交您的 App](../publish/app-submissions.md) 到市集。

4.  在「開發人員中心」儀表板上檢閱您的[廣告績效報告](../publish/advertising-performance-report.md)。

<span id="interstitialbestpractices10"/>
## 插入式廣告最佳做法與原則


如需有關如何有效地使用插入式廣告及必需遵守之原則的詳細資訊，請參閱[插入式廣告最佳做法與原則](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

<span id="targetplatform10"/>
## 移除參考錯誤：以特定 CPU 平台為目標 (XAML 和 HTML)


使用 Microsoft Advertising 程式庫時，您在專案中將無法以 \[任何 CPU\] 為目標。 如果專案的目標是 \[任何 CPU\]，當您將參照新增到 Microsoft Advertising 程式庫之後，便可能會在專案中看到警告。 如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 如需詳細資訊，請參閱[已知問題](known-issues-for-the-advertising-libraries.md)。

## 相關主題


* [使用 C 的插入式廣告範例程式碼#](interstitial-ad-sample-code-in-c.md)
* [使用 JavaScript 的插入式廣告範例程式碼](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的廣告範例](http://aka.ms/githubads)

 

 



<!--HONumber=Nov16_HO1-->


