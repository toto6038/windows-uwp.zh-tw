---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "了解如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 應用程式使用 AdControl 類別來顯示橫幅廣告。"
title: "HTML 5 和 JavaScript 中的 AdControl"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, 廣告, AdControl, 廣告控制項, JavaScript, HTML"
ms.openlocfilehash: 44417516d773ea4faf103f6d4cdaf0bc8f290921
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 和 JavaScript 中的 AdControl

本文會逐步說明如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 應用程式使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別來顯示橫幅廣告。

如需示範如何將橫幅廣告新增到 JavaScript/HTML app 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>先決條件


* 對於 UWP 應用程式：請使用 Visual Studio 2015 或較新版本安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。
* 對於 Windows 8.1 或 Windows Phone 8.1 應用程式：請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 與 Visual Studio 2015 或 Visual Studio 2013。

> [!NOTE]
> 如果您將橫幅廣告新增至 UWP app，而且您已安裝 Windows 10 SDK 版本 10.0.14393 (年度更新版) 或更新版本的 Windows SDK，則還必須安裝 WinJS 程式庫。 這個程式庫原本包含在舊版的適用於 Windows 10 的 Windows SDK 中，但是從 Windows 10 SDK 版本 10.0.14393 (年度更新版) 起必須另外安裝。 若要安裝 WinJS，請參閱[取得 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

## <a name="code-development"></a>程式碼開發

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3.  在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。

4.  在 **\[參考管理員\]** 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 [通用 Windows]****，按一下 [擴充功能]****，然後選取 [適用於 JavaScript 的 Microsoft Advertising SDK (Version 10.0)]**** 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 [Windows 8.1]****，按一下 [擴充功能]****，然後選取 [適用於 Windows 8.1 Native 的 Microsoft Advertising SDK (JS)]**** 旁邊的核取方塊。

    -   Windows Phone 8.1 專案：展開 **\[Windows Phone 8.1\]**，按一下 **\[擴充功能\]**，然後選取 **\[Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)\]** 旁邊的核取方塊。

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

5.  在 **\[參考管理員\]** 中，按一下 [確定]。

6.  開啟 index.html 檔案 (或其他適用於您專案的 HTML 檔案)。

7.  在 **&lt;head&gt;** 區段中專案的 default.css 及 main.js JavaScript 參考後方，將參考新增至 ad.js。

    在 UWP 專案中，新增下列程式碼。

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    在 Windows 8.1 或 Windows Phone 8.1 專案中，新增下列程式碼。

    ``` HTML
    <!-- Advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > [!NOTE]
    > 這一行必須放在 **&lt;head&gt;** 區段所包含的 main.js 之後，否則建置專案時會發生錯誤。 若您的專案目標為 Windows 8.1 或 Windows Phone 8.1，則您專案中的預設 HTML 檔案會名為 default.html 而非 index.html，且您專案中的預設 JavaScript 檔案會名為 default.js 而非 main.js。

8.  修改 default.html 檔案 (或其他 html 檔案，視您的專案而定) 中的 **&lt;body&gt;** 區段，以包含 **AdControl** 的 div。 在 **AdControl** 中，將 **applicationId** 和 **adUnitId** 屬性指派為[測試模式值](test-mode-values.md)中提供的測試值。 同時調整控制項的高度與寬度，使其成為[橫幅廣告受支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!NOTE]
    > 每個 **AdControl** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元 ID* 和*應用程式 ID*。 在這些步驟中，您將指派測試廣告單元 ID 和應用程式 ID 值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 在您發行應用程式到市集之前，您必須[以 Windows 開發人員中心的實際值取代這些測試值](#release)。

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  編譯並執行應用程式來查看其中的廣告。

下面的範例示範簡單 UWP 應用程式的完整 index.html。

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 開發人員中心發行包含即時廣告的應用程式

1.  在開發人員中心儀表板中，移至應用程式的 [\[利用廣告獲利\]](../publish/monetize-with-ads.md) 頁面，並[建立廣告單元](../monetize/set-up-ad-units-in-your-app.md)。 廣告單元類型請指定 **\[橫幅\]**。 記下廣告單元識別碼與應用程式識別碼。

2. 如果您的應用程式是適用於 Windows 10 的 UWP 應用程式，您可以選擇為應用程式中的 **AdControl** 控制項啟用廣告流量分配，方法是透過 [\[利用廣告獲利\]](../publish/monetize-with-ads.md#mediation) 頁面中的 [\[廣告流量分配\]](../publish/monetize-with-ads.md) 設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路如 Taboola 和 Smaato 的廣告，以及 Microsoft 應用程式促銷活動廣告。

3.  在您的程式碼中，將測試的廣告單元值 (**applicationId** 和 **adUnitId**)，用在開發人員中心產生的實際值取代。

4.  使用開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md) 到市集。

5.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。             

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理您應用程式中多個廣告控制項的廣告單元

您可以在單一應用程式中使用多個 **AdControl** 物件 (例如，您應用程式中的每個頁面可能會裝載不同的 **AdControl** 物件)。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/monetize-with-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [為您的 App 設定廣告單元](../monetize/set-up-ad-units-in-your-app.md)
