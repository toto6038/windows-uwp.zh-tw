---
title: 多人遊戲的使用說明
description: 描述如何在 Xbox Live 的多人遊戲 2015年中實作常見的工作。
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.date: 08/29/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，多人遊戲 2015
ms.localizationpriority: medium
ms.openlocfilehash: 20bf3486491e8173ce946a3a5dc2e4bc83305c83
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648633"
---
# <a name="multiplayer-how-tos"></a>多人遊戲操作說明

本主題包含有關如何實作與使用多人遊戲 2015年相關的特定工作的資訊。

* MPSD 工作階段變更通知訂閱
* 建立 MPSD 工作階段
* MPSD 工作階段設定仲裁程式
* 管理標題啟用
* 讓使用者可聯結
* 傳送遊戲邀請
* 加入遊戲工作階段從大廳工作階段
* 加入從標題啟用 MPSD 工作階段
* 設定使用者的目前活動
* 更新 MPSD 工作階段
* 讓 MPSD 工作階段
* 填滿期間配對的開啟工作階段的位置
* 建立比對票證
* 取得符合票證狀態

## <a name="subscribe-for-mpsd-session-change-notifications"></a>MPSD 工作階段變更通知訂閱

| 注意                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 訂閱的工作階段的變更要求相關聯的播放程式設為作用中工作階段。 ConnectionRequiredForActiveMembers 欄位也必須設定為工作階段的 /constants/system/capabilities 物件中，則為 true。 這個欄位通常是設定工作階段範本中。 請參閱[MPSD 工作階段範本](multiplayer-session-directory.md)。 |



若要接收 MPSD 工作階段變更通知，標題可以依照下列程序。

1.  使用相同**XboxLiveContext 類別**由同一位使用者的所有呼叫的物件。 訂用帳戶會繫結，此物件的存留期。 如果有多個本機使用者，使用個別**XboxLiveContext**每位使用者的物件。
2.  實作事件處理常式**RealTimeActivityService.MultiplayerSessionChanged 事件**並**RealTimeActivityService.MultiplayerSubscriptionsLost 事件**。
3.  如果訂閱一個以上的使用者的變更，將程式碼加入您**MultiplayerSessionChanged**事件處理常式，以避免不必要的工作。 使用**RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 屬性**並**RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 屬性**。 使用這些屬性可看到的上次變更的追蹤和忽略的較舊的變更。
4.  呼叫**MultiplayerService.EnableMultiplayerSubscriptions 方法**允許訂用帳戶。
5.  建立本機工作階段物件和聯結為作用中的該工作階段。
6.  針對每位使用者在進行呼叫**MultiplayerSession.SetSessionChangeSubscription 方法**，傳遞工作階段變更要通知的類型。
7.  立即寫入工作階段 MPSD 中所述**How to:更新多人連線的工作階段**。

下列流程圖說明如何著手 Multplayer 訂閱此程序中所述的事件。

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>剖析重複的工作階段變更通知

當有多個相同的工作階段的通知訂閱的使用者時，該工作階段的所有變更會都觸發 shoulder 點選每個使用者。 但其中一種會所有重複項目。 標題時仍建議標題，訂閱通知的工作階段中的每位使用者，應該忽略它已經被通知要; 的任何變更您可以使用分支和 ChangeNumber 屬性。

若要偵測多重 shoulder 點選，標題應該：

-   每個**RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 屬性**看過，最新的存放區的數值**RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 屬性**。
-   如果具有較高的潛在 tap **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 屬性**比上次看到的**RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 屬性**、 處理它，然後更新最新**RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 屬性**。
-   如果沒有更高 shoulder 點選**RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 屬性**該**RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 屬性**，略過它。 這項變更已經過處理。

| 注意                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 屬性**值必須追蹤所**RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 屬性**，而不使用工作階段。 可能**RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 屬性**若要變更的值 (和**RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 屬性**重設) 內的工作階段存留期。 |

