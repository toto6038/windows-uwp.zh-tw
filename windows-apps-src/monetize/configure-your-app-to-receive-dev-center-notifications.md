---
Description: 了解如何註冊您的 UWP 應用程式，以接收來自合作夥伴中心傳送推播通知。
title: 設定您的應用程式以接收目標式推播通知
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10 uwp，Microsoft Store Services SDK，目標推播通知，合作夥伴中心
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: a23da0bf740abfeece0047b8afab2ebff987f9d1
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335046"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>設定您的應用程式以接收目標式推播通知

您可以使用**推播通知**合作夥伴中心，以直接與客戶交流，傳送頁面設為目標的推播通知至通用 Windows 平台 (UWP) 應用程式安裝所在的裝置。 例如，您可以使用目標式推播通知來促使客戶採取行動，像是為您的 App 評分或嘗試新功能。 您可以傳送數種不同類型的推播通知，包括快顯通知、磚通知及原始 XML 通知。 您也可以追蹤從您的推播通知產生的 App 啟動率。 如需有關此功能的詳細資訊，請參閱[傳送推播通知給您 App 的客戶](../publish/send-push-notifications-to-your-apps-customers.md)。

您可以從合作夥伴中心為您的客戶傳送目標推播通知之前，您必須使用的方法[StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager)中註冊您的應用程式，才能接收 Microsoft Store Services SDK 類別通知。 您可以使用這個類別的其他方法來通知您的應用程式已啟動以回應目標推播通知 （如果您想要追蹤的應用程式啟動次數所產生的通知速率） 的合作夥伴中心並停止接收通知。

## <a name="configure-your-project"></a>設定您的專案

在撰寫任何程式碼之前，請先依照下列步驟，在您的專案中新增對 Microsoft Store Services SDK 的參考：

