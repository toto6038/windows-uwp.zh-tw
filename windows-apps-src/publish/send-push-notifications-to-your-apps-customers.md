---
author: JnHs
Description: Learn how to send notifications from Partner Center to your app to encourage groups of customers to take an action, such as rating an app or buying an add-on.
title: 傳送特定對象的推播通知給您的 App 客戶
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 目標式通知, 推播通知, 快顯通知, 磚
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 7c2cf6c9cbd4aa0b25afea47a2fe82774c3c87a7
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7445817"
---
# <a name="send-notifications-to-your-apps-customers"></a>傳送通知給您的應用程式客戶

在對的時間以適當的訊息和客戶互動，是您身為 App 開發人員的致勝關鍵。 通知可以鼓勵您的客戶採取行動，例如為應用程式評分、購買附加元件、嘗試新功能或下載另一個應用程式 (或許可透過您提供的[促銷代碼](generate-promotional-codes.md)免費取得)。

[合作夥伴中心](https://partner.microsoft.com/dashboard)提供資料導向的客戶交流平台，您可以使用傳送通知給所有應用程式的客戶，或只鎖定符合您在[客戶所定義之條件應用程式的 Windows 10 客戶子集區隔](create-customer-segments.md)。 您也可以建立通知傳送到一個以上的應用程式的客戶。

> [!IMPORTANT]
> 這些通知只能用於 UWP app。

考慮您的通知內容時，請牢記︰
- 通知中的內容必須符合Microsoft Store[內容原則](https://docs.microsoft.com/legal/windows/agreements/store-policies#content_policies)。
- 您的通知內容不可包含機密資訊或可能具敏感性的資訊。
- 雖然我們會全力按照預定時間提供您的通知，但有時可能會發生影響傳遞的延遲問題。
- 請勿傳送通知太過頻繁。 每 30 分鐘多於一次可能會顯得很冒昧 (對許多案例來說，最好低於這個頻率)。
- 請注意，如果使用您的 App (並且在確定區隔會員資格後使用其 Microsoft 帳戶登入) 的客戶稍後將裝置交給某人使用，那麼其他人可能會看到針對原來客戶的通知。 如需詳細資訊，請參閱[針對確定目標推播通知設定您的 App](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers)。
- 如果針對多個應用程式客戶傳送相同通知，您無法以某個客戶區隔為目標；通知將被傳送至您所選取應用程式的所有客戶。


## <a name="getting-started-with-notifications"></a>開始使用通知

有三件重要的事必須執行，您才能使用通知和客戶互動。

1. **登錄您的 app 以接收推播通知。** 在您的應用程式中新增對 Microsoft Store Services SDK 的參考，然後再新增幾行程式登錄通知通道合作夥伴中心和您的應用程式之間的程式碼執行此動作。 我們將使用該管道傳遞您的通知給您的客戶。 如需詳細資訊，請參閱[設定您的應用程式以接收目標式推播通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。
2. **決定要設定為目標的客戶。** 您可以將通知傳送給 App 所有的客戶或 (針對單一 App 所建立的通知) 稱為*區隔*的客戶群組 (您可以根據人口統計資料或營收準則定義此群組)。 如需詳細資訊，請參閱[建立客戶區隔](create-customer-segments.md)。
3. **建立您的通知內容並將其送出。** 例如，您可能會建立鼓勵新客戶為您的 App 評等的通知，或傳送促進購買附加元件之特殊交易的通知。


## <a name="to-create-and-send-a-notification"></a>若要建立並傳送通知

請依照下列步驟來建立在合作夥伴中心的通知，並將它傳送給特定客戶區隔。

> [!NOTE]
> 應用程式可以從合作夥伴中心收到通知之前，您必須先呼叫[RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)方法，在您的應用程式註冊您的應用程式以接收通知。 這個方法適用於 [Microsoft Store服務 SDK](http://aka.ms/store-em-sdk)。 如需如何呼叫這個方法的詳細資訊 (包括程式碼範例)，請參閱[設定您的應用程式以接收目標式推播通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。

1. 在[合作夥伴中心](https://partner.microsoft.com/dashboard)，展開 [**互動**] 區段，然後選取**通知**。
2. 在 **\[通知\]** 頁面上，選取 **\[新通知\]**。
3. 在**選取範本**] 區段中，選擇您想要傳送，然後按一下 **[確定]** 的[通知類型](#notification-template-types)。
4. 在下一個頁面上，使用下拉式功能表選擇您想要產生通知的**\[單一應用程式\]** 或**\[多個應用程式\]**。 您可以只選取應用程式已[設定為使用 Microsoft Store Services SDK 接收通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。
5. 在 **\[通知設定\]** 區段中選擇 **\[通知\]** 的名稱，然後 (如果適用) 選擇您想要將通知傳送到的 **\[客戶群組\]**。 （傳送至多個應用程式的通知只能傳送到這些 app 的所有客戶。）如果您想要使用尚未建立的客戶區隔，請選取**\[建立新的客戶群組\]**。 請注意，這需要 24 小時才能將新區隔用於通知。 如需詳細資訊，請參閱[建立客戶區隔](create-customer-segments.md)。
6. 如果您想要指定何時傳送通知，請清除 **\[立即傳送通知\]** 核取方塊，然後選擇特定的日期和時間 (除非您指定要使用每個客戶的當地時區，否則對所有客戶皆以 UTC 顯示)。
7. 如果您想要讓通知在某個時間點到期，請清除 **\[通知永遠不會到期\]** 核取方塊，然後選擇特定的到期日期和時間 (以 UTC 表示)。
8. **傳送至單一應用程式的通知：** 如果您想要篩選收件者，以便只將通知傳送給使用特定語言的人，或是位於特定時區的人，請核取 **\[使用篩選器\]** 核取方塊。 然後，您可以指定您要使用的語言和/或時區選項。
8. **傳送至多個單一應用程式的通知：** 指定只要針對一個裝置上（每個客戶）的最後一個使用中應用程式或每個裝置上的所有應用程式傳送通知。
10. 在 **\[通知內容\]** 區段的 **\[語言\]** 功能表中，選擇您希望通知以何種語言顯示。 如需詳細資訊，請參閱[翻譯您的通知](#translate-your-notifications)。
11. 在 **\[選項\]** 區段中，輸入文字並設定您想要的任何其他選項。 如果您使用範本開始，則預設會提供一部分，但您可以視需要變更。

    可用的選項會依據您使用的通知類型而不同。 部分選項包括：

    * **啟用類型** (互動式快顯通知類型)。 您可以選擇 **\[前景\]**、**\[背景\]** 或 **\[通訊協定\]**。
    * **啟動** (互動式快顯通知類型)。 您可以選擇讓通知開啟 app 或網站。
    * **追蹤應用程式啟動速率** (互動式快顯通知類型)。 如果您想要測量透過每個通知與客戶互動的狀況，請選取此核取方塊。 如需詳細資訊，請參閱[測量通知效能](#measure-notification-performance)。
    * **持續時間** (互動式快顯通知類型)。 您可以選擇 **\[短期\]** 或 **\[長期\]**。
    * **案例** (互動式快顯通知類型)。 您可以選擇 **\[預設\]**、**\[警示\]**、**\[提醒\]** 或 **\[來電\]**。
    * **基底 URI** (互動式快顯通知類型)。 如需詳細資訊，請參閱 [BaseUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri)。
    * **新增影像查詢** (互動式快顯通知類型)。 如需詳細資訊，請參閱 [addImageQuery](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements)。
    * **視覺**。 影像、視訊或音效。 如需詳細資訊，請參閱[視覺效果](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual)。
    * **輸入**/**動作**/**選取項目** (互動式快顯通知類型)。 可讓使用者與通知互動。 如需詳細資訊，請參閱[調適型和互動式快顯通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)。
    * **繫結** (互動式磚類型)。 快顯通知範本。 如需詳細資訊，請參閱[繫結](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-binding)。

    > [!TIP]
    > 請嘗試使用[通知視覺化檢視](https://www.microsoft.com/store/apps/9nblggh5xsl1)應用程式來設計和測試您的調適型磚和互動式快顯通知。

12. 選取 **\[儲存為草稿\]** 以在稍後繼續處理通知，如果您已完成，請選取 **\[傳送\]**。


## <a name="notification-template-types"></a>通知範本類型

您可以選擇各種不同的通知範本。

-   **空白 (快顯通知)。** 以可自訂的空白快顯通知開始。 快顯通知是顯示在畫面上的快顯 UI，無論使用者是在另一個 app、在 \[開始\] 畫面或是在桌面，都能讓 app 與客戶進行通訊。
-   **空白 (磚)。** 以可自訂的空白磚通知開始。 磚是 app 在 \[開始\] 畫面上的呈現方式。 磚可以「動態」呈現，表示磚所顯示的內容會隨著通知而變更。
-   **要求評等 (快顯通知)。** 要求客戶為您的 app 評等的快顯通知。 當客戶選取通知時，會顯示 app 的Microsoft Store評等頁面。
-   **要求意見反應 (通知)。** 要求客戶為您的 app 提供意見的快顯通知。 當客戶選取通知時，會顯示應用程式的 [意見反應中樞] 頁面。
    > [!NOTE]
    > 如果您選擇這個範本類型，在 [**啟動**] 方塊中，記得將 {PACKAGE_FAMILY_NAME} 預留位置值取代為您的應用程式實際套件系列名稱 (PFN)。 您可以在 [App 身分識別](view-app-identity-details.md)頁面 (**\[App 管理\]** >  **\[App 身分識別\]**) 上找到 app 的 PFN。

    ![意見反應快顯通知 \[啟動\] 方塊](images/push-notifications-feedback-toast-launch-box.png)

-   **交叉宣傳 (快顯通知)。** 宣傳您選擇的其他 app 的快顯通知。 當客戶選取通知時，會顯示其他應用程式的Microsoft Store清單。
    > [!NOTE]
    > 如果您選擇這個範本類型，在 [**啟動**] 方塊中，記得將 **{您想在此促銷的 ProductId}** 預留位置值取代為您想聯合促銷之項目的實際Microsoft Store識別碼。 您可以在 [App 身分識別](view-app-identity-details.md)頁面 (**\[App 管理\]** >  **\[App 身分識別\]**) 上找到Microsoft Store識別碼。

    ![交叉宣傳快顯通知 \[啟動\] 方塊](images/push-notifications-promote-toast-launch-box.png)

-   **宣傳銷售 (快顯通知)。** 可用來宣告 app 優惠的快顯通知。 當客戶選取通知時，會顯示您的 app 的Microsoft Store清單。
-   **提示更新 (快顯通知)。** 鼓勵執行您的舊版 app 的客戶安裝最新版的快顯通知。 當客戶選取通知時，Microsoft Store 應用程式將會啟動，並顯示 **\[下載與更新\]** 清單。 請注意，此範本只能用於單一 App，您無法以特定客戶區隔為目標，也不能定義傳送的時間；我們一律將此通知排程在 24 小時內傳送，但會盡最大努力將目標設定為所有尚未執行您的最新版本 App 的使用者。


## <a name="measure-notification-performance"></a>測量通知效能

您可以測量透過每個通知與客戶互動的狀況。


### <a name="to-measure-notification-performance"></a>測量通知效能

1.  當您建立通知時，請在 **\[通知內容\]** 區段選取 **\[追蹤 app 啟動速率\]** 核取方塊。
2.  在您的應用程式中呼叫[ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch)方法，以通知合作夥伴中心，您的應用程式已啟動來回應特定對象的通知。 這個方法由 Microsoft Store Services SDK 提供。 如需如何呼叫這個方法的詳細資訊，請參閱[設定您的應用程式會接收到合作夥伴中心的通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。


### <a name="to-view-notification-performance"></a>檢視通知效能

當您設定好通知和您的應用程式以測量通知效能，如上文所述時，您可以看到您的通知執行狀況。

若要檢閱每個通知的詳細的資料：

1.  在合作夥伴中心，展開**互動**區段，然後選取**通知**。
2.  在表格中的現有的通知，選取 [**進行中**或**已完成**，並再看看 [**傳遞速率**和**應用程式啟動速率**欄來查看每個通知的整體效能。
3.  若要查看更詳細的效能詳細資料，請選取通知名稱。 在 **\[傳遞統計資料\]** 區段中，您可以檢視下列通知 **\[狀態\]** 類型的 **\[計數\]** 和 **\[百分比\]** 資訊︰
    * **失敗**︰通知因為某些原因未傳遞。 例如，如果 Windows 通知服務中發生問題，這個情況就會發生。
    * **通道到期失敗**： 無法傳遞通知，因為應用程式與合作夥伴中心之間的通道已到期。 例如，如果客戶長時間未開啟您的 app，這個情況就會發生。
    * **傳送中**︰通知在待傳送的佇列中。
    * **已傳送**︰已傳送通知。
    * **啟動**︰通知已傳送、客戶按一下通知，因此開啟您的應用程式。 請注意，這只會追蹤 app 啟動。 這個狀態不包括邀請客戶採取其他行動 (例如啟動Microsoft Store以留下評等) 的通知。
    * **不明**︰我們無法判斷這個通知的狀態。

分析您的通知的使用者活動資料：

1.  在合作夥伴中心，展開**互動**區段，然後選取**通知**。
2.  在**通知**頁面上，按一下 [**分析**] 索引標籤。此索引標籤會顯示下列資料：
    * 為您的快顯通知和控制中心通知各種不同的使用者動作狀態的圖形檢視。
    * 世界地圖檢視的按一下-透過-率為您的快顯通知和控制中心通知。
3. 在頁面頂端附近，您可以選取您想要顯示資料的時間週期。 預設選項是 30D（30 天），但您可以選擇在 3、6 或 12 個月期間顯示資料，或指定自訂的日期範圍。 您也可以展開**篩選器**來篩選的所有應用程式和市場的資料。

## <a name="translate-your-notifications"></a>翻譯您的通知

為使通知發揮最大的影響力，請考慮將它們翻譯成您客戶偏好的語言。 合作夥伴中心可讓您更輕鬆地自動翻譯您的通知，利用[Microsoft Translator](https://www.microsoft.com/translator/home.aspx)服務的強大功能。

1.  當您以您的預設語言撰寫通知後，請選取 **\[新增語言\]** (在 **\[通知內容\]** 區段的 **\[語言\]** 功能表之下)。
2.  在 **\[新增語言\]** 視窗中選取顯示通知的他種語言，然後選取 **\[更新\]**。
您的通知將會自動翻譯為您在 **\[新增語言\]** 視窗選擇的語言，而這些程式語言將新增到 **\[語言\]** 功能表。
3.  若要查看您通知的翻譯，請在 **\[語言\]** 功能表中選取剛才新增的語言。

關於翻譯，需注意幾件事︰
 - 您可以在語言的 **\[內容\]** 方塊中輸入不同於自動翻譯的內容以將它覆寫。
 - 如果您在覆寫自動翻譯後新增另一個文字方塊到英文版的通知，新的文字方塊將不會新增至翻譯的通知。 在這種情況下，您必須手動將新的文字方塊新增到每個翻譯的通知。
 - 如果您在通知翻譯後變更英文的文字，我們將會自動更新翻譯的通知以符合變更。 不過，如果您先前選擇覆寫初始翻譯，就不會發生這個狀況。

## <a name="related-topics"></a>相關主題
- [適用於 UWP app 的磚](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Windows 推播通知服務 (WNS) 概觀](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [通知視覺化檢視應用程式](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync() 方法](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
