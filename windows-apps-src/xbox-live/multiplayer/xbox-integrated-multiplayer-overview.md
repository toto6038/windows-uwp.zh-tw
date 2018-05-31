---
title: Xbox 整合式多人遊戲概觀
author: KevinAsgari
description: 了解 Xbox 整合式多人遊戲 (XIM)，適用於 Xbox Live 遊戲的多合一多人遊戲/網路功能/聊天解決方案。
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5af9a5c386dc620c19183747750081ea1bd5a66d
ms.sourcegitcommit: db09dcb08da5995c46c2729896e56be3774ee5ba
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2018
ms.locfileid: "1563390"
---
# <a name="xbox-integrated-multiplayer-overview"></a>Xbox 整合式多人遊戲概觀

 Xbox 整合式多人遊戲 (XIM) 是獨立的介面，可透過 Xbox Live 服務的能力輕鬆地將多人即時網路功能與聊天通訊新增至遊戲。 它圍繞幾個重要的概念：

 - **XIM 網路** - 一組互相連接、參與特定多人遊戲體驗的使用者的邏輯表示，以及描述該集合的基本狀態。 參與者一次只能在一個 XIM 網路中，但可以在概念性 XIM 網路間順暢地移動。
 - `xim_player` - 物件代表在本機或遠端裝置上登入和參與 XIM 網路的個人使用者。 加入、離開、然後重新加入相同 XIM 網路的單一實體使用者會被視為兩個獨立玩家執行個體。
 - `xim_authority` - 物件表示具有權限和責任在 XIM 網路中管理所有參與者的一致遊戲特定狀態檢視的單一實體。 此物件是選用的。 在此軟體版本中，目前未提供其功能。
 - `xim_state_change` - 結構代表向本機裝置傳送有關 XIM 網路特定方面非同步變更的通知。
 - `xim::start/finish_processing_state_changes` - 應用程式呼叫這對方法，以便讓每個 UI 框架執行非同步作業、擷取要在 `xim_state_change` 結構形式中處理的結果，然後在完成時釋放相關聯的資源。
 - **配對** - 此選用程序探索其他有類似興趣或技能等級以參與 XIM 網路的遠端玩家，而不需要現有社交關係。

從高階角度來看，應用程式使用程式庫將本機裝置上的一組使用者設定為要移到 XIM 網路的新玩家。 當遠端裝置上的應用程式執行個體對它們自己的使用者執行同一件事，而且每個持續開始 + 完成處理狀態變更每個 UI 框架時，會為每個合作執行個體提供`xim_state_change`更新，描述加入 XIM 網路的本機及遠端`xim_player`。

在 XIM 網路中，應用程式可以傳送自己的遊戲特定資料訊息，例如虛擬人偶動作更新。 這些可能會針對其他玩家或自動選取`xim_authority`以常駐於參與裝置（根據最佳網路品質、穩定性、玩家評價以及其他因素），讓該`xim_authority`裝置上的應用程式可以傳送訊息給玩家，通常為了網路效率而合併以及為了仲裁衝突。 所有收到的訊息都會傳遞至應用程式做為`xim_state_change`，指示預定的來源和本機的目的地。

若隱私權設定和應用程式設定允許，也會提供所有玩家之間的語音和文字交談通訊。 如果玩家已經啟用語音轉換文字或文字轉換語音，XIM 會透明地執行此轉換，分別為傳入的語音音訊傳遞交談文字訊息，並為傳出的交談文字訊息播放合成語音。

當玩家停止參與 XIM 網路（正常結束或因為網路連線性問題），會提供`xim_state_change`更新給應用程式執行個體，指出`xim_player`已離開。 如果選為`xim_authority`的裝置離開，同樣會通知應用程式執行個體 authority 已開始移動，可透過選擇性地提供資料緩衝區給取代`xim_authority`進行重新同步處理，開始「調解」任何遊戲特定狀態。 新的 authority 可以隨自己喜好解譯來自所有玩家的調解資料，並依序提供「已調解」的資料緩衝區給所有玩家，完成 authority 移轉和重新同步處理程序。 也為新參與裝置提供相同的基本調解程序，因此當玩家加入 XIM 網路，authority 可以輕鬆地提供目前權威性遊戲狀態，無需任何額外的驗證碼。 請注意，在此軟體版本中目前無法提供 authority 狀態變更。

應用程式可以判斷要透過數種方式參與的 XIM 網路。 通常應用程式一開始會自動將本機使用者移到使用者好友可用的新網路，本機使用者可以傳送邀請或啟用 XIM 網路做為可加入的活動探索（例如透過玩家卡）。 這些社交探索使用者準備好之後，應用程式會啟始 Xbox Live「配對」程序，並將所有玩家移到新的 XIM 網路，該網路也包含額外「符合」的遠端玩家，視需要填寫團隊/對手清單。 然後，該多人遊戲體驗完成時，應用程式執行個體會將其本機玩家 (以及選擇性的原始配對前遠端玩家)，移回到新的私人 XIM 網路，或透過配對的另一個隨機 XIM 網路。 在整個過程中，語音和文字交談維持使用。 玩家在 XIM 網路間輕鬆移動，是 API 的核心，反映良好、高度社交遊戲體驗的現代預期。

若要開始，請參閱[使用 XIM](xbox-integrated-multiplayer/using-xim.md)。

## <a name="xims-relationship-to-other-modules"></a>XIM 與其他模組的關係

XIM 的設計目的是提供方便、多合一遊戲介面，符合基本多人遊戲需求。 它封裝數個模組的功能--尤其是 Xbox 服務 API (XSAPI)`multiplayer_manager`模組、`Microsoft::Xbox::GameChat`程式庫和`Windows::Networking::XboxLive`安全多人遊戲網路--成為單一簡化的 API。 建置不需要絕對最大彈性或控制項的多人遊戲時，這可以減少一般相關的程式碼、工作和概念數量。 不過，需求不符合 XIM 簡化假設的應用程式，可能會想要改為直接使用這些元件。

雖然 XIM 旨在免除透過基礎元件管理像是多人遊戲工作階段目錄 (MPSD) 工作階段文件或網路傳輸等項目，它不會防止這些元件同時/並排使用為個別玩家名單或通訊網格的一部分。 在此情況下，確保 XIM 和其機制之間合作網路資源使用，是應用程式的責任。 XIM 目前支援「頻外保留」，以便使用做為使用者清單僅由外部輸入所驅動的專用聊天解決方案。

Xbox Live 提供對多人遊戲有價值的許多其他功能，但和設定多人聊天和網路通訊較不直接相關，因此不包裝在這個模組中。 建議應用程式在 Xbox 服務 API (XSAPI) 中尋找玩家意見反應、成就、排行榜、儲存空間及更多。


## <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>透過頻外保留，使用 XIM 做為專用聊天解決方案

大部分的應用程式使用 XIM 來處理集結玩家的每個層面。 畢竟，如其名，「Xbox 整合式多人遊戲」著重於組合端點對端點支援常見多人遊戲案例所需的所有功能。 不過某些使用專用網際網路伺服器實作自己的網路解決方案的應用程式，也想要建立可靠、低延遲、低成本、對等聊天通訊的優點。 XIM 為因應此需求，目前支援延伸模式利用其簡化對等通訊，以加強 XIM API 外部玩家管理。

> 如需有關透過頻外保留使用 XIM 的詳細文件，請參閱[XIM 保留](xbox-integrated-multiplayer/xim-reservations.md)。