## <a name="create-an-mpsd-session"></a>建立 MPSD 工作階段


| 注意                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 根據預設，第一個成員加入它時，會建立 MPSD 工作階段。 如果您的標題邏輯預期存在或不存在於聯結階段標題，它可以傳遞給寫入方法的適當的寫入模式的值在工作階段更新期間。 |



標題必須執行下列命令來建立新的工作階段：

1.  建立新**XboxLiveContext 類別**物件。 您的標題會一次建立此物件、 加以儲存，和重複使用它，視需要在原始程式碼。 尤其是當使用工作階段的訂用帳戶，就必須使用完全相同的內容。
2.  建立新**MultiplayerSession 類別**準備 MPSD 建立新的工作階段時所需的所有工作階段資料的物件。
3.  工作階段寫入 MPSD 之前進行必要的變更。 例如，若要呼叫的工作階段中加入成員時**MultiplayerSession.Join 方法**，用戶端將會告訴 MPSD 呼叫來更新工作階段時加入的隱藏的本機要求資料。
4.  完成本機變更，請將它們寫入至 MPSD 中所述**How to:更新多人連線的工作階段**。
5.  接收新**MultiplayerSession** MPSD，有許多欄位填入的物件。
6.  使用從現在開始，新的工作階段物件，並捨棄舊的複本，其中包含隱藏的要求，以建立新的工作階段。

### <a name="example"></a>範例

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Partner Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>MPSD 工作階段設定仲裁程式




標題會使用下列程序，已建立的工作階段設定仲裁程式。

| 注意                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 成員已經加入工作階段，並包含其安全的裝置位址之前，沒有可用的成員 （可能的主機） 的裝置權杖。 |

1.  使用裝置權杖擷取的主機候選項目從 MPSD **MultiplayerSession.Members 屬性**。

| 注意                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 如果工作階段已建立 SmartMatch 配對，您的用戶端可以使用可從 MPSD 透過主機候選項目的**MultiplayerSession.HostCandidates 屬性**。 |

2.  從主機候選項目清單中選取必要的主機。
3.  呼叫**MultiplayerSession.SetHostDeviceToken 方法**MPSD 的本機快取中設定裝置權杖。 如果設定主機裝置權杖呼叫成功，則本機裝置權杖會取代主機的 token。
4.  如果嘗試設定主機裝置權杖時收到的 HTTP/412 狀態碼，則查詢的工作階段資料，並查看主機裝置權杖是否為本機主控台。 如果不是本機主控台，另一個主控台已指定為仲裁程式。

| 注意                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 您的用戶端應該處理與其他 HTTP 代碼，分開的 HTTP/412 狀態碼，因為 HTTP/412 不會指出標準的失敗。 如需有關狀態碼的詳細資訊，請參閱[多人遊戲工作階段狀態碼](multiplayer-session-status-codes.md)。 |

5.  中所述，更新的工作階段中 MPSD， **How to:更新多人連線的工作階段**。

| 注意                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果您有沒有更好的演算法，用戶端可以實作 greedy （窮盡） 演算法，每個主機候選項目嘗試將自己設定為主機，如果沒有其他人已完成動作。 如需詳細資訊，請參閱 <<c0> [ 工作階段仲裁程式](mpsd-session-details.md)。 |

## <a name="manage-title-activation"></a>管理標題啟用

Xbox One 引發**CoreApplicationView.Activated 事件**通訊協定啟用期間。 在內容中的多人遊戲的 API，在使用者接受邀請，或加入另一位使用者時，會引發此事件。 這些動作會觸發標題必須回應所加入的使用者帶入遊戲與目標使用者啟用。

| 注意                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| 您的標題應該預期新的啟動引數，在任何時間，並永遠不會比照長度。 |

標題必須執行下列主要步驟，以處理標題啟用。

