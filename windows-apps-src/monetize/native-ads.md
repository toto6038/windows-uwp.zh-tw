---
author: mcleanbyron
description: "了解如何將原生廣告加入您的 UWP App。"
title: "原生廣告"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, 廣告控制項, 原生廣告"
ms.openlocfilehash: 47a69e48f04c670a462c34083af1117d2c7908d8
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2017
---
# <a name="native-ads"></a>原生廣告

原生廣告是以元件為基礎的廣告格式，其中每一項廣告創意 (例如標題、影像、描述和喚起行動文字) 都會當做個別元素傳送到您的應用程式。 您可以使用您自己的字型、色彩、動畫以及其他 UI 元件，將這些元素項目整合到您的應用程式中，拼接出符合您應用程式外觀與感覺的低調使用者體驗，同時從廣告獲得高利潤。

對於廣告商而言，原生廣告可以提供高績效的展示位置，因為廣告體驗密切地與應用程式整合，因此使用者通常會與這些類型的廣告進行更多互動。

> [!NOTE]
> 若要提供原生廣告給市集中的應用程式公用版本，您必須從開發人員中心儀表板的 **\[利用廣告獲利\]** 頁面建立**原生**廣告單元。 目前僅有加入試驗計劃的選定開發人員才可以建立**原生**廣告單元，但我們很快就要將這項功能提供給所有的開發人員。 如果您想要加入我們的試驗計劃，請透過電子郵件與我們連絡：aiacare@microsoft.com。

> [!NOTE]
> 原生廣告目前僅支援適用於 Windows 10 的 XAML 型 UWP app。 計劃於未來版本的 Microsoft Advertising SDK 支援使用 HTML 與 JavaScript 撰寫的 UWP App。

## <a name="prerequisites"></a>必要條件

* 安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) (10.0.4 版或更新版本) 與 Visual Studio 2015 或 Visual Studio 的更新版本。 如需安裝指示，請參閱[本文](install-the-microsoft-advertising-libraries.md)。 您可以透過 MSI 安裝程式在開發電腦上安裝 SDK，也可以透過 NuGet 套件在特定專案中安裝 SDK 以供使用。

## <a name="integrate-a-native-ad-into-your-app"></a>將原生廣告整合至您的應用程式

請依照下列指示，將原生廣告整合至您的應用程式，並確認您的原生廣告實作可顯示測試廣告。

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft Advertising SDK 的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3.  在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。

4.  在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。

5.  在 **\[參考管理員\]** 中，按一下 [確定]。

6. 在您的應用程式的適當程式碼檔案中 (例如在 MainPage.xaml.cs 中，或部分其他頁面的程式碼檔案)，新增下列命名空間參考。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

7.  在您的應用程式的適當位置中 (例如，在 ```MainPage``` 中或部分其他頁面)，為您的插播式廣告宣告 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.aspx) 物件和代表原生廣告的應用程式識別碼和廣告單元識別碼的幾個字串欄位。 以下程式碼範例指派 `myAppId` 和 `myAdUnitId` 欄位至原生廣告的[測試值](test-mode-values.md)。

    > [!NOTE]
    > 每個 **NativeAdsManager** 都有對應的*廣告單元*，由我們的服務用來提供廣告給原生廣告控制項，且每個廣告單元都包含*廣告單元 ID* 和*應用程式 ID*。 在這些步驟中，您將指派測試廣告單元 ID 和應用程式 ID 值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到市集之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

8.  在啟動時執行的程式碼中 (例如，頁面的建構函式中)，起始 **NativeAdsManager** 物件並為物件的 [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.adready.aspx) 和 [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.erroroccurred.aspx) 事件連結事件處理常式。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

9.  當您準備就緒可以顯示原生廣告時，呼叫 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.requestad.aspx) 方法來擷取廣告。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

