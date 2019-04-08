---
title: 在 UWP 中使用 Webpush 和 VAPID 替代的推播通道
description: 使用替代的推播通道 VAPID 的通訊協定，從 UWP 應用程式的說明
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10 uwp、 WinRT API WNS
localizationpriority: medium
ms.openlocfilehash: ba8630a2e877345adeac7eb443dd3e418d3ed277
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639523"
---
# <a name="alternate-push-channels-using-webpush-and-vapid-in-uwp"></a>在 UWP 中使用 Webpush 和 VAPID 替代的推播通道 
從開始 Fall Creators Update，UWP 應用程式可以使用 web 推播 VAPID 驗證傳送推播通知。  

## <a name="introduction"></a>簡介
Web 推播標準的引進可讓網站可以像是更多應用程式，即使使用者不在網站傳送通知。

以允許向供應商在推播伺服器的網站建立 VAPID 的驗證通訊協定無關的方式。 與使用 VAPID 的通訊協定的所有廠商，網站可以傳送推播通知，而不需要知道它執行所在的瀏覽器。 這是重大的改進實作每個平台不同的推播通訊協定。 

UWP 應用程式可以使用 webpush 和 VAPID 傳送這些優點，以及使用推播通知。 這些通訊協定可以儲存為新的應用程式的開發時間，並簡化現有應用程式的跨平台支援。 此外，企業應用程式或側載應用程式現在可以傳送通知而不需註冊 Microsoft Store 中。 希望這會開啟新的方式，跨所有平台與使用者互動。  

## <a name="alternate-channels"></a>替代通道 
在 UWP，這些 VAPID 通道稱為替代的通道，並提供類似的功能給 web 推播通道。 它們可以觸發應用程式的背景工作執行，請啟用訊息加密，並允許從單一應用程式的多個通道。 如需有關不同的通道型別之間的差異的詳細資訊，請參閱[選擇正確的通道](channel-types.md)。

使用替代的通道是存取推播通知，如果您的應用程式無法使用主要的通道，或如果您想要您的網站和應用程式之間共用程式碼的好方法。 設定通道是方便又熟悉的任何人已使用 web 標準的推播或從事 Windows 之前的推播通知。

## <a name="code-example"></a>程式碼範例

對 UWP 應用程式的替代通道所設定的基本程序是類似於設定的主要或次要通道。 首先，註冊與通道[WNS server](windows-push-notification-services--wns--overview.md)。 接著，註冊執行為背景工作。 傳送通知，並觸發背景工作之後，處理事件。  

### <a name="get-a-channel"></a>取得通道 
若要建立其他通道，應用程式必須提供兩項資訊： 其伺服器和它建立的通道名稱的公開金鑰。 伺服器金鑰的相關詳細資料可用於 web 的推播規格，但建議使用標準 web 推播程式庫伺服器上，以產生金鑰。  

頻道識別碼是特別重要，因為應用程式可以建立多個替代的通道。 每個通道必須識別將會使用該通道傳送任何通知承載包含唯一識別碼。  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
應用程式會傳送回到其伺服器通道，並將它儲存在本機。 儲存在本機的通道識別碼可讓應用程式，以區別通道模組和更新通道，以防止其到期。

每個其他類型的推播通知通道，例如 web 通道將會過期。 每次啟動您的應用程式時，以避免過期而不需要您的應用程式知道通道，建立新的通道。    

### <a name="register-for-a-background-task"></a>註冊以進行背景工作 

一旦您的應用程式已建立替代的通道，它應該註冊要接收通知，在前景或背景。 下列範例會使用一個處理序模型以在背景中接收通知註冊。  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>接收通知 

一旦應用程式已註冊要接收通知，它必須能夠處理內送通知。 因為單一應用程式都可以註冊多個通道，請務必檢查之前處理通知的通道識別碼。  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

請注意，是否通知來自主要通道，然後通道識別碼將不會設定。  

## <a name="note-on-encryption"></a>請注意加密 

您可以使用任何加密配置更有用的應用程式。 在某些情況下，它就足以依賴伺服器和任何 Windows 裝置之間的 TLS 標準。 在其他情況下，它可能會使用於 web 的推播加密配置或另一種配置設計。  

如果您想要使用其他加密形式，關鍵在於將未經處理。標頭屬性。 它包含所有已納入推播伺服器的 POST 要求的加密標頭。 從該處，您的應用程式可以使用的金鑰來解密訊息。  

## <a name="related-topics"></a>相關主題
- [通知的通道型別](channel-types.md)
- [Windows 推播通知服務 (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel 類別](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager 類別](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


