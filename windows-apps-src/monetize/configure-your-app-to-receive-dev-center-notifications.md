---
author: mcleanbyron
Description: "了解如何登錄您的 UWP app 以接收您從「Windows 開發人員中心」傳送的推播通知。"
title: "設定您的應用程式以接收開發人員中心推播通知"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, Microsoft Store Services SDK, 目標式推播通知, 開發人員中心"
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: a7a4c9800d5053e4b20cea578ff03a970179c7bf
ms.lasthandoff: 02/08/2017

---

# <a name="configure-your-app-to-receive-dev-center-push-notifications"></a>設定您的應用程式以接收開發人員中心推播通知

您可以使用「Windows 開發人員中心」儀表板中的 **\[推播通知\]** 頁面，藉由傳送目標式推播通知給已安裝您「通用 Windows 平台」(UWP) app 的裝置，來直接進行客戶業務開發。 例如，您可以使用目標式推播通知來促使客戶採取行動，像是為您的 App 評分或嘗試新功能。 您可以傳送數種不同類型的推播通知，包括快顯通知、磚通知及原始 XML 通知。 您也可以追蹤從您的推播通知產生的 App 啟動率。 如需有關此功能的詳細資訊，請參閱[傳送推播通知給您 App 的客戶](../publish/send-push-notifications-to-your-apps-customers.md)。

您必須先使用 Microsoft Store Services SDK 中 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 類別的方法來登錄您的 App 以接收通知，才能從「開發人員中心」傳送目標式推播通知給您的客戶。 您可以使用此類別的其他方法來通知「開發人員中心」您的 App 已啟動來回應目標式推播通知 (如果您想要追蹤從您的通知產生的 App 啟動率)，也可以停止接收通知。

## <a name="configure-your-project"></a>設定您的專案

在撰寫任何程式碼之前，請先依照下列步驟，在您的專案中新增對 Microsoft Store Services SDK 的參考：

