---
author: mcleanbyron
Description: "您可以記錄來自您 UWP app 的自訂事件，然後在「Windows 開發人員中心」儀表板上的「使用方式報告」中檢閱這些事件。"
title: "為開發人員中心記錄自訂事件"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: 61874c700ecd31c7246effef5b05ffbf1153dfd5

---

# 為開發人員中心記錄自訂事件

「Windows 開發人員中心」儀表板中的[使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)可讓您取得已在「通用 Windows 平台」(UWP) app 中定義之自訂事件的相關資訊。 自訂事件是代表您 App 中事件或活動的任意字串。 例如，遊戲可能會定義名為 *firstLevelPassed*、*secondLevelPassed* 等等的自訂事件，當使用者通過遊戲中的每個層級時，便會記錄這些事件。

若要記錄來自您 App 的自訂事件，請將自訂事件字串傳遞給 Microsoft Store Services SDK 所提供的 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 方法。 您可以在「開發人員中心」儀表板中[使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)的 [自訂事件] 區段中，檢閱自訂事件的發生次數總計。

## 先決條件

您必須先在「市集」中發佈 App，才能在儀表板中 App 的「使用方式報告」中檢閱自訂記錄事件。

## 如何記錄自訂事件

1. 如果您尚未這麼做，請在您的開發電腦上[安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 除了用於記錄自訂事件的 API 之外，此 SDK 也提供其他功能 (例如，在 App 中使用 A/B 測試來執行實驗，以及顯示廣告) 的 API。 
2. 在 Visual Studio 中，開啟您的專案。
3. 在 [方案總管] 中，於專案的 [參考] 節點上按一下滑鼠右鍵，然後按一下 [加入參考]。
4. 在 [參考管理員] 中，展開 [通用 Windows]，然後按一下 [擴充功能]。
5. 在 SDK 清單中，按一下 [Microsoft Engagement Framework] 旁邊的核取方塊，然後按一下 [確定]。
7. 將下列陳述式新增到您要記錄自訂事件的每個程式碼檔案頂端。

  ```csharp
  using Microsoft.Services.Store.Engagement;
  ```
8. 在您要記錄自訂事件的每個程式碼區段中，取得 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 物件，然後呼叫 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 方法。 將您的自訂事件字串傳送給該方法。

  ```csharp
  StoreServicesCustomEventLogger logger = StoreServicesCustomEventLogger.GetDefault();
  logger.Log("myCustomEvent");
  ```

## 相關主題

* [使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Log 方法](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Nov16_HO1-->


