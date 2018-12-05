---
Description: The Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. This provides a mechanism to deliver new updates to your users in a power-efficient and dependable way.
title: Windows 推播通知服務 (WNS) 概觀
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f131ad229b4ba22f7fa4652aa302e3596819f206
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8693565"
---
# <a name="windows-push-notification-services-wns-overview"></a>Windows 推播通知服務 (WNS) 概觀
 

Windows 推播通知服務 (WNS) 可以讓協力廠商開發人員從自己的雲端服務傳送快顯通知、磚、徽章和原始更新。 這提供一種機制，用省電又可靠的方法，將最新的更新資訊傳送給使用者。

## <a name="how-it-works"></a>運作方式


下圖顯示傳送推播通知的完整資料流程。 流程有三個步驟：

1.  您的應用程式從通用 Windows 平台要求推播通知通道。
2.  Windows 要求 WNS 建立通知通道。 這個通道以統一資源識別元 (URI) 的形式傳回呼叫裝置。
3.  Windows 將通知通道 URI 傳回您的應用程式。
4.  您的應用程式將 URI 傳回您自己的雲端服務。 您接著將 URI 儲存在您自己的雲端服務，當您傳送通知時，就可以存取該 URI。 URI 是您自己的應用程式和服務之間的介面，您必須負責以安全和可靠的 Web 標準實作這個介面。
5.  如果您的雲端服務有要傳送的更新，其會使用通道 URI 通知 WNS。 而做法是透過安全通訊端層 (SSL) 發出 HTTP POST 要求，包括通知承載。 這個步驟需要驗證。
6.  WNS 接收要求，並將通知路由到適當裝置。

![推播通知的 WNS 資料流程圖表](images/wns-diagram-01.png)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>註冊您的應用程式與接收雲端服務認證


您的應用程式必須先在市集儀表板註冊，您才能夠使用 WNS 傳送通知。 這樣做會將您應用程式的認證提供給您，您的雲端服務向 WNS 進行驗證時要使用該認證。 這些認證由套件安全性識別碼 (SID) 與祕密金鑰組成。 若要執行這項註冊，登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。 建立您的應用程式之後，您可以依照**應用程式管理 - WNS/MPNS** 頁面的指示來擷取認證。 如果您想要使用 Live 服務解決方案，請遵循此頁面的 **Live 服務網站**連結。

每個應用程式都有自己雲端服務的一組認證。 這些認證無法用於傳送通知給任何其他應用程式。

