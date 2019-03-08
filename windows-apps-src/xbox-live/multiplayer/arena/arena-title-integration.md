---
title: Arena 標題整合指南
description: 了解如何在 Xbox Live 競技場平台支援建置標題。
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 競技場、 聯賽
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659883"
---
# <a name="arena-title-integration-guide"></a>Arena 標題整合指南

## <a name="introduction"></a>簡介

Xbox 的競技場平台可讓聯賽召集人 (TOs) 建立及操作項目的支援競技場的比賽。 Arena 支援同時 Xbox Live，可讓 「 發行者 」 和使用者執行比賽，以及第三方 TOs 人員競技場與整合。 聯賽，例如單淘汰或瑞士的樣式取決於 收件者。

本主題將逐步引導您，標題開發人員，了解如何在您的標題中實作競技場的支援。 遊戲競技場啟用之後，它會使用任何支援的聯賽格式，選取 收件者。 若要協調為根據的聯賽結構標題的相符項目。 Arena 啟用標題然後將適當的使用者放入每個相符項目，在遊戲，並比對結果回報給 收件者。

## <a name="overview"></a>概觀

Xbox Arena 使用熟悉的 Xbox 多人連線遊戲開發的概念。 如果您不熟悉 Xbox 多人遊戲系統，我們建議您檢閱該文件，然後再繼續。 此程序是類似於使用者接受邀請到多人遊戲。 當使用者播放競技場聯賽中的相符項目時，快顯通知快顯通知使用者。 接受快顯通知會導致您會收到標準的競技場通訊協定啟用事件的標題。 如果您的標題不正在執行，它會啟動這一次。

聯賽相符項目本身是由使用多人遊戲工作階段目錄 (MPSD) 工作階段進行協調。 競技場，在工作階段的競技場平台，而不是所建立您的標題。 這是由使用工作階段範本和其他遊戲組態都由您為標題 「 發行者 」。 聯賽相符項目參與者會有已經加入至工作階段。 為標題 「 發行者 」，您必須設定用於競技場，以支援報告的結果仲裁的工作階段範本。 稍後在本文件中包含的工作階段的完整需求。 也可用來建立任何額外的工作階段，它需要您的標題。

