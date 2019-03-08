---
title: 多人連線的工作階段瀏覽
description: 了解如何實作多人連線的工作階段瀏覽使用 Xbox Live 多人連線遊戲。
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.date: 10/16/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 579c71ef9266fb9a1ee4ef0538d1beffec0bb4ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660673"
---
# <a name="multiplayer-session-browse"></a>多人連線的工作階段瀏覽

多人遊戲工作階段瀏覽是 2016 年 11 月推出的新功能，可讓查詢的開啟多人遊戲的遊戲工作階段符合指定的準則清單的標題。

## <a name="what-is-session-browse"></a>什麼是工作階段瀏覽？

在工作階段瀏覽案例中，在遊戲中的播放程式可擷取一份可聯結的遊戲工作階段。 這份清單中的每個工作階段項目包含一些遊戲，玩家可以用來協助他們選取要加入的工作階段有關的其他中繼資料。  它們也可以篩選根據中繼資料的工作階段的清單。 一旦播放器看到需要進行這些遊戲工作階段，它們可以加入工作階段。

播放程式也可以建立新的遊戲工作階段，和以招募而不是依賴配對的其他播放程式中使用工作階段瀏覽。

工作階段瀏覽不同於傳統的配對案例，播放程式自我選取他們想要加入，請在配對中的遊戲工作階段，播放程式通常會按一下嘗試將會自動放在播放程式的 「 尋找遊戲 」 按鈕適當的遊戲工作階段。 手動和速度較慢的程序一律不可選取客觀地最佳的遊戲，工作階段瀏覽時，它會提供更多的控制播放程式，並可視為挑選主觀更棒的遊戲。

通常會在遊戲中包含這兩個工作階段瀏覽和配對案例。 通常配對用於常播放遊戲的模式，而工作階段瀏覽用於自訂的遊戲。

**範例：** John 可能興趣相投，hero 戰役競技場樣式多人連線遊戲，但想要玩遊戲時，所有播放程式其主圖中隨機選取。 他可以擷取一份開啟的遊戲工作階段，並找出包含"隨機 hero 」 描述，或如果遊戲的 UI 可讓它，他可以選取 「 隨機主要 」 遊戲模式，並擷取標記，表示它們是 「 RandomHero"gam 工作階段es。

當他發現他喜歡的遊戲時，他會加入遊戲。 當夠多的人已加入工作階段時，遊戲的工作階段主機可以啟動遊戲。

### <a name="roles"></a>角色

在工作階段瀏覽的遊戲可能要招募針對特定角色的播放程式。 比方說，播放程式可能會想要建立指定的工作階段包含 5 個以上的突擊類別，而必須包含至少 2 個他角色和至少 1 個儲存槽角色遊戲工作階段。

當其他播放器適用於工作階段時，它們可以預先選取其角色和服務將不會允許他們加入此工作階段，如果沒有開啟的位置，所選角色。

另一個範例是播放器想要保留 2 個插槽加入朋友，遊戲都可以指定 「 friends 」 角色，而只是工作階段主機的朋友的播放程式可能會填滿專門用來 「 friends 」 角色的 2 個插槽。

如需有關角色的詳細資訊，請參閱 <<c0> [ 多人遊戲角色](multiplayer-roles.md)。



## <a name="how-does-session-browse-work"></a>工作階段瀏覽如何運作？

工作階段瀏覽主要適用於使用的搜尋控制代碼。 搜尋控制代碼會包含工作階段，以及有關工作階段，也就是搜尋屬性的其他中繼資料參考的資料封包。

當標題建立新的遊戲工作階段適用於工作階段的瀏覽，它會搜尋控制代碼建立工作階段。 搜尋控制代碼會儲存在多人遊戲服務目錄 (MPSD)，以維護標題的搜尋控制代碼。

當需要擷取一份工作階段標題時，標題可以將 MPSD，會傳回符合搜尋準則的搜尋控制代碼清單的搜尋查詢。 標題然後可以使用工作階段的清單，以便對玩家顯示一份可聯結的遊戲。

