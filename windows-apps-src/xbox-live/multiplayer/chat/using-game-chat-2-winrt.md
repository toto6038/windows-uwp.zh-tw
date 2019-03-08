---
title: 使用遊戲的聊天 2 WinRT 投影
description: 了解如何使用 WinRT 投影中的 Xbox Live 遊戲聊天 2，將您的遊戲中加入語音通訊。
ms.date: 04/11/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 遊戲聊天 2、 遊戲的對談、 語音通訊
ms.localizationpriority: medium
ms.openlocfilehash: 1cb0578151d4262d61f5fbc078bebab721fb3bfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639163"
---
# <a name="using-game-chat-2-winrt-projections"></a>使用遊戲聊天 2 （WinRT 投影）

這是簡短的逐步解說中，使用遊戲聊天 2 的C#API。 想要存取遊戲聊天 2 透過 c + + 的遊戲開發人員應該會看到[聊天 2 使用遊戲](using-game-chat-2.md)。

## <a name="prerequisites-a-nameprereq"></a>必要條件 <a name="prereq">

若要取用遊戲聊天 2，您必須包含[Microsoft.Xbox.Services.GameChat2 nuget 套件](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/)。

開始使用遊戲聊天 2 撰寫程式碼之前，您必須設定您的應用程式 AppXManifest 宣告 「 麥克風 」 裝置功能。 AppXManifest 功能詳述於個別的平台文件中; 區段中的更多詳細資料下列程式碼片段會顯示 「 麥克風 」 裝置功能節點應該存在於 [封裝/功能] 節點下，否則會封鎖對談：

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="initialization-a-nameinit"></a>初始化 <a name="init">

開始使用程式庫互動以具現化`GameChat2ChatManager`物件的並行聊天使用者數目上限必須是可以加入至執行個體。 若要變更此值，`GameChat2ChatManager`必須處置物件，並重新建立所需的值。 您可能只有一個`GameChat2ChatManager`具現化一次。

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

`GameChat2ChatManager`物件有可能在任何時間設定的其他 （選擇性） 屬性。 下列程式碼範例假設，變數會提供值，再使用它們來設定屬性`myGameChat2ChatManager`物件。

```cs
GameChat2SpeechToTextConversionMode mySpeechToTextConversionMode;
GameChat2SharedDeviceCommunicationRelationshipResolutionMode mySharedDeviceResolutionMode;
GameChat2CommunicationRelationship myDefaultLocalToRemoteCommunicationRelationship;
float myDefaultAudioRenderVolume;
GameChat2AudioEncodingTypeAndBitrate myAudioEncodingTypeAndBitRate;

...

myGameChat2ChatManager.SpeechToTextConversionMode = mySpeechToTextConversionMode;
myGameChat2ChatManager.SharedDeviceResolutionMode = mySharedDeviceResolutionMode;
myGameChat2ChatManager.DefaultLocalToRemoteCommunicationRelationship = myDefaultLocalToRemoteCommunicationRelationship;
myGameChat2ChatManager.DefaultAudioRenderVolume = myDefaultAudioRenderVolume;
myGameChat2ChatManager.AudioEncodingTypeAndBitRate = myAudioEncodingTypeAndBitRate;
```

## <a name="configuring-users-a-nameconfig"></a>設定使用者 <a name="config">

一旦初始化執行個體時，必須以 GC2 執行個體加入本機使用者。 在此範例中，使用者 A 將代表的本機使用者。

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

然後您必須加入遠端使用者和識別碼，將用來代表遠端的 [端點] 上的每個遠端使用者。 [端點] 由應用程式所擁有的物件，實作`IGameChat2Endpoint`介面。 下列範例示範的範例實作`MyEndpoint`可實作類別`IGameChat2Endpoint`。

```cs
class MyEndpoint : IGameChat2Endpoint
{
    private uint endpointIdentifier;
    public MyEndpoint(uint identifier)
    {
        endpointIdentifier = identifier;
    }

    // Implementing IsSameEndpointAs, the only method on the IGameChat2Endpoint interface
    public bool IsSameEndpointAs(IGameChat2Endpoint gameChat2Endpoint)
    {
        return endpointIdentifier == ((MyEndpoint)gameChat2Endpoint).endpointIdentifier;
    }
}
```

在下列程式碼範例中，使用者 B、 C 和 D 都是要加入的遠端使用者。 是一個遠端裝置上的使用者 B 和 C 和 D 的使用者位於遠端的另一個裝置。 這個範例假設將設定的變數，`myGameChat2ChatManager`的執行個體`GameChat2ChatManager`和 「 MyEndpoint 」 是一個類別，實作`IGameChat2Endpoint`。