1.  設定事件處理常式**CoreApplicationView.Activated 事件**。 這個處理常式會觸發每次發生時的通訊協定啟用，即使標題已在執行中。
2.  在標題啟用開始的工作階段和訂閱的工作階段變更通知。 請參閱**How to:MPSD 工作階段變更通知訂閱**。
3.  您可以將使用者加入為作用中工作階段。 請參閱**How to:加入從標題啟用 MPSD 工作階段**。
4.  為活動的工作階段，公開透過 UI 設定檔設定大廳的工作階段。 請參閱**How to:設定使用者的目前活動**。
5.  加入遊戲的工作階段為作用中的使用者。 現在使用者可以連線到對等電腦，然後輸入玩遊戲或大廳。

下列流程圖說明如何處理標題啟用。

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>讓使用者可聯結

若要讓使用者可聯結，標題必須執行下列作業：

1.  建立工作階段物件，並修改所需的屬性。
2.  您可以將使用者加入為作用中工作階段。 請參閱**How to:加入從標題啟用 MPSD 工作階段**。
3.  決定使用者是否已指定為工作階段仲裁程式。
4.  如果使用者不是仲裁程式，請移至步驟 7。
5.  如果使用者是仲裁者，呼叫**MultiplayerSession.SetHostDeviceToken 方法**。
6.  嘗試寫入工作階段使用的呼叫來**MultiplayerService.TryWriteSessionAsync 方法**。
7.  設定工作階段為作用中的工作階段。 請參閱**How to:設定使用者的目前活動**。

下列流程圖說明可讓使用者聯結其他玩家在遊戲期間所採取的步驟。

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>傳送遊戲邀請

標題可以讓播放程式來傳送遊戲邀請如下：

-   傳送邀請大廳工作階段。
-   傳送邀請使用泛型的 Xbox 平台遊戲的工作階段參考邀請 UI。

若要傳送邀請玩家的遊戲，標題必須執行下列作業：

1.  將邀請遊戲播放可聯結。 請參閱**How to:讓使用者可聯結**。
2.  判斷是否要透過大廳工作階段傳送的邀請，或是使用邀請 UI。
3.  如果使用大廳工作階段，請傳送使用的呼叫來邀請**MultiplayerService.SendInvitesAsync 方法**。 這個方法可能需要的遊戲中 UI 名冊使用建置**SystemUI.ShowPeoplePickerAsync 方法**或**PartyChat.GetPartyChatViewAsync 方法**。
4.  如果使用邀請 UI，請呼叫**SystemUI.ShowSendGameInvitesAsync 方法**向邀請 UI。
5.  處理**RealTimeActivityService.MultiplayerSessionChanged 事件**之後的遠端播放程式聯結本機的播放程式。
6.  對於遠端的播放器，實作標題啟用程式碼。 請參閱**How to:管理標題啟用**。

下列流程圖說明如何傳送邀請。

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>加入遊戲工作階段從大廳工作階段

Windows 10 裝置上的遊戲工作階段必須有`userAuthorizationStyle`功能設定為 **，則為 true**如果它們不是大型的工作階段。 這表示`joinRestriction`屬性不可以是 「 無 」，這表示，工作階段不能公開可聯結直接。

常見的案例是建立收集器，，，然後將這些播放程式移到工作階段或對戰的遊戲大廳工作階段。 但如果遊戲工作階段不是可公開的聯結，那麼遊戲的用戶端將無法加入此遊戲工作階段，除非它們符合`joinRestriction`太過侷限在大部分情況下，此案例中的設定。

方案是用來連結大廳工作階段和遊戲的工作階段的傳輸控制代碼。  標題可以執行這項操作執行下列動作：

1. 當您建立遊戲的工作階段時，使用`multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)`API，以建立連結大廳工作階段及遊戲的工作階段的傳輸控制代碼。
2. 大廳工作階段，而不是遊戲工作階段的工作階段參考中儲存的傳輸控制代碼 GUID。
3. 當標題想要將成員從大廳的工作階段移至遊戲的工作階段時，每個用戶端會使用從大廳的工作階段的傳輸控制代碼藉由加入遊戲工作階段`multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)`API。
4. MPSD 查閱大廳工作階段，以確認嘗試加入遊戲工作階段使用的傳輸控制代碼的成員也大廳工作階段中。
5. 如果成員大廳工作階段中，他們將能夠存取的遊戲工作階段。

