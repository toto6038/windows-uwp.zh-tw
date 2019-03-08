---
title: Xbox Live 服務組態
description: 了解如何設定您的遊戲的 Xbox Live 服務。
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 服務組態
ms.localizationpriority: medium
ms.openlocfilehash: d12c66e61a189c13ddbcd96dd99caa351206ecf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640903"
---
# <a name="xbox-live-service-configuration"></a>Xbox Live 服務組態

## <a name="what-is-service-configuration"></a>什麼是服務組態？

您可能熟悉的一些 Xbox Live 功能這類[成就](achievements-2017/achievements.md)，[排行榜](leaderboards-and-stats-2017/leaderboards.md)並[配對](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking)。

如果您不這樣做，我們將簡短說明排行榜做為範例。 排行榜讓玩家能看到值，表示相較於其他玩家的成就。  比方說高分數的 arcade 遊戲、 單圈時間在賽車遊戲中或在第一人稱射擊的致勝。 但不同於 arcade 機器只顯示最高的分數，從該實體機器已播放的球員，在 Xbox live 就可以顯示從世界各地的最高得分。

但針對此動作，您需要執行一些一次性設定，以便 Xbox Live 知道您排行榜。 比方說的值是否應該以遞增或遞減值，排序和資料的哪個組件它應該會排序。

