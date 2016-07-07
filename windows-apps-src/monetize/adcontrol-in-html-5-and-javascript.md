---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "了解如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML app 使用 AdControl 類別來顯示橫幅廣告。"
title: "HTML 5 和 Javascript 中的 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 6e96b085132126a2c3e7b0b0b86124aba4cd651e

---

# HTML 5 和 Javascript 中的 AdControl


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文會逐步說明如何在 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML app 使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別來顯示橫幅廣告。 本文中不會使用 **AdMediatorControl** 或廣告流量分配。

如需示範如何將橫幅廣告新增到 JavaScript/HTML app 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

## 先決條件


* 使用 Visual Studio 2015 或 Visual Studio 2013 安裝 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。

> 
            **注意** 如果您已安裝 Windows 10 Anniversary SDK Preview Build 14295 或更新版本，並搭配 Visual Studio 2015，您也必須安裝 WinJS 程式庫。 這個程式庫原本包含在舊版的適用於 Windows 10 的 Windows SDK 中，但是從 Windows 10 Anniversary SDK 預覽版 14295 起必須另外安裝。 若要安裝 WinJS，請參閱[取得 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

## 程式碼開發

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 \[任何 CPU\]，請將您的專案更新成使用架構特定的建置輸出 (例如，\[x86\])。 如果專案的目標是 \[任何 CPU\]，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱專案中因目標為 \[任何 CPU\] 所造成的參考錯誤。

3.  在 \[方案總管\] 視窗中的 \[參考\] 上按一下滑鼠右鍵，然後選取 \[加入參考\]。

4.  在 \[參考管理員\] 中，根據您的專案類型選取下列其中一項參考︰

    -   對於通用 Windows 平台 (UWP) 專案：展開 \[通用 Windows\]，按一下 \[擴充功能\]，然後選取 \[適用於 JavaScript 的 Microsoft Advertising SDK (Version 10.0)\] 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 \[Windows 8.1\]，按一下 \[擴充功能\]，然後選取 \[適用於 Windows 8.1 Native 的 Microsoft Advertising SDK (JS)\] 旁邊的核取方塊。

    -   對於 Windows 8.1 專案：展開 \[Windows Phone 8.1\]，按一下 \[擴充功能\]，然後選取 \[Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)\] 旁邊的核取方塊。

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > 
            **注意** 這個影像是 Visual Studio 2015 建置適用於 Windows 10 的 UWP 專案。 如果您是使用 Visual Studio 2013 建置 Windows 8.1 或 Windows Phone 8.1 的 app，則畫面看起來會不同。

5.  在 \[參考管理員\] 中，按一下 \[確定\]。

6.  開啟 default.html 檔案 (或其他 html 檔案，視您的專案而定)。

7.  在 **&lt;head&gt;** 區段中，在專案的 default.css 和 default.js JavaScript 參考後面新增 ad.js 的參考。

    在 UWP 專案中，新增下列程式碼。

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    在 Windows 8.1 或 Windows Phone 8.1 專案中，新增下列程式碼。

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > 
            **注意** 這一行必須放置於 **&lt;head&gt;** 區段所包含的 default.js 之後，否則建置專案時會發生錯誤。

8.  修改 default.html 檔案 (或其他 html 檔案，視您的專案而定) 中的 **&lt;body&gt;** 區段，以包含 **AdControl** 的 div。 指派 **AdControl** 中的 **applicationId** 和 **adUnitId** 屬性，以測試[測試模式值](test-mode-values.md)中所提供的值，並調整控制項的高度與寬度，以符合其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > **注意**  
    在提交 App 之前，請以實際值取代測試的 **applicationId** 和 **adUnitId** 值。

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
          data-win-control="MicrosoftNSJS.Advertising.AdControl"
          data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

9.  編譯並執行 App 來查看其中的廣告。

## 使用 Windows 開發人員中心發行包含即時廣告的 App


1.  在開發人員中心儀表板中，移至 App 的 \[營利\]\[利用廣告營利\] 頁面，並建立獨立的 Microsoft Advertising 單位。 單位類型請選取 \[橫幅\]。 記下廣告單位識別碼與應用程式識別碼。

2.  在您的程式碼中，將測試的廣告單位值 (**applicationId** 和 **adUnitId**)，用在開發人員中心產生的實際值取代。

3.  使用開發人員中心儀表板[提交您的 App](../publish/app-submissions.md) 到市集。

4.  在開發人員中心儀表板上檢閱[廣告績效報告](../publish/advertising-performance-report.md)。

## 完成範例 UWP 專案的 default.html。


``` syntax
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>My_Windows_10_Ad_Funded_JavaScript_App</title>

    <!-- WinJS references -->
    <link href="//Microsoft.WinJS.2.0.Preview/css/ui-dark.css" rel="stylesheet" />
    <script src="//Microsoft.WinJS.2.0.Preview/js/base.js"></script>
    <script src="//Microsoft.WinJS.2.0.Preview/js/ui.js"></script>

    <!-- My_Windows_10_Ad_Funded_JavaScript_App references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>

    <!-- Microsoft advertising required references -->
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

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
 

 



<!--HONumber=Jun16_HO4-->


