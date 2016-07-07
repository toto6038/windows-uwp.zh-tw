---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "了解如何安裝 Microsoft Advertising 程式庫。"
title: "安裝 Microsoft Advertising 程式庫"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 0951818ceaf3d96543f9f97ec6993d08fdaab2b8


---

# 安裝 Microsoft Advertising 程式庫


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

適用於 Windows 應用程式的 Microsoft Advertising 程式庫已包含在 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) 之中。 這個 SDK 是 Visual Studio 2015 和 Visual Studio 2013 的延伸模組。

如需安裝指示，請參閱[利用 Microsoft Store Engagement and Monetization SDK 讓您的 App 獲利及吸引客戶](https://msdn.microsoft.com/windows/uwp/monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk)。

> 
            **注意** 如果您已安裝 Windows 10 Anniversary SDK Preview Build 14295 或更新版本並搭配 Visual Studio 2015，若您想要將廣告新增到 JavaScript/HTML App 中，便同時需要安裝 WinJS 程式庫。 這個程式庫原本包含在舊版的適用於 Windows 10 的 Windows SDK 中，但是從 Windows 10 Anniversary SDK 預覽版 14295 起必須另外安裝。 若要安裝 WinJS，請參閱[取得 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

## 針對廣告和廣告流量分配的程式庫名稱


Microsoft Store Engagement and Monetization SDK 包含兩組廣告程式庫：Microsoft Advertising 的程式庫 (能為 XAML 和 JavaScript/HTML App 提供 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 類別)，以及廣告流量分配程式庫的程式庫 (能提供 **AdMediatorControl** 類別)。

本文件說明使用 Microsoft Advertising 程式庫的 **AdControl** 和 **InterstitialAd** 類別來顯示 Microsoft 橫幅或插入式廣告影片的方式。 如需使用廣告流量分配的資訊，請參閱[使用廣告流量分配來獲得最佳收益](https://msdn.microsoft.com/windows/uwp/monetize/use-ad-mediation-to-maximize-revenue)。


在您可以在 App 程式碼中使用任何廣告控制項之前，您必須在專案中參考適當的程式庫。 下列表格列出 Microsoft Store Engagement and Monetization SDK 中每個程式庫的名稱，排序方式與它們在 Visual Studio 的 \[參考管理員\] 對話方塊中所呈現的方式相同。


<table>
    <thead>
        <tr><th>控制項名稱</th><th>專案類型</th><th>在參考管理員中的程式庫名稱</th><th>版本號碼</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">
            **AdControl** 和 **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>適用於 XAML 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>適用於 Windows 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">
            **AdControl** 和 **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>適用於 JavaScript 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>適用於 Windows 8.1 Native (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>適用於 Windows Phone 8.1 Native (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">
            **AdMediatorControl** (僅 XAML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising Universal SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>適用於 Windows 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 



<!--HONumber=Jun16_HO4-->


