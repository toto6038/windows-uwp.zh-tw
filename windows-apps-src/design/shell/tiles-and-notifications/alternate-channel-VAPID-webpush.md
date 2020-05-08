---
title: 在 UWP 中使用 VAPID 的替代推播通道
description: 從 Windows 應用程式使用替代推播通道與 VAPID 通訊協定的指示
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10，uwp，WinRT API，WNS
localizationpriority: medium
ms.openlocfilehash: 382dca376e2393d83c2803043b61db76226b3995
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970863"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>在 Windows 中使用 VAPID 的替代推播通道 
從秋季建立者更新開始，Windows 應用程式可以使用 VAPID authentication 來傳送推播通知。  

> [!NOTE]
> 這些 Api 適用于裝載其他網站的網頁瀏覽器，並代表他們建立通道。  如果您想要將 webpush 通知新增至您的 web 應用程式，建議您遵循 W3C 和 WhatWG 標準來建立服務工作者並傳送通知。

## <a name="introduction"></a>簡介
引進 web 推送標準可讓網站更像應用程式般作用，即使使用者不在網站上也可以傳送通知。

已建立 VAPID authentication 通訊協定，以允許網站以廠商中立的方式向推播伺服器進行驗證。 透過使用 VAPID 通訊協定的所有廠商，網站可以傳送推播通知，而不需要知道它正在執行的瀏覽器。 這是針對每個平臺執行不同推送通訊協定的重大改善。 

Windows 應用程式也可以使用 VAPID 來傳送具有這些優點的推播通知。 這些通訊協定可以節省新應用程式的開發時間，並簡化現有應用程式的跨平臺支援。 此外，企業應用程式或側載應用程式現在可以傳送通知，而不需要在 Microsoft Store 中註冊。 希望這會開啟新的方式來與所有平臺上的使用者互動。  

## <a name="alternate-channels"></a>替代通道 
在 UWP 中，這些 VAPI資料通道稱為替代通道，並提供類似的功能給 web 推播通道。 他們可以觸發應用程式背景工作來執行、啟用訊息加密，並允許來自單一應用程式的多個通道。 如需不同通道類型之間差異的詳細資訊，請參閱[選擇正確的頻道](channel-types.md)。

如果您的應用程式無法使用主要通道，或如果您想要在您的網站與應用程式之間共用程式碼，則使用替代通道是存取推播通知的絕佳方式。 在之前使用 web 推送標準或使用 Windows 推播通知的任何人，都可以輕鬆且熟悉設定通道。

## <a name="code-example"></a>程式碼範例

為 Windows 應用程式設定替代通道的基本程式，與設定主要或次要通道的步驟類似。 首先，向[WNS 伺服器](windows-push-notification-services--wns--overview.md)註冊通道。 然後，註冊以背景工作的方式執行。 傳送通知並觸發背景工作之後，請處理事件。  

### <a name="get-a-channel"></a>取得通道 
若要建立替代通道，應用程式必須提供兩項資訊：其伺服器的公開金鑰，以及它所建立之通道的名稱。 伺服器金鑰的詳細資料可在 web push 規格中取得，但我們建議您在伺服器上使用標準的 web 推送程式庫來產生金鑰。  

通道識別碼特別重要，因為應用程式可以建立多個替代通道。 每個通道都必須以唯一識別碼來識別，該 ID 將包含在該通道傳送的任何通知承載中。  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
應用程式會將通道備份傳送到其伺服器，並將它儲存在本機。 將通道識別碼儲存在本機，可以讓應用程式區分通道和更新通道，以避免其過期。

如同每個其他類型的推播通知通道，web 通道也會過期。 若要在您的應用程式不知道的情況下防止通道過期，請在每次啟動應用程式時建立新的通道。    

### <a name="register-for-a-background-task"></a>註冊背景工作 

當您的應用程式建立了替代通道之後，它應該註冊以在前景或背景中接收通知。 下列範例會註冊，以使用單一進程模型在背景中接收通知。  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>接收通知 

一旦應用程式註冊接收通知，就必須能夠處理傳入通知。 由於單一應用程式可以註冊多個通道，因此請務必先檢查通道識別碼，然後再處理通知。  

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

請注意，如果通知來自主要通道，則不會設定通道識別碼。  

## <a name="note-on-encryption"></a>加密時請注意 

您可以使用您發現對應用程式更有用的任何加密配置。 在某些情況下，依賴伺服器和任何 Windows 裝置之間的 TLS 標準就已足夠。 在其他情況下，使用 web 推送加密配置或設計的另一個配置可能更有意義。  

如果您想要使用另一種形式的加密，則金鑰是使用原始的。標頭屬性。 它包含對推播伺服器 POST 要求中所包含的所有加密標頭。 您的應用程式可以從該處使用金鑰來解密訊息。  

## <a name="related-topics"></a>相關主題
- [通知通道類型](channel-types.md)
- [Windows 推播通知服務 (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel 類別](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager 類別](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


