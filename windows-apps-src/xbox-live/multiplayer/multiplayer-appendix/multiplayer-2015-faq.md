---
title: 多人遊戲 2015 年常見問題與疑難排解
description: Xbox Live 的多人遊戲 2015年和疑難排解的相關常見問題的問題。
ms.assetid: 75823f10-b342-4e20-b885-e5ad4392bc3d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 多人遊戲
ms.localizationpriority: medium
ms.openlocfilehash: 171d80f4fc925d95d80043f40bb387b045a4fe23
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625523"
---
# <a name="multiplayer-2015-faq-and-troubleshooting"></a>多人遊戲 2015 年常見問題與疑難排解

-   我正在開發新的標題。 我應該使用哪一個多人遊戲的 API 項目？
-   如何從服務存取新的多人遊戲 API？
-   我的標題訂閱變更多個工作階段嗎？
-   將使用者立即移除如果他/她會失去網路連線或關閉主控台？
-   我要如何找出要使用何種 SCID、 工作階段範本和沙箱？
-   有我可以到我的標題的比較要求主體的範例嗎？
-   呼叫 MPSD 時收到 HTTP/403 程式碼。
-   呼叫 MPSD 時收到 HTTP/404 程式碼。
-   為什麼我會收到 HTTP/404 程式碼呼叫時**MultiplayerService.WriteSessionByHandleAsync**或是**MultiplayerService.GetCurrentSessionByHandleAsync**？
-   呼叫 MPSD 時收到 HTTP/412 程式碼。
-   我收到 HTTP/400，405、 409、 503，等程式碼呼叫 MPSD 時。
-   什麼可以或應該變更工作階段範本中的 我的標題？
-   我收到錯誤，指出我的工作階段未初始化。
-   我的工作階段尚未建立，並收到 HTTP/204 程式碼。
-   何時輪詢 MPSD？
-   發生什麼情況的玩家，已保留或受邀至工作階段不會聯結它嗎？
-   為什麼會建立的工作階段未發現的配對？
-   2015 多人遊戲正確使用合作對象的方式以及 2014年多人遊戲中使用它們的方式之間的主要差異是什麼？
-   如果遊戲工作階段已開啟的玩家資料，而且正在進行時，為什麼會使用者無法以尋找工作階段開始後予以支援聯結嗎？
-   如果遊戲工作階段已開啟，可以只是已加入遊戲的使用者只需加入此工作階段，然後開始而不需要等候此保留項目嗎？
-   當大型遊戲工作階段會在我的標題中播放時，為什麼不工作階段的所有成員看到遊戲邀請快顯通知？
-   我看到不一致的遊戲行為，並已收到參考遊戲的合作對象功能的通訊協定啟用資訊。
-   為什麼我看到 v105 工作階段的文件的語法在我的追蹤中雖然我已經設定 v107 工作階段範本？


### <a name="i-am-developing-a-new-title-which-multiplayer-api-elements-should-i-use"></a>我正在開發新的標題。 我應該使用哪一個多人遊戲的 API 項目？

2014 多人遊戲功能將繼續適用於現有的項目，但相關聯的 API 項目最有可能是已被取代。 我們強烈建議 2015年多人使用，在 2015 年發行的準備您的用戶端時。


### <a name="how-can-i-access-the-new-multiplayer-api-from-a-service"></a>如何從服務存取新的多人遊戲 API？

請參閱[呼叫 MPSD](multiplayer-session-directory.md)。


### <a name="can-my-title-subscribe-to-changes-for-more-than-one-session"></a>我的標題訂閱變更多個工作階段嗎？

是，標題就可以訂閱收到每個連接的最多 10 個工作階段中的變更。


### <a name="will-a-user-be-removed-immediately-if-heshe-loses-a-network-connection-or-turns-off-the-console"></a>將使用者立即移除如果他/她會失去網路連線或關閉主控台？

Web 通訊端連線可讓 MPSD 快速偵測用戶端中斷連線，並將用戶端設定為非作用中。 只要在非使用中移除逾時到期，則會移除工作階段的成員。 如需詳細資訊，請參閱 <<c0> [ 工作階段逾時](mpsd-session-details.md)。


### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>我要如何找出要使用何種 SCID、 工作階段範本和沙箱？

如果您不參與初始註冊您的標題，建立這項資訊時，表示您沒有存取此資訊。 請連絡您開發人員的帳戶管理員，使用者可以取得這項資料。


### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-to-the-one-for-my-title"></a>有我可以到我的標題的比較要求主體的範例嗎？

請參閱中的要求結構**MultiplayerSessionRequest (JSON)**


### <a name="i-am-getting-an-http403-code-when-calling-mpsd"></a>呼叫 MPSD 時收到 HTTP/403 程式碼。

這通常是權限或範圍問題。 收集的 Fiddler 追蹤，以取得詳細資訊，然後檢查 訊息傳回做為 HttpResponse 常見 403 訊息主體的一部分：

-   您無法存取要求的服務組態。
    1.  請確認您使用的帳戶具有存取權的沙箱。
    2.  請確認您是在正確的沙箱中。
     3.  如果您是使用憑證驗證，並收到此錯誤，請連絡您的開發人員帳戶管理員。   您無法存取要求的工作階段。 呼叫的使用者必須具備多人的權限，和私用的工作階段只能讀取工作階段的成員。

    您沒有可查看工作階段中，可能是因為您嘗試存取具有私用的可見性的工作階段。 如果可見性設定為 [私人] 時，只有該工作階段的成員可以讀取工作階段的文件。

| 注意                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 使用者必須具有金級帳戶，以註冊新的工作階段。 沒有黃金播放的使用者帳戶權限，註冊新的工作階段的要求會傳回 HTTP/403。 |

-   要求本文不能包含現有成員的參考，除非主體的驗證包括伺服器。

    您無法加入至工作階段的另一位使用者代表使用者。 您只能邀請。 索引設定為 「 保留\_&lt;數字&gt;」 邀請播放程式。


### <a name="i-am-getting-an-http404-code-when-calling-mpsd"></a>呼叫 MPSD 時收到 HTTP/404 程式碼。

收集 Fiddler 追蹤取得詳細資訊，然後執行下列作業：

1.  檢查一般的 404 訊息 HttpResponse 主體的一部分傳回的訊息：
    -   要求的服務組態會是無效或未設定工作階段。 請確定使用 SCID 正確無誤。
    -   找不到要求的工作階段。 請確定已建立的工作階段和工作階段範本正確，然後才能擷取它。 您可以使用 PUT 呼叫來建立工作階段。

2.  請檢查您正在使用的 URI。
3.  重新啟動主控台，或改用新的使用者。
4.  請檢查錯誤程式碼或其他解決方案的遊戲開發人員論壇。
5.  請確認為空白的結果不刪除工作階段。 工作階段可以成為空的因為逾時的使用者。當發生這種情況時，通常是因為工作階段的所有成員在逾時套用到，例如 「 就緒 」 或 「 非作用中 」 狀態。 請參閱[使用者的工作階段狀態](mpsd-session-details.md)的使用者狀態詳細資料。


### <a name="why-am-i-getting-an-http404-code-when-calling-multiplayerservicewritesessionbyhandleasync-or-multiplayerservicegetcurrentsessionbyhandleasync"></a>為什麼我會收到 HTTP/404 程式碼呼叫時**MultiplayerService.WriteSessionByHandleAsync**或是**MultiplayerService.GetCurrentSessionByHandleAsync**？

如果您的標題以回應包含控制代碼識別碼聯結通訊協定啟用的控制代碼存取 MPSD 工作階段，在 通訊協定啟用的控制代碼識別碼可能已過時的其中一個原因如下：

-   從該處標題起始聯結 Xbox shell 中的 UI 檢視可能過期。 某些 UI 的檢視，比方說，使用者的設定檔卡片，不會自動重新整理連接控制代碼開啟時。 若要避免收到 HTTP/404 程式碼，應關閉標題，並將其重新開啟要加入之前重新整理資料的檢視中。
-   加入標題的使用者可能已切換活動標題從 Xbox 殼層 UI 中選取聯結作業之後的工作階段。 因此 HTTP/404 程式碼的情況應該很少見。

在這些情況下，程式碼應該會顯示聯結的使用者錯誤訊息，指出聯結失敗。


