---
title: 探索 Xbox 比賽
description: 了解如何建立您的遊戲聯賽探索的 UI。
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 競技場，聯賽、 ux
ms.localizationpriority: medium
ms.openlocfilehash: 71a065d6d4da290f2ce3345c7da0026d4c116335
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632473"
---
# <a name="discovering-xbox-tournaments"></a>探索 Xbox 比賽

玩家有多個選項在 Xbox 儀表板或 Xbox 應用程式在 PC 上，若要深入了解參與，或觀賞聯賽。 也有機會增加聯賽探索從您的標題中。  

玩家可以找到的競技場啟用的比賽：

* 從 Xbox 瑳中開啟最近播放的遊戲聯賽中樞。
* 瀏覽聯賽播放、 現正播放，或註冊的播放程式設定檔。
* 他們隸屬於回應 club 張貼的群組 (LFG) 小組。
* 監看聯賽中的資料流 Mixer 應用程式、 網站或監看式樞紐中遊戲中樞/競技場。
* 瀏覽中 Xbox 的遊戲的中樞。

## <a name="promoting-tournaments-in-game"></a>升級聯賽中遊戲

人們觀看比賽，因為他們感興趣的標題。 Xbox 遊戲都有獨一無二的先機，來攔截玩家他們要尋找新的方法，以擴充其遊戲體驗的重要時刻。

> [!TIP]
> **UX 建議：** 升級決策時刻的比賽。
>
> 當玩家必須決定是否要保留播放或結束 （例如，在遊戲啟動、 主功能表中，多人遊戲 功能表中或比對的結尾），有時引發聯賽的感知。

### <a name="1--use-a-dynamic-announcement-feature-example-message-of-the-day"></a>1.使用動態公告功能 (範例：在一天的訊息）

標題有內建宣告功能其遊戲的 UI 通常會使用允許立即更新的內容管理系統。 這可用於將新的內容推送至玩家，完全不用仰賴內容更新或內容的可下載版本。 比方說，最後一戰使用介面的社群公告和秘訣 」 訊息的一天 」 功能。 聯賽升級是最適合此案例。

**Dopad na uživatele**

* 玩家啟動遊戲的工作階段之前引入的新具競爭力的遊戲的機會。
* 風險是間歇性 （發行時，結合其他聲明）。
* 若要深入了解，玩家會被導向到離開遊戲，並移至競技場中樞。
* 此功能不需要進行內容更新來填入。
* 相關性和促銷活動的時間會保持彈性。

###### <a name="ui-example-message-of-the-day"></a>UI 範例：「 一天的訊息 」

![當日聯賽訊息](../../images/arena/arena-ux-motd.png)

#### <a name="a-main-heading-ex-play-watch-learn"></a>A. 主要標題，例如：播放、 觀看、 學習  

告知所有比賽還是特定的玩家。

#### <a name="b-go-to-arena-hub"></a>B. 請移至競技場中樞  

深層連結至遊戲中樞/聯賽-建議您升級您的遊戲相關的所有比賽時。  
-或者-  
深層連結至遊戲中樞/聯賽/詳細資料-時宣告特定的聯賽，建議使用。

#### <a name="c-game-exit-confirmation"></a>C. 遊戲結束確認

螢幕項目 A 和 B 會導致將使用者登出遊戲 Xbox 儀表板。 設定該預期玩家會讓之前的項目的 UI 中的決策。

###### <a name="ui-example-exit-game-confirmation"></a>UI 範例：結束遊戲的確認
![聯賽結束遊戲確認](../../images/arena/arena-ux-exit-confirm.png)

### <a name="2--promote-tournaments-on-the-main-menu"></a>2.升級主功能表上的比賽

在此範例中，宣告會是互動式廣告的即將發生的事件。 它會提供足夠的資訊來吸引玩家若要深入了。 在 **選取**玩家引導競技場中樞，讓他們可以管理其註冊的比賽詳細資料頁面。

###### <a name="ui-example-a-tournament-ad-displayed-alongside-the-main-menu"></a>UI 範例：顯示在主功能表與聯賽 ad

![](../../images/arena/arena-ux-promo.png)

> [!TIP]
> **UX 建議**  
> 公告應包含剛好足夠的詳細資料，以協助玩家決定他們是否想要深入了。 例如，***描述***，***熱門程度***，並***計時***。

## <a name="browsing-tournaments-in-game"></a>瀏覽聯賽中遊戲