```cs
string userBXuid;
string userCXuid;
string userDXuid;
MyEndpoint myRemoteEndpointOne;
MyEndpoint myRemoteEndpointTwo;

...

IGameChat2ChatUser remoteUserB = myGameChat2ChatManager.AddRemoteUser(userBXuid, myRemoteEndpointOne);
IGameChat2ChatUser remoteUserC = myGameChat2ChatManager.AddRemoteUser(userCXuid, myRemoteEndpointTwo);
IGameChat2ChatUser remoteUserD = myGameChat2ChatManager.AddRemoteUser(userDXuid, myRemoteEndpointTwo);
```

現在您可以設定每個遠端使用者與本機使用者之間的通訊關聯性。 在此範例中，假設使用者 A 和 B 使用者位於相同的小組，並允許雙向通訊。 `GameChat2CommunicationRelationship.SendAndReceiveAll` 被定義來代表雙向通訊。 使用者 A 的關聯性設為使用者 B，其中：

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

現在假設使用者 C 和 D 會 'spectators'，而且應該可以接聽使用者的但未說明。 `GameChat2CommunicationRelationship.ReceiveAll` 會定義此單向通訊。 設定與關聯性：

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

在任何時間點是否已新增至的遠端使用者`GameChat2ChatManager`執行個體，但是您尚未與任何本機使用者-都沒問題的通訊已設定 ！ 這可能會在使用者決定小組，或可以任意變更讀出通道的情況下發生。 遊戲的對談 2 只會快取已新增執行個體，就很有用，通知遊戲聊天 2 的所有可能的使用者，即使它們不能說出任何特定時間點的本機使用者時間的使用者資訊 （例如隱私權的關聯性和評價）。

最後，假設使用者 D 離開遊戲，且應從本機的遊戲的對談 2 執行個體中移除。 可以使用下列呼叫來完成：

```cs
remoteUserD.Remove();
```

使用者物件會立即失效時`Remove()`呼叫。 移除時，會快取使用者的最後一個狀態。 移除前，任何使用者物件失效之後呼叫的告知性方法將會反映使用者的狀態。 其他方法會擲回錯誤時呼叫。

## <a name="processing-data-frames-a-namedata"></a>處理資料框架 <a name="data">

遊戲的對談 2 並沒有自己的傳輸層級;這必須由應用程式提供。 此外掛程式由應用程式的規則，經常呼叫`GameChat2ChatManager.GetDataFrames()`。 這個方法是遊戲聊天 2 提供應用程式的輸出資料的方式。 它被設計來快速地操作，使它可以輪詢專用的網路執行緒中的常見問題。 這提供方便的地方，而不需擔心網路計時或複雜的多執行緒的回呼的不可預測性擷取所有已排入佇列的資料。

當`GameChat2ChatManager.GetDataFrames()`呼叫時，所有已排入佇列的資料會回報一份`IGameChat2DataFrame`物件。 應用程式應逐一查看清單中，檢查`TargetEndpointIdentifiers`，並使用應用程式的網路層的資料傳送至適當的遠端應用程式執行個體。 在此範例中，`HandleOutgoingDataFrame`是在傳送資料的函式`Buffer`中指定每個 「 端點 」 上的每個使用者`TargetEndpointIdentifiers`根據`TransportRequirement`。

```cs
IReadOnlyList<IGameChat2DataFrame> frames = myGameChat2ChatManager.GetDataFrames();
foreach (IGameChat2DataFrame dataFrame in frames)
{
    HandleOutgoingDataFrame(
        dataFrame.Buffer,
        dataFrame.TargetEndpointIdentifiers,
        dataFrame.TransportRequirement
        );
}
```

資料框架處理的次數越頻繁，音訊終端使用者可明顯的延遲會越低。 音訊被聯合成 40 毫秒資料框架;這是建議的輪詢間隔。

## <a name="processing-state-changes-a-namestate"></a>處理狀態變更 <a name="state">

遊戲的對談 2 提供應用程式，例如接收的文字訊息，透過應用程式的一般、 經常呼叫來更新`GameChat2ChatManager.GetStateChanges()`方法。 它設計成能迅速操作，您可以在您的 UI 轉譯迴圈中呼叫每個圖形畫面格。 這提供方便的地方，而不需擔心網路計時或複雜的多執行緒的回呼的不可預測性擷取所有佇列中的變更。

當`GameChat2ChatManager.GetStateChanges()`是呼叫，所有已排入佇列的更新會報告一份`IGameChat2StateChange`物件。 應用程式應該：
1. 逐一查看清單
2. 檢查其更特定類型的基底結構
3. 轉型的基礎結構至對應的詳細型別
4. 處理視需要更新。 

