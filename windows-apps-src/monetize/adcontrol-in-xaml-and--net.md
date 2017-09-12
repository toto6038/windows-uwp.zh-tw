---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "了解如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 XAML 應用程式使用 AdControl 類別來顯示橫幅廣告。"
title: "XAML 和 .NET 中的 AdControl"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10 , UWP, 廣告, AdControl, 廣告控制項, XAML, .NET, 逐步解說"
ms.openlocfilehash: be273ca4c17edb4affa5e0abb4b3317b03893280
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-xaml-and-net"></a>XAML 和 .NET 中的 AdControl


本文會逐步說明如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 XAML 應用程式使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別來顯示橫幅廣告。

如需示範如何將橫幅廣告新增到使用 C# 和 C++ 的 XAML 應用程式的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>先決條件

* 對於 UWP 應用程式：請使用 Visual Studio 2015 或較新版本安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。
* 對於 Windows 8.1 或 Windows Phone 8.1 應用程式：請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 與 Visual Studio 2015 或 Visual Studio 2013。

## <a name="code-development"></a>程式碼開發

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

1.  在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。

2.  在 **\[參考管理員\]** 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 **\[通用 Windows\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。

    -   Windows 8.1 專案：展開 **\[Windows 8.1\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 Windows 8.1 XAML 的 Ad Mediator SDK\]** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

    -   對於 Windows Phone 8.1 專案：展開 **\[Windows Phone 8.1\]**，按一下 **\[擴充功能\]**，然後選取 **\[適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK\]** 旁邊的核取方塊。 這個選項會將 Microsoft Advertising 和 Ad Mediator 程式庫都新增到專案，但您可以忽略 Ad Mediator 程式庫。

    ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

3.  在 **\[參考管理員\]** 中，按一下 [確定]。

4.  於內嵌 **Microsoft.Advertising.WinRT.UI** 命名空間的頁面修改 XAML。 例如，在 Visual Studio 產生的預設範例應用程式 (同此應用程式中的 MyAdFundedWindows10AppXAML)，該 XAML 頁面是 **MainPage.XAML**。

    Visual Studio 產生的 MainPage.xaml 中的 **Page** 區段會有下列程式碼。

    ``` xml
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

    ``` xml
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

5. 在 **Grid** 標籤中，加入 **AdControl** 程式碼。 在 **Page** 中，將 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 和 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 屬性指派為[測試模式值](test-mode-values.md)中提供的測試值。 同時調整控制項的高度與寬度，使其成為[橫幅廣告受支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!NOTE]
    > 每個 **AdControl** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元 ID* 和*應用程式 ID*。 在這些步驟中，您將指派測試廣告單元 ID 和應用程式 ID 值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到市集之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

    完整的 **Grid** 標籤就像這個程式碼。

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    MainPage.xaml 的完整程式碼看起來應該像這樣。

    ``` xml
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
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  編譯並執行應用程式來查看其中的廣告。

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 開發人員中心發行包含即時廣告的應用程式

1.  在開發人員中心儀表板中，移至應用程式的 [\[利用廣告獲利\]](../publish/monetize-with-ads.md) 頁面，並[建立廣告單元](../monetize/set-up-ad-units-in-your-app.md)。 單位類型請指定 **\[橫幅\]**。 記下廣告單位識別碼與應用程式識別碼。

2. 如果您的應用程式是適用於 Windows 10 的 UWP 應用程式，您可以選擇為應用程式中的 **AdControl** 控制項啟用廣告流量分配，方法是透過 [\[利用廣告獲利\]](../publish/monetize-with-ads.md#mediation) 頁面中的 [\[廣告流量分配\]](../publish/monetize-with-ads.md) 設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路如 Taboola 和 Smaato 的廣告，以及 Microsoft 應用程式促銷活動廣告。

3.  在您的程式碼中，將測試的廣告單位值 (**ApplicationId** 和 **AdUnitId**)，用在開發人員中心產生的實際值取代。

4.  使用開發人員中心儀表板將[您的應用程式提交](../publish/app-submissions.md)至市集。

5.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理您應用程式中多個廣告控制項的廣告單元

您可以在單一應用程式中使用多個 **AdControl** 物件 (例如，您應用程式中的每個頁面可能會裝載不同的 **AdControl** 物件)。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/monetize-with-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="notes"></a>注意

* C#：如需如何將事件處理器指派給 **AdControl** 事件的範例，請參閱 [XAML 屬性範例](xaml-properties-example.md)。 接著，參閱 [C# 中的 AdControl 事件](adcontrol-events-in-c.md)，以查看以 C# 撰寫之事件處理常式的範例程式碼。

* C ++：目前的 Microsoft Advertising 程式庫版本支援 C++。 **AdControl** 類別是以原生 C++ 實作，並不會載入 .NET CLR。 如需示範如何使用 C++ 中之 **AdControl** 的程式碼範例，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

* Visual Basic：如需如何將事件處理常式指派給 **AdControl** 事件的範例，請參閱 [XAML 屬性範例](xaml-properties-example.md)。

* 錯誤處理：若要了解如何處理錯誤，請參閱 [AdControl 錯誤處理](adcontrol-error-handling.md)。

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [為您的 App 設定廣告單元](../monetize/set-up-ad-units-in-your-app.md)
