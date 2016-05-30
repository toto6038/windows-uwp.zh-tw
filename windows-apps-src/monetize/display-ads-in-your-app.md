---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Microsoft Store Engagement and Monetization SDK 可提供您幾種方式來使用廣告讓您的應用程式獲利。
title: 在您的應用程式中顯示廣告
---

# 在您的應用程式中顯示廣告


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

[Microsoft Store Engagement and Monetization SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) 可提供您幾種方式來使用廣告讓您的應用程式獲利。

## 顯示使用 Microsoft Advertising 程式庫的橫幅廣告及影片插入式廣告

併入橫幅廣告及影片插入式廣告，以透過您的 Windows app 獲得更多利潤。 適用於電腦、平板電腦和手機之 Windows 應用程式中顯示的廣告。 您可以使用 Windows 開發人員中心儀表板即時監視您的廣告績效。

若要在您的 app 中包含廣告，請在發佈於 Microsoft Store Engagement and Monetization SDK 的廣告程式庫中，使用 **AdControl** 和 **InterstitialAd** 控制項。 您可在適用於 Windows 10、Windows 8.1、Windows Phone 8.1 和 Windows Phone 8 的 XAML 或 JavaScript/HTML app 中，使用這些控制項顯示 Microsoft 提供的橫幅廣告與影片插入式廣告。

如需詳細資訊，請參閱[使用 Microsoft Advertising 程式庫顯示廣告](display-ads-using-the-microsoft-advertising-libraries.md)。 發佈含廣告的 app 後，使用[廣告績效報告](../publish/advertising-performance-report.md)追蹤廣告的效能。                                           

## 針對多個廣告網路的橫幅廣告，使用廣告流量分配

您可在 XAML app 中使用 **AdMediatorControl** 類別，以顯示來自多個廣告網路的橫幅廣告來最佳化廣告收益。 將此控制項新增至您的 app 後，在 Windows 開發人員中心儀表板中設定廣告流量分配設定，而我們會負責針對來自您選擇之廣告網路的橫幅廣告要求進行流量分配。 如需詳細資訊，請參閱[使用廣告流量分配來獲得最佳廣告收益](use-ad-mediation-to-maximize-revenue.md)。

## Microsoft Advertising 程式庫和廣告流量分配之間的差異

Microsoft Store Engagement and Monetization SDK 已包含適用於 Microsoft 廣告與廣告流量分配的程式庫。 不過，這些程式庫提供不同的類別且具備不同的用途。

* 如果您想要在 XAML 或 JavaScript app 中顯示橫幅或影片插入式廣告，請使用 Microsoft Advertising 程式庫的 **AdControl** 和 **InterstitialAd** 類別。
* 如果您想要在 XAML app 中顯示來自多個廣告網路的橫幅廣告，請使用廣告流量分配程式庫中的 **AdMediatorControl** 類別。

如需詳細資訊，請參閱[有何差異：AdMediatorControl 或 AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md)。

## 相關主題

* [Microsoft Store Engagement and Monetization SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)
* [利用廣告讓 App 獲利]( http://go.microsoft.com/fwlink/p/?LinkId=699559)


<!--HONumber=May16_HO2-->