1. 如果您尚未這麼做，請在您的開發電腦上[安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 
2. 在 Visual Studio 中，開啟您的專案。
3. 在 \[方案總管\] 中，於專案的 \[參考\] 節點上按一下滑鼠右鍵，然後按一下 \[加入參考\]。
4. 在 \[參考管理員\] 中，展開 \[通用 Windows\]，然後按一下 \[擴充功能\]。
5. 在 SDK 清單中，按一下 \[Microsoft Engagement Framework\] 旁邊的核取方塊，然後按一下 \[確定\]。

## <a name="register-for-push-notifications"></a>登錄推播通知

若要註冊您的應用程式，以接收來自合作夥伴中心目標推播通知：

1. 在您的專案中，找出在啟動時執行的程式碼區段，您可以在其中登錄您的 App 以接收通知。
2. 將下列陳述式新增到程式碼檔案頂端。

    [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. 取得 [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 物件，然後呼叫您稍早所識別之啟動程式碼中的其中一個 [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 多載。 每次啟動您的 App 時，都應該呼叫此方法。

  * 如果您想要建立自己的通知的通道 URI 的合作夥伴中心時，呼叫[RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)多載。

      [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > 如果您的 App 也會呼叫 [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) 來建立 WNS 的通知通道，請確定您的程式碼不會同時呼叫 [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) 和 [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 多載。 如果您需要呼叫這兩種方法，請務必依序呼叫它們，然後等待一個方法傳回後，再呼叫另一個方法。

  * 如果您想要指定的通道 URI，用來從合作夥伴中心，呼叫的目標推播通知[RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)多載。 例如，如果您的 App 已經使用「Windows 推播通知服務」(WNS)，而您想要使用相同的通道 URI，您可能就會想要這麼做。 您必須先建立一個 [StoreServicesNotificationChannelParameters](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) 物件，並指派 [CustomNotificationChannelUri](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) 屬性給您的通道 URI。

      [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> 在您呼叫 **RegisterNotificationChannelAsync** 方法時，會在 App 的本機應用程式資料存放區中建立名為 MicrosoftStoreEngagementSDKId.txt 的檔案 (位於 [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalFolder) 屬性傳回的資料夾)。 此檔案包含目標式推播通知基礎結構所使用的識別碼。 請確定您的 App 不會修改或刪除此檔案。 否則，您的使用者可能會收到多次通知，或通知可能無法以其他方式正常運作。

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>目標式推播通知是如何路由至客戶

當您的 app 呼叫 **RegisterNotificationChannelAsync**，這個方法會收集目前登入裝置之客戶的 Microsoft 帳戶。 稍後，您將目標推播通知傳送至包含此客戶的區段時，合作夥伴中心就會傳送通知給此客戶的 Microsoft 帳戶相關聯的裝置。

如果啟動您的 App 的客戶在仍登入其 Microsoft 帳戶的裝置時，將裝置借給別人使用，請留意其他人可能會看到以原來客戶為目標的通知。 這可能會產生非預期的結果，尤其是針對需要客戶登入才能使用服務的 App。 若要在本案例中防止其他使用者查看您的目標式通知，請在登出您的 app 時呼叫[UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 方法。 如需詳細資訊，請參閱本文稍後的[取消推播通知登錄](#unregister)。

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>當使用者啟動您的 App 時，App 會如何回應

您的應用程式註冊以接收通知，而且您之後[將推播通知傳送給您的應用程式的客戶，從合作夥伴中心](../publish/send-push-notifications-to-your-apps-customers.md)，當使用者啟動您的應用程式，回應中會呼叫其中一個應用程式中的下列進入點推播通知。 如果您想要在使用者啟動您 App 時執行某個程式碼，您可以將該程式碼新增到您 App 中的這些進入點其中之一。

  * 如果推播通知具有前景啟用類型，請覆寫您專案中 **App** 類別的 [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) 方法，並將您的程式碼新增到此方法中。

  * 如果推播通知具有背景啟用類型，請將您的程式碼新增到您[背景工作](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法中。

例如，您可能會想要針對已在您 App 中購買任何付費附加元件的 App 使用者提供免費的附加元件，來獎勵這些使用者。 在此情況下，您可以傳送推播通知給以這些使用者為目標的[客戶區隔](../publish/create-customer-segments.md)。 接著，您可以在上面所列的其中一個進入點中，新增程式碼來給予他們免費的 [App 內購買](in-app-purchases-and-trials.md)。

## <a name="notify-partner-center-of-your-app-launch"></a>通知的應用程式啟動的合作夥伴中心

如果您選取**追蹤應用程式啟動速率**選項，在合作夥伴中心，呼叫目標推播通知[ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch)從您的應用程式中的適當的進入點方法您的應用程式已啟動以回應推播通知通知合作夥伴中心。

此方法也會傳回您 App 的原始啟動引數。 當您選擇要追蹤您的推播通知的應用程式啟動速率時，不透明的追蹤識別碼新增至在合作夥伴中心內啟動的啟動引數，以協助追蹤應用程式。 您必須傳遞您的應用程式的啟動引數[ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch)方法，並且此方法將追蹤識別碼傳送至合作夥伴中心、 啟動引數中移除的追蹤識別碼並傳回原始啟動您的程式碼的引數。

您呼叫此方法的方式取決於推播通知的啟用類型：

* 如果推播通知具有前景啟用類型，請從您 App 中的 [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) 方法覆寫呼叫此方法，然後將已傳遞的 [ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) 物件中所提供的引數傳遞給此方法。 下列程式碼範例假設您的程式碼檔案有 **Microsoft.Services.Store.Engagement** 和 **Windows.ApplicationModel.Activation** 命名空間的 **using** 陳述式。

  [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* 如果推播通知具有背景啟用類型，請從您[背景工作](../launch-resume/support-your-app-with-background-tasks.md)的 [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法呼叫此方法，然後將已傳遞的 [ToastNotificationActionTriggerDetail](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) 物件中所提供的引數傳遞給此方法。 下列程式碼範例假設您的程式碼檔案有 **Microsoft.Services.Store.Engagement**、**Windows.ApplicationModel.Background** 及 **Windows.UI.Notifications** 命名空間的 **using** 陳述式。

  [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>取消推播通知登錄

如果您想要停止接收應用程式的目標從合作夥伴中心，呼叫的推播通知[UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync)方法。

[!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

請注意，此方法會讓用於通知的通道變成無效，因此 App 將不會再收到來自「任何」服務的推播通知。 已關閉之後，通道不能再針對任何服務，包括目標推播通知從合作夥伴中心及其他使用 WNS 的通知。 若要繼續將推播通知傳送給此 App，此 App 必須要求新通道。

## <a name="related-topics"></a>相關主題

* [傳送推播通知給您的應用程式客戶](../publish/send-push-notifications-to-your-apps-customers.md)
* [Windows 推播通知服務 (WNS) 概觀](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)
* [如何要求、 建立和儲存通知通道](https://docs.microsoft.com/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