### <a name="i-am-getting-an-http412-code-when-calling-mpsd"></a>呼叫 MPSD 時收到 HTTP/412 程式碼。

如果工作階段已經存在，下列要求會傳回 HTTP/412:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-None-Match: *


如果工作階段的 etag 不符合 If-match 標頭，下列要求會傳回 HTTP/412:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D


請參閱[同步處理的工作階段更新](multiplayer-session-directory.md)如需詳細資訊。


### <a name="i-am-getting-an-http400-405-409-503-etc-code-when-calling-mpsd"></a>我收到 HTTP/400，405、 409、 503，等程式碼呼叫 MPSD 時。

收集 Fiddler 追蹤，以取得詳細資訊，並檢查訊息，則為 HttpResponse 主體的一部分傳回。 這會提供足夠的資訊來識別並修正錯誤，或您可以搜尋解決方案的開發人員論壇。

您也可以取得回應主體如果您使用 XSAPI，如中所述[疑難排解 Xbox Live 服務 API](../../using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md)。 或者，可以使用您的程式碼**XboxLiveContextSettings.EnableServiceCallRoutedEvents**屬性，以將輸出傳送至您的標題 UI。


### <a name="what-can-or-should-i-change-in-the-session-templates-for-my-title"></a>什麼可以或應該變更工作階段範本中的 我的標題？

工作階段範本模式對於您的工作階段，而且您無法覆寫已設定的範本中的常數。 不過，您可以將新屬性加入範本。 請參閱[MPSD 工作階段範本](multiplayer-session-directory.md)如需詳細資訊。


### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>我收到錯誤，指出我的工作階段未初始化。

範例回應時發生錯誤：

400- \[ResponseBody\]:此工作階段設定為需要至少 2 個成員，以啟動受管理的初始設定。

無法建立工作階段，因為會包含在要求中沒有足夠的工作階段成員保留項目 「 初始化 」 的欄位設定為 true。 您的程式碼可以設定此欄位使用的成員*initializeRequested*參數**MultiplayerSession.AddMemberReservation**方法或**MultiplayerSession.Join**方法。

初始化工作階段範本中指定時，請確定該 「 初始化 」:"true"設定為足夠的成員保留項目將配對 QoS。 如需詳細資訊，請參閱 <<c0> [ 目標工作階段初始化與 QoS](smartmatch-matchmaking.md)。


### <a name="my-session-is-not-being-created-and-im-getting-an-http204-code"></a>我的工作階段尚未建立，並收到 HTTP/204 程式碼。

此狀態碼表示沒有任何使用者已新增至您建立的時工作階段。 如果工作階段，當建立，且工作階段的空白逾時為 0 （預設值） 中沒有任何使用者，不會建立工作階段。

請確定您在建立工作階段時加入至少一名玩家。 專用的伺服器的情況下，取得何者嘗試建立比對或誰會建立比對，並將該播放程式在比對的初始播放播放程式。 或者，您可以變更或移除工作階段的空白逾時。 如需詳細資訊，請參閱 <<c0> [ 工作階段逾時](mpsd-session-details.md)。


### <a name="when-should-i-poll-mpsd"></a>何時輪詢 MPSD？

您的遊戲，必須避免輪詢 MPSD。 如果標題必須探索 MPSD 工作階段的變更，它應該訂閱的工作階段變更事件。 如需詳細資訊，請參閱[How to:MPSD 工作階段變更通知訂閱](multiplayer-how-tos.md)。


### <a name="what-happens-if-a-player-who-was-reserved-or-invited-to-the-session-does-not-join-it"></a>發生什麼情況的玩家，已保留或受邀至工作階段不會聯結它嗎？

這取決於播放程式會收到通知，遊戲工作階段已準備好時，標題正在執行。 如果播放程式會在標題中，而且不接受遊戲工作階段通知標題 UI 中的，負責要移除的遊戲工作階段中的播放程式的標題**MultiplayerSession.Leave**方法。

如果標題限制或未執行，此命令介面提供通知，會通知播放程式的位置是否可用。 如果播放程式會拒絕或忽略的系統通知，MPSD 會移除該人員在遊戲工作階段中。