```cs
IReadOnlyList<IGameChat2StateChange> stateChanges = myGameChat2ChatManager.GetStateChanges();
foreach (IGameChat2StateChange stateChange in stateChanges)
{
    switch (stateChange.Type)
    {
        case GameChat2StateChangeType.CommunicationRelationshipAdjusterChanged:
        {
            MyHandleCommunicationRelationshipAdjusterChanged(stateChange as GameChat2CommunicationRelationshipAdjusterChangedStateChange);
            break;
        }
        case GameChat2StateChangeType.TextChatReceived:
        {
            MyHandleTextChatReceived(stateChange as GameChat2TextChatReceivedStateChange);
            break;
        }

        ...
    }
}
```

## <a name="text-chat-a-nametext"></a>文字對談 <a name="text">

若要傳送的文字對談，請使用`GameChat2ChatUserLocal.SendChatText()`。 例如：

```cs
localUserA.SendChatText("Hello");
```

遊戲的對談 2 會產生資料框架，其中包含此訊息;資料框架的目標端點將會是已設定為接收來自本機使用者的文字的使用者相關聯。 遠端端點所處理的資料時，會透過公開訊息`GameChat2TextChatReceivedStateChange`。 如同語音聊天權限及隱私權限制會遵守文字聊天。 如果使用者的一組已設定為允許文字聊天，但權限或隱私權限制不允許該通訊，文字訊息皆會予以捨棄。