此設定會在[合作夥伴中心](https://partner.microsoft.com/dashboard)大部分的情況。 但某些開發人員會使用[Xbox 開發人員入口網站 (XDP)](https://xdp.xboxlive.com)。

如果您是開發您的標題為 Xbox Live 創作者計劃的一部分，您會使用[合作夥伴中心](https://partner.microsoft.com/dashboard)，您可以閱讀[開始啟動與 Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md)以了解如何設定。

如果您是ID@Xbox開發人員或工作與 「 發行者 」，是 Microsoft 合作夥伴，請閱讀上。

## <a name="choose-your-development-portal"></a>選擇您的開發入口網站

如先前所述，有兩個不同的入口網站，可用來設定 Xbox Live 服務。 在合作夥伴中心[ https://partner.microsoft.com/dashboard ](https://partner.microsoft.com/dashboard)並在 Xbox 開發入口網站 (XDP) [ https://xdp.xboxlive.com ](https://xdp.xboxlive.com)。

合作夥伴中心建議從現在開始，所有的項目，但某些功能，您仍然可以使用 XDP。 本節將協助建議您設定您的標題的位置。

您可以根據您所選擇的入口網站中找到特定的服務設定頁面的相關資訊：

* [合作夥伴中心設定](configure-xbl/windows-dev-center.md)
* [Xbox 開發入口網站設定](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration)若要存取此連結，您必須擁有 Microsoft 帳戶 (MSA)，已啟用完整的 Xbox Live 的存取權。

如果您已經設定的標題，您可以向下捲動至[取得您的識別碼](#get_ids)以了解如何取得設定您的標題所需的各種識別項。

### <a name="pcmobile-uwp-game-only"></a>只有電腦/Mobile UWP 遊戲
合作夥伴中心建議的設定和管理在 Windows 10 電腦和/或 Windows 10 行動裝置執行的 UWP 遊戲。

#### <a name="using-xdp-to-configure-uwp-titles"></a>若要設定 UWP 標題使用 XDP

若要使用 XDP 設定 UWP 標題，如果您有下列需求的其中一個：

1. 您使用的競爭。
2. 您有現有的使用者、 群組和權限安裝 XDP 您想要繼續使用。
3. 您正在使用只能在 XDP 聯賽工具或多人遊戲工作階段目錄的工作階段歷程記錄檢視器等工具。
4. 您正在開發將有 Xbox One XDK 型遊戲與下列比賽相同 UWP PC/行動版本之間的跨平台播放的標題。

如果您不屬於其中一種類別，您應該使用合作夥伴中心。 否則您可以看到下列如何使用 XDP 設定 UWP 標題。

若要設定的 UWP 應用程式的 Xbox Live 服務中使用 XDP 有幾個重要的注意事項：

* **一旦憑證/retail XDP 中發行遊戲的 Xbox Live 服務組態，還有不返回 ！** Xbox Live 服務組態，遊戲必須留在 XDP 中生命週期的遊戲的標題。
* **合作夥伴中心從 XDP 沒有移轉路徑。** 如果您在 XDP 啟動您的 Xbox Live 設定，您必須手動重新建立它在合作夥伴中心如果您想要移動它。

基於這些兩個考量，我們建議使用合作夥伴中心 PC/行動遊戲，除非您歸類到其中一個上面所述的類別。

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>跨播放之間 Xbox One 和 PC/行動遊戲 ###
Xbox One 和電腦，又稱為跨播放之間的跨裝置遊戲是展示 Windows 10 體驗。 在此案例中，在 Xbox One 和 PC 版本的遊戲會共用相同的 Xbox Live 設定。

此案例中也涵蓋了您有現有的 Xbox One XDK 的遊戲，並想要新增跨播放遊戲的 UWP 版本支援的情況。

若要實作跨播放，請執行下列作業：

* **若要設定及發佈您的 XDK 遊戲使用 XDP。** 合作夥伴中心不支援這一次的 Xbox One XDK 的遊戲。
* **使用單一 Xbox Live 服務組態，您在中建立 XDP XDK 及遊戲的 UWP 版本。** XDP 現在有新的功能，可讓共用單一 Xbox Live 服務組態的 XDK 版本和 UWP 版本之間的遊戲的遊戲。
* **使用合作夥伴中心擷取，並發行您的 UWP 遊戲。** 不過，請勿設定 Xbox Live 服務使用合作夥伴中心，因為您的遊戲將會使用您在 XDP 中建立的服務組態。
* **不分割 XDP 與合作夥伴中心之間的 Xbox Live 服務組態。** XDP 」 和 「 合作夥伴中心並不知道彼此，並發行一個來源的服務組態會覆寫從其他來源發行組態。 這可能會造成使用者資料會遺失 （遺漏的成就，它們的遊戲儲存等） 可以建立不正確的使用者體驗。 基於這個理由，**我們所需的 100%的 Xbox Live 服務組態為了在 XDP 跨 play XDK + UWP 遊戲。**

此程序，包括它們的項目中沒有更多詳細資料*未*自助服務中[開始使用跨玩遊戲](get-started-with-partner/get-started-with-cross-play-games.md)指南

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>不同版本的 Xbox One 和 PC/行動遊戲，並不是跨 play
您可以決定區隔您的遊戲的 Xbox One 版本與下列比賽相同的電腦/行動版本。 在此情況下，您會建立兩項個別產品，並只分別依照指引 Xbox One XDK 只和 PC/Mobile UWP 遊戲。

您無法使用這兩個版本相同的服務組態在此情況下，而且您必須以手動方式建立的遊戲，每個不同版本的服務組態 XDP 中或在合作夥伴中心適當。

<a name="get_ids"></a>

## <a name="get-your-ids"></a>取得您的識別碼

若要啟用 Xbox Live 服務，您必須取得數個識別碼，以設定您的開發套件和程式的標題。 這些可以藉由 Xbox Live 服務組態取得。

如果您目前沒有 XDP 或合作夥伴中心中的標題，請參閱上一節[Xbox Live 服務設定入口網站](#xbox_live_portals)指導方針。

### <a name="critical-ids"></a>重要識別碼

有三個識別碼是很重要的標題和 Xbox one 的應用程式開發： 沙箱識別碼、 標題識別碼和 SCID。

需要有使用一種開發套件的沙箱識別碼時，標題的識別碼和 SCID 不需要進行初始開發，但所需的任何使用 Xbox Live 服務。 因此，建議您取得所有的三次。

#### <a name="sandbox-ids"></a>沙箱識別碼

沙箱會提供您的開發套件在開發期間，確保您擁有全新的環境來開發和測試您的標題內容隔離。 沙箱識別碼會識別您的沙箱。 雖然多個主控台可存取一個沙箱主控台只可以在任何時候，存取一個沙箱。

沙箱識別碼會區分大小寫。

**合作夥伴中心**

如果您要設定您的標題在合作夥伴中心，您會取得您的沙箱識別碼"Xbox Live"根組態 頁面上，如下所示：

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

如果您要設定您的標題 XDP 上，您取得您的沙箱識別碼概觀 頁面上為您的產品，如下所示：

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>服務設定識別碼 (SCID)

在開發中，您將會建立事件、 成積、 和其他線上功能的主機。 這些都是屬於您的服務組態，並且需要存取 SCID。

SCIDs 會區分大小寫。

**合作夥伴中心**

若要擷取您 SCID 在合作夥伴中心內，瀏覽至 Xbox Live 服務區段，並移至*Xbox Live 設定*。 您 SCID 會顯示在資料表如下所示：

![](images/getting_started/devcenter_scid.png)

**XDP**

若要擷取您 SCID XDP 上的，瀏覽您標題底下的 [產品 Setup] 區段，您會看到的標題識別碼 」 和 「 SCID。

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>標題的識別碼

標題的識別碼可唯一識別您的 Xbox Live 服務的標題。 它用在整個服務來讓使用者存取項目的即時內容，他們的使用者統計資料、 成積、 和等，並啟用即時多人遊戲的功能。

標題的識別碼可以是區分大小寫，根據使用方式和位置。

**合作夥伴中心**

在合作夥伴中心您標題的識別碼位於相同的資料表中 SCID *Xbox Live 設定*頁面。

**XDP**

因為 SCID XDP 上您標題的識別碼被取自相同的位置。

<a name="xbox_live_portals"></a>
