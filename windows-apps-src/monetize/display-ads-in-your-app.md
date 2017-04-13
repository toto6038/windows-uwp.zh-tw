---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft Store Services SDK 提供您數種方式來透過廣告讓您的 App 獲利。"
title: "在您的應用程式中顯示廣告"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, 廣告, 橫幅, 插入式"
ms.openlocfilehash: ceae17d34ba400876f912ba9932d76f59d773e63
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="display-ads-in-your-app"></a>在您的應用程式中顯示廣告


通用 Windows 平台 (UWP) 和 Windows 市集提供數種方式來透過廣告讓您的應用程式獲利。

## <a name="display-banner-and-interstitial-ads-using-the-microsoft-advertising-libraries"></a>顯示使用 Microsoft Advertising 程式庫的橫幅廣告和插播式廣告

在您的 app 中包括橫幅廣告或插播式廣告，讓您的 app 獲利更多。

* *橫幅廣告*是使用 app 的部分頁面 (通常是位於頁面頂端或底部) 的小廣告。
* *插播式廣告*是全螢幕廣告，通常強制使用者觀賞影片或點選連結，才能在應用程式或遊戲中繼續。 我們針對 UWP 應用程式支援兩種類型的插播式廣告：影片和橫幅。

若要在 App 中包含這些類型的廣告，可使用隨 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (適用於 UWP App) 及[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) (適用於 Windows 8.1 和 Windows Phone 8.x 應用程式) 一起散發之廣告程式庫中的 **AdControl** 和 **InterstitialAd** 控制項。

您可以使用 Windows 開發人員中心儀表板中的[廣告績效報告](../publish/advertising-performance-report.md)即時監視您的廣告績效。

下列主題提供關於 Windows Advertising 程式庫常見工作的資訊。

|  工作    | 主題 |               
|----------|-------|
| 安裝與開始使用 Microsoft Advertising 程式庫。     | 請參閱[開始使用 Microsoft Advertising 程式庫](get-started-with-microsoft-advertising-libraries.md)。        |
| 在您的 XAML/C# app 中顯示橫幅廣告。     | 請參閱 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)。        |
| 在您的 HTML/JavaScript app 中顯示橫幅廣告。     | 請參閱 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。        |
| 在您的 Windows Phone Silverlight 8.x 應用程式中顯示橫幅廣告。     | 請參閱 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。        |
| 在您的應用程式中顯示插入式廣告。     | 請參閱[插入式廣告](interstitial-ads.md)。       |
| 在搭配使用 HTML 和 JavaScript 撰寫的通用 Windows 平台 (UWP) App 的影片內容中新增廣告。   |  請參閱[使用 HTML 5 與 JavaScript，將廣告新增至影片內容](add-advertisements-to-video-content.md)。  |
| 下載示範如何將橫幅廣告和插入式廣告新增到 App 的範例專案。     |請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。       |
| 處理您 app 中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 錯誤：     | 請參閱[錯誤處理](error-handling-with-advertising-libraries.md)和 [AdControl 錯誤處理](adcontrol-error-handling.md)下方的逐步解說。       |
| 報告 Microsoft Advertising 程式庫錯誤。     | 造訪[支援頁面](https://go.microsoft.com/fwlink/p/?LinkId=331508)。        |
| 取得社群支援。     | 造訪[論壇](http://go.microsoft.com/fwlink/p/?LinkId=401266)。       |

                            

## <a name="use-ad-mediation-for-banner-ads-windows-81-and-windows-phone-8x"></a>使用橫幅廣告的廣告流量分配 (Windows 8.1 和 Windows Phone 8.x)

對於 Windows 8.1 和 Windows Phone 8.x 應用程式，您可使用 **AdMediatorControl** 類別，以顯示來自多個廣告網路的橫幅廣告來最佳化廣告收益。 將此控制項新增至您的 App 後，在 Windows 開發人員中心儀表板中設定廣告流量分配設定，而我們會負責針對來自您選擇之廣告網路的橫幅廣告要求進行流量分配。 如需詳細資訊，請參閱[使用廣告流量分配來獲得最佳廣告收益](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)。

> [!NOTE]
> Windows 10 的 UWP app 目前不支援使用 **AdMediatorControl** 類別的廣告流量分配。 即將推出適用於 UWP app 的伺服器端流量分配，且其橫幅廣告 (**AdControl**) 和插播式廣告 (**InterstitialAd**) 使用相同 API。 如需將 UWP App 中的 **AdMediatorControl** 移轉至 **AdControl** 的指南，請參閱[針對 UWP App 從 AdMediatorControl 移轉到 AdControl](migrate-from-admediatorcontrol-to-adcontrol.md)。

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Windows Phone 8.x Silverlight 專案的廣告支援

Windows Phone 8.x Silverlight 專案中已不再支援某些開發人員案例。 如需詳細資訊，請參閱下表。

|  平台版本  |  現有專案    |   新專案  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  如果您現有的 Windows Phone 8.0 Silverlight 專案是使用之前所推出的 Universal Ad Client SDK 或 Microsoft Advertising SDK 中的 **AdControl** 或 **AdMediatorControl**，且該應用程式已在 Windows 市集發行，則您可以修改並重新建置該專案，並在裝置上偵錯或測試變更。 不支援在模擬器中偵錯或測試專案。  |  不支援。  |
| Windows Phone 8.1 Silverlight    |  如果您現有的 Windows Phone 8.1 Silverlight 專案是使用先前 SDK 中的 **AdControl** 或 **AdMediatorControl**，您可以修改並重新建置該專案。 不過，若要偵錯或測試應用程式，您必須在模擬器中執行該應用程式，並針對「應用程式識別碼」和「廣告單位識別碼」使用[測試值](test-mode-values.md)。 不支援在裝置上偵錯或測試應用程式。  |   您可以在新的 Windows Phone 8.1 Silverlight 專案中新增 **AdControl** or **AdMediatorControl**。 不過，若要偵錯或測試應用程式，您必須在模擬器中執行該應用程式，並針對「應用程式識別碼」和「廣告單位識別碼」使用[測試值](test-mode-values.md)。 不支援在裝置上偵錯或測試應用程式。 |

## <a name="related-topics"></a>相關主題

* [Microsoft Store Services SDK](microsoft-store-services-sdk.md)
* [利用廣告讓 App 獲利](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [廣告績效報告](../publish/advertising-performance-report.md)