10.  原生廣告準備好可供您的應用程式使用時，會呼叫您的 **AdReady** 事件處理常式，而代表原生廣告的 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) 物件會傳遞至 *e* 參數。 使用 **NativeAd** 屬性來取得原生廣告的每個元素，並將這些元素顯示在頁面上。 請務必也要呼叫 [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) 方法來註冊做為原生廣告容器的 UI 元素，這樣才能正確追蹤廣告的曝光次數和點擊數。
  > [!NOTE]
  > 原生廣告的一些元素是必要項目，必須一律顯示在您的應用程式中。 如需詳細資訊，請參閱[需求與指導方針](#requirements-and-guidelines)。

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

11.  定義 **ErrorOccurred** 事件的事件處理常式來處理原生廣告的相關錯誤。 下列範例會在測試期間將錯誤資訊寫入 Visual Studio **\[輸出\]** 視窗。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

12.  編譯並執行應用程式來查看其中的測試廣告。

<span id="release" />
## <a name="release-your-app-with-live-ads"></a>發行包含即時廣告的應用程式

在您確認原生廣告實作可以成功顯示測試廣告之後，請依照這些指示來設定您的應用程式顯示實際廣告，並提交更新後的應用程式至市集。

1.  請確定您的原生廣告實作遵循原生廣告的[需求和指導方針](#requirements-and-guidelines)。

2.  在開發人員中心儀表板中，移至應用程式的 [\[利用廣告獲利\]](../publish/monetize-with-ads.md) 頁面，並[建立廣告單元](../monetize/set-up-ad-units-in-your-app.md)。 廣告單元類型請指定 **\[原生\]**。 記下廣告單元識別碼與應用程式識別碼。
    > [!IMPORTANT]
    > 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

3. 您可以選擇為原生廣告啟用廣告流量分配，方法是在 [\[利用廣告獲利\]](../publish/monetize-with-ads.md) 頁面的 [\[廣告流量分配\]](../publish/monetize-with-ads.md#mediation) 區段進行設定。 廣告流量分配可以透過顯示來自多個廣告網路的廣告，讓您獲得最大的廣告收益並充分發揮應用程式促銷功能。

4.  在您的程式碼中，將測試的廣告單元值 (也就是 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 建構函式的 *applicationId* 和 *adUnitId* 參數)，更換為在開發人員中心產生的實際值。

5.  使用開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md)到市集。

6.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。

<span id="requirements-and-guidelines" />
## <a name="requirements-and-guidelines"></a>需求與指導方針

原生廣告讓您對於如何呈現廣告給使用者擁有極大的掌控權。 請依照這些需求與指導方針，協助確保廣告商的訊息可以觸及您的使用者，同時也可協助避免提供令人混淆的原生廣告體驗給使用者。

### <a name="register-the-container-for-your-native-ad"></a>註冊原生廣告的容器

在程式碼中，您必須呼叫 **NativeAd** 物件的 [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) 方法來註冊做為原生廣告容器的 UI 元素，並可選擇您想要的任何特定控制項來註冊做為廣告的可點選目標。 這樣才能正確追蹤廣告的曝光次數和點擊數。

**RegisterAdContainer** 方法有兩個多載可供使用：

* 如果您希望所有個別原生廣告元素的整個容器都是可點選的，請呼叫 [RegisterAdContainer(FrameworkElement)](https://msdn.microsoft.com/library/windows/apps/mt809188.aspx) 方法並傳遞容器控制項給方法。 例如，如果您在全部裝載於 **StackPanel** 的不同控制項中顯示所有原生廣告元素，而您希望整個**StackPanel** 都是可點選的，那麼請傳遞 **StackPanel** 給此方法。

* 如果您只希望某些原生廣告元素是可點選的，請呼叫 [RegisterAdContainer (FrameworkElement、IVector(FrameworkElement))](https://msdn.microsoft.com/library/windows/apps/mt809189.aspx) 方法。 只有傳遞給第二個參數的控制項是可點選的。

### <a name="required-native-ad-elements"></a>必要的原生廣告元素

您至少必須一律顯示下列原生廣告元素給您原生廣告設計中的使用者。 如果未加入下列元素，您的廣告單元可能會績效不佳、收入低。

1. 一律顯示原生廣告標題 (在 **NativeAd** 物件的 [Title](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.title.aspx) 屬性中提供)。 提供可顯示至少 25 個字元的空間。 如果標題較長，以省略符號取代其他文字。
2. 一律顯示至少一個下列元素，協助區分原生廣告體驗和您應用程式的其餘部分，並明確呼叫廣告商提供的內容：
  * 可區分的 *ad* 圖示 (於 **NativeAd** 物件的 [AdIcon](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.adicon.aspx) 屬性提供)。 此圖示由 Microsoft 提供。
  * *贊助廠商*文字 (於 **NativeAd** 物件的 [SponsoredBy](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.sponsoredby.aspx) 屬性提供)。 此文字由廣告商提供。
  * 另一個顯示*贊助廠商*文字的方法是由您選擇顯示某些其他文字，以協助區分原生廣告體驗和您應用程式的其餘部分，例如「贊助內容」、「促銷內容」、「建議內容」等。

### <a name="user-experience"></a>使用者體驗

原生廣告應該要與您應用程式的其餘部分有明確界定，且周圍有空間以避免意外點按。 使用邊框、不同的背景或其他 UI 來區隔廣告內容和您應用程式的其餘部分。 請記住，意外點擊廣告長期下來對您的廣告收益或一般使用者體驗不會有幫助。

### <a name="description"></a>描述

如果您選擇要顯示廣告的描述 (於 **NativeAd** 物件的 [Description](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.description.aspx)屬性提供)，請提供至少可顯示 75 個字元的空間。 我們建議您使用動畫來顯示廣告描述的完整內容。

### <a name="call-to-action"></a>喚起行動

*喚起行動*文字 (於 **NativeAd**物件的 [CallToAction](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.calltoaction.aspx) 屬性提供) 是廣告的關鍵元件。 如果您選擇要顯示此文字，請依照下列指導方針操作︰

* 一律在可點選控制項 (例如按鈕或超連結) 上顯示*喚起行動*文字給使用者。
* 一律完整顯示*喚起行動*文字。
* 確保*喚起行動*文字與廣告商的其餘促銷文字區隔開來。

### <a name="learn-and-optimize"></a>了解和最佳化

我們建議對於您應用程式中的每個不同原生廣告位置，建立並使用不同的廣告單元。 這可讓您取得每個原生廣告位置的個別報告資料，您可以使用此資料來進行變更，以最佳化各原生廣告位置的效能。

## <a name="related-topics"></a>相關主題

* [利用廣告獲利](../publish/monetize-with-ads.md)
* [為您的 App 設定廣告單元](../monetize/set-up-ad-units-in-your-app.md)
