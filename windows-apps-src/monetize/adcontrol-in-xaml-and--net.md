---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: 了解如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 XAML app 使用 AdControl 類別來顯示橫幅廣告。
title: XAML 和 .NET 中的 AdControl
---

# XAML 和 .NET 中的 AdControl


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文會逐步說明如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 XAML app 使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別來顯示橫幅廣告。 本文中不會使用 **AdMediatorControl** 或廣告流量分配。

如需示範如何將橫幅廣告新增到使用 C# 和 C++ 的 XAML app 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## 先決條件

* 使用 Visual Studio 2015 或 Visual Studio 2013 安裝 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。

## 程式碼開發

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 [任何 CPU]****，請將您的專案更新成使用架構特定的建置輸出 (例如，[x86]****)。 如果專案的目標是 [任何 CPU]****，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

1.  在 [方案總管]**** 視窗中的 [參考]**** 上按一下滑鼠右鍵，然後選取 [加入參考]****。

2.  在 [參考管理員]**** 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 [通用 Windows]****，按一下 [擴充功能]****，然後選取 [適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)]**** 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 [Windows 8.1]****，按一下 [擴充功能]****，然後選取 [適用於 Windows 8.1 XAML 的 Ad Mediator SDK]**** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

    -   對於 Windows Phone 8.1 專案：展開 [Windows Phone 8.1]****，按一下 [擴充功能]****，然後選取 [適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK]**** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

  ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

    > **注意** 這個影像是 Visual Studio 2015 建置適用於 Windows 10 的 UWP 專案。 如果您是使用 Visual Studio 2013 建置 Windows 8.1 或 Windows Phone 8.1 的 app，則畫面看起來會不同。

3.  在 [參考管理員]**** 中，按一下 [確定]。
4.  於內嵌 **Microsoft.Advertising.WinRT.UI** 命名空間的頁面修改 XAML。 例如，在 Visual Studio 產生的預設範例 App (同此 App 中的 MyAdFundedWindows10AppXAML)，該 XAML 頁面是 **MainPage.XAML**。

    Visual Studio 產生的 MainPage.xaml 中的 **Page** 區段會有下列程式碼。

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

    新增 **Microsoft.Advertising.WinRT.UI** 命名空間參考，使 MainPage.xaml 的 **Page** 區段程式碼如下：

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

5.  在 **Grid** 標籤中，加入 **AdControl** 程式碼。

    1.  在 **Page** 中，將 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 屬性指派為[測試模式值](test-mode-values.md)中提供的測試值。

        > **注意** 在提交 App 之前，請以實際值取代測試值。

    2.  調整控制項的高度和寬度，以符合[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    完整的 **Grid** 標籤就像這個程式碼。

    ``` syntax
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
    </Grid>
    ```

    MainPage.xaml 的完整程式碼看起來應該像這樣。

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
        </Grid>
    </Page>
    ```

6.  編譯並執行 App 來查看其中的廣告。

## 使用 Windows 開發人員中心發行包含即時廣告的 App


1.  在開發人員中心儀表板中，移至 App 的 [營利]****&gt;[利用廣告營利]**** 頁面，並[建立獨立的 Microsoft Advertising 單位](../publish/monetize-with-ads.md)。 單位類型請選取 [橫幅]****。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值 (**ApplicationId** 和 **AdUnitId**)，用在開發人員中心產生的實際值取代。

3.  使用開發人員中心儀表板[提交您的 App](../publish/app-submissions.md) 到市集。

4.  在開發人員中心儀表板上檢閱[廣告績效報告](../publish/advertising-performance-report.md)。

## 注意

C#：如需如何將事件處理器指派給 **AdControl** 事件的範例，請參閱 [XAML 屬性範例](xaml-properties-example.md)。 接著，以 C# 撰寫之事件處理器的範例程式碼，請[參閱 C# 中的 AdControl 事件](adcontrol-events-in-c.md)。

Visual Basic：如需如何將事件處理器指派給 **AdControl** 事件的範例，請參閱 [XAML 屬性範例](xaml-properties-example.md)。

C ++：目前版本的 Microsoft advertising 程式庫支援 C++。 **AdControl** 會載入 CLR 並使用管制型 C++。

錯誤處理：若要了解如何處理錯誤，請參閱 [AdControl 錯誤處理](adcontrol-error-handling.md)。

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->


