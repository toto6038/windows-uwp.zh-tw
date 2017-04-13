---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "了解在提交 app 到市集之前，如何從 Windows 開發人員中心儀表板新增應用程式識別碼和廣告單元識別碼。"
title: "在您的應用程式中設定廣告單元"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 廣告, 廣告單位"
ms.openlocfilehash: daf0887462a4c84aa827a6261793a0eaf4d512ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-up-ad-units-in-your-app"></a>在您的應用程式中設定廣告單元




當您在 App 中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 來顯示廣告時，您必須指定應用程式識別碼和廣告單元識別碼。 當您開發您的 app 時，使用適當的[測試應用程式識別碼和廣告單元識別碼值](test-mode-values.md)，以查看您的 app 於測試期間呈現廣告的方式。

在您完成測試您的 App 且準備好將 App 提交至 Windows 開發人員中心時，您必須更新您的 App 程式碼，以使用 [Windows 開發人員中心儀表板](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)的應用程式識別碼和廣告單位識別碼值。 如果您嘗試在實際 App 中使用測試值，您的 App 將不會收到即時廣告。

為您的實際 app 設定應用程式識別碼和廣告單元：

1.  在 Windows 開發人員中心儀表板上，按一下 **\[創造營收\] &gt; \[利用廣告獲利\]**。

2.  在此頁面的 **\[Microsoft Advertising 廣告單元\]** 區段中，建立廣告單元。 針對廣告單元類型，從下列選項選取：

  * 如果您的 app 使用 **AdControl** 顯示橫幅廣告，選取 **\[封面\]** 做為廣告單元類型。

  * 如果您的 app 使用 **InterstitialAd** 顯示插播式影片廣告或插播式橫幅廣告，選取 **\[插播式影片\]** 或 **\[插播式橫幅\]**（請務必針對您想要顯示的插播式廣告類型選取適當選項）。

3.  您會看到每個已產生的廣告單元的**「應用程式識別碼」**和**「廣告單元識別碼」**。 若要在 app 中顯示廣告，您需要在 app 程式碼中使用這些值：

  * 如果您的 app 顯示橫幅廣告，請將這些值指派給 **AdControl** 物件的 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 屬性。 如需詳細資訊，請參閱 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md) 和 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。

  * 如果您的 App 顯示插播式廣告，請將這些值傳遞給 **InterstitialAd** 物件的 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法。 如需詳細資訊，請參閱[插播式廣告](interstitial-ads.md)。

如需 **\[利用廣告獲利\]** 頁面的詳細資訊，請參閱[利用廣告獲利](../publish/monetize-with-ads.md)。

## <a name="related-topics"></a>相關主題

* [測試模式值](test-mode-values.md)
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [插播式廣告](interstitial-ads.md)


 

 
