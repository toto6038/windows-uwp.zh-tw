---
author: Xansky
Description: You can log custom events from your UWP app and review those events in the Usage report in Partner Center.
title: 為合作夥伴中心記錄自訂事件
ms.author: mhopkins
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, 記錄事件
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 47c1eb02434dc71cb7da949d58ec38cf3b4cf65a
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5970341"
---
# <a name="log-custom-events-for-partner-center"></a>為合作夥伴中心記錄自訂事件

合作夥伴中心中的[使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)可讓您取得您已經在您的通用 Windows 平台 (UWP) 應用程式中定義的自訂事件的相關資訊。 自訂事件是代表您 App 中事件或活動的任意字串。 例如，遊戲可能會定義名為 *firstLevelPassed*、*secondLevelPassed* 等等的自訂事件，當使用者通過遊戲中的每個層級時，便會記錄這些事件。

若要記錄來自您 App 的自訂事件，請將自訂事件字串傳遞給 Microsoft Store Services SDK 所提供的 [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 為您的自訂事件[使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)合作夥伴中心中的**自訂事件**] 區段中，您可以檢閱總計。

> [!NOTE]
> 您登入合作夥伴中心的自訂事件是與[Windows 事件](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx)、 無關，它們不會出現在**事件檢視器**。

## <a name="prerequisites"></a>必要條件

您可以檢閱自訂記錄事件的**使用方式報告**中，在合作夥伴中心的應用程式之前，必須在市集中發行您的應用程式。

## <a name="how-to-log-custom-events"></a>如何記錄自訂事件

1. 如果您尚未這麼做，請在您的開發電腦上[安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。

2. 在 Visual Studio 中，開啟您的專案。

3. 在 \[方案總管\] 中，於專案的 **\[參考\]** 節點上按一下滑鼠右鍵，然後按一下 **\[加入參考\]**。

4. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**，然後按一下 **\[擴充功能\]**。

5. 在 SDK 清單中，按一下 **\[Microsoft Engagement Framework\]** 旁邊的核取方塊，然後按一下 **\[確定\]**。

6. 將下列陳述式新增到您要記錄自訂事件的每個程式碼檔案頂端。
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. 在您要記錄自訂事件的每個程式碼區段中，取得 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 物件，然後呼叫 [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 將您的自訂事件字串傳送給該方法。
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > 如果您的應用程式使用長名稱記錄許多自訂事件，[使用報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)可能需要很長的時間來載入。 建議讓您的自訂事件使用簡短名稱。 

## <a name="related-topics"></a>相關主題

* [使用方式報告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Log 方法](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