當工作階段已滿，或否則無法聯結時，標題可以移除搜尋控制代碼 MPSD，讓工作階段將不再會出現在工作階段瀏覽查詢中。

>[!NOTE]
> 搜尋控制代碼的用意在於要呈現給使用者使用時顯示的工作階段的清單。 使用搜尋功能在處理背景配對不有效的並考慮改用[SmartMatch](multiplayer-manager/play-multiplayer-with-matchmaking.md)

## <a name="set-up-a-session-for-session-browse"></a>設定的工作階段的工作階段瀏覽

若要使用搜尋功能在處理工作階段，工作階段必須有下列功能設為 true:

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> `userAuthorizationStyle`功能僅適用於 UWP 遊戲所需，但我們建議您實作它們所有的 Xbox Live 的遊戲，包括 XDK 的遊戲，因為它可確保未來的可攜性。

>[!NOTE]
> 設定`userAuthorizationStyle`功能的預設值`readRestriction`並`joinRestriction`工作階段`local`而不是`none`。 這表示項目必須使用搜尋功能在處理或傳輸加入遊戲工作階段的控制代碼。

當您設定您的 Xbox Live 服務時，您可以在工作階段範本中設定這些功能。

工作階段瀏覽 中，您應該只能建立搜尋控制代碼會用於實際的遊戲，不適用於大廳工作階段的工作階段。

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>是工作階段的擁有者什麼意思？

雖然許多遊戲工作階段類型，例如 SmartMatch 或只遊戲、 朋友不需要擁有者，針對工作階段瀏覽工作階段您可能想要有擁有者。 

具有擁有者管理的工作階段擁有者有一些優點。 擁有者可以從工作階段中，移除其他成員，或變更其他成員的擁有權狀態。

若要使用工作階段的擁有者，工作階段必須有下列功能設為 true:

* `hasOwners`

如果工作階段的擁有者有封鎖的 Xbox Live 的成員，該成員就無法加入此工作階段。

使用時[多人遊戲角色](multiplayer-roles.md)，您可以將其設定，只有擁有者可以指派角色給使用者。

如果所有擁有者保留工作階段，則服務會在工作階段為基礎的動作`ownershipPolicy.migration`定義工作階段的原則。 如果原則是 「 舊 」，則會設定 media player 已在工作階段最長的新擁有者。 如果原則是 「 endsession"（預設值如果不提供），服務就會結束工作階段檔案，並從工作階段中移除所有剩餘的播放程式中。


## <a name="search-handles"></a>搜尋控制代碼

搜尋控制代碼會儲存在 MSPD，做為 JSON 結構。 除了包含工作階段的參考，搜尋功能在處理也會包含搜尋方式，稱為搜尋屬性的其他中繼資料。

工作階段只能有一個為它建立在任何時間的搜尋控制代碼。

若要建立使用 Xbox Live Api 中的工作階段的搜尋控制代碼，您先建立`multiplayer::multiplayer_search_handle_request`物件，並接著傳遞至該物件`multiplayer::multiplayer_service::set_search_handle()`方法。

### <a name="search-attributes"></a>搜尋屬性

搜尋屬性是由下列元件所組成：

`tags` 標籤是字串的描述元，可讓使用者將分類的遊戲工作階段，類似於雜湊標記。 標記必須以字母開頭，不能包含空格，且必須少於 100 個字元。
範例的標記：「 ProRankOnly"，"Norocketlaunchers 」、 「 cityMaps"。

`strings` 字串是文字的變數，以及字串名稱必須以字母開頭，不能包含空格，且必須少於 100 個字元。

字串中繼資料範例："武器"="刀械 + 手槍 + 槍"、"MapName"="UrbanCityAssault 」、 「 描述"="有趣的遊戲，歡迎使用新的人員。 」

`numbers` -數字都是數值的變數，和數字的名稱必須以字母開頭，不能包含空格，且必須少於 100 個字元。 Xbox Live Api 會擷取為 float 類型的數字的值。

