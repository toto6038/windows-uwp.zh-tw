---
author: Xansky
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: 深入了解在 XAML 應用程式中使用 Microsoft Advertising 程式庫開發之常見問題的解決方案。
title: XAML 和 C# 的疑難排解指南
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 廣告, AdControl,疑難排解, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 4f3ebd2877dd786161c76d89e17a2667c07e65b3
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5401903"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>XAML 和 C# 的疑難排解指南

本主題包含在 XAML 應用程式中使用 Microsoft Advertising 程式庫開發之常見問題的解決方案。

* [XAML](#xaml)
  * [沒有顯示 AdControl](#xaml-notappearing)
  * [黑色方塊閃爍然後消失](#xaml-blackboxblinksdisappears)
  * [廣告沒有重新整理](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [沒有顯示 AdControl](#csharp-adcontrolnotappearing)
  * [黑色方塊閃爍然後消失](#csharp-blackboxblinksdisappears)
  * [廣告沒有重新整理](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>沒有顯示 AdControl

1.  確定已在 Package.appxmanifest 中選取 **\[網際網路 (用戶端)\]** 功能。

2.  檢查應用程式識別碼和廣告單位識別碼。 這些識別碼必須符合從 Windows 開發人員中心取得的應用程式識別碼和廣告單位識別碼。 如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  檢查 **Height** 和 **Width** 屬性。 這兩個屬性必須設定為其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  檢查元素的位置。 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 必須在可檢視區域內。

5.  檢查 **Visibility** 屬性。 選擇性的 **Visibility** 屬性不得設為 collapsed 或 hidden。 可以在行內 (如下所示) 或外部樣式表中設定這個屬性。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  檢查 **AdControl** 的父項。 如果 **AdControl** 元素位於父元素中，則父元素的狀態必須是使用中且可見。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  確定 **AdControl** 沒有在檢視區隱藏。 **AdControl** 必須可見，廣告才能正確顯示。

8.  不應在模擬器中測試 **ApplicationId** 和 **AdUnitId** 的實際值。 若要確保 **AdControl** 如預期般運作，請對 **ApplicationId** 和 **AdUnitId** 都使用[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑色方塊閃爍然後消失

1.  再次檢查前述[沒有顯示 AdControl](#xaml-notappearing) 一節中的所有步驟。

2.  處理 **ErrorOccurred** 事件，並以傳遞到事件處理常式的訊息來判斷是否發生問題及擲回的問題類型為何。 如需詳細資訊，請參閱 [XAML/C# 錯誤處理的逐步解說](error-handling-in-xamlc-walkthrough.md)。

    此範例示範 **ErrorOccurred** 事件處理常式。 第一個程式碼片段是 XAML UI 標記。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    此範例示範對應的 C# 程式碼。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    造成黑色方塊的常見錯誤為「沒有可用的廣告」。 這個錯誤代表要求沒有傳回可用的廣告。

3.  [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 運作正常。

    根據預設，**AdControl** 會在無法顯示廣告時摺疊。 相同父元素中的其他子元素可能會移動，以填滿已摺疊之 **AdControl** 的空位，直到下一次發出要求時才會展開。

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>廣告沒有重新整理

1.  檢查 [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled) 屬性。 根據預設，這個選用的屬性會設為 **True**。 當設為 **False** 時，必須使用 [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh) 方法來擷取另一個廣告。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  檢查對 **Refresh** 方法的呼叫。 當使用自動重新整理時，無法使用 **Refresh** 來擷取另一個廣告。 使用手動重新整理時，**Refresh** 應於最少 30 秒至 60 秒 (依裝置目前的數據連線而定) 之後才呼叫。

    下列程式碼片段顯示如何使用 **Refresh** 方法的範例。 第一個程式碼片段是 XAML UI 標記。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    這個程式碼片段顯示 UI 標記後置的 C# 程式碼範例。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** 運作正常。 有時候，同樣的廣告可能連續出現超過一次，使之看起像是沒有重新整理。

<span id="csharp"/>

## <a name="c"></a>C\# #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>沒有顯示 AdControl

1.  確定已在 Package.appxmanifest 中選取 **\[網際網路 (用戶端)\]** 功能。

2.  確定 **AdControl** 已具現化。 如果 **AdControl** 未具現化，將無法使用。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  檢查應用程式識別碼和廣告單位識別碼。 這些識別碼必須符合從 Windows 開發人員中心取得的應用程式識別碼和廣告單位識別碼。 如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  檢查 **Height** 和 **Width** 參數。 這兩個屬性必須設定為其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  確保 **AdControl** 新增至父元素。 若要顯示，**AdControl** 必須新增為父控制項的子項 (例如，**StackPanel** 或 **Grid**)。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  檢查 **Margin** 參數。 **AdControl** 必須在可檢視區域內。

7.  檢查 **Visibility** 屬性。 選用的 **Visibility** 屬性必須設為 **Visible**。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  檢查 **AdControl** 的父項。 該父元素必須為使用中且可見。

9. 不應在模擬器中測試 **ApplicationId** 和 **AdUnitId** 的實際值。 若要確保 **AdControl** 如預期般運作，請對 **ApplicationId** 和 **AdUnitId** 都使用[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑色方塊閃爍然後消失

1.  再次檢查上方[沒有顯示 AdControl](#csharp-adcontrolnotappearing) 一節中的所有步驟。

2.  處理 **ErrorOccurred** 事件，並以傳遞到事件處理常式的訊息來判斷是否發生問題及擲回的問題類型為何。 如需詳細資訊，請參閱 [XAML/C# 錯誤處理的逐步解說](error-handling-in-xamlc-walkthrough.md)。

    下列範例顯示實作錯誤呼叫所需的基本程式碼。 這個 XAML 程式碼會定義用來顯示錯誤訊息的 **TextBlock**。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    這個 C# 程式碼會擷取錯誤訊息並在 **TextBlock** 中顯示。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    造成黑色方塊的常見錯誤為「沒有可用的廣告」。 這個錯誤代表要求沒有傳回可用的廣告。

3.  **AdControl** 運作正常。 有時候，同樣的廣告可能連續出現超過一次，使之看起像是沒有重新整理。

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>廣告沒有重新整理

1.  檢查 [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) 屬性 (其屬於您的 **AdControl**) 是否設為 false。 根據預設，這個選用的屬性會設為 **true**。 當設為 **false** 時，必須使用 **Refresh** 方法來擷取另一個廣告。

2.  檢查對 [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) 方法的呼叫。 使用自動重新整理 (**IsAutoRefreshEnabled** 為 **true**) 時，**Refresh** 無法用來擷取其他廣告。 使用手動重新整理 (**IsAutoRefreshEnabled** 為 **false**) 時，僅應在至少 30 到 60 秒後 (取決於裝置的目前資料連線) 才呼叫 **Refresh**。

    下列範例示範如何呼叫 **Refresh** 方法。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  **AdControl** 運作正常。 有時候，同樣的廣告可能連續出現超過一次，使之看起像是沒有重新整理。

 

 