1. 如果您尚未這麼做，請在您的開發電腦上[安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 除了用於登錄 App 以接收通知的 API 之外，此 SDK 也提供其他功能 (例如，在 App 中使用 A/B 測試來執行實驗，以及顯示廣告) 的 API。
2. 在 Visual Studio 中，開啟您的專案。
3. 在 \[方案總管\] 中，於專案的 \[參考\]**** 節點上按一下滑鼠右鍵，然後按一下 \[加入參考\]****。
4. 在 \[參考管理員\]**** 中，展開 \[通用 Windows\]****，然後按一下 \[擴充功能\]****。
5. 在 SDK 清單中，按一下 **\[Microsoft Engagement Framework\]** 旁邊的核取方塊，然後按一下 **\[確定\]**。

## <a name="register-for-push-notifications"></a>登錄推播通知

登錄您的 App 以接收來自「開發人員中心」目標式推播通知：

1. 在您的專案中，找出在啟動時執行的程式碼區段，您可以在其中登錄您的 App 以接收「開發人員中心」通知。
2. 將下列陳述式新增到程式碼檔案頂端。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. 取得 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 物件，然後呼叫您稍早所識別之啟動程式碼中的其中一個 [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync.aspx) 多載。 每次啟動您的 App 時，都應該呼叫此方法。

  * 如果您希望「開發人員中心」建立自己的通知通道 URI，請呼叫 [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 多載。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]

    <span/>
    >**重要**&nbsp;&nbsp;如果您的 App 也會呼叫 [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) 來建立 WNS 的通知通道，請確定您的程式碼不會同時呼叫 [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) 和 [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 多載。 如果您需要呼叫這兩種方法，請務必依序呼叫它們，然後等待一個方法傳回後，再呼叫另一個方法。

  * 如果您想要指定要用於來自「開發人員中心」之目標式推播通知的通道 URI，請呼叫 [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://msdn.microsoft.com/library/windows/apps/mt771191.aspx) 多載。 例如，如果您的 App 已經使用「Windows 推播通知服務」(WNS)，而您想要使用相同的通道 URI，您可能就會想要這麼做。 您必須先建立一個 [StoreServicesNotificationChannelParameters](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.aspx) 物件，並指派 [CustomNotificationChannelUri](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri.aspx) 屬性給您的通道 URI。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

  >#### <a name="understanding-how-your-app-responds-when-the-user-launches-your-app"></a>了解當使用者啟動您的 App 時，App 會如何回應

  >在您登錄 App 以接收通知並[從開發人員中心傳送推播通知給您 App 的客戶](../publish/send-push-notifications-to-your-apps-customers.md)之後，當使用者啟動 App 來回應您的推播通知時，就會呼叫下列其中一個您 App 中的進入點。 如果您想要在使用者啟動您 App 時執行某個程式碼，您可以將該程式碼新增到您 App 中的這些進入點其中之一。

  >* 如果推播通知具有前景啟用類型，請覆寫您專案中 **App** 類別的 [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) 方法，並將您的程式碼新增到此方法中。

  >* 如果推播通知具有背景啟用類型，請將您的程式碼新增到您[背景工作](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法中。

  >例如，您可能會想要針對已在您 App 中購買任何付費附加元件的 App 使用者提供免費的附加元件，來獎勵這些使用者。 在此情況下，您可以傳送推播通知給以這些使用者為目標的[客戶區隔](../publish/create-customer-segments.md)。 接著，您可以在上面所列的其中一個進入點中，新增程式碼來給予他們免費的[在應用程式內購買](in-app-purchases-and-trials.md)。

## <a name="notify-dev-center-of-your-app-launch"></a>通知開發人員中心您的 App 啟動

如果您針對「開發人員中心」推播通知選取 **\[追蹤 App 啟動率\]** 選項，請從您 App 中的適當進入點呼叫 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 方法來通知「開發人員中心」您的 App 已啟動來回應推播通知。

此方法也會傳回您 App 的原始啟動引數。 在您選擇追蹤「開發人員中心」推播通知的 App 啟動率之後，系統會在啟動引數中新增一個隱晦的追蹤識別碼，以協助在「開發人員中心」中追蹤 App 啟動。 您必須將 App 的啟動引數傳送給 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 方法，而此方法會將追蹤識別碼傳送給「開發人員中心」、從啟動引數中移除追蹤識別碼，然後將原始啟動引數傳回給您的程式碼。

您呼叫此方法的方式取決於目標式推播通知的啟用類型：

* 如果推播通知具有前景啟用類型，請從您 App 中的 [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) 方法覆寫呼叫此方法，然後將已傳遞的 [ToastNotificationActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.toastnotificationactivatedeventargs.aspx) 物件中所提供的引數傳遞給此方法。 下列程式碼範例假設您的程式碼檔案有 **Microsoft.Services.Store.Engagement** 和 **Windows.ApplicationModel.Activation** 命名空間的 **using** 陳述式。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* 如果推播通知具有背景啟用類型，請從您[背景工作](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法呼叫此方法，然後將已傳遞的 [ToastNotificationActionTriggerDetail](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.toastnotificationactiontriggerdetail.aspx) 物件中所提供的引數傳遞給此方法。 下列程式碼範例假設您的程式碼檔案有 **Microsoft.Services.Store.Engagement**、**Windows.ApplicationModel.Background** 及 **Windows.UI.Notifications** 命名空間的 **using** 陳述式。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

## <a name="unregister-for-push-notifications"></a>取消推播通知登錄

如果您想要讓 App 停止接收目標式「Windows 開發人員中心」推播通知，請呼叫 [UnregisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 方法。

> [!div class="tabbedCodeSnippets"]
[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

請注意，此方法會讓用於通知的通道變成無效，因此 App 將不會再收到來自*「任何」*服務的推播通知。 將通道關閉之後，該通道即永遠無法再用於任何服務，包括目標式「Windows 開發人員中心」推播通知及其他使用 WNS 的通知。 若要繼續將推播通知傳送給此 App，此 App 必須要求新通道。

## <a name="related-topics"></a>相關主題

* [傳送推播通知給您的 App 客戶](../publish/send-push-notifications-to-your-apps-customers.md)
* [Windows 推播通知服務 (WNS) 概觀](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [如何要求、建立以及儲存通知通道](https://msdn.microsoft.com/library/windows/apps/xaml/hh868221)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)