範例數字的中繼資料："MinLevel" = 25, "MaxRank" = 10.

>**注意：** 字母大小寫的標籤和字串值會保留在服務中，但您必須使用 tolower （） 函式，當您查詢標記。 這表示標籤和字串值是目前所有被視為較低的情況下，即使它們包含大寫字元。

在 Xbox Live Api 中，您可以透過設定搜尋屬性`set_tags()`， `set_stringsmetadata()`，並`set_numbers_metadata()`方法`multiplayer_search_handle_request`物件。


### <a name="additional-details"></a>其他詳細資料

當您擷取的搜尋控制代碼時，結果也會包含其他有用的資料有關工作階段中，例如，如果工作階段已關閉，是否有任何聯結限制上的工作階段等等。

在 Xbox Live Api，請在這些詳細資料，以及搜尋屬性中，會包含在`multiplayer_search_handle_details`的搜尋查詢之後，會傳回。

### <a name="remove-a-search-handle"></a>移除搜尋控制代碼

當您想要從工作階段瀏覽，例如當工作階段已滿時，移除工作階段或工作階段已關閉，您可以刪除搜尋控制代碼。

您可以使用 Xbox Live Api`multiplayer_service::clear_search_handle()`方法移除搜尋控制代碼。

### <a name="example-create-a-search-handle-with-metadata"></a>範例：建立含有中繼資料的搜尋控制代碼

下列程式碼示範如何使用 c + + Xbox Live 的多人遊戲 Api 的搜尋控制代碼建立工作階段。

```cpp
auto searchHandleReq = multiplayer_search_handle_request(sessionBrowseRef);

searchHandleReq.set_tags(std::vector<string_t> val);
searchHandleReq.set_numbers_metadata(std::unordered_map<string_t, double> metadata);
searchHandleReq.set_strings_metadata(std::unordered_map<string_t, string_t> metadata);

auto result = xboxLiveContext->multiplayer_service().set_search_handle(searchHandleReq)
.then([](xbox_live_result<void> result)
{
  if (result.err())
  {
    // handle error
  }
});
```


## <a name="create-a-search-query-for-sessions"></a>建立工作階段的搜尋查詢

當擷取的搜尋控制代碼清單，您可以使用搜尋查詢，以將結果限制為符合特定準則的工作階段。

