---
author: Xansky
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: 了解如何在 Windows 10 的 JavaScript/HTML 應用程式 (UWP) 中使用 AdControl 類別來顯示橫幅廣告。
title: HTML 5 和 JavaScript 中的 AdControl
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, AdControl, 廣告控制項, JavaScript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: df5623b8c73dc6c96c2869156d22da64f6a6b58d
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7436272"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 和 JavaScript 中的 AdControl

本文會逐步說明如何在 Windows 10 的通用 Windows 平台 (UWP) JavaScript/HTML 應用程式中使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 類別來顯示橫幅廣告。

如需示範如何將橫幅廣告新增到 JavaScript/HTML app 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>先決條件

* 使用 Visual Studio 2015 或更新版本的 Visual Studio 來安裝 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)。 如需安裝指示，請參閱[本文](install-the-microsoft-advertising-libraries.md)。

> [!NOTE]
> 如果您已安裝 Windows 10 SDK 版本 10.0.14393 （年度更新版） 或更新版本的 Windows SDK，您還必須安裝[WinJS](https://github.com/winjs/winjs)程式庫。 這個程式庫原本包含在舊版的適用於 Windows 10 的 Windows SDK 中，但是從 Windows 10 SDK 版本 10.0.14393 (年度更新版) 起必須另外安裝。 

## <a name="integrate-a-banner-ad-into-your-app"></a>將橫幅廣告整合至您的應用程式

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

    > [!NOTE]
    > 如果您正在使用現有的專案，請在專案中開啟 Package.appxmanifest 檔案，並確保選取 **\[網際網路 (用戶端)\]** 功能。 您的應用程式需要這項功能來接收測試廣告和即時廣告。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在您的專案中新增 Microsoft Advertising SDK 的參考：

    1. 在 **\[方案總管\]** 視窗中的 **\[參考\]** 上按一下滑鼠右鍵，然後選取 **\[加入參考\]**。
    2.  在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**、按一下 **\[擴充功能\]**，然後選取 **\[Microsoft Advertising SDK for JavaScript\]** (Version 10.0) 旁邊的核取方塊。
    3.  在 **\[參考管理員\]** 中，按一下 \[確定\]。

6.  開啟 index.html 檔案 (或其他適用於您專案的 HTML 檔案)。

7.  在 **&lt;head&gt;** 區段中專案的 default.css 及 main.js JavaScript 參考後方，將參考新增至 ad.js。

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > 這一行必須放在 **&lt;head&gt;** 區段所包含的 main.js 之後，否則建置專案時會發生錯誤。

8.  修改 default.html 檔案 (或其他 html 檔案，視您的專案而定) 中的 **&lt;body&gt;** 區段，以包含 **AdControl** 的 **div**。 將 **AdControl** 的 **applicationId** 和 **adUnitId** 屬性指派為[測試廣告單元值](set-up-ad-units-in-your-app.md#test-ad-units)。 同時調整控制項的**高度**與**寬度**，使其成為[橫幅廣告受支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!NOTE]
    > 每個 **AdControl** 都有對應的*廣告單元*，由我們的服務用來提供廣告給控制項，且每個廣告單元都包含*廣告單元 ID* 和*應用程式 ID*。 在這些步驟中，您將指派測試廣告單元 ID 和應用程式 ID 值到您的控制項。 這些測試值只能在您應用程式的測試版本中使用。 您將您的應用程式發行至市集之前，您必須[取代這些測試值，請以實際值](#release)從合作夥伴中心。

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  編譯並執行應用程式來查看其中的廣告。

下面的範例示範簡單應用程式的完整 index.html。

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

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>在 JavaScript 中以程式設計方式建立 AdControl

上述步驟顯示如何在您的 HTML 標記中宣告 **AdControl**。 或者，您也可以以程式設計方式，使用 JavaScript 建立 **AdControl**。 此範例假設您透過識別碼 **myAd**，在 HTML 中使用現有的 **div**。

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

此範例假設您已宣告名為 **myAdError**、**myAdRefreshed** 和 **myAdEngagedChanged** 的事件處理常式方法。

若您使用此程式碼但未看到廣告，則可嘗試將 **position:relative** 的屬性，插入於包含 **AdControl** 的 **div**。 這將會覆寫 **IFrame** 的預設設定。 將會正確顯示廣告，除非由於此屬性的值致使未顯示這些廣告。 請注意，可能會有長達 30 分鐘的時間無法使用新廣告單位。

> [!NOTE]
> 此範例中所顯示的 *applicationId* 和 *adUnitId* 值是[測試模式值](set-up-ad-units-in-your-app.md#test-ad-units)。 您必須[將這些值，請以實際值取代為](set-up-ad-units-in-your-app.md#live-ad-units)從合作夥伴中心提交您的應用程式提交之前。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>發行包含即時廣告的應用程式

1. 請確定您在應用程式中使用橫幅廣告的方式遵循我們的[橫幅廣告指南](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)。

1.  在合作夥伴中心，移至[應用程式內廣告](../publish/in-app-ads.md)頁面，並[建立廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。 廣告單元類型請指定 **\[橫幅\]**。 記下廣告單元識別碼與應用程式識別碼。
    > [!NOTE]
    > 測試廣告單元和即時 UWP 廣告單元的應用程式識別碼值有不同的格式。 測試應用程式識別碼值為 GUID。 當您在合作夥伴中心建立即時 UWP 廣告單元時，廣告單元的應用程式識別碼值一律符合您應用程式 （microsoft Store 識別碼值範例類似於 9NBLGGH4R315） 的 「 市集識別碼。

2. 您可以選擇為 **AdControl** 啟用廣告流量分配，方法是在 [\[應用程式內廣告\]](../publish/in-app-ads.md) 頁面的 [\[利用廣告獲利\]](../publish/in-app-ads.md#mediation) 區段進行設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路如 Taboola 和 Smaato 的廣告，以及 Microsoft 應用程式促銷活動廣告。

3.  在程式碼，請在合作夥伴中心產生的實際值取代測試的廣告單元值 （**applicationId**和**adUnitId**）。

4.  [您的應用程式提交](../publish/app-submissions.md)至市集使用合作夥伴中心。

5.  檢閱您的[廣告績效報告](../publish/advertising-performance-report.md)合作夥伴中心。             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理您應用程式中多個廣告控制項的廣告單元

您可以在單一應用程式中使用多個 **AdControl** 物件 (例如，您應用程式中的每個頁面可能會裝載不同的 **AdControl** 物件)。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/in-app-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [橫幅廣告指南](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [為您的 App 設定廣告單元](set-up-ad-units-in-your-app.md)
* [JavaScript 錯誤處理的逐步解說](error-handling-in-javascript-walkthrough.md)
