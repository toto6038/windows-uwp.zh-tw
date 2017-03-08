---
author: shawjohn
Description: "了解如何從 Windows 開發人員中心傳送特定對象的推播通知到您的 app，以鼓勵客戶採取行動，例如為 app 評等或購買附加元件。"
title: "傳送特定對象的推播通知給您的應用程式客戶"
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: a2bd3308863b6343a7616bf86e0b0036f1631bcd
ms.lasthandoff: 02/08/2017

---

# <a name="send-targeted-push-notifications-to-your-apps-customers"></a>傳送特定對象的推播通知給您的應用程式客戶

在對的時間以適當的訊息和客戶互動，是您身為應用程式開發人員的致勝關鍵。 Windows 開發人員中心提供資料導向的客戶交流平台，可供您傳送推播通知給所有客戶，或只傳送給符合您在[客戶區隔](create-customer-segments.md)中定義之條件的 Windows 10 客戶子集。

您可以使用特定對象的推播通知來鼓勵您的客戶採取行動，例如為 app 評等、購買附加元件、嘗試新功能或下載另一個 app。

> **重要** 特定對象的推播通知只能用於 UWP 應用程式。

## <a name="getting-started-with-push-notifications"></a>開始使用推播通知

有三件重要的事必須執行，您才能使用推播通知和客戶互動。
1. **登錄您的 app 以接收推播通知。** 您要在您的 app 中加入 Microsoft Store Services 的引用，然後再新增幾行於開發人員中心和您的 app 之間登錄通知通道的程式碼。 我們將使用該管道傳遞您的推播通知給您的客戶。 如需詳細資訊，請參閱[設定您的應用程式以接收開發人員中心通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。
2. **建立做為特定對象的一個或多個客戶區隔。** 您可以根據人口統計或收入條件，將客戶群組為不同的區隔。 如需詳細資訊，請參閱[建立客戶區隔](create-customer-segments.md)。
3. **建立推播通知，並將它傳送到特定的客戶區隔。** 例如，傳送通知以鼓勵新客戶為您的 app 評等，或傳送以特殊優惠購買附加元件的通知。

## <a name="to-create-and-send-a-targeted-push-notification"></a>建立和傳送特定對象的推播通知

1. 如果您還沒有這麼做，請安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)，並在您 app 的啟動程式碼中呼叫 [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 方法來登錄 app 以接收通知。 如需如何呼叫這個方法的詳細資訊，請參閱[設定您的 app 以接收開發人員中心通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。
2.    在 [Windows 開發人員中心儀表板](https://developer.microsoft.com/dashboard/overview)中，選取您的 app。
3.    在左邊的導覽功能表中，展開 **\[服務\]**，選取 **\[推播通知\]**。
4.    在 **\[特定對象的推播通知\]** 頁面上，選取 **\[新通知\]**。
5.    在 **\[選取範本\]** 區段中，選擇您想傳送的通知類型。 如需詳細資訊，請參閱[通知範本類型](#notification-template-types)。
  ![通知範本](images/push-notifications-template.png)
6.    在 **\[通知設定\]** 區段中選擇通知的 **\[名稱\]** ，然後選擇您想要傳送通知的 **\[客戶群組\]**。
如果您尚未建立區隔，請選取 **\[建立新的客戶群組\]**。 請注意，在 24 小時後，新的區隔才能使用於通知。 如需詳細資訊，請參閱[建立客戶區隔](create-customer-segments.md)。
7.    如果您想要指定傳送通知的對象，請清除 **\[立即傳送通知\]** 核取方塊，然後選擇特定的日期和時間。
8.    如果您想讓通知在某個時間點到期，請清除 **\[通知永遠不會到期\]** 核取方塊，然後選擇特定的到期日期和時間。
9.    在 **\[通知內容\]** 區段的 **\[語言\]** 功能表中，選擇您希望通知以何種語言顯示。 如需詳細資訊，請參閱[翻譯您的通知](#translate-your-notifications)。
10.    在 **\[選項\]** 區段中，輸入文字並設定您想要的任何其他選項。 如果您使用範本開始，則預設會提供一部分，但您可以視需要變更。
   可用的選項會依據您使用的通知類型而不同。 部分選項包括：
   - **啟用類型** (互動式快顯通知類型)。 您可以選擇 **\[前景\]**、**\[背景\]** 或 **\[通訊協定\]**。
   - **啟動** (互動式快顯通知類型)。 您可以選擇讓通知開啟 app 或網站。
   - **追蹤應用程式啟動速率** (互動式快顯通知類型)。 如果您想要測量透過每個通知與客戶互動的狀況，請選取此核取方塊。 如需詳細資訊，請參閱[測量通知效能](#measure-notification-performance)。
   - **持續時間** (互動式快顯通知類型)。 您可以選擇 **\[短期\]** 或 **\[長期\]**。
   - **案例** (互動式快顯通知類型)。 您可以選擇 **\[預設\]**、**\[警示]**、**\[提醒\]** 或 **\[來電\]**。
   - **基底 URI** (互動式快顯通知類型)。 如需詳細資訊，請參閱 [BaseUri](https://msdn.microsoft.com/library/windows/apps/br208712)。
   - **新增影像查詢** (互動式快顯通知類型)。 如需詳細資訊，請參閱 [addImageQuery](https://msdn.microsoft.com/library/windows/apps/br230847)。
   - **視覺**。 影像、視訊或音效。 如需詳細資訊，請參閱[視覺效果](https://msdn.microsoft.com/library/windows/apps/br230847)。
   - **輸入**/**動作**/**選取項目** (互動式快顯通知類型)。 可讓使用者與通知互動。 如需詳細資訊，請參閱[調適型和互動式快顯通知](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md#actions)。
   - **繫結** (互動式磚類型)。 快顯通知範本。 如需詳細資訊，請參閱[繫結](https://msdn.microsoft.com/library/windows/apps/br230843)。

   > **提示** 請嘗試使用[通知視覺化檢視](https://www.microsoft.com/store/apps/9nblggh5xsl1) app 來設計和測試您的調適型磚和互動式快顯通知。

11.    選取 **\[儲存為草稿\]** 以在稍後繼續處理通知，如果您已完成，請選取 **\[傳送\]**。

> **注意** 通知中的內容必須符合市集[內容原則](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies)。

## <a name="notification-template-types"></a>通知範本類型

您可以選擇各種不同的通知範本。

-    **空白 (快顯通知)。** 以可自訂的空白快顯通知開始。 快顯通知是顯示在畫面上的快顯 UI，無論使用者是在另一個 app、在 \[開始\] 畫面或是在桌面，都能讓 app 與客戶進行通訊。
-    **空白 (磚)。** 以可自訂的空白磚通知開始。 磚是 app 在 \[開始\] 畫面上的呈現方式。 磚可以「動態」呈現，表示磚所顯示的內容會隨著通知而變更。
-    **要求評等 (快顯通知)。** 要求客戶為您的 app 評等的快顯通知。 當客戶選取通知時，會顯示 app 的市集評等頁面。
-    **要求意見反應 (通知)。** 要求客戶為您的 app 提供意見的快顯通知。 當客戶選取通知時，會顯示 app 的 \[意見反應中樞\] 頁面。
   > **注意** 如果您選擇這個範本類型，在 [啟動]**** 方塊中，記得將 {PACKAGE_FAMILY_NAME} 預留位置值取代為您的應用程式實際套件系列名稱 (PFN)。 您可以在 [App 身分識別](view-app-identity-details.md)頁面 (**\[App 管理\]** > **\[App 身分識別\]**) 上找到 app 的 PFN。

   ![意見反應快顯通知 \[啟動\] 方塊](images/push-notifications-feedback-toast-launch-box.png)
-    **交叉宣傳 (快顯通知)。** 宣傳您選擇的其他 app 的快顯通知。 當客戶選取通知時，會顯示其他 app 的市集清單。
  > **注意** 如果您選擇這個範本類型，在 [啟動]**** 方塊中，記得將 **{您想在此促銷的 ProductId}** 預留位置值取代為您想聯合促銷之項目的實際市集識別碼。 您可以在 [App 身分識別](view-app-identity-details.md)頁面 (**\[App 管理\]** > **\[App 身分識別\]**) 上找到市集識別碼。

  ![交叉宣傳快顯通知 \[啟動\] 方塊](images/push-notifications-promote-toast-launch-box.png)
-    **宣傳銷售 (快顯通知)。** 可用來宣告 app 優惠的快顯通知。 當客戶選取通知時，會顯示您的 app 的市集清單。
- **提示更新 (快顯通知)。** 鼓勵執行您的舊版 app 的客戶安裝最新版的快顯通知。 當客戶選取通知時，會顯示市集 app 中的 **\[下載與更新\]** 清單。 請注意，您不需要建立客戶區隔就能使用此範本。 我們會將這個通知排程在 24 小時內，並將對象鎖定為尚未執行您的 app 最新版的所有使用者。

## <a name="measure-notification-performance"></a>測量通知效能

您可以測量透過每個通知與客戶互動的狀況。

###<a name="to-measure-notification-performance"></a>測量通知效能

1.    當您建立通知時，請在 **\[通知內容\]** 區段選取 **\[追蹤 app 啟動速率\]** 核取方塊。
2.    在您的 app 中呼叫 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 方法，即可通知開發人員中心您的 app 已啟動，以回應特定對象的通知。 這個方法由 Microsoft 市集 SDK 提供。 如需如何呼叫這個方法的詳細資訊，請參閱[設定您的 app 以接收開發人員中心通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。

###<a name="to-view-notification-performance"></a>檢視通知效能

當您依上述方式設定通知和 app 以[測量通知效能](#to-measure-notification-performance)後，可以使用儀表板來查看通知執行狀況。

1.  在儀表板中，選取您的其中一個 app。
2.  展開左側功能表的 **\[服務\]** 區段，然後選取 **\[推播通知\]** 以查看該 app 關聯的通知。
3.    在 **\[特定對象的推播通知\]** 頁面上，選取 **\[進行中\]** 或 **\[完成\]**，再查看 **\[傳遞速率\]** 和 **\[App 啟動速率\]** 欄位以了解每個通知的整體效能。
4.  若要查看更詳細的效能詳細資料，請選取通知名稱。 **\[傳遞統計資料\]** 區段就會顯示，並針對下列通知 **\[狀態\]** 類型顯示 **\[計數\]** 和 **\[百分比\]** 資訊︰
 - **失敗**︰通知因為某些原因未傳遞。 例如，如果 Windows 通知服務中發生問題，這個情況就會發生。
 - **通道到期失敗**︰無法傳遞通知，因為應用程式與開發人員中心之間的通道已到期。 例如，如果客戶長時間未開啟您的 app，這個情況就會發生。
 - **傳送中**︰通知在待傳送的佇列中。
 - **已傳送**︰已傳送通知。
 - **啟動**︰通知已傳送、客戶按一下通知，因此開啟您的應用程式。 請注意，這只會追蹤 app 啟動。 這個狀態不包括邀請客戶採取其他行動 (例如啟動市集以留下評等) 的通知。
 - **不明**︰我們無法判斷這個通知的狀態。

## <a name="translate-your-notifications"></a>翻譯您的通知

為使通知發揮最大的影響力，請考慮將它們翻譯成您客戶偏好的語言。 開發人員中心利用 [Microsoft Translator](https://msdn.microsoft.com/library/dd576287.aspx) 服務，讓您可以輕鬆地自動翻譯您的通知。

1.    當您以您的預設語言撰寫通知後，請選取 **\[新增語言\]** (在 **\[通知內容\]** 區段的 **\[語言\]** 功能表之下)。
2.    在 **\[新增語言\]** 視窗中選取顯示通知的他種語言，然後選取 **\[更新\]**。
您的通知將會自動翻譯為您在 **\[新增語言\]** 視窗選擇的語言，而這些程式語言將新增到 **\[語言\]** 功能表。
3.    若要查看您通知的翻譯，請在 **\[語言\]** 功能表中選取剛才新增的語言。

關於翻譯，需注意幾件事︰
 - 您可以在語言的 **\[內容\]** 方塊中輸入不同於自動翻譯的內容以將它覆寫。
 - 如果您在覆寫自動翻譯後新增另一個文字方塊到英文版的通知，新的文字方塊將不會新增至翻譯的通知。 在這種情況下，您必須手動將新的文字方塊新增到每個翻譯的通知。
 - 如果您在通知翻譯後變更英文的文字，我們將會自動更新翻譯的通知以符合變更。 不過，如果您先前選擇覆寫初始翻譯，就不會發生這個狀況。

## <a name="related-topics"></a>相關主題
- [UWP app的磚、徽章及通知](../controls-and-patterns/tiles-badges-notifications.md)
- [Windows 推播通知服務 (WNS) 概觀](../controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview.md)
- [Windows 推播通知服務 (WNS) 概觀 (Windows 執行階段 app)](https://msdn.microsoft.com/library/windows/apps/hh913756.aspx)
- [通知視覺化檢視 app](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync() 方法](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [客戶區隔與推播通知︰新的 Windows 開發人員中心測試人員計畫功能 (部落格文章)](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)

