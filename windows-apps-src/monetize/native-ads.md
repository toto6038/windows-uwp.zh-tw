---
author: Xansky
description: 了解如何將原生廣告加入您的 UWP App。
title: 原生廣告
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告控制項, 原生廣告
ms.localizationpriority: medium
ms.openlocfilehash: 123934c911f342dd57033c8e204e58bc00a5f18f
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5933896"
---
# <a name="native-ads"></a>原生廣告

原生廣告是以元件為基礎的廣告格式，其中每一項廣告創意 (例如標題、影像、描述和喚起行動文字) 都會當做個別元素傳送到您的應用程式。 您可以使用您自己的字型、色彩、動畫以及其他 UI 元件，將這些元素項目整合到您的應用程式中，拼接出符合您應用程式外觀與感覺的低調使用者體驗，同時從廣告獲得高利潤。

對於廣告商而言，原生廣告可以提供高績效的展示位置，因為廣告體驗密切地與應用程式整合，因此使用者通常會與這些類型的廣告進行更多互動。

> [!NOTE]
> 原生廣告目前僅支援適用於 Windows 10 的 XAML 型 UWP app。 計劃於未來版本的 Microsoft Advertising SDK 支援使用 HTML 與 JavaScript 撰寫的 UWP App。

## <a name="prerequisites"></a>必要條件

* 使用 Visual Studio 2015 或更新版本的 Visual Studio 來安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。 如需安裝指示，請參閱[本文](install-the-microsoft-advertising-libraries.md)。

## <a name="integrate-a-native-ad-into-your-app"></a>將原生廣告整合至您的應用程式

請依照下列指示，將原生廣告整合至您的應用程式，並確認您的原生廣告實作可顯示測試廣告。

1. 在 Visual Studio 中，開啟您的專案或建立新專案。
    > [!NOTE]
    > 如果您正在使用現有的專案，請在專案中開啟 Package.appxmanifest 檔案，並確保選取 **\[網際網路 (用戶端)\]** 功能。 您的應用程式需要這項功能來接收測試廣告和即時廣告。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft Advertising SDK 的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在您的專案中新增 Microsoft Advertising SDK 的參考：

    1. 在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。
    2.  在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。
    3.  在 **\[參考管理員\]** 中，按一下 [確定]。

4. 在您的應用程式的適當程式碼檔案中 (例如在 MainPage.xaml.cs 中，或部分其他頁面的程式碼檔案)，新增下列命名空間參考。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  在您的應用程式的適當位置中 (例如，在 ```MainPage``` 中或部分其他頁面)，為您的插播式廣告宣告 [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 物件和代表原生廣告的應用程式識別碼和廣告單元識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位至原生廣告的[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。
    > [!NOTE]
    > 每個 **NativeAdsManagerV2** 都有對應的*廣告單元*，由我們的服務用來提供廣告給原生廣告控制項，且每個廣告單元都包含*廣告單元 ID* 和*應用程式 ID*。 在這些步驟中，您將指派測試廣告單元 ID 和應用程式 ID 值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到 Microsoft Store 之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **NativeAdsManagerV2** 物件並為物件的 **AdReady** 和 **ErrorOccurred** 事件連結事件處理常式。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  當您準備就緒可以顯示原生廣告時，呼叫 **RequestAd** 方法來擷取廣告。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  原生廣告準備好可供您的應用程式使用時，會呼叫您的 [AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) 事件處理常式，而代表原生廣告的 [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 物件會傳遞至 *e* 參數。 使用 **NativeAdV2** 屬性來取得原生廣告的每個元素，並將這些元素顯示在頁面上。 請務必也要呼叫 **RegisterAdContainer** 方法來註冊做為原生廣告容器的 UI 元素，這樣才能正確追蹤廣告的曝光次數和點擊數。
    > [!NOTE]
    > 原生廣告的一些元素是必要項目，必須一律顯示在您的應用程式中。 如需詳細資訊，請參閱我們的[原生廣告指南](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)。

    例如，假設您的應用程式包含 ```MainPage``` (或其他頁面) 並具有下列 **StackPanel**。 這個 **StackPanel** 包含一系列用來顯示原生廣告不同元素的控制項，包括標題、描述、影像、*贊助廠商*文字，以及可顯示*喚起行動*文字的按鈕。

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    下列程式碼範例示範 **AdReady** 事件處理常式，在 **StackPanel** 的控制項中顯示原生廣告的每個元素，接著呼叫 **RegisterAdContainer** 方法來註冊 **StackPanel**。 此程式碼假設它是從包含 **StackPanel** 之頁面的程式碼後置檔案執行。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

9.  Define an event handler for the **ErrorOccurred** event to handle errors related to the native ad. 下列範例會在測試期間將錯誤資訊寫入 Visual Studio **\[輸出\]** 視窗。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  編譯並執行應用程式來查看其中的測試廣告。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>發行包含即時廣告的應用程式

在您確認原生廣告實作可以成功顯示測試廣告之後，請依照這些指示來設定您的應用程式顯示實際廣告，並提交更新後的應用程式至市集。

1.  請確定您的原生廣告實作遵循我們的[原生廣告指南](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)。

2.  在開發人員中心儀表板中，移至[應用程式內廣告](../publish/in-app-ads.md)頁面，並且[建立廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。 廣告單元類型請指定 **\[原生\]**。 記下廣告單元識別碼與應用程式識別碼。
    > [!NOTE]
    > 測試廣告單元和即時 UWP 廣告單元的應用程式識別碼值有不同的格式。 測試應用程式識別碼值為 GUID。 當您在儀表板中建立即時 UWP 廣告單元時，廣告單元的應用程式識別碼值一律符合您應用程式的 Microsoft Store 識別碼 (Microsoft Store 識別碼值範例類似於 9NBLGGH4R315)。

3. 您可以選擇為原生廣告啟用廣告流量分配，方法是在 [\[應用程式內廣告\]](../publish/in-app-ads.md) 頁面的 [\[利用廣告獲利\]](../publish/in-app-ads.md#mediation) 區段進行設定。 廣告流量分配可以透過顯示來自多個廣告網路的廣告，讓您獲得最大的廣告收益並充分發揮應用程式促銷功能。

4.  在您的程式碼中，將測試的廣告單元值 (也就是 [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) 建構函式的 *applicationId* 和 *adUnitId* 參數)，更換為在開發人員中心產生的實際值。

5.  使用開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md)到 Microsoft Store。

6.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>管理您應用程式中多個原生廣告的廣告單元

您可以在單一應用程式中使用多個原生廣告位置。 在本案例中，我們建議您將不同的廣告單元指派給每個原生廣告位置。 針對原生廣告使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/in-app-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [原生廣告指南](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [應用程式內廣告](../publish/in-app-ads.md)
* [為您的 App 設定廣告單元](set-up-ad-units-in-your-app.md)
