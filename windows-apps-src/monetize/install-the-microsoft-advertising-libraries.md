---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "了解如何安裝 Microsoft Advertising 程式庫。"
title: "安裝 Microsoft Advertising 程式庫"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 通知, 廣告, 安裝, SDK, 程式庫"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 034b597c945f9f12700ac442e4b3014b0bc84c78
ms.lasthandoff: 02/07/2017

---

# <a name="install-the-microsoft-advertising-libraries"></a>安裝 Microsoft Advertising 程式庫




就適用於 Windows 10 的「通用 Windows 平台」(UWP) 應用程式而言，Microsoft Advertising 程式庫是包含在 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 中。 這個 SDK 是 Visual Studio 2015 和更新版本的擴充功能。 如需有關安裝這個 SDK 的詳細資訊，請參閱[這篇文章](microsoft-store-services-sdk.md)。

> **注意**&nbsp;&nbsp;如果已經安裝 Windows 10 SDK (14393) 或更新的版本，如果您想要將廣告加入到 JavaScript/HTML UWP 應用程式，您也必須安裝 WinJS 程式庫。 這個程式庫原本包含在舊版的 Windows 10 SDK 中，但從 Windows 10 SDK (14393) 開始，就必須另外安裝這個程式庫。 若要安裝 WinJS，請參閱[取得 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

針對適用於 Windows 8.1 和 Windows Phone 8.x 的 XAML 與 JavaScript/HTML App，Microsoft Advertising 程式庫隨附於[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 中。 這個 SDK 是 Visual Studio 2015 和 Visual Studio 2013 的擴充功能。

針對 Windows Phone Silverlight 8.x App，Microsoft Advertising 程式庫是在您可以下載並安裝到專案的 NuGet 套件中所提供。 如需詳細資訊，請參閱 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。

## <a name="library-names-for-advertising"></a>適用於廣告的程式庫名稱


在 Microsoft Store Services SDK 以及適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK 中有數個不同的廣告程式庫可供使用：

* Microsoft Store Services SDK 包含 Microsoft Advertising 程式庫 (其提供適用於 XAML 和 JavaScript/HTML App 的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 類別)。

* 適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK 包含兩組廣告程式庫：適用於 Microsoft Advertising 的程式庫 (能為 XAML 和 JavaScript/HTML App 提供 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 類別)，以及適用於廣告流量分配的程式庫 (能提供 **AdMediatorControl** 類別)。

本文件說明如何使用 Microsoft Advertising 程式庫的 **AdControl** 和 **InterstitialAd** 類別來顯示橫幅或插入式廣告影片。 如需針對 Windows 8.1 和 Windows Phone 8.x App 使用廣告流量分配的相關資訊，請參閱[使用廣告流量分配來獲得最佳收益](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)。

>**注意**&nbsp;&nbsp;Windows 10 的 UWP 應用程式目前不支援使用 **AdMediatorControl** 類別的廣告流量分配。 即將推出適用於 UWP app 的伺服器端流量分配，且其橫幅廣告 (**AdControl**) 和插入式影片廣告 (**InterstitialAd**) 使用相同 API。

在您可以於 App 程式碼中使用任何廣告控制項之前，您必須在專案中參考適當的程式庫。 下列表格列出每個程式庫的名稱，排序方式與它們在 Visual Studio 的 **\[參考管理員\]** 對話方塊中所呈現的方式相同。


<table>
    <thead>
        <tr><th>控制項名稱</th><th>專案類型</th><th>在參考管理員中的程式庫名稱</th><th>版本號碼</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">**AdControl** 和 **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>適用於 XAML 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>適用於 Windows 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">**AdControl** 和 **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>適用於 JavaScript 的 Microsoft Advertising SDK</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>適用於 Windows 8.1 Native (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>適用於 Windows Phone 8.1 Native (JS) 的 Microsoft Advertising SDK</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">**AdMediatorControl** (僅 XAML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising Universal SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>適用於 Windows 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>適用於 Windows Phone 8.1 XAML 的 Ad Mediator SDK</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 

