---
author: PatrickFarley
title: 透過遠端工作階段連接裝置
description: 將裝置加入遠端工作階段，以建立跨多部裝置的共用體驗。
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.author: pafarley
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，連接裝置、 遠端系統、 rome 的 project rome
ms.localizationpriority: medium
ms.openlocfilehash: 8e5226b23a454bf48add22d590a3ff247c629e4f
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "4021392"
---
# <a name="connect-devices-through-remote-sessions"></a>透過遠端工作階段連接裝置

「遠端工作階段」功能可讓應用程式透過工作階段連接到其他裝置，以進行明確的應用程式傳訊，或進行系統管理資料代理交換，例如在 Windows 全像攝影版裝置之間用於全像攝影共用的 **[SpatialEntityStore](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialentitystore)**。

任何 Windows 裝置都可以建立遠端工作階段，而且任何 Windows 裝置都可以要求加入其中 (雖然工作階段可能只在受邀之後才看得到)，包括其他使用者登入的裝置。 本指南針對所有會使用遠端工作階段的主要案例來提供基本範例程式碼。 您可以將此程式碼納入現有的應用程式專案，並視需要進行修改。 如需端對端實作，請參閱[測驗遊戲範例應用程式](https://github.com/microsoft/Windows-appsample-remote-system-sessions))。

## <a name="preliminary-setup"></a>初步設定

### <a name="add-the-remotesystem-capability"></a>新增 remoteSystem 功能

為了讓您的應用程式能夠啟動遠端裝置上的應用程式，您必須將 `remoteSystem` 功能新增至應用程式套件資訊清單。 您可以使用套件資訊清單設計工具，在 \[功能\]**** 索引標籤上選取 \[遠端系統\]**** 來新增此功能，或手動將下列程式碼行新增至專案的 _Package.appxmanifest_ 檔案。

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>在裝置上啟用跨使用者探索
遠端工作階段是專為連接多個不同的使用者而設計，因此相關的裝置都必須啟用 [跨使用者共用]。 這是可以使用 **RemoteSystem** 類別的靜態方法查詢的系統設定：

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

若要變更此設定，使用者必須開啟 **\[設定\]** App。 在 **\[系統\]** > **\[共用體驗\]** > **\[在裝置間共用\]** 功能表中有下拉式方塊，使用者可以在那裡指定其系統可以與哪些裝置共用。

![共用體驗設定頁面](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>包含必要的命名空間
為了使用本指南中所有的程式碼片段，您的類別檔案需要下列 `using` 陳述式。

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>建立遠端工作階段

若要建立遠端工作階段執行個體，您必須從 **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 物件開始著手。 使用下列架構來建立新的工作階段，以及處理其他裝置的加入要求。

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>將遠端工作階段設定為僅限透過邀請加入

如果您想要讓遠端工作階段無法公開供探索，您可以將其設定為僅限透過邀請加入。 只有收到邀請的裝置才可以傳送加入要求。 

程序大多與上述相同，但建構 **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** 執行個體時，您要傳入已設定的 **[RemoteSystemSessionOptions](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** 物件。

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

若要傳送邀請，您必須有接收端遠端系統的參考 (透過一般遠端系統探索取得)。 只需將此參考傳入工作階段物件的 **[SendInvitationAsync](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** 方法即可。 工作階段中所有的參與者都有遠端工作階段的參考 (請參閱下一節)，因此任何參與者皆可傳送邀請。

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>探索並加入遠端工作階段

探索遠端工作階段的程序是由 **[RemoteSystemSessionWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** 類別處理，類似於探索個人遠端系統。

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

取得 **[RemoteSystemSessionInfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** 執行個體時，可以用來向控制對應工作階段的裝置發出加入要求裝置。 接受的加入要求將以非同步方式傳回包含所加入工作階段之參考的 **[RemoteSystemSessionJoinResult](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** 物件。

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

可以同時將裝置加入多個工作階段。 因此，最好是將加入功能從實際與每個工作階段的互動脫離。 只要 **[RemoteSystemSession](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession)** 執行個體的參考保留在應用程式中，就可以透過該工作階段嘗試進行通訊。

## <a name="share-messages-and-data-through-a-remote-session"></a>透過遠端工作階段分享訊息和資料

### <a name="receive-messages"></a>接收訊息

您可以使用 **[RemoteSystemSessionMessageChannel](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** 執行個體 (代表單一全工作階段範圍的通訊管道) 來和工作階段中的其他參與者裝置交換訊息及資料。 只要已初始化，它就會開始接聽訊息。

>[!NOTE]
>傳送和接收訊息時，必須將訊息從位元組陣列序列化並還原序列化。 這項功能已包含下列範例中，但為了達到更好的程式碼模組化，您可以個別實作該功能。 如需這項功能的範例，請參閱[範例應用程式](https://github.com/microsoft/Windows-appsample-remote-system-sessions))。

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>傳送訊息

建立管道後，將訊息傳送給所有的工作階段參與者就很簡單。

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

若要傳送訊息只限給特定參與者，您必須先起始探索程序，才能取得工作階段中遠端系統參與者的參考。 這與探索工作階段以外遠端系統的程序相似。 使用 **[RemoteSystemSessionParticipantWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** 執行個體來尋找工作階段的參與者裝置。

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

取得工作階段參與者的參考清單時，您可以傳送訊息給任何一組參與者。

若要傳送訊息給單一參與者 (最好由使用者在畫面上選取)，只需將參考傳入方法中，如下所示。

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

若要傳送訊息給多個參與者 (最好由使用者在畫面上選取)，請將這些參與者新增至清單物件，然後傳入方法中，如下所示。

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>相關主題
* [已連線的應用程式與裝置 (Project Rome)](connected-apps-and-devices.md)
* [遠端系統 API 參考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
