---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "了解在提交 app 到市集之前，如何從 Windows 開發人員中心儀表板新增應用程式識別碼和廣告單元識別碼。"
title: "在您的 App 中設定廣告單元"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 96c09de9321f67dc26cc3538f2655bd598f134f9


---

# 在您的 App 中設定廣告單元


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

當您在 app 中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 來顯示廣告時，您必須指定應用程式識別碼和廣告單元識別碼。 當您開發您的 app 時，使用適當的[測試應用程式識別碼和廣告單元識別碼值](test-mode-values.md)，以查看您的 app 於測試期間呈現廣告的方式。

在您完成測試您的 App 且準備好將 App 提交至 Windows 開發人員中心時，您必須更新您的 App 程式碼，以使用 [Windows 開發人員中心儀表板](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)的應用程式識別碼和廣告單位識別碼值。 如果您嘗試在實際 App 中使用測試值，您的 App 將不會收到即時廣告。

為您的實際 app 設定應用程式識別碼和廣告單元：

1.  在 Windows 開發人員中心儀表板上，按一下 \[創造營收] &gt; \[利用廣告獲利\]。
2.  在此頁面的 \[Microsoft Advertising 廣告單元\] 區段中，建立廣告單元。 對於廣告單元類型，如果使用 **AdControl** 請選取 \[橫幅\]，如果使用 **InterstitialAd** 則請選取 \[影片插入式\]。 如需此頁面的詳細資訊，請參閱[利用廣告賺取獲利](../publish/monetize-with-ads.md)。

3.  您會看到每個已產生的廣告單元的「應用程式識別碼」和「廣告單元識別碼」。 若要在 app 中顯示廣告，您需要在 app 程式碼中使用這些值：

    * 如果您的 app 顯示橫幅廣告，請將這些值指派給 **AdControl** 物件的 **ApplicationId** 和 **AdUnitId** 屬性。

    * 如果您的 app 顯示影片插入式廣告，請將這些值傳遞到 **InterstitialAd** 物件的 **RequestAd** 方法。

> **重要：**如果您的 app 使用廣告流量分配顯示來自 Microsoft 的橫幅廣告 (也就是，它會使用 **AdMediatorControl** 物件)，則您不需要求廣告單元。 在這個案例中，系統會自動產生廣告單元。 如需詳細資訊，請參閱[有何差異：AdMediatorControl 或 AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md)。

 

## 相關主題

[測試模式值](test-mode-values.md)


 

 



<!--HONumber=Jun16_HO4-->


