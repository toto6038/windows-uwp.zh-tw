---
author: Xansky
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: 了解如何在 Windows 10 的 XAML 應用程式 (UWP) 中使用 AdControl 類別來顯示橫幅廣告。
title: XAML 和 .NET 中的 AdControl
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10 , UWP, 廣告, AdControl, 廣告控制項, XAML, .NET, 逐步解說
ms.localizationpriority: medium
ms.openlocfilehash: 600939c91b67b57e3c900d33cebe659bcae6ef7a
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5932767"
---
# <a name="adcontrol-in-xaml-and-net"></a>XAML 和 .NET 中的 AdControl


本文會逐步說明如何在使用 C# 實作的適用於 Windows 10 的通用 Windows 平台 (UWP) XAML 應用程式中，使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 類別來顯示橫幅廣告。

> [!NOTE]
> Microsoft Advertising SDK 也支援使用 C++ 實作的 XAML 應用程式。 如需完整的範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>必要條件

* 使用 Visual Studio 2015 或更新版本的 Visual Studio 來安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。 如需安裝指示，請參閱[本文](install-the-microsoft-advertising-libraries.md)。

## <a name="integrate-a-banner-ad-into-your-app"></a>將橫幅廣告整合至您的應用程式

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

    > [!NOTE]
    > 如果您正在使用現有的專案，請在專案中開啟 Package.appxmanifest 檔案，並確保選取 **\[網際網路 (用戶端)\]** 功能。 您的應用程式需要這項功能來接收測試廣告和即時廣告。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在您的專案中新增 Microsoft Advertising SDK 的參考：

    1. 在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。
    2.  在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[適用於 XAML 的 Microsoft Advertising SDK (Version 10.0)\]** 旁邊的核取方塊。
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

5. 在 **Grid** 標籤中，加入 **AdControl** 程式碼。 將 [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 和 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 屬性指派給[測試廣告單元值](set-up-ad-units-in-your-app.md#test-ad-units)。 同時調整控制項的**高度**與**寬度**，使其成為[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!NOTE]
    > 每個 **AdControl** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元 ID* 和*應用程式 ID*。 在這些步驟中，您將指派測試廣告單元 ID 和應用程式 ID 值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到 Microsoft Store 之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

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

## <a name="release-your-app-with-live-ads"></a>發行包含即時廣告的應用程式

1. 請確定您在應用程式中使用橫幅廣告的方式遵循我們的[橫幅廣告指南](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)。

2.  在開發人員中心儀表板中，移至[應用程式內廣告](../publish/in-app-ads.md)頁面，並且[建立廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。 廣告單元類型請指定 **\[橫幅\]**。 記下廣告單元識別碼與應用程式識別碼。
    > [!NOTE]
    > 測試廣告單元和即時 UWP 廣告單元的應用程式識別碼值有不同的格式。 測試應用程式識別碼值為 GUID。 當您在儀表板中建立即時 UWP 廣告單元時，廣告單元的應用程式識別碼值一律符合您應用程式的 Microsoft Store 識別碼 (Microsoft Store 識別碼值範例類似於 9NBLGGH4R315)。

3. 您可以選擇為 **AdControl** 啟用廣告流量分配，方法是在 [\[應用程式內廣告\]](../publish/in-app-ads.md) 頁面的 [\[利用廣告獲利\]](../publish/in-app-ads.md#mediation) 區段進行設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路如 Taboola 和 Smaato 的廣告，以及 Microsoft 應用程式促銷活動廣告。

4.  在您的程式碼中，將測試的廣告單位值 (**ApplicationId** 和 **AdUnitId**)，用在開發人員中心產生的實際值取代。

5.  使用開發人員中心儀表板將[您的應用程式提交](../publish/app-submissions.md)至 Microsoft Store。

6.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理您應用程式中多個廣告控制項的廣告單元

您可以在單一應用程式中使用多個 **AdControl** 物件 (例如，您應用程式中的每個頁面可能會裝載不同的 **AdControl** 物件)。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/in-app-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [橫幅廣告指南](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [XAML/C# 錯誤處理的逐步解說](error-handling-in-xamlc-walkthrough.md)。
* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [為您的 App 設定廣告單元](set-up-ad-units-in-your-app.md)
