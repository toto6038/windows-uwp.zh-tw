---
Description: 您可以從 UWP 應用程式記錄自訂事件，並在合作夥伴中心的 [使用方式] 報表中檢查這些事件。
title: 記錄合作夥伴中心的自訂事件
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, 記錄事件
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 5a1df08b62199bf1249af8bfbbb00921874a671c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364101"
---
# <a name="log-custom-events-for-partner-center"></a>記錄合作夥伴中心的自訂事件

合作夥伴中心中的 [使用方式報告](../publish/usage-report.md) 可讓您取得已在通用 WINDOWS 平臺 (UWP) 應用程式中定義之自訂事件的相關資訊。 自訂事件是代表您 App 中事件或活動的任意字串。 例如，遊戲可能會定義名為 *firstLevelPassed*、*secondLevelPassed* 等等的自訂事件，當使用者通過遊戲中的每個層級時，便會記錄這些事件。

若要記錄來自您 App 的自訂事件，請將自訂事件字串傳遞給 Microsoft Store Services SDK 所提供的 [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 您可以在合作夥伴中心的 [使用方式][報表](../publish/usage-report.md)的 [**自訂事件**] 區段中，查看自訂事件的總發生次數。

> [!NOTE]
> 您登入合作夥伴中心的自訂事件與 [Windows 事件](/windows/desktop/Events/windows-events)無關，而且它們不會出現在 **事件檢視器**中。

## <a name="prerequisites"></a>先決條件

您必須先在存放區中發佈您的應用程式，才能在合作夥伴中心中檢查應用程式 **使用方式報告** 中的自訂記錄事件。

## <a name="how-to-log-custom-events"></a>如何記錄自訂事件

1. 如果您尚未這麼做，請在您的開發電腦上[安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。

2. 在 Visual Studio 中，開啟您的專案。

3. 在方案總管中，以滑鼠右鍵按一下專案的 [ **參考** ] 節點，然後按一下 [ **加入參考**]。

4. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**，然後按一下 **\[擴充功能\]**。

5. 在 Sdk 清單中，按一下 [ **Microsoft Engagement 架構** ] 旁的核取方塊，然後按一下 **[確定]**。

6. 將下列陳述式新增到您要記錄自訂事件的每個程式碼檔案頂端。
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="EngagementNamespace":::

7. 在您要記錄自訂事件的每個程式碼區段中，取得 [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 物件，然後呼叫 [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 將您的自訂事件字串傳送給該方法。
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="Log":::

    > [!NOTE]
    > 如果您的應用程式使用長名稱記錄許多自訂事件，[使用報告](../publish/usage-report.md)可能需要很長的時間來載入。 建議讓您的自訂事件使用簡短名稱。 

## <a name="related-topics"></a>相關主題

* [使用方式報告](../publish/usage-report.md)
* [Log 方法](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