支援文字聊天輸入和顯示是為了協助工具 (請參閱[協助工具](#access)如需詳細資訊)。

## <a name="accessibility-a-nameaccess"></a>協助工具 <a name="access">

需要支援輸入的文字對談和顯示。 需要文字輸入，因為即使在平台或遊戲的內容類型，在過去一直沒有普遍使用的實體鍵盤上，使用者可以設定要使用文字轉換語音的輔助技術的系統。 同樣地，文字顯示需要，因為使用者可以設定要使用語音轉換文字的系統。 這些喜好設定偵測不到本機使用者藉由檢查`GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled`和`GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled`屬性分別，而且您可能想要有條件地啟用文字機制。

### <a name="text-to-speech"></a>文字轉換語音

當使用者擁有已啟用，文字轉換語音`GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled`將會是 'true'。 偵測到此狀態時，應用程式必須提供文字輸入的方法。 一旦您擁有實際或虛擬鍵盤所提供的文字輸入，將字串傳遞至`GameChat2ChatUserLocal.SynthesizeTextToSpeech()`方法。 遊戲的對談 2 會偵測並合成根據字串和使用者的存取語音喜好設定的音訊資料。 例如：

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

合成的這項作業一部分的音訊會傳輸至已設定為接收音訊從這個本機使用者的所有使用者。 如果`GameChat2ChatUserLocal.SynthesizeTextToSpeech()`上並沒有文字轉換語音啟用的遊戲聊天 2 會不採取任何動作的使用者呼叫。

### <a name="speech-to-text"></a>語音轉換文字

當使用者語音轉換文字啟用，`GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled`將會是 true。 偵測到此狀態時，應用程式必須準備好提供與轉譯的聊天訊息相關聯的 UI。 GC 會自動轉譯每個遠端使用者的音訊，並將它透過公開`GameChat2TranscribedChatReceivedStateChange`。

### <a name="speech-to-text-performance-considerations"></a>語音轉換文字效能考量

啟用語音轉換文字時，每個遠端裝置上的遊戲聊天 2 執行個體就會起始與語音服務端點的 web 通訊端連線。 每個遠端的遊戲聊天 2 用戶端會將上傳音訊語音服務端點，透過此 websocket;語音服務端點偶而會傳回文字記錄訊息到遠端裝置。 遠端裝置接著傳送文字記錄訊息 （亦即文字訊息） 到本機裝置，其中將轉譯的訊息傳送遊戲聊天 2 應用程式來呈現。

因此，語音轉換文字的主要效能成本會是網路使用量。 大部分的網路流量是音訊的編碼的上傳。 Websocket 上傳已經完成編碼遊戲聊天 2 「 正常 」 的語音聊天路徑; 中的音訊應用程式可以透過位元速率控制`GameChat2ChatManager.AudioEncodingTypeAndBitrate`。

## <a name="ui-a-nameui"></a>UI <a name="UI">

建議是，任何位置播放程式會顯示，特別是在一份玩家代號，例如計分板中，您也顯示為使用者的意見反應的靜音/說話的圖示。 `IGameChat2ChatUser.ChatIndicator`屬性表示該播放程式的對談的目前、 即時狀態。 下列範例示範如何擷取的指標值`IGameChat2ChatUser`物件指向的變數 'userA'，若要判斷特定圖示常值，指派給 'iconToShow' 變數：

```cs
switch (userA.ChatIndicator)
{
    case GameChat2UserChatIndicator.Silent:
    {
        iconToShow = Icon_InactiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.Talking:
    {
        iconToShow = Icon_ActiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.LocalMicrophoneMuted:
    {
        iconToShow = Icon_MutedSpeaker;
        break;
    }
    
    ...
}
```

值`IGameChat2ChatUser.ChatIndicator`預期會經常變更播放程式開始和停止談話，例如。 它被設計來支援輪詢每個 UI 畫面格結果的應用程式。

## <a name="muting-a-namemute"></a>靜音 <a name="mute">

`GameChat2ChatUserLocal.MicrophoneMuted`屬性可用來切換的本機使用者的麥克風靜音的狀態。 當麥克風靜音時，將會不擷取任何從該麥克風的音訊。 如果使用者是在共用的裝置，例如 Kinect 靜音的狀態就會套用到所有使用者。 這個方法不會反映硬體靜音控制項 （例如按鈕上使用者的耳機）。 沒有任何方法來擷取使用者的音訊裝置透過遊戲聊天 2 的硬體靜音狀態。

`GameChat2ChatUserLocal.SetRemoteUserMuted()`方法可用來切換靜音狀態相對於特定的本機使用者的遠端使用者。 當遠端使用者設為靜音時，本機使用者不會聽到任何聲音，或者收到來自遠端使用者的文字訊息。

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>不正確的信譽自動靜音 <a name="automute">

通常是遠端使用者將會開始 unmuted。 遊戲的對談 2 將啟動使用者靜音的狀態 （1） 的遠端使用者不是本機使用者的朋友，以及 （2） 遠端使用者的信譽不佳旗標。 當使用者這項作業，因為靜音`IGameChat2ChatUser.ChatIndicator`會傳回`GameChat2UserChatIndicator.ReputationRestricted`。 此狀態將會覆寫的第一個呼叫`GameChat2ChatUserLocal.SetRemoteUserMuted()`，其中包含目標使用者的遠端使用者。

## <a name="privilege-and-privacy-a-namepriv"></a>權限及隱私權 <a name="priv">

之上通訊關聯性設定的遊戲，遊戲的對談 2 會強制執行權限及隱私權限制。 當使用者第一次新增; 遊戲的對談 2 會執行特殊權限及隱私權限制查閱使用者的`IGameChat2ChatUser.ChatIndicator`一律會傳回`GameChat2UserChatIndicator.Silent`直到這項作業完成為止。 如果使用者使用的通訊會受到保護隱私權或權限限制，使用者的`IGameChat2ChatUser.ChatIndicator`會傳回`GameChat2UserChatIndicator.PlatformRestricted`。 平台的通訊限制適用於語音和文字的聊天;絕對不會執行個體，其中文字聊天封鎖平台限制，但語音聊天不是，反之亦然。

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()` 可用來協助您辨別使用者無法進行通訊，因為不完整的權限及隱私權作業時。 它會傳回強制執行遊戲聊天 2 的表單中的通訊關聯性`GameChat2CommunicationRelationship`原因關聯性可能不等於的形式設定的關聯性`GameChat2CommunicationRelationshipAdjuster`。 例如，如果查閱作業仍在進行中`GameChat2CommunicationRelationshipAdjuster`會`GameChat2CommunicationRelationshipAdjuster.Initializing`。 此方法應該用於開發和偵錯的情況;不應該用來影響 UI (請參閱[UI](#UI))。

## <a name="cleanup-a-namecleanup"></a>清除 <a name="cleanup">

當應用程式不再需要透過遊戲聊天 2 的通訊時，您應該呼叫`GameChat2ChatManager.Dispose()`。 這可讓遊戲聊天 2 回收所管理的連絡事宜配置的資源。

## <a name="how-to-configure-popular-scenarios"></a>如何設定受歡迎的案例

### <a name="push-to-talk"></a>推播至演講

應該使用實作推播至 talk `GameChat2ChatUserLocal.MicrophoneMuted`。 設定`MicrophoneMuted`為 false，可允許語音和`MicrophoneMuted`為 true，將其限制。 這個屬性變更將會提供遊戲聊天 2 的最低延遲回應。

### <a name="teams"></a>小組

假設使用者 A 和使用者 B 位於 Team 藍色，而 C 使用者和使用者 D 位於 Team Red。 每位使用者是在應用程式的唯一執行個體。

在使用者 A 的裝置：

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

使用者 B 在裝置上：

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

在 使用者 C 的裝置：

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

使用者 D 在裝置上：

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>廣播

假設使用者 A 是讓訂單的領導者，而只能接聽使用者 B、 C 和 D。 每個播放程式是唯一的裝置上。

在使用者 A 的裝置：

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

使用者 B 在裝置上：

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

在 使用者 C 的裝置：

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

使用者 D 在裝置上：

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```