### <a name="why-would-a-created-session-not-be-found-by-matchmaking"></a>為什麼會建立的工作階段未發現的配對？

Xbox One 上只要建立工作階段是不夠的配對，來尋找新的工作階段。 您必須建立符合票證，啟動廣告配對服務的工作階段。 請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。


### <a name="what-is-the-key-difference-between-the-way-parties-are-properly-used-by-2015-multiplayer-and-the-way-they-were-used-in-2014-multiplayer"></a>2015 多人遊戲正確使用合作對象的方式以及 2014年多人遊戲中使用它們的方式之間的主要差異是什麼？

在 2015年的多人遊戲，多人遊戲的 API 會定義任何系統層級的遊戲合作對象，只對談的合作對象。 而不是使用遊戲的合作對象，標題會使用工作階段控制聯結、 邀請，和相關功能。 2014 多人遊戲，Xbox One 上的多人遊戲 API 以突顯的方式用於遊戲的合作對象概念 (**合作對象**類別)，這會有效地實作系統層級聯結大廳，而不是遊戲邀請。


### <a name="if-a-game-session-has-open-player-slots-and-supports-join-in-progress-why-would-users-not-be-able-to-find-the-session-once-it-has-started"></a>如果遊戲工作階段已開啟的玩家資料，而且正在進行時，為什麼會使用者無法以尋找工作階段開始後予以支援聯結嗎？

遊戲的工作階段啟動時，不會再於配對服務公告。 仲裁程式如果在工作階段中變成可用的玩家資料，而且仲裁程式 （主機） 想要吸引新的播放程式，必須為的進行中工作階段建立新的相符項目票證**MatchmakingService.CreateMatchTicketAsync**方法。 票證會通告此工作階段，並尋找更多玩家。 請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。


### <a name="if-a-game-session-is-open-can-a-user-who-has-just-joined-a-game-simply-join-the-session-and-start-playing-without-having-to-wait-for-the-reservation"></a>如果遊戲工作階段已開啟，可以只是已加入遊戲的使用者只需加入此工作階段，然後開始而不需要等候此保留項目嗎？

是。 這是在其中您的標題會使用多個工作階段來追蹤在遊戲工作階段中的子群組的播放程式的情況下特別有用。 加入使用者可能會加入代表他/她的群組，此工作階段，則需要加入更大的遊戲工作階段。


### <a name="when-large-game-sessions-are-playing-in-my-title-why-arent-all-session-members-seeing-the-game-invite-toast"></a>當大型遊戲工作階段會在我的標題中播放時，為什麼不工作階段的所有成員看到遊戲邀請快顯通知？

當您的標題會將使用者新增至透過加入工作階段時，標題永遠設定**MultiplayerSessionMember.InitializeRequested**屬性設為 true。 這會告訴 MPSD 移出初始化階段遊戲之前等待其餘的工作階段的成員。 否則，使用者必須非常短的視窗，在其中加入和快顯通知及工作階段變更的通知將會錯過通知。


### <a name="i-am-seeing-inconsistent-game-behavior-and-have-received-protocol-activation-information-referencing-game-party-features"></a>我看到不一致的遊戲行為，並已收到參考遊戲的合作對象功能的通訊協定啟用資訊。

這表示您混用 2014年多人遊戲和 2015年多人遊戲的功能。 2015 多人遊戲的 API 應該永遠不會用於針對 2014年的多人所撰寫的程式碼。


### <a name="why-am-i-seeing-the-syntax-for-v105-session-documents-in-my-traces-although-i-have-configured-a-v107-session-template"></a>為什麼我看到 v105 工作階段的文件的語法在我的追蹤中雖然我已經設定 v107 工作階段範本？

MPSD 執行自動根據用戶端要求的工作階段的文件版本之間的轉換。 目前所有的 Xbox 服務 Api 會使用 v105 MPSD 的要求。 這可能會導致 v107 工作階段範本之間的不同語法和 v105 追蹤，但沒有任何其他功能的影響。 工作階段範本應該設定為 v107 中。

© 2015 Microsoft Corporation. 保留一切權利。
上提交您的意見反應<https://forums.xboxlive.com/spaces/22/index.html>。
版本：2.0.90731.0
