---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "了解在提交 app 到市集之前，如何從 Windows 開發人員中心儀表板新增應用程式識別碼和廣告單元識別碼。"
title: "在您的 App 中設定廣告單元"
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: 705955faf7ddd67f80098f8c3ac7b2844553de95


---

# 在您的 App 中設定廣告單元




當您在 App 中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 來顯示廣告時，您必須指定應用程式識別碼和廣告單元識別碼。 當您開發您的 app 時，使用適當的[測試應用程式識別碼和廣告單元識別碼值](test-mode-values.md)，以查看您的 app 於測試期間呈現廣告的方式。

在您完成測試您的 App 且準備好將 App 提交至 Windows 開發人員中心時，您必須更新您的 App 程式碼，以使用 [Windows 開發人員中心儀表板](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)的應用程式識別碼和廣告單位識別碼值。 如果您嘗試在實際 App 中使用測試值，您的 App 將不會收到即時廣告。

為您的實際 app 設定應用程式識別碼和廣告單元：

1.  在 Windows 開發人員中心儀表板上，按一下 \[創造營收] &gt; \[利用廣告獲利\]。
2.  在此頁面的 \[Microsoft Advertising 廣告單元\] 區段中，建立廣告單元。 對於廣告單元類型，如果使用 **AdControl** 請選取 \[橫幅\]，如果使用 **InterstitialAd** 則請選取 \[影片插入式\]。 如需此頁面的詳細資訊，請參閱[利用廣告賺取獲利](../publish/monetize-with-ads.md)。

3.  您會看到每個已產生的廣告單元的「應用程式識別碼」和「廣告單元識別碼」。 若要在 app 中顯示廣告，您需要在 app 程式碼中使用這些值：

    * 如果您的 app 顯示橫幅廣告，請將這些值指派給 **AdControl** 物件的 **ApplicationId** 和 **AdUnitId** 屬性。

    * 如果您的 App 顯示影片插入式廣告，請將這些值傳遞到 **InterstitialAd** 物件的 **RequestAd** 方法。

 

## 相關主題

[測試模式值](test-mode-values.md)


 

 



<!--HONumber=Aug16_HO3-->