如需如何註冊應用程式的詳細資訊，請參閱[如何使用 Windows 通知服務 (WNS) 進行驗證](https://msdn.microsoft.com/library/windows/apps/hh465407)。

## <a name="requesting-a-notification-channel"></a>要求通知通道


執行可接收推播通知的應用程式時，該應用程式必須先透過 [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_) 要求通知通道。 如需查看完整討論與程式碼範例，請參閱[如何要求、建立以及儲存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)。 這個 API 可傳回以唯一方式連結至呼叫端應用程式及其磚的通道 URI，而所有的通知類型都可以透過它來傳送。

應用程式順利建立通道 URI 後，會將通道 URI 連同應該與此 URI 關聯的任何應用程式特定中繼資料一起傳送到雲端服務。

### <a name="important-notes"></a>重要事項

-   我們不保證應用程式的通知通道 URI 一律保持相同。 我們建議每次執行應用程式時要求新通道，並在 URI 變更時更新本身的服務。 開發人員不得修改通道 URI，而是要將它視為黑箱字串。 在這個時候，通道 URI 會在 30 天後到期。 如果您的 windows 10 應用程式會定期更新其通道在背景中的，則您可以下載的[推播和定期通知範例](http://go.microsoft.com/fwlink/p/?linkid=231476)如 Windows8.1 並重複使用其原始程式碼和/或其示範的模式。
-   雲端服務與用戶端應用程式之間的介面要由您 (開發人員) 實作。 我們建議應用程式完成與本身服務的驗證程序，並透過安全通訊協定 (像是 HTTPS) 傳輸資料。
-   雲端服務務必確定通道 URI 使用「notify.windows.com」網域，這一點非常重要。 在任何情況下服務都不可以將通知推播至其他任何網域的通道。 如果應用程式的回呼遭到竄改，惡意攻擊者可能會提交通道 URI 來詐騙 WNS。 如果不檢查網域，您的雲端服務可能會不知不覺地將資訊曝露給攻擊者。
-   如果您的雲端服務嘗試將通知傳遞到已過期的通道，WNS 將傳回[回應碼 410](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#WNSResponseCodes)。 您回應這該代碼的方式為讓您的服務不繼續嘗試傳送通知到該 URI。

## <a name="authenticating-your-cloud-service"></a>驗證您的雲端服務


若要傳送通知，雲端服務必須透過 WNS 進行驗證。 當您向 Microsoft Store 儀表板註冊您的應用程式時，便會發生這個程序的第一個步驟。 進行註冊期間，將會提供應用程式套件安全性識別碼 (SID) 與祕密金鑰。 您的雲端服務會使用這個資訊向 WNS 進行驗證。

WNS 驗證配置使用 [OAuth 2.0](http://go.microsoft.com/fwlink/p/?linkid=226787) 通訊協定的用戶端認證設定檔進行實作。 雲端服務提供本身的認證 (套件 SID 與祕密金鑰) 向 WNS 進行驗證。 然後會收到傳回的存取權杖。 這個存取權杖可讓雲端服務傳送通知。 每個傳送至 WNS 的通知要求都需要有權杖。

高階資訊鏈結如下所示：

1.  雲端服務遵循 OAuth 2.0 通訊協定，透過 HTTPS 將本身的認證傳送至 WNS。 如此即可向 WNS 驗證服務。
2.  如果驗證成功，WNS 就會傳回存取權杖。 所有後續通知要求都將使用這個存取權杖，直至到期為止。

![雲端服務驗證的 WNS 圖表](images/wns-diagram-02.png)

在向 WNS 進行驗證時，雲端服務會透過安全通訊端階層 (SSL) 提交 HTTP 要求。 參數使用「application/x-www-for-urlencoded」格式提供。 在「client_id」欄位中提供您的套件 SID，並在「client_secret」欄位中提供您的祕密金鑰。 如需語法詳細資訊，請參閱[存取權杖要求](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#access_token_request)參考。

**注意：** 這是只是範例，您可以成功使用您自己的程式碼中的不剪下-和貼上程式碼。

 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

WNS 驗證雲端服務，如果成功，便傳送「200 確定」回應。 存取權杖由包含在 HTTP 回應主體 (使用「application/json」媒體類型) 內的參數傳回。 您的服務收到存取權杖後，就可以傳送通知。

以下範例顯示成功的驗證回應，其中包括存取權杖。 如需語法詳細資訊，請參閱[推播通知服務要求和回應標頭](https://msdn.microsoft.com/library/windows/apps/hh465435)。

``` http
 HTTP/1.1 200 OK   
 Cache-Control: no-store
 Content-Length: 422
 Content-Type: application/json
 
 {
     "access_token":"EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=", 
     "token_type":"bearer"
 }
```

### <a name="important-notes"></a>重要事項

-   這個程序支援草稿版本 V16 的 OAuth 2.0 通訊協定。
-   OAuth 要求建議 (RFC) 使用「用戶端」一詞來表示雲端服務。
-   當 OAuth 草稿完成時，這個程序可能會變更。
-   存取權杖可在多個通知要求重複使用。 藉此雲端服務只需驗證一次便能夠傳送多個通知。 不過，當存取權杖到期時，雲端服務必須重新驗證才能收到新的存取權杖。

## <a name="sending-a-notification"></a>傳送通知


使用通道 URI，雲端服務就能在有提供給使用者的更新時傳送通知。

上述存取權杖可在多個通知要求重複使用；雲端伺服器不需要針對每個通知要求新的存取權杖。 如果存取權杖已經過期，通知要求會傳回錯誤。 如果存取權杖被拒絕，建議您不要嘗試多次重新傳送您的通知。 如果遇到這種錯誤，您必須要求新存取權杖並重新傳送通知。 如需確切的錯誤碼，請參閱[推播通知回應碼](https://msdn.microsoft.com/library/windows/apps/hh465435)。

1.  雲端服務向通道 URI 發出 HTTP POST。 這項要求必須透過 SSL 傳送，並且要包含必要的標頭與通知承載。 授權標頭必須包含取得的存取權杖以進行授權。

    以下提供一個要求範例。 如需語法詳細資訊，請參閱[推播通知回應碼](https://msdn.microsoft.com/library/windows/apps/hh465435)。

    如需製作通知承載的詳細資訊，請參閱[快速入門：傳送推播通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)。 磚、快顯通知或徽章推播通知的承載是以 XML 內容來提供的，它們遵守各自已定義的[彈性磚結構描述](adaptive-tiles-schema.md)或[傳統磚結構描述](https://msdn.microsoft.com/library/windows/apps/br212853)。 原始通知的承載則沒有指定的結構。 它完全是由應用程式定義的。

    ``` http
     POST https://cloud.notify.windows.com/?token=AQE%bU%2fSjZOCvRjjpILow%3d%3d HTTP/1.1
     Content-Type: text/xml
     X-WNS-Type: wns/tile
     Authorization: Bearer EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=
     Host: cloud.notify.windows.com
     Content-Length: 24

     <body>
     ....
    ```

2.  WNS 會發出回應指出已經收到通知並在下一個機會出現時進行傳送。 不過，WNS 不會提供裝置或應用程式已經收到您通知的端對端確認。

以下圖表說明資料流程

![傳送通知的 WNS 圖表](images/wns-diagram-03.png)

### <a name="important-notes"></a>重要事項

-   WNS 不保證通知的可靠性或延遲。
-   在任何情況下通知都不應該包含機密或敏感資料。
-   若要傳送通知，雲端服務必須先向 WNS 驗證並接收存取權杖。
-   存取權杖只允許雲端服務傳送通知給建立權杖的單一應用程式。 一個存取權杖無法用於傳送通知給多個應用程式。 因此，如果您的雲端服務支援多個應用程式，在將通知推播至每個通道 URI 時，您必須提供應用程式的正確存取權杖。
-   當裝置離線時，WNS 預設將針對每個應用程式最多儲存五個磚通知 (已啟用佇列的情況，否則只儲存一個磚通知)，以及針對每個通道 URI 儲存一個徽章通知，而且不會有任何原始通知。 這個預設的快取行為可以透過 [X-WNS-Cache-Policy 標頭](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache)來變更。 請注意，當裝置離線時，絕不會儲存快顯通知。
-   在通知內容針對使用者個人化的案例，WNS 建議雲端服務在收到更新後立即傳送更新。 這個情況的範例包括社交媒體摘要更新、立即通訊邀請、新訊息通知或警示。 或者，您可能遇到將相同的一般性更新頻繁傳送給大量使用者子集的情況；例如，氣象、股票與新聞更新。 WNS 指導方針規定，這些更新的頻率應該最多每 30 分鐘一次。 如果例行更新的頻率更頻繁，使用者或 WNS 可能會判斷有濫用的行為。

## <a name="expiration-of-tile-and-badge-notifications"></a>磚和徽章通知的到期時間


根據預設，磚和徽章通知會在下載後的三天到期。 當通知到期的時候，會從磚或佇列中移除內容，不再對使用者顯示。 最佳作法是在所有的磚和徽章通知上設定到期時間 (使用一個對您的應用程式有意義的時間)，如此您的磚內容就不會超過內容的時效性。 明確的到期時間對於已定義存留時間的內容而言很重要。 如果您的雲端服務停止傳送通知或使用者從網路中斷連線一段期間時，這也可以確保會移除過時內容。

您的雲端服務可以藉由設定 X-WNS-TTL HTTP 標頭，指定在傳送通知之後該通知將維持有效狀態的時間 (秒)，以便設定每個通知的到期時間。 如需詳細資訊，請參閱[推播通知服務要求和回應標頭](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_ttl)。

例如，在股市交易日，您可以將股價更新到期時間設定為傳送間隔時間的兩倍 (例如，如果是每半小時傳送通知一次，則是接收後的一小時)。 另一個範例是新聞應用程式可能決定每日新聞磚更新的適當到期時間為一天。

## <a name="push-notifications-and-battery-saver"></a>推播通知和省電模式


省電模式會限制裝置上的背景活動，藉以延長電池使用時間。 Windows 10 可讓使用者設定省電模式，以便在電池電力低於指定的閾值時自動開啟。 開啟省電模式時，便會停用推播通知的接收，以節省能源。 但是有一些例外狀況。 下列 windows 10 省電模式設定 （[**設定**] app 中找到） 可讓您的應用程式甚至省電模式開啟時接收推播通知。

-   **允許在省電模式中接收來自任何應用程式的推播通知**：此設定可讓所有應用程式在省電模式開啟時接收推播通知。 請注意，此設定僅適用於 windows 10 桌面版本 （家用版、 專業版、 企業版和教育版）。
-   **一律允許**：此設定可讓特定應用程式在省電模式開啟時，於背景執行，包括接收推播通知。 此清單是由使用者手動維護。

沒有任何方式檢查這兩個設定的狀態，無法您可以檢查省電模式的狀態。 在 windows 10，使用[**EnergySaverStatus**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus)屬性檢查省電模式狀態。 您的應用程式也可以使用 [**EnergySaverStatusChanged**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged) 事件接聽省電模式的變更。

如果您的應用程式非常依賴推播通知，建議通知使用者，他們在省電模式開啟時可能不會收到通知，並讓他們可以輕鬆地調整**省電模式設定**。 在 windows 10，使用省電模式設定 URI 配置`ms-settings:batterysaver-settings`，您可以提供設定應用程式的便利連結。

**提示：** 時向使用者通知省電模式設定，建議您提供要在未來隱藏訊息的方式。 例如，以下範例中的 `dontAskMeAgainBox` 核取方塊會在 [**LocalSettings**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalSettings) 中保存使用者的喜好設定。

 

以下是如何檢查省電模式開啟 windows 10 中的範例。 此範例會通知使用者並啟動 [設定] 應用程式以進入**省電模式設定**。 如果使用者不想再收到通知，`dontAskAgainSetting` 可讓他們隱藏訊息。

```cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;
using Windows.System;
using Windows.System.Power;
...
...
async public void CheckForEnergySaving()
{
   //Get reminder preference from LocalSettings
   bool dontAskAgain;
   var localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
   object dontAskSetting = localSettings.Values["dontAskAgainSetting"];
   if (dontAskSetting == null)
   {  // Setting does not exist
      dontAskAgain = false;
   }
   else
   {  // Retrieve setting value
      dontAskAgain = Convert.ToBoolean(dontAskSetting);
   }
   
   // Check if battery saver is on and that it's okay to raise dialog
   if ((PowerManager.EnergySaverStatus == EnergySaverStatus.On)
         && (dontAskAgain == false))
   {
      // Check dialog results
      ContentDialogResult dialogResult = await saveEnergyDialog.ShowAsync();
      if (dialogResult == ContentDialogResult.Primary)
      {
         // Launch battery saver settings (settings are available only when a battery is present)
         await Launcher.LaunchUriAsync(new Uri("ms-settings:batterysaver-settings"));
      }

      // Save reminder preference
      if (dontAskAgainBox.IsChecked == true)
      {  // Don't raise dialog again
         localSettings.Values["dontAskAgainSetting"] = "true";
      }
   }
}
```

這是此範例之 [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) 的 XAML。

```xaml
<ContentDialog x:Name="saveEnergyDialog"
               PrimaryButtonText="Open battery saver settings"
               SecondaryButtonText="Ignore"
               Title="Battery saver is on."> 
   <StackPanel>
      <TextBlock TextWrapping="WrapWholeWords">
         <LineBreak/><Run>Battery saver is on and you may 
          not receive push notifications.</Run><LineBreak/>
         <LineBreak/><Run>You can choose to allow this app to work normally
         while in battery saver, including receiving push notifications.</Run>
         <LineBreak/>
      </TextBlock>
      <CheckBox x:Name="dontAskAgainBox" Content="OK, got it."/>
   </StackPanel>
</ContentDialog>
```

## <a name="related-topics"></a>相關主題


* [傳送本機磚通知](sending-a-local-tile-notification.md)
* [快速入門：傳送推播通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [如何透過推播通知更新徽章](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [如何要求、建立以及儲存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [如何攔截執行應用程式的通知](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [如何使用 Windows 推播通知服務 (WNS) 進行驗證](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [推播通知服務要求和回應標頭](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [推播通知的指導方針和檢查清單](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [原始通知](https://msdn.microsoft.com/library/windows/apps/hh761488)
 

 