## <a name="join-an-mpsd-session-from-a-title-activation"></a>加入從標題啟用 MPSD 工作階段

當使用者選擇加入朋友的活動，或接受邀請使用 Xbox 殼層 UI 時，標題就會啟動以表示哪些工作階段使用者想要加入的參數。 標題必須處理這項啟動，並將使用者新增至對應的工作階段。

以下是標題應該遵循下列步驟：

1.  實作事件處理常式**CoreApplicationView.Activated 事件**。 此事件通知的標題的啟用。
2.  當處理常式引發時，檢查**IActivatedEventArgs.Kind 屬性**。 如果它設定為通訊協定，將事件引數轉換**ProtocolActivatedEventArgs 類別**。
3.  檢查**ProtocolActivatedEventArgs**物件。 如果 URI 所示**ProtocolActivatedEventArgs.Uri 屬性**符合任一 inviteHandleAccept （對應至已接受邀請） 或 activityHandleJoin （對應至透過殼層 UI 聯結），剖析查詢字串URI，其格式為一般的 URI 查詢字串的索引鍵/值組，擷取下列欄位：
    -   針對已接受邀請：
        1.  控制代碼
        2.  invitedXuid
        3.  senderXuid
    -   聯結：
        1.  控制代碼
        2.  joinerXuid
        3.  joineeXuid

4.  啟動項目的多人遊戲的程式碼，其中應包含呼叫**MultiplayerService.EnableMultiplayerSubscriptions 方法**。
5.  建立本機**MultiplayerSession 類別**物件，使用**MultiplayerSession 建構函式 (XboxLiveContext)**。
6.  呼叫**MultiplayerSession.Join 方法 （字串、 布林值、 布林值）** 加入工作階段。 使用下列參數設定，以便聯結設定為使用中：
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  呼叫**MultiplayerSession.SetSessionChangeSubscription 方法**shoulder-要點選加入之後變更的工作階段時。
8.  呼叫**MultiplayerService.WriteSessionByHandleAsync 方法**，使用 取得步驟 3 中所述的控制代碼。 現在使用者成員的工作階段，並可以使用工作階段中的資料來連線到遊戲。

## <a name="set-the-users-current-activity"></a>設定使用者的目前活動

使用者的目前活動會顯示在標題 Xbox 儀表板的使用者體驗。 透過工作階段，或透過標題啟用，則可以設定使用者的活動。 在後者的情況下，使用者會輸入透過配對或啟動遊戲的工作階段。

| 注意                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 透過工作階段設定的活動可以刪除藉由呼叫**MultiplayerService.ClearActivityAsync 方法**。 |

若要將工作階段設定為使用者的目前活動，此標題會呼叫**MultiplayerService.SetActivityAsync 方法**，工作階段傳遞的工作階段的參考。

若要設定透過標題啟用的使用者的目前活動，請參閱**How to:加入從標題啟用 MPSD 工作階段**。

## <a name="update-an-mpsd-session"></a>更新 MPSD 工作階段

| 注意                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 當您的標題會更新現有的工作階段，使用多人遊戲的 API 時，請記住，其所使用的本機複本，直到它會呼叫寫入工作階段。 |

若要更新現有的工作階段，標題必須：

