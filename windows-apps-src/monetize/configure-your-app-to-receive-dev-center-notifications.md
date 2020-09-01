---
Description: 瞭解如何註冊您的 UWP 應用程式，以接收從合作夥伴中心傳送的推播通知。
title: 設定您的應用程式以接收目標式推播通知
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、Microsoft Store Services SDK、目標推播通知合作夥伴中心
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: d6a420befac980574cf64e8a599d122df4c99ef4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155652"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>設定您的應用程式以接收目標式推播通知

您可以使用合作夥伴中心中的 [ **推播通知** ] 頁面，將目標推播通知傳送至已安裝通用 WINDOWS 平臺 (UWP) 應用程式的裝置，藉此直接與客戶互動。 例如，您可以使用目標式推播通知來促使客戶採取行動，像是為您的 App 評分或嘗試新功能。 您可以傳送數種不同類型的推播通知，包括快顯通知、磚通知及原始 XML 通知。 您也可以追蹤從您的推播通知產生的 App 啟動率。 如需有關此功能的詳細資訊，請參閱[傳送推播通知給您 App 的客戶](../publish/send-push-notifications-to-your-apps-customers.md)。

在您可以從合作夥伴中心將目標推播通知傳送給客戶之前，您必須在 Microsoft Store Services SDK 中使用 [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 類別的方法來註冊您的應用程式，以接收通知。 您可以使用此類別的其他方法來通知合作夥伴中心您的應用程式已啟動，以回應目標推播通知 (如果您想要追蹤由通知) 所產生的應用程式啟動率，以及停止接收通知的速率。

## <a name="configure-your-project"></a>設定您的專案

在撰寫任何程式碼之前，請先依照下列步驟，在您的專案中新增對 Microsoft Store Services SDK 的參考：

1. 如果您尚未這麼做，請在您的開發電腦上[安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 
2. 在 Visual Studio 中，開啟您的專案。
3. 在方案總管中，以滑鼠右鍵按一下專案的 [ **參考** ] 節點，然後按一下 [ **加入參考**]。
4. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**，然後按一下 **\[擴充功能\]**。
5. 在 Sdk 清單中，按一下 [ **Microsoft Engagement 架構** ] 旁的核取方塊，然後按一下 **[確定]**。

## <a name="register-for-push-notifications"></a>註冊推播通知

註冊您的應用程式以接收來自合作夥伴中心的目標推播通知：

1. 在您的專案中，找出在啟動時執行的程式碼區段，您可以在其中登錄您的 App 以接收通知。
2. 將下列陳述式新增到程式碼檔案頂端。

    [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. 取得 [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 物件，然後呼叫您稍早所識別之啟動程式碼中的其中一個 [RegisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 多載。 每次啟動您的 App 時，都應該呼叫此方法。

  * 如果您想要合作夥伴中心建立自己的通知通道 URI，請呼叫 [RegisterNotificationChannelAsync ( # B1 ](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 多載。

      [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > 如果您的 App 也會呼叫 [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) 來建立 WNS 的通知通道，請確定您的程式碼不會同時呼叫 [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) 和 [RegisterNotificationChannelAsync()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 多載。 如果您需要呼叫這兩種方法，請務必依序呼叫它們，然後等待一個方法傳回後，再呼叫另一個方法。

  * 如果您想要指定要用於來自合作夥伴中心之目標推播通知的通道 URI，請呼叫 [RegisterNotificationChannelAsync (StoreServicesNotificationChannelParameters) ](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 多載。 例如，如果您的 App 已經使用「Windows 推播通知服務」(WNS)，而您想要使用相同的通道 URI，您可能就會想要這麼做。 您必須先建立一個 [StoreServicesNotificationChannelParameters](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) 物件，並指派 [CustomNotificationChannelUri](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) 屬性給您的通道 URI。

      [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> 在您呼叫 **RegisterNotificationChannelAsync** 方法時，會在 App 的本機應用程式資料存放區中建立名為 MicrosoftStoreEngagementSDKId.txt 的檔案 (位於 [ApplicationData.LocalFolder](/uwp/api/Windows.Storage.ApplicationData.LocalFolder) 屬性傳回的資料夾)。 此檔案包含目標式推播通知基礎結構所使用的識別碼。 請確定您的 App 不會修改或刪除此檔案。 否則，您的使用者可能會收到多次通知，或通知可能無法以其他方式正常運作。

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>目標式推播通知是如何路由至客戶

當您的 app 呼叫 **RegisterNotificationChannelAsync**，這個方法會收集目前登入裝置之客戶的 Microsoft 帳戶。 稍後，當您將目標推播通知傳送至包含此客戶的區段時，合作夥伴中心會將通知傳送至與此客戶 Microsoft 帳戶相關聯的裝置。

如果啟動您的 App 的客戶在仍登入其 Microsoft 帳戶的裝置時，將裝置借給別人使用，請留意其他人可能會看到以原來客戶為目標的通知。 這可能會產生非預期的結果，尤其是針對需要客戶登入才能使用服務的 App。 若要在本案例中防止其他使用者查看您的目標式通知，請在登出您的 app 時呼叫[UnregisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 方法。 如需詳細資訊，請參閱本文稍後的[取消推播通知登錄](#unregister)。

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>當使用者啟動您的 App 時，App 會如何回應

註冊您的應用程式以接收通知，而且您將 [推播通知從合作夥伴中心傳送至應用程式的客戶](../publish/send-push-notifications-to-your-apps-customers.md)之後，當使用者啟動應用程式以回應您的推播通知時，將會呼叫應用程式中的下列其中一個進入點。 如果您想要在使用者啟動您 App 時執行某個程式碼，您可以將該程式碼新增到您 App 中的這些進入點其中之一。

  * 如果推播通知具有前景啟用類型，請覆寫您專案中 **App** 類別的 [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) 方法，並將您的程式碼新增到此方法中。

  * 如果推播通知具有背景啟用類型，請將您的程式碼新增到您[背景工作](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法中。

例如，您可能會想要針對已在您 App 中購買任何付費附加元件的 App 使用者提供免費的附加元件，來獎勵這些使用者。 在此情況下，您可以傳送推播通知給以這些使用者為目標的[客戶區隔](../publish/create-customer-segments.md)。 接著，您可以在上面所列的其中一個進入點中，新增程式碼來給予他們免費的 [App 內購買](in-app-purchases-and-trials.md)。

## <a name="notify-partner-center-of-your-app-launch"></a>通知合作夥伴中心您的應用程式啟動

如果您在合作夥伴中心中為您的目標推播通知選取 [ **追蹤應用程式啟動率** ] 選項，請從應用程式中適當的進入點呼叫 [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 方法，以通知合作夥伴中心您的應用程式已啟動，以回應推播通知。

此方法也會傳回您 App 的原始啟動引數。 當您選擇追蹤推播通知的應用程式啟動率時，不透明的追蹤識別碼會新增至啟動引數，以協助追蹤合作夥伴中心中的應用程式啟動。 您必須將應用程式的啟動引數傳遞至 [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 方法，這個方法會將追蹤識別碼傳送給合作夥伴中心、從啟動引數移除追蹤識別碼，然後將原始啟動引數傳回給您的程式碼。

您呼叫此方法的方式取決於推播通知的啟用類型：

* 如果推播通知具有前景啟用類型，請從您 App 中的 [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) 方法覆寫呼叫此方法，然後將已傳遞的 [ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) 物件中所提供的引數傳遞給此方法。 下列程式碼範例假設您的程式碼檔案有 **Microsoft.Services.Store.Engagement** 和 **Windows.ApplicationModel.Activation** 命名空間的 **using** 陳述式。

  [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* 如果推播通知具有背景啟用類型，請從您[背景工作](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法呼叫此方法，然後將已傳遞的 [ToastNotificationActionTriggerDetail](/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) 物件中所提供的引數傳遞給此方法。 下列程式碼範例假設您的程式碼檔案有 **Microsoft.Services.Store.Engagement**、**Windows.ApplicationModel.Background** 及 **Windows.UI.Notifications** 命名空間的 **using** 陳述式。

  [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>取消推播通知登錄

如果您想要讓應用程式停止從合作夥伴中心接收目標推播通知，請呼叫 [UnregisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 方法。

[!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

請注意，此方法會使用於通知的通道失效，因此應用程式不會再從 *任何* 服務接收推播通知。 在關閉之後，就無法再次針對任何服務使用通道，包括來自合作夥伴中心的目標推播通知，以及使用 WNS 的其他通知。 若要繼續將推播通知傳送給此 App，此 App 必須要求新通道。

## <a name="related-topics"></a>相關主題

* [傳送推播通知給您的應用程式客戶](../publish/send-push-notifications-to-your-apps-customers.md)
* [Windows 推播通知服務 (WNS) 概觀](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
* [如何要求、建立以及儲存通知通道](/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)