搜尋查詢語法[OData](https://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092)樣式語法，與只支援下列運算子：

 運算子 | 描述
 --- | ---
 eq | 等於
 ne | 不等於
 gt | 大於
 ge | 大於或等於
 lt | 小於
 le | 小於或等於
 和 | 邏輯 AND
 或 | 邏輯或者 （請參閱下列附註）

您也可以使用 lambda 運算式和`tolower`標準函式。 目前不支援任何其他 OData 函式。

搜尋的標記或字串值時，您必須使用 'tolower' 函式，在搜尋查詢中，因為服務目前只支援搜尋小寫的字串。

Xbox Live 服務只會傳回前 100 個符合搜尋查詢的結果。 您的遊戲應該讓玩家能精簡其搜尋查詢，如果結果是過於廣泛。

>[!NOTE]
>  邏輯 Or 是查詢中支援篩選的字串;不過只有一個 OR 允許且它必須是根目錄中的查詢。 您不能有多個 Or 在查詢中，也可以建立查詢，可能會導致，或未在頂端的查詢結構的大部分層級。

### <a name="search-handle-query-examples"></a>搜尋控制代碼的查詢範例

在符合 rest 限制的呼叫中，[篩選] 會是您可以在其中指定將對所有的搜尋控制代碼的查詢結果中執行的 OData 篩選語言字串。  
在多人遊戲 2015 Api 中，您可以指定搜尋篩選條件字串*searchFilter*參數`multiplayer_service.get_search_handles()`方法。  

目前支援下列的篩選條件案例：

 篩選依據 | 搜尋篩選條件字串
 --- | ---
 單一成員 xuid '1234566' | "session/memberXuids/any(d:d eq '1234566')"
 單一擁有者 xuid '1234566' | "session/ownerXuids/any(d:d eq '1234566')"
 字串 'forzacarclass' 等於 'classb' | "tolower(strings/forzacarclass) eq 'classb'"
 數字 'forzaskill' 等於 6 | 「 數字/forzaskill eq 6"
 數字 'halokdratio' 大於 1.5 | 「 數字/halokdratio t 1.5"
 標記 'coolpeopleonly' | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 不包含標記 'cursingallowed' 的工作階段 | "tags/any(d:tolower(d) ne 'cursingallowed')"
 工作階段未包含 'rank' 的數目等於 0 | 「 數字/排名 ne 0 」
 不包含字串 'forzacarclass' 的工作階段，等於 'classa' | 「 tolower(strings/forzacarclass) ne 'classa' 」
 標記 'coolpeopleonly' 和 'halokdratio' 等於 7.5 的數字 | "tags/any(d:tolower(d) eq 'coolpeopleonly') eq true and numbers/halokdratio eq 7.5"
 數字 'halodkratio' greater than or equal 1.5，數字 ' rank' 小於 60 和數字 'customnumbervalue' 小於或等於 5 | 「 數字/halokdratio ge 1.5 和數字/排名 l 60 和數字/customnumbervalue le 5 」
 成就 id '123456' | "achievementIds/any(d:d eq '123456')"
 語言代碼 'en' | 「 語言 eq 'en' 」
 排定的時間，會傳回所有的排程時間小於或等於指定的時間 | "session/scheduledTime le '2009-06-15T13:45:30.0900000Z'"
 已張貼的時間、 傳回所有張貼時間小於指定的時間 | "session/postedTime lt '2009-06-15T13:45:30.0900000Z'"
 工作階段的註冊狀態 | "session/registrationState eq 'registered'"
 工作階段的成員數目等於 5 | 「 工作階段/membersCount eq 5 」
 工作階段成員的目標數目大於 1 | 「 工作階段/targetMembersCount l 1 」
 其中最大工作階段成員的計數是小於 3 | 「 工作階段/maxMembersCount lt 3"
 其中的工作階段成員目標計數與工作階段的成員數目之間的差異是小於或等於 5 | 「 工作階段/targetMembersCountRemaining le 5 」
 工作階段成員的最大計數和工作階段的成員數目之間的差異大於 2 | 「 工作階段/maxMembersCountRemaining t 2"
 其中的工作階段成員目標計數與工作階段的成員數目之間的差異是小於或等於 15。</br> 如果角色並沒有指定的目標，然後此查詢會篩選針對工作階段成員的最大計數和工作階段的成員數目之間的差異。 | 「 工作階段/需要 le 15 」
 「 確認 」 與該角色的成員數目等於 5 角色類型 「 lfg 」 的角色 | "session/roles/lfg/confirmed/count eq 5"
 「 確認 」 的情況下，該角色的目標是大於 1 的地方角色類型 「 lfg 」 的角色。</br> 如果角色並沒有指定的目標，則會改為使用角色的最大值。 | 「 工作階段/角色/lfg/確認/目標 l 1 」
 角色 [已確認] 角色的輸入 「 lfg 」 角色的目標和與該角色的成員數目之間的差異所在小於或等於 15。</br> 如果角色並沒有指定的目標，然後此查詢會篩選針對角色的最大值和與該角色的成員數目之間的差異。 | 「 工作階段/角色/lfg/確認/需求 le 15 」
 指向包含特定關鍵字的工作階段的所有搜尋控制代碼 | "session/keywords/any(d:tolower(d) eq 'level2')"
 指向 屬於特定 scid 工作階段的所有搜尋控制代碼 | 「 工作階段/scid eq '151512315' 」
 使用特定的範本名稱的工作階段所指向的所有搜尋控制代碼 | "session/templateName eq 'mytemplate1'"
 所有的搜尋控制代碼已標記為 'elite' 或 '槍枝' 大於 15 的數字和字串 '氏族' 等於 '紫色' | "tags/any(a:tolower(a) eq 'elite') or number/guns gt 15 and string/clan eq 'purple'"

### <a name="refreshing-search-results"></a>重新整理的搜尋結果

 您的遊戲應該避免自動重新整理工作階段的清單，而是提供 UI，可讓播放程式 （可能是在之後精簡其搜尋準則以進一步篩選結果），以手動方式重新整理清單。

 如果 media player 會嘗試加入工作階段，但該工作階段為完整或已關閉，您的遊戲應該重新整理的搜尋結果。

 太多搜尋重新整理可能會導致服務節流，因此您的標題應該限制的查詢可以重新整理的速率。

 若要減少服務通話量，搜尋控制代碼會包含自訂的工作階段屬性可用來儲存和查詢快速變更的工作階段屬性。 這類屬性不應該儲存在搜尋屬性。

### <a name="example-query-for-search-handles"></a>範例： 查詢搜尋控制代碼

 下列程式碼示範如何查詢搜尋控制代碼。 此 API 傳回的集合`multiplayer_search_handle_details`代表所有符合查詢的搜尋控制代碼的物件。

```cpp
 auto result = multiplayer_service().get_search_handles(scid, template, orderBy, orderAscending, searchFilter)
 .then([](xbox_live_result<std::vector<multiplayer_search_handle_details>> result)
 {
   if (result.err())
   {
      // handle error
   }
   else
   {
      // parse result.payload
   }
 });

 /* Payload element properties

 multiplayer_search_handle_details
 {
   string_t& handle_id();
   multiplayer_session_reference& session_reference();
   std::vector<string_t>& session_owner_xbox_user_ids();
   std::vector<string_t>& tags();
   std::unordered_map<string_t, double>& numbers_metadata();
   std::unordered_map<string_t, string_t>& strings_metadata();
   std::unordered_map<string_t, multiplayer_role_type>& role_types();
 }
 */
```

## <a name="join-a-session-by-using-a-search-handle"></a>加入工作階段使用的搜尋控制代碼

一旦您有您想要加入的工作階段所擷取的搜尋控制代碼，應該使用標題`MultiplayerService::WriteSessionByHandleAsync()`或`multiplayer_service::write_session_by_handle()`將本身新增至階段。

> [!NOTE]
> `WriteSessionAsync()`和`write_session()`方法不能用來加入工作階段瀏覽工作階段。

下列程式碼示範如何加入工作階段之後擷取的搜尋控制代碼。

```cpp
void Sample::BrowseSearchHandles()
{
    auto context = m_liveResources->GetLiveContext();
    context->multiplayer_service().get_search_handles(...)
    .then([this](xbox_live_result<std::vector<multiplayer_search_handle_details>> searchHandles)
    {
        if (searchHandles.err())
        {
            LogErrorFormat( L"BrowseSearchHandles failed: %S\n", searchHandles.err_message().c_str() );
        }
        else
        {
            m_searchHandles = searchHandles.payload();

            // Join the game session

            auto handleId = m_searchHandles.at(0).handle_id();
            auto sessionRef = multiplayer_session_reference(m_searchHandles.at(0).session_reference());
            auto gameSession = std::make_shared<multiplayer_session>(m_liveResources->GetLiveContext()->xbox_live_user_id(), sessionRef);
            gameSession->join(web::json::value::null(), false, false, false);

            context->multiplayer_service().write_session_by_handle(gameSession, multiplayer_session_write_mode::update_existing, handleId)
            .then([this, sessionRef](xbox_live_result<std::shared_ptr<multiplayer_session>> writeResult)
            {
                if (!writeResult.err())
                {
                    // Join the game session via MPM
                    m_multiplayerManager->join_game(sessionRef.session_name(), sessionRef.session_template_name());
                }
            });
        }
    });
}
```