1.  變更目前的工作階段，視需要，比方說，藉由呼叫**MultiplayerSession.Leave 方法**。
2.  所有的變更時，請將本機變更寫入 MPSD，使用任何一種方法：

    -   **MultiplayerService.WriteSessionAsync 方法**
    -   **MultiplayerService.WriteSessionByHandleAsync 方法**。
    -   **MultiplayerService.TryWriteSessionAsync 方法**
    -   **MultiplayerService.TryWriteSessionByHandleAsync 方法**

    若要設定的寫入模式**SynchronizedUpdate**如果寫入共用的部分，其他項目也可以修改。 請參閱[同步處理的工作階段更新](multiplayer-session-directory.md)如需詳細資訊。

    Write 方法寫入伺服器上的聯結，並取得最新的工作階段中，從要探索其他工作階段的成員和遊戲機的安全的裝置位址 (SDAs)。 如需有關如何建立這些主控台之間的網路連線的詳細資訊，請參閱 < **Xbox One 上的 Winsock 簡介**。

3.  捨棄舊的本機工作階段物件，並使用新擷取的工作階段物件，讓未來的動作為基礎的最新的已知的工作階段狀態。

## <a name="leave-an-mpsd-session"></a>讓 MPSD 工作階段

若要允許使用者將保留在工作階段，標題必須執行下列作業。

1.  呼叫**MultiplayerSession.Leave 方法**遊戲工作階段。
2.  中所述，更新的遊戲的工作階段中 MPSD， **How to:更新多人連線的工作階段**。
3.  如果有必要，請呼叫**離開**大廳工作階段的方法，並更新該工作階段。
4.  如有必要大廳工作階段，關閉多人遊戲的 API 取消註冊**RealTimeActivityService.MultiplayerSubscriptionsLost 事件**而**RealTimeActivityService.MultiplayerSessionChanged 事件**。

下列流程圖說明如何保留工作階段，然後關閉。

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>填滿期間配對的開啟工作階段的位置

若要填寫戰時票證工作階段中開啟的位置，標題必須遵循下列步驟如下所示：

1.  存取期間配對建立票證工作階段的最新的工作階段狀態。
2.  從 大廳工作階段中加入可用玩家玩遊戲。
3.  決定票證工作階段是否已滿。
4.  如果工作階段已滿，繼續遊戲。
5.  如果工作階段尚未完整，建立符合票證中所述**How to:建立比對票證**。 請務必建立票證*preserveSession*參數設定為 Always。
6.  繼續進行配對。 請參閱[使用 SmartMatch 配對](using-smartmatch-matchmaking.md)。

下列流程圖說明如何填滿期間配對的開啟工作階段的位置。

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>建立比對票證

若要建立的相符項目票證，配對 scout 必須：

1.  呼叫**MatchmakingService.CreateMatchTicketAsync 方法**，傳遞票證工作階段的參考。 方法會讀取 MPSD 的票證工作階段，並啟動工作階段中的使用者的配對。 在內部的方法呼叫**POST (/serviceconfigs/ {scid} /hoppers/ {hoppername})**。
2.  設定*preserveSession*絕對配對服務是否以符合工作階段的成員到新的工作階段或另一個現有的工作階段的參數。 設定*preserveSession*參數為 永遠允許重複使用現有的遊戲工作階段繼續玩遊戲的票證工作階段的標題。 已提交的工作階段會保留，並將任何相符的播放程式加入至該工作階段，就可以確定配對服務。

3.  使用**CreateMatchTicketResponse.EstimatedWaitTime 屬性**傳入**CreateMatchTicketResponse 類別**物件來設定使用者期望的配對的時間。
4.  使用**CreateMatchTicketResponse.MatchTicketId 屬性**取消配對，如有需要藉由刪除票證工作階段的回應物件中傳回。 票證刪除會使用**MatchmakingService.DeleteMatchTicketAsync 方法**。

## <a name="get-match-ticket-status"></a>取得符合票證狀態

您的標題應該執行下列命令來擷取相符項目票證狀態：

1.  取得**MultiplayerSession 類別**票證工作階段的物件。
2.  使用**MultiplayerSession.MatchmakingServer 屬性**若要存取**MatchmakingServer 類別**配對中所使用的物件。
3.  請檢查**MatchmakingServer**物件來判定配對程序、 工作階段中，與目標工作階段的參考，一般的等候時間的狀態，如果已找到相符項目。