Arena Api 提供足夠的資料來建立遊戲內的瀏覽經驗標題。 如果您想要建置瀏覽功能，新增**聯賽**多人遊戲的一節中的進入點。

> [!TIP]
> **UX 建議**  
> 做為多人遊戲的遊戲模式呈現比賽。  
> 因為 Xbox Arena 是一項新功能，請務必保持可見第 1 層或第 2 層級。

###### <a name="ui-example-tournaments-listed-as-an-additional-mode-in-the-multiplayer-section"></a>UI 範例：做為額外的模式，多人遊戲的一節中所列的比賽

![聯賽模式](../../images/arena/arena-ux-tournament-mode.png)

### <a name="provide-a-comprehensive-list-of-tournaments"></a>提供完整的比賽清單

讓玩家快速檢查的比賽中，其目前註冊狀態，並瀏覽遊戲工作階段之間的比賽。

#### <a name="user-impact"></a>Dopad na uživatele

* 離開檢查聯賽狀態儀表板中的遊戲的玩家的需要降至最低。
* 當手寫筆玩家錯過 Xbox 競技場快顯通知，可做為絕佳的備份解決方案。
* 未包含額外的費用，遊戲的開發人員管理。
* 玩家通往競技場 UI 來聯結，註冊，簽入，植入和小組的資訊。

> [!NOTE]  
> Arena Api 不會提供查詢使用者產生的比賽 （產生俱樂部）。


> [!TIP]
> **UX 建議**  
> 建立可以調整，並提供方法來管理大型的比賽清單的玩家的 UI。

### <a name="filters"></a>篩選器

篩選由比賽**所有**， **Active** （其中涵蓋了所有項目直到聯賽結束）， **Canceled**，以及**已完成**狀態，而比賽玩家有或將參與 （例如，我聯賽）。

###### <a name="ui-example"></a>UI 範例：

![聯賽 [篩選] 畫面](../../images/arena/arena-ux-filters.png)

> [!NOTE]  
> 加入自訂的篩選，外部 API 的支援是可行的*但不是建議使用*。

之後，它提出要求，但它無法將查詢參數上指定其他屬性可以篩選您的標題。 這可讓這種類型的 UI 篩選依不可靠的其他內容。 Arena Api 一次擷取結果的標題指定編碼數目 — 比方說，指定標題可能**maxItems** 10 個比賽的值。 這可協助您管理效能的標題，而吃不消大型清單與使用者的風險降至最低。 不過，您的遊戲所提供的任何自訂篩選器會僅適用於要求在該時間的 10 個項目。 這可能導致不一致的已篩選的結果數目之後每個 API 呼叫。 比方說，第 10 個比賽傳回一組可能包含 2 相關自訂篩選器，例如 '遊戲'。 但是，接下來的 10 可能顯示 9 這類項目，依此類推。 您完全填入每個自訂的篩選條件的標題的唯一可靠方式就是要求每個作用中的聯賽，然後再篩選它們全部，因此我們不建議。

#### <a name="filter-recommendations"></a>篩選建議：

##### <a name="all"></a>ALL

檢視所有**Active**， **Canceled**，並**已完成**比賽，依照最新。

結果：

* 聯賽狀態
* 聯賽開始日期
* 聯賽狀態 — 比方說，註冊開放
* 在儀表板聯賽詳細資料頁面的深層連結

##### <a name="my-tournaments"></a>我的比賽

檢視比賽中目前註冊的參與者。

結果：

* 目前已登入的使用者參與的比賽
* 聯賽狀態
* 輸入聯賽相符項目 （適用於 [比對準備就緒] 狀態） 的方法
* 在儀表板的聯賽詳細資料頁面的深層連結

##### <a name="active"></a>使用中

檢視已建立的所有比賽： 近期、 進行註冊，簽入和播放。

結果：

* 已開啟，以註冊，目前的使用者不具有已加入的比賽
* 報名截止前的剩餘時間

##### <a name="completedcanceled"></a>完成/已取消

檢視最近的結果。 對於剛結束的比賽可以經過一段時間，而不是只需消失完成之後過期。

結果：

* 在儀表板的聯賽詳細資料頁面的深層連結
* 聯賽狀態
* 結束日期/時間

##### <a name="canceled"></a>已取消

檢視已取消的比賽。

結果：

* 在儀表板的聯賽詳細資料頁面的深層連結
* 取消作業的日期/時間。

> [!div class="nextstepaction"]
> [藉由使用競技場 UI 聯結聯賽](arena-ux-join-tournament.md)
