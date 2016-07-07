---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "在 Microsoft Store Engagement and Monetization SDK 中檢閱 Microsoft Advertising 程式庫的版本資訊。"
title: "Microsoft Advertising 程式庫的版本資訊"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 8e2114e969b27d579f62195f026cfcfd9672a94a

---

# Microsoft Advertising 程式庫的版本資訊


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節提供 Microsoft Store Engagement and Monetization SDK 中 Microsoft Advertising 程式庫目前版本的版本資訊。 這些程式庫支援 Windows 10、Windows 8.1、 Windows Phone 8.1 和 Windows Phone 8 的 XAML 和 JavaScript/HTML 應用程式。

## 安裝


Microsoft advertising 程式庫是做為 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) 的一部分來提供使用。 針對 Windows Phone 8.x Silverlight 以外的所有專案類型，曾用來在較舊的 Microsoft Universal Ad Client SDK 及 Microsoft Advertising SDK 獨立版本中發佈的 Microsoft Advertising 組件，現在會利用 Microsoft Store Engagement and Monetization SDK 進行安裝。 如需有關如何安裝的 SDK，而且包含在它的程式庫的詳細資訊，請參閱[安裝 Microsoft Advertising 程式庫](install-the-microsoft-advertising-libraries.md)。

若要取得適用於 Windows Phone 8.x Silverlight 專案的 Microsoft Advertising 組件，請安裝 Microsoft Store Engagement and Monetization SDK，在 Visual Studio 中開啟您的專案，然後移至 \[專案\]\[加入已連接服務\]\[Ad Mediator\] 以自動下載組件。 完成後，若您不想使用廣告流量分配，請將 Ad Mediator 參照從您的專案中移除。 如需詳細資訊，請參閱 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。

## 了解 Microsoft Advertising 程式庫和廣告流量分配之間的差異

雖然 Microsoft Advertising 程式庫和廣告流量分配是由 Microsoft Store Engagement and Monetization SDK 所提供，這些程式庫有不同的目的。 如果您想要在 XAML 或 JavaScript app 中顯示來自 Microsoft 的橫幅或影片插入式廣告，請使用 Microsoft Advertising 程式庫的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 類別。 如果您想要在 XAML 應用程式中 (JavaScript/HTML 應用程式不支援廣告流量分配) 顯示來自多個廣告網路的橫幅廣告，請在廣告流量分配程式庫中使用 **AdMediatorControl** 類別。 如需詳細資訊，請參閱[有何差異：AdMediatorControl 或 AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md)。

## 解除安裝之前的版本

安裝 Microsoft Store Engagement and Monetization SDK 之前，強烈建議您解除安裝 Microsoft Universal Ad Client SDK 或 Microsoft Advertising SDK 先前所有的執行個體。

## 目標架構特定的建置輸出

使用 Microsoft Advertising 程式庫時，您在專案中將無法以 \[任何 CPU\] 為目標。 如果專案的目標是 \[任何 CPU\]，當您將參照新增到 Microsoft Advertising 程式庫之後，便可能會在專案中看到警告。 如果要移除這項警告，請將您的專案更新成使用架構特定的建置輸出 (例如 **x86**)。 如需詳細資訊，請參閱[已知問題](known-issues-for-the-advertising-libraries.md)。

## C++ 支援

Microsoft Advertising 程式庫 (其中包括 **AdControl** 和 **InterstitialAd** 類別) 支援使用 Windows 執行階段互通性 (*interop*) 以C++ 和 DirectX 撰寫的 app。 如需使用 XAML 和 C++ 程式設計的詳細資訊與相關範例，請參閱[類型系統](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx)。

## 沒有工具箱控制項

在 Microsoft Store Engagement and Monetization SDK 目前版本的 Microsoft Advertising 程式庫中，沒有任何可供拖曳 **AdControl** 或 **InterstitialAd** 到您 app 中設計表面的工具箱控制項。 如需在您的標記和程式碼中新增這些控制項的相關指示，請參閱[開發人員逐步解說](developer-walkthroughs.md)。

## 緯度和經度屬性不再提供


            **AdControl** 類別不再有適用於 UWP app 的 **Latitude** 和 **Longitude** 屬性。 相反地，廣告控制項內建的程式碼會偵測，並代替 app 將這些值傳送至廣告伺服器。

## 重要須知：

請務必完整閱讀使用者授權合約 (EULA)。 請參閱主題 [重要須知 - EULA](important-notice-eula.md)。

 

 



<!--HONumber=Jun16_HO4-->


