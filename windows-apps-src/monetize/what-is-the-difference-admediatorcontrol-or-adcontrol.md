---
author: mcleanbyron
ms.assetid: 9165f709-71d7-42cf-9b30-3190fe029fb4
description: "了解 Microsoft Advertising 程式庫中 AdControl 類別和廣告流量分配程式庫中 AdMediatorControl 類別之間的差異。"
title: "有何差異 - AdMediatorControl 或 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: 8a5b02dbc40f3f0cd9be32aa7d5184e60a3b2707
ms.openlocfilehash: 291e1c4d707e8987d29ae5840248918543d7d12a

---

# 有何差異：AdMediatorControl 或 AdControl


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

當您想要顯示 Microsoft 的橫幅廣告或插入式影片廣告，請在您的 App 中使用適用於 XAML 和 JavaScript 的 Microsoft Advertising 程式庫。 這些程式庫不同於廣告流量分配程式庫，廣告流量分配程式庫是用來顯示來自多個廣告網路的廣告。 使用此 Microsoft Advertising 程式庫 (XAML 和 JavaScript) 文件的時機：

* 在 XAML 或 JavaScript App 中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)，而不是 **AdMediatorControl** 時。
* 正在尋找廣告流量分配使用的基礎 **AdControl** API的參考資訊。

Microsoft Advertising 程式庫和廣告流量分配程式庫均包含於 Microsoft Store Engagement 和 Monetization SDK 中。 如需安裝此 SDK 以及包含在此 SDK 中 Microsoft Advertising 程式庫差異的詳細資訊，請參閱[安裝 Microsoft Advertising 程式庫](install-the-microsoft-advertising-libraries.md)。

>**注意** 若要顯示插入式廣告，請使用 **InterstitialAd** 控制項。 **AdControl** 和 **AdMediatorControl** 無法顯示插入式廣告。 如需詳細資訊，請參閱[插入式廣告](interstitial-ads.md)。

 

## 廣告流量分配


顯示 Microsoft 橫幅廣告 (不是插入式廣告) 的建議方式是在 App 中使用 **AdMediatorControl**。 **AdMediatorControl** 會顯示來自多個廣告網路的橫幅廣告。

當您在專案中使用 **AdMediatorControl** 時，您必須使用 Visual Studio 的「已連線的服務」****功能指定要使用哪個廣告網路。 Visual Studio 會針對某些廣告網路，以程式設計方式嘗試擷取必要的組件。 如果有任何無法自動擷取的組件，您必須針對每個廣告網路來安裝它們。 如需廣告流量分配的詳細資訊，請參閱[使用廣告流量分配獲得最佳收益](use-ad-mediation-to-maximize-revenue.md)。

**AdMediatorControl** 會抽象化廣告單位識別碼與應用程式識別碼的使用。 那些識別碼是由 **AdMediatorControl** 管理，與 Windows 開發人員中心儀表板中的廣告流量分配設定搭配使用。 如需詳細資訊，請參閱[提交您的 App 並設定廣告流量分配](submit-your-app-and-configure-ad-mediation.md)。

**AdMediatorControl** 使用它本身的屬性和語法，針對每個它所分配流量的廣告服務支援 API。 如需詳細資訊，請參閱[新增和使用廣告流量分配者控制項](add-and-use-the-ad-mediator-control.md)。

## AdControl


如果您只想要顯示來自 Microsoft 的橫幅廣告 (沒有其他廣告網路)，您可以在 XAML 和 JavaScript app 中單獨使用 **AdControl**。 測試使用 **AdControl** 的 App 時，請針對應用程式識別碼及廣告單位識別碼使用[測試模式值](test-mode-values.md)。 在您完成測試 App 後並準備好要提交至 Windows 開發人員中心時，請使用 Windows 開發人員中心儀表板取得您的即時廣告單位識別碼和應用程式識別碼，然後更新您的程式碼以使用這些值。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md)。

 

 



<!--HONumber=Jun16_HO4-->


