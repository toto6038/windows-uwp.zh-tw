---
title: Xbox 整合多人遊戲
description: 了解 Xbox 整合式多人遊戲 (XIM)，適用於 Xbox Live 遊戲的多合一多人遊戲/網路功能/聊天解決方案。
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.date: 01/25/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 xbox 整合式多人遊戲
localizationpriority: medium
ms.openlocfilehash: aa82b1042f710d81d83b98767802c7538fd53fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641773"
---
# <a name="xbox-integrated-multiplayer-xim"></a>Xbox 整合式多人遊戲 (XIM)

- [概觀](#overview)
- [概念](#concepts)
- [功能](#features)
- [關聯性的其他模組](#relationship-to-other-modules)

## <a name="overview"></a>概觀

Xbox 整合式多人遊戲 (XIM) 是獨立的介面，可透過 Xbox Live 服務的能力輕鬆地將多人即時網路功能與聊天通訊新增至遊戲。 XIM 介面不需要的專案，選擇 編譯以 C + + /CX 與傳統的 c + +;它可以搭配使用。 實作也不會擲回例外狀況做為報告，所以您可以使用它輕鬆地從無例外狀況的專案如果慣用的非嚴重錯誤。

若要開始，請參閱[使用 XIM](xbox-integrated-multiplayer/using-xim.md)。 如果您使用C#，請參閱[使用 XIM C# ](xbox-integrated-multiplayer/using-xim-cs.md)。

## <a name="concepts"></a>概念

XIM 會導向幾個重要概念：

- XIM 網路-一組互連式使用者參與特定的多人遊戲體驗，以及描述該集合的基本狀態的邏輯表示法。 參與者一次只能在一個 XIM 網路中，但可以在概念性 XIM 網路間順暢地移動。
- 配對-探索其他類似的興趣或技能層級參與 XIM 網路，而不需要現有的社交關聯性的遠端播放程式的選擇性程序。
- 查詢-而不需要現有的社交關聯性，參與者之間，網路探索 XIM 的選擇性程序。
- `xim_player` 層表示個別的人類使用者，本機或遠端裝置上登入，並參與 XIM 網路的物件。 加入、離開、然後重新加入相同 XIM 網路的單一實體使用者會被視為兩個獨立玩家執行個體。
- `xim_state_change` -一種結構，代表 XIM 網路的本機裝置，在某些方面的非同步變更有關的通知。
- `xim::start_processing_state_changes` 並`xim::finish_processing_state_changes`-對方法，由應用程式來執行非同步作業，以擷取的形式處理結果的每個 UI 畫面格呼叫`xim_state_change`結構，然後釋放相關聯的資源時完成。

在極高的層級，遊戲的應用程式會使用 XIM 文件庫設定的使用者登入的本機裝置上要移至 XIM 網路上，為新的播放程式。 應用程式呼叫`xim::start_processing_state_changes`和`xim::finish_processing_state_changes`每個 UI 畫面格。 在遠端裝置上的應用程式執行個體新增到 XIM 網路的使用者，提供每個參與的執行個體`xim_state_change`描述本機和遠端的更新`xim_player`s 加入該 XIM 網路。 當玩家停止參與 XIM 網路（正常結束或因為網路連線性問題），會提供`xim_state_change`更新給應用程式執行個體，指出`xim_player`已離開。

應用程式可以判斷要透過數種方式參與的 XIM 網路。 通常應用程式一開始會自動將本機使用者移到使用者好友可用的新網路，本機使用者可以傳送邀請或啟用 XIM 網路做為可加入的活動探索（例如透過玩家卡）。 這些社交探索使用者準備好之後，應用程式會啟始 Xbox Live「配對」程序，並將所有玩家移到新的 XIM 網路，該網路也包含額外「符合」的遠端玩家，視需要填寫團隊/對手清單。 然後，該多人遊戲體驗完成時，應用程式執行個體會將其本機玩家 (以及選擇性的原始配對前遠端玩家)，移回到新的私人 XIM 網路，或透過配對的另一個隨機 XIM 網路。 在整個過程中，語音和文字交談維持使用。 玩家在 XIM 網路間輕鬆移動，是 API 的核心，反映良好、高度社交遊戲體驗的現代預期。

而不是用戶端伺服器模型 XIM 網路在邏輯上完全連接網狀結構的對等裝置。 本文件章節中所述，任何播放程式可以直接傳送至任何其他透過 API。 任何參與的裝置可以叫用會影響整體 XIM 網路狀態的所有方法。

如果應用程式不會妨礙有效地同時修改相同的 XIM 網路狀態的多個參與者，XIM 就會使用簡單的最後一個寫入 wins 衝突解決。 這表示 XIM 不會造成任何角色的概念為 「 主機 」 或 「 伺服器 」。 XIM 也不會限制應用程式使用自己的概念，例如將應用程式定義角色移轉到另一個參與者等玩家要離開 XIM 網路時的支援。

## <a name="features"></a>功能

- 遊戲提供語音和文字的聊天通訊，會遵守並採用 player 隱私權設定

    若隱私權設定和應用程式設定允許，也會提供所有玩家之間的語音和文字交談通訊。 如果玩家已經啟用語音轉換文字或文字轉換語音，XIM 會透明地執行此轉換，分別為傳入的語音音訊傳遞交談文字訊息，並為傳出的交談文字訊息播放合成語音。

- 可讓傳送遊戲專屬資料訊息的遊戲

    在 XIM 網路中，應用程式可以傳送自己的遊戲特定資料訊息，例如虛擬人偶動作更新。 所有收到的訊息都會傳遞至應用程式做為`xim_state_change`，指示預定的來源和本機的目的地。

- 函式做為專用的對談的解決方案透過頻外保留項目

    如需有關透過頻外保留使用 XIM 的詳細文件，請參閱[XIM 保留](xbox-integrated-multiplayer/xim-reservations.md)。

- 例外狀況，且可以搭配 C + + /CX 或傳統的 c + +

## <a name="relationship-to-other-modules"></a>關聯性的其他模組

XIM 的設計目的是提供方便、多合一遊戲介面，符合基本多人遊戲需求。 它封裝數個模組的功能--尤其是 Xbox 服務 API (XSAPI)`multiplayer_manager`模組、`xbox::services::game_chat_2`程式庫和`Windows::Networking::XboxLive`安全多人遊戲網路--成為單一簡化的 API。 建置不需要絕對最大彈性或控制項的多人遊戲時，這可以減少一般相關的程式碼、工作和概念數量。 不過，需求不符合 XIM 簡化假設的應用程式，可能會想要改為直接使用這些元件。

雖然 XIM 旨在免除透過基礎元件管理像是多人遊戲工作階段目錄 (MPSD) 工作階段文件或網路傳輸等項目，它不會防止這些元件同時/並排使用為個別玩家名單或通訊網格的一部分。 在此情況下，確保 XIM 和其機制之間合作網路資源使用，是應用程式的責任。 XIM 目前支援「頻外保留」，以便使用做為使用者清單僅由外部輸入所驅動的專用聊天解決方案。

Xbox Live 提供對多人遊戲有價值的許多其他功能，但和設定多人聊天和網路通訊較不直接相關，因此不包裝在這個模組中。 尋找 player 成就、 排行榜、 儲存體及更多的 Xbox 服務 API (XSAPI)，建議應用程式。