您的標題會使用工作階段設定的相符項目和報告結果。 通訊協定啟用回應，並與其互動 MPSD 工作階段是整合競技場標題所需的最低層級。 透過 Xbox 競技場 UI 支援瀏覽及註冊比賽。 （選擇性） 項目可以從聯賽中樞服務，啟用更豐富的標題中體驗取得聯賽的其他資訊。 比方說，藉由使用聯賽中樞，您的標題可以顯示小組名稱 中，例如內容，並探索新的比對工作階段的使用者。 此資料流程的綠色箭號，在下列圖表中，會說明和詳細資料中所述[體驗需求和最佳做法](#experience-requirements-and-best-practices)一節。 較淡的箭頭表示小組的工作階段的參考變更經過一段時間，當使用者移動聯賽中的項目。

![](../../images/arena/tournament-flow.png)


Arena 通訊協定啟用 URI 包含資訊聯賽，相符項目，並透過比對時，可以叫用您的標題的深層連結的工作階段。 深層連結會回到 Xbox 競技場 UI 中的使用者。 更詳細地說明這些 URI 元件[通訊協定啟用](#1protocol-activation)一節[競技場整合的基本需求](#basic-requirements-for-arena-integration)。

## <a name="basic-requirements-for-arena-integration"></a>Arena 整合的基本需求

本章節提供技術的指導方針和詳細資料，整合您的標題支援競技場的最低需求。 這種整合會利用描述下的概觀圖中的橘色箭號的資料流程。

![](../../images/arena/arena-data-flow.png)

您的標題是從 Xbox 競技場 UI 啟用通訊協定。 這可能源自聯賽的詳細資料頁面或比對任何其他進入點的快顯通知。 使用者參與相符項目時，會發生的步驟如下：

1.  您的標題為 Xbox 競技場 ui 啟用通訊協定。
2.  您的標題使用的啟動參數，來播放單一相符項目。
3.  透過比對時，您的標題會報告結果 MPSD 遊戲工作階段。
4.  標題可讓使用者返回競技場 UI 選項。

下列各節會瀏覽每個詳細步驟。

### <a name="1--protocol-activation"></a>1.通訊協定啟用

Arena UI 一開始會項目通訊協定啟動您的職稱時相符項目可供使用者。 有兩種情況下： 可能會在這個階段中，啟動您的標題，或可能已經在執行。 這兩種情況會類似於標題啟動以回應使用者接受邀請給多人連線遊戲時，會發生什麼事。

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>如果您的標題會啟動在通訊協定啟用

**Activated**第一次啟動您的職稱時就會引發事件。 Arena 通訊協定啟動該初始啟動時，使用者嘗試播放聯賽中啟動您的標題。 具有您盡快前往相符項目，並略過功能表，然後登入畫面的標題。 參與使用者 Xbox 使用者識別碼 (XUID) 所提供的啟動 URI，並已登入。

#### <a name="if-your-title-is-already-running"></a>如果您的標題已在執行中

**Activated**事件也可能會引發競技場通訊協定啟用時已在執行您的標題。 **啟用**事件只會觸發明確的使用者動作 （作快顯通知，指出相符項目，或跳到相符項目從 Xbox 競技場 UI）。 如果您的標題功能表螢幕上等候，就立即跳到聯賽相符項目。 如果您的標題會收到通訊協定啟用期間的遊戲，就讓使用者選擇来離開遊戲，並在最有利的方式，它可以報名參加聯賽相符項目。 讓使用者有機會視需要儲存他們的遊戲。

通訊協定啟動會傳遞至您的標題，透過`CoreApplicationView.Activated`事件。 如果`IActivatedEventArgs.Kind`之事件引數的屬性設定為`Protocol`啟用的通訊協定啟動過程中，且引數可以轉換成`ProtocolActivatedEventArgs`類別，其中的通訊協定啟用 URI 是可透過`Uri`屬性。

具有您檢查通訊協定啟用 URI XUID 的標題。 如果它不符合目前的播放程式，您的標題也會需要切換使用者內容。

#### <a name="the-protocol-activation-uri"></a>通訊協定啟動 URI

Uri 的範本是︰

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

在主控台上時代項目，啟用結構描述會稍有不同：

```
ms-xbl-{titleIdHex}://
```

這是與邀請時相同。 基於此目的，標題識別碼必須是八個十六進位字元，因此會包含前置字元為零，如有必要。

必須先確定`Uri.Host`（緊接在"://"分隔符號是名稱） 是 「 聯賽 」。 這就是如何競技場的通訊協定啟用的區別在於啟用其他功能，例如遊戲的邀請。

查詢字串引數會是 URL 編碼，並不區分大小寫。 標題不應該相依參數的順序，就應該忽略無法辨認的參數。


Paramater(s) | 描述
--- | ---
**action** | 唯一支援的動作是 「 joinGame"。 如果**動作**遺漏或另一個值所指定的是，略過啟用。
**joinerXuid** | **JoinerXuid**是 XUID 的登入的使用者嘗試播放聯賽相符項目中。 您的標題必須允許切換成這位使用者的內容。 如果**joinerXuid**未簽署，標題必須提示使用者執行該動作所啟動的帳戶選擇器。 XUIDs 一律會以十進位表示。
**organizerId tournamentId** | **OrganizerId**並**tournamentId**、 結合時，表單聯賽相符項目相關聯的唯一識別碼。 如果您選擇要顯示在您的標題，請使用這個識別項比賽中樞中，從擷取聯賽的詳細的資訊。
**teamId** | **TeamId**是小組的聯賽的內容中的唯一識別碼的使用者 (藉由指定**joinerXuid**參數) 的成員。 像是**organizerId**並**tournamentId**參數，您可以使用**teamId**來選擇性地擷取從聯賽中樞的 小組的相關資訊。
**scid templateName]、 [名稱** | 在一起，這些識別的工作階段。 這些是工作階段的 MPSD URI 路徑中的相同三個參數：</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`。</br></br>在 XSAPI，它們的三個參數`multiplayer_session_reference `建構函式。
**returnUri returnPfn** | **ReturnUri**使用者返回 Xbox 競技場 UI 是通訊協定啟用的 uri。 **ReturnPfn**參數可能會或可能不存在。 如果是，它是產品系列名稱 (PFN) 之應用程式的目的是要處理**returnUri**。 示範如何使用這些值的範例程式碼，請參閱[回到 Xbox 競技場 UI](#4returning-to-the-xbox-arena-ui)。

### <a name="2--playing-the-match"></a>2.播放相符項目

聯賽召集人建立 MPSD 工作階段時，會將使用者設定為非作用中的成員，該工作階段。 您的標題必須立即播放程式為 作用中使用設定`multiplayer_session::join()`。 這表示 Xbox Live 和使用者會在您的標題，並準備好播放的相符項目中的其他使用者。

比對的開始時間是在工作階段中`/servers/arbitration/constants/system/startTime`做為標準的 UTC 格式的日期時間值 (例如"2009年-06-15T13:45:30.0900000Z")。 (開始時間也會提供為 XSAPI `multiplayer_session_arbitration_server::arbitration_start_time()`，開始在 1703 XDK)。 收件者可以建立之前的開始時間到目前為止的工作階段，如它想 （包括並行的開始時間）。 相符項目會傳送通知給相符項目參與者在開始時間，讓標題永遠不會啟動，並在開始時間之前。 開始時間也是處 MPSD 可讓您報告結果比對的最早時間。

您的標題會查看每個成員`/member/{index}/constants/system/team`屬性 (`multiplayer_session_member::team_id()`)，找出其 team 每位使用者位於。

您的標題也會使用工作階段來判斷相符項目設定，例如地圖和模式。 在工作階段範本，或做為自訂常數的聯賽定義的一部分，則可以設定這些標題特有的設定。 (如需詳細資訊，請參閱 <<c0> [ 競技場設定標題](#configuring-a-title-for-arena)。)以下是範例：

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

您也可以使用從為 JavaScript Object Notation (JSON) 物件的工作階段擷取這些設定`multiplayer_session_constants::session_custom_constants_json()`API。

一般情況下，您的標題應該相同的方式，它會將自己的 MPSD 工作階段將競技場工作階段。 比方說，它可以建立控制代碼，並訂閱 RTA 通知。 但有一些差異。 比方說，您的標題無法變更遊戲工作階段的名冊，或使用服務品質 (QoS) 功能的工作階段中，而且必須參與仲裁。 本文件稍後提供的工作階段的完整詳細資料。

### <a name="3--reporting-results"></a>3.報告結果

比對的結果會回報給競技場和透過工作階段的若要使用名為仲裁的功能。 仲裁是使用工作階段安全地播放的相符項目和報表結果的架構。

仲裁式工作階段中，這表示它有固定的時間軸仲裁 framework 強制執行時，便會提供給您的標題中的通訊協定啟用步驟的工作階段。 下圖顯示仲裁時間軸。

![](../../images/arena/arbitration-timeline.png)

至少一名玩家必須 forfeit 時間，也就是開始時間之前工作階段中作用中 (`multiplayer_session_arbitration_server::arbitration_start_time()`) 再加上 forfeit 逾時。Forfeit 逾時是工作階段中，以毫秒為單位，在數`/constants/system/arbitration/forfeitTimeout`(`multiplayer_session_constants::forfeit_timeout()`)。 如果有其他任何已加入 forfeit 時間之前為作用中工作階段，則取消比對。

仲裁逾時是以毫秒為單位，在`/constants/system/arbitration/arbitrationTimeout`(`multiplayer_session_constants::arbitration_timeout()`)。 仲裁時間是開始時間加上仲裁逾時的值，並代表的時間的播放程式必須已完成比對並回報結果。 此值是由 「 發行者 」 設定中的工作階段範本或遊戲模式。 將它設定為允許太多的時間，視您的標題所需完成比對。

您的標題可以報告在任何時間的開始時間和仲裁時間之間的結果。 Forfeit 時間與仲裁情況下之後每個工作階段的作用中成員已送出結果, 之間的任何時候，就會發生仲裁。 比方說，如果作用中工作階段在 forfeit 只包含一個成員，它們可以 （且應該） 張貼的結果，並會發生仲裁。 無論在仲裁時有多少筆結果，如果已經沒有，會發生仲裁。 如果達到仲裁時間時，會不提交任何結果，在比對所有參與者都有機會遺失。

您也可讓遊戲伺服器強制仲裁，隨時都只需要撰寫仲裁式的結果。

使用者是否已仲裁式的工作階段中 （可能是因為仲裁逾時到期、 遊戲伺服器將會進行仲裁在工作階段，或使用者加入最遲），您的標題結束比對，並向使用者顯示仲裁式的結果。

仲裁結果一律會包含每個小組的結果。 當個別的播放程式會將結果寫入至工作階段時，它包含不只是其小組的結果，但完整設定的每個小組的結果。

JSON 中的結果看起來像這樣。

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

在此範例中，小組識別碼是 「 紅色 」 和 「 藍色 」。 第二個 red team 會出現在第一個 blue team。 其他可能值為**結果**是：

* 「 贏得 」
* 「 遺失 」
* 「 draw 」
* 「 noshow"

如果**結果**是 「 排名 」，排名以外省略該小組的任何項目。

個別使用者將比對結果寫入`/members/{index}/properties/system/arbitration`(`multiplayer_session::set_current_user_member_arbitration_results()`)。 下列範例說明如何標題可能會建構和提交一組結果，假設您的標題，不小組 （也就是所有播放程式會對小組的其中一個）。

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

MPSD 仲裁之後，會在最終的結果`/servers/arbitration/properties/system`(`multiplayer_session::arbitration_server()`)，以及一些其他屬性如下所示。

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

可能性**resultState**包括：

* 「 已完成 」:所有使用中的播放程式會報告結果。
* 「 partialresults 」:部分但並非所有使用中的播放程式會報告仲裁逾時之前的結果。
* 「 /noresults 」:沒有任何作用中的播放器報告仲裁逾時之前的結果。
* 「 取消 」:沒有使用中的播放程式到達 forfeit 逾時之前。

在 「 /noresults 」 和 「 取消 」，會省略結果。

**ResultSource** MPSD 執行個別玩家所報告的結果為基礎的仲裁時，是 「 仲裁 」。 遊戲伺服器可以寫入 /servers/arbitration/properties/system 本身，略過仲裁。 在此情況下，它會指出**resultSource**的 「 伺服器 」。 此外，也可以重新撰寫的遊戲伺服器 （也就是修正或調整） 的結果，在其中它的大小寫設定**resultSource** 「 調整 」。

**ResultConfidenceLevel**是介於 0 到 100 之間的整數，表示結果的共識的層級。 100 表示所有播放程式表示同意。 0 表示沒有達成共識，且結果已選擇基本上是隨機的提交。 當遊戲伺服器設定的仲裁結果時，它會設定**resultConfidenceLevel**-通常為 100，除非因故甚至是遊戲伺服器本身不確定 （例如，如果它報告自己每位玩家的結果投票的程序）。 設定**resultConfidenceLevel**也當結果會調整，以反映在調整過的結果中的信心。

當**resultSource**是 「 仲裁 」 (和**resultState**是"completed"或"partialresults 」)，提供的結果是至少其中一個播放器回報的結果完全相同複本。

### <a name="4--returning-to-the-xbox-arena-ui"></a>4.回到 Xbox 競技場 UI

相符項目上方 （或可能放棄比對進行中的播放程式要求的回應），形成播放程式返回 Xbox 競技場 UI，從它們已加入比對的選項。 這可以完成後續的比對結果 畫面或任何其他結束的遊戲 UI。

若要返回競技場 UI，叫用**returnUri**為傳入的通訊協定啟動 URI，藉由從使用`Windows::System::Launcher`類別。 請務必要包含的使用者內容。

啟動 API 會公開比通用 Windows 平台 (UWP) 遊戲的紀元遊戲稍有不同。 API 的紀元版本不允許提供，PFN，因此甚至可以忽略 PFN 有啟用 URI。

若要讓使用者返回競技場 UI 時期的範例程式碼如下。

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

可以設定 UWP 遊戲**TargetApplicationPackageFamilyName**屬性上的**LauncherOptions**要傳回的 PFN，如果已提供通訊協定啟用的 URI。 這可確保特定的應用程式會啟動和使用者會進入存放區，如果尚未安裝應用程式。

若要返回競技場 UI 中的使用者對 UWP 應用程式的範例程式碼如下。

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

叫用後競技場 UI，您的標題會繼續執行，可能在該相同的畫面，等候另一個通訊協定啟用的事件。 這樣一來，玩家有另一個相符項目，若要播放，您的標題準備就緒。 這可以加速使用者體驗，因為播放程式會從符合項目，您的標題和 Xbox 競技場 UI 之間切換。

### <a name="handling-errors-and-edge-cases"></a>處理錯誤和極端案例

上一節中的流程描述黃金透過播放相符項目路徑。 有許多地方，您可能會看到非預期的行為，或可能會引發問題。 本節涵蓋數個可能的錯誤或邊緣案例，並提供處理它們在您的標題中的建議。

#### <a name="game-session-not-found"></a>找不到的遊戲工作階段

如果您的標題會啟動具有相符項目，工作階段 ref，用戶端無法再從 MPSD 要求該工作階段時，收到錯誤，呈現給使用者指出他們都無法加入比對發生錯誤。

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>使用者嘗試加入已經的相符項目

如果使用者嘗試聯結相符項目之後已經報告結果時，封鎖該用戶端無法啟動新的相符項目，並顯示給使用者的錯誤。 您可以檢查是否結果已經報告由逐一查看工作階段的成員以查看是否有任何的`/members/{index}/arbitrationStatus`(`multiplayer_session_member::tournament_arbitration_status`) 設為 「 完成 」。

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>無法建立 p2p 連線的遊戲用戶端

如果您的標題會使用多人遊戲的用戶端之間的通訊 (p2p) 連線，且沒有支援轉送，有可能無法組成 p2p 連線在一起會比對的使用者所在的執行個體。

用戶端是否能夠針對比對擷取的工作階段，但無法連線至其他用戶端，報告每個小組下的 「 draw 」 結果`/members/{index}/properties/system/arbitration/results`(`multiplayer_session::set_current_user_member_arbitration_results()`)。 這表示比對具有不當時，到 Xbox Live。 它也可防止的入侵其中使用者可以向前聯賽強制 p2p 連線失敗。

#### <a name="game-client-disconnects-mid-match"></a>遊戲的用戶端中斷連線的中間的相符項目

其中一個最常見的錯誤狀況時，用戶端中斷連線時播放電腦是相符項目。 根據比對和您的實作的狀態，有幾個選項來處理此情況下：

* 如果聯賽相符項目是與其中一個，或如果小組的所有成員會都中斷其他用戶端和/或您的遊戲服務，我們建議您回報為已中斷連線的小組的 「 遺失 」。 這可防止在其中使用者可以比對，它們會失去報告時，防止結果強制中斷連線的情況。
* 如果符合的多個成員，以促進團隊之間的子集用於小組的用戶端中斷連線的中間的相符項目，您可以選擇在該點結束比對，或允許它繼續進行其餘的使用者。 如果您選擇繼續比對，您可能也可以選擇性地允許重新加入比對已中斷連線的使用者。

#### <a name="full-teams-not-present"></a>完整的小組不存在

如果您的標題支援比對與兩個或以上的小組，您可能會遇到某些小組成員會啟動您的標題，其相符的項目失敗的情況。 在此情況下，您可以選擇允許小組合作，而不需要其小組成員，播放，並選擇性地允許加入進行中的比對該小組成員的成員。

或者，您可以自動強制執行結果。 如果您這樣做，請等到 forfeitTimeout 時間，讓所有參與者都有機會，將相符項目之前強制執行結果。 這可讓您調整視窗中，而不需要更新您的標題聯賽的遊戲模式的變更。

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>比對的長度超過 arbitrationTimeout

設定的值**arbitrationTimeout**必須大於最大的相符項目可能需要花費的時間長度。 也就是說，您的標題可能功能的模式的可能相符項目長度會很長，或在其中沒有任何上限。 在這些情況下，您考慮：

* 具有固定的最大的時間長度的唯一模式限制聯賽播放。
* 通知的時間限制，使用者和報告的結果之前**arbitrationTimeout**根據中比對的決勝局。

因為**arbitrationTimeout**到期將會觸發自動結果比對，千萬別等到完整**arbitrationTimeout**期間回報的標題中的結果之前。  相反地，建置時間緩衝區中 (例如**arbitrationTimeout** -1000 毫秒)，或使用不同的值來控制的最大相符項目長度。

#### <a name="other-cases"></a>其他情況下

其他失敗情況下，一般指引是通知錯誤的使用者，並返回 Xbox 競技場 UI 中的使用者。

## <a name="experience-requirements-and-best-practices"></a>體驗需求和最佳做法

Arena 只會顯示在標題的個別的相符項目，因為可以經過一段時間引入新的格式，而不需要更新您的標題。 因此格式化整合的標題必須簡單且有足夠的彈性，才能使用多個競賽競技場的基準使用者體驗。

（選擇性） 您可以使用您的標題中的聯賽中樞的資料，讓比賽中的遊戲體驗更加整合的一部分。  這項功能可以 'xbox::services::tournaments' 下找到。  您必須能夠使用這些 Api:
* 在您的標題 （聯賽名稱、 小組名稱等等） 的 UI 中的 surface 聯賽詳細資料
* 提供探索聯賽中您的標題
* 讓使用者在您競技場相符項目之間的標題

## <a name="configuring-a-title-for-arena"></a>設定標題的競技場

若要啟用競技場的標題，一些額外的步驟時所需設定在 Xbox 開發人員入口網站 (XDP) 或[合作夥伴中心](https://partner.microsoft.com/dashboard)。

### <a name="enabling-arena-for-your-title"></a>啟用競技場標題

若要啟用競技場，請移至 [服務組態] 頁面中 XDP 標題並選取 '競技場'。

![](../../images/arena/arena-configure-xdp.png)

在這裡，您會擁有數個選項：

* **Arena 已啟用**– 選取此核取方塊，以啟用此沙箱的競爭。
* **Arena 功能**– 本章節包含核取方塊以啟用使用者產生的比賽，在沙箱中，並實現跨播放，這可讓使用者在參與相同的比賽的多個平台。
* **Arena 平台**– 可讓您選取的平台，可播放聯賽標題。
* **聯賽資產**– （前身為中的 「 多人遊戲與配對 」 一節。）這些是您的標題的聯賽映像。

也可以在合作夥伴中心中啟用競技場**聯賽**Xbox Live 服務下的功能表。

![在合作夥伴中心內的競技場功能表](../../images/arena/Arena_On_WDC.JPG)

您必須將發佈服務組態，您的變更才會生效。 透過 UDC 目前不支援自助競技場組態。 如果您使用 UDC 服務組態，可與您開發帳戶 Manager 上架 Arena 使用。

### <a name="setting-up-game-modes"></a>設定遊戲模式

遊戲模式是 Xbox Live 的功能，可讓發行者預先設定的聯賽相符項目。 這些遊戲模式可在聯賽建立時設定規則強制執行的 Xbox Live 和聯賽的標題。 標題發行者上，您必須至少一個遊戲模式，以啟用使用者產生的比賽，針對您的標題。 遊戲模式的項目包括：

#### <a name="supports-ugt"></a>支援 UGT 嗎？

遊戲模式可與使用者產生的比賽 (UGTs) 搭配使用。 如果您的標題設定為支援 UGT，您可以標示遊戲模式，以便他們可以選取 Xbox Live 的使用者建立標題的比賽時。

#### <a name="team-size"></a>小組規模

這是可以參與小組聯賽的使用者數目。 針對單一使用者職稱或聯賽中，遊戲模式會有一個小組大小。 Arena 目前不支援變數團隊規模比賽。

#### <a name="forfeittimeout"></a>forfeitTimeout

這之後的毫秒數，開始比對的相符項目會取消，如果播放程式顯示為它之前的時間 （亦即，如果沒有玩家會設定本身為作用中工作階段中）。 您可以將此設為至少夠久的時間來啟動您的標題，並從他們會看到快顯通知的時間登入聯賽相符項目播放程式的時間量。

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

這之後的毫秒數，開始比對的時間之後, 將接受更多結果。 將它設定為 forfeit 逾時加上可能花費的時間長度超過最長相符項目。 如此一來，即使播放程式會開始播放 forfeit 時間之前，他們仍具有足夠的時間才能完成比對。 如果不會報告結果的時間**arbitrationTimeout**，遺失的所有相符項目的參與者接收。 Xbox 競技場也會等候直到**arbitrationTimeout**所有作用中的成員開始仲裁之前的報告結果。

這個逾時值會做為防護網，萬一發生錯誤，而且播放程式未回報的結果。 而不是停止整個聯賽，這個逾時值，請放聯賽會等候的時間量上限。 基於這個理由，它不應該設定太高，因為它會判斷每個聯賽捨入的最大長度。

#### <a name="name"></a>名稱

這是遊戲模式的使用者對象名稱。 這個值是可當地語系化。

#### <a name="description"></a>描述

這是使用者面向描述遊戲的模式。 這個值是可當地語系化。

#### <a name="custom"></a>自訂

**自訂**區段的遊戲模式是開發人員可以在該處插入聯賽比對任何特定標題的組態設定的屬性包。 [自訂] 區段中定義的值會寫入到相符項目 MPSD 工作階段維持`/properties/custom/`。

目前，遊戲模式安裝程式不支援透過 XDP （簡稱 udc）。 若要建立標題的遊戲模式，請連絡您的開發人員帳戶管理員。

### <a name="requirements-for-the-session-template"></a>如需工作階段的範本的需求

為標題的開發人員，您必須提供工作階段範本 收件者使用相符的項目中建立工作階段時。 有一些內容的範本的預期動作。

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

不過您想要可以設定此處未顯示其他屬性。

Arena 工作階段永遠是第 1 版。 設定**inviteProtocol**為了"tournamentGame 」 可讓比對已準備好傳送通知給聯賽參與者。 **memberInitialization**設為 null，以停用 QoS。 必須設定的遊戲功能，因為工作階段代表相符項目，以及仲裁功能所需的結果報告。 **大型**，**廣播**，並**blockBadMsaReputation**必須停用功能，因為它們會干擾工作階段的作業。

您的標題可以自訂範本，請使用範本的所有比賽的固定都值的設定區段中指定它自己的設定。 範例如下。

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

最後， **maxMembersCount**就必須進行系統設定。 請將它設定為 可以玩聯賽相符項目中的播放程式總數。 玩家人數可以進一步限制由遊戲模式設定，因此請確定工作階段中設定的值代表最高支援標題的播放程式總數。 例如，如果相符項目，支援您的遊戲的玩家的最大數目為 5 的 vs。5，設定**maxMembersCount**為 10。 此 MPSD 範本可以用來播放任何數目的相符項目支援最多**maxMembersCount**。
