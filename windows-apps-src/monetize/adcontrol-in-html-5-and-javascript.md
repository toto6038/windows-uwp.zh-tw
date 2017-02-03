---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "了解如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 應用程式使用 AdControl 類別來顯示橫幅廣告。"
title: "HTML 5 和 JavaScript 中的 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: 509cfe00050c5b5b3997af0e2906676f946d9278

---

# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 和 JavaScript 中的 AdControl

本文會逐步說明如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 應用程式使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別來顯示橫幅廣告。 本文中不會使用 **AdMediatorControl** 或廣告流量分配。

如需示範如何將橫幅廣告新增到 JavaScript/HTML 應用程式的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>先決條件


* 對於 UWP 應用程式︰請安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 與 Visual Studio 2015。
* 對於 Windows 8.1 或 Windows Phone 8.1 應用程式：請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 與 Visual Studio 2015 或 Visual Studio 2013。

> **備註**&nbsp;&nbsp;若您已安裝 Windows 10 年度 SDK 預覽版 14295 或更新版本與 Visual Studio 2015，則必須同時安裝 WinJS 程式庫。 這個程式庫原本包含在舊版的適用於 Windows 10 的 Windows SDK 中，但是從 Windows 10 Anniversary SDK 預覽版 14295 起必須另外安裝。 若要安裝 WinJS，請參閱[取得 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

## <a name="code-development"></a>程式碼開發

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 [任何 CPU]****，請將您的專案更新成使用架構特定的建置輸出 (例如，[x86]****)。 如果專案的目標是 [任何 CPU]****，您將無法於下列步驟中成功加入 Microsoft Advertising 程式庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3.  在 [方案總管]**** 視窗中的 [參考]**** 上按一下滑鼠右鍵，然後選取 [加入參考]****。

4.  在 \[參考管理員\] 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 [通用 Windows]****，按一下 [擴充功能]****，然後選取 [適用於 JavaScript 的 Microsoft Advertising SDK (Version 10.0)]**** 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 [Windows 8.1]****，按一下 [擴充功能]****，然後選取 [適用於 Windows 8.1 Native 的 Microsoft Advertising SDK (JS)]**** 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 [Windows Phone 8.1]****，按一下 [擴充功能]****，然後選取 [Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)]**** 旁邊的核取方塊。

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > **備註**&nbsp;&nbsp;此映像適用於為 Windows 10 建置 UWP 專案的 Visual Studio 2015。 如果您是使用 Visual Studio 2013 建置 Windows 8.1 或 Windows Phone 8.1 的應用程式，則畫面看起來會不同。

5.  在 [參考管理員]**** 中，按一下 [確定]。

6.  開啟 index.html 檔案 (或其他適用於您專案的 HTML 檔案)。

7.  在 **&lt;head&gt;** 區段中專案的 default.css 及 main.js JavaScript 參考後方，將參考新增至 ad.js。

  在 UWP 專案中，新增下列程式碼。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <!-- Microsoft advertising required references -->
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  在 Windows 8.1 或 Windows Phone 8.1 專案中，新增下列程式碼。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <!-- Microsoft advertising required references -->
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

  <span/>
  >**備註**&nbsp;&nbsp;此行必須在包含 main.js 後置於 **&lt;head&gt;** 區段，否則您會在建置專案時發生錯誤。 若您的專案目標為 Windows 8.1 或 Windows Phone 8.1，則您專案中的預設 HTML 檔案會名為 default.html 而非 index.html，且您專案中的預設 JavaScript 檔案會名為 default.js 而非 main.js。

8.  修改 default.html 檔案 (或其他 html 檔案，視您的專案而定) 中的 **&lt;body&gt;** 區段，以包含 **AdControl** 的 div。 指派 **AdControl** 中的 **applicationId** 和 **adUnitId** 屬性為[測試模式值](test-mode-values.md)中所提供的值，並調整控制項的高度與寬度，以符合其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

  > **注意**&nbsp;&nbsp;在提交應用程式之前，請以實際值取代測試的 **applicationId** 和 **adUnitId** 值。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
  </div>
  ```

9.  編譯並執行應用程式來查看其中的廣告。

## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 開發人員中心發行包含即時廣告的應用程式


1.  在開發人員中心儀表板中，移至應用程式的 [營利]**** &gt;[利用廣告營利]**** 頁面，並[建立獨立的 Microsoft Advertising 單位](../publish/monetize-with-ads.md)。 單位類型請指定 [橫幅]****。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值 (**applicationId** 和 **adUnitId**)，用在開發人員中心產生的實際值取代。

3.  使用開發人員中心儀表板[提交您的應用程式](../publish/app-submissions.md) 到市集。

4.  在開發人員中心儀表板中檢閱您的[廣告效益報表](../publish/advertising-performance-report.md)。

## <a name="complete-indexhtml-for-a-sample-uwp-project"></a>為範例 UWP 專案完成 index.html

> [!div class="tabbedCodeSnippets"]
``` html
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
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
 

 



<!--HONumber=Dec16_HO2-->


