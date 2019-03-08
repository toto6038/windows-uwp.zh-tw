---
title: 新功能-2016 年 11 月 Xbox Live SDK
description: 新功能-2016 年 11 月 Xbox Live SDK
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a9efeec63399916444de9ba33d4e587a914f2a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658563"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>新功能-2016 年 11 月 Xbox Live SDK

請參閱[的新功能-2016 年 8 月](1608-whats-new.md)文章新增了哪些功能在 2016 年 8 月版本。

## <a name="xbox-services-api"></a>Xbox 服務 API

### <a name="simplified-achievements"></a>簡化的成就

* [簡化的成就](../achievements-2017/simplified-achievements.md)是新且更簡單的方式，設定及使用成就。  您不再需要傳送事件，並讓決定是否解除鎖定您的成就的 Xbox Live 服務。  您的標題是負責這項決定。
* 如果您已經簡化成就的早期試驗的一部份我們也會新增離線支援。
* 還有稱為展示使用是多麼的 SimplifiedAchievements 新範例。

### <a name="multiplayer"></a>多人遊戲

* [工作階段瀏覽](../multiplayer/session-browse.md)是為您的使用者，若要尋找多人遊戲的新方式。  工作階段瀏覽可讓玩家来搜尋的開啟多人遊戲的遊戲工作階段符合指定的準則的清單。
* [多人遊戲 Manager](../multiplayer/multiplayer-manager.md)現在已自動填入功能。  如果啟用，多人遊戲管理員會發現透過配對來填滿期間遊戲的開啟位置的成員。
* 進入生產階段前版[XIM （Xbox 整合式的多人）](../multiplayer/xbox-integrated-multiplayer.md)現已供 XDK 開發。  XIM 是獨立的介面，輕鬆地將多人遊戲的即時網路和聊天通訊新增至您的遊戲，透過 Xbox Live 服務的強大功能。

### <a name="social-manager"></a>身分證管理員

* 許多[社交 Manager](../social-platform/intro-to-social-manager.md) API 變更：
    * 社交管理員已離開的實驗性的命名空間。 特別感謝人是早期採用者，並給予意見反應 ！
    * `xbox_social_user`
        * `string_t` 物件已變更為`char*`物件
        * 自訂組態選項的六個的限制記錄`social_manager_presence_title_record`每 `social_manager_presence_record`
    * `social_event`
        * 傳回`std::vector<xbox_user_id_container>`而不是 `std::vector<xbox_social_user>`
        * 這個向量可以傳遞到新的 API， `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` API 現在會傳回`std::vector<xbox_social_user*>`。 這些指標會在下一個呼叫失效 `social_manager::do_work()`
        * `get_copy_of_users` 傳回`std::vector<xbox_social_user>`為目前的使用者，在呼叫端 social_user_group 的複本。 呼叫此函式可能會影響`do_work`完成時間。
* 身分證管理員現在將永遠不會無法在初始化之後。 身分證 Manager 會在重試 RTA 自動中斷連線。 `error`和`rta_disconnect_error`事件已被取代，移除
* 標題可以指定自訂的記憶體配置器。 請參閱新的文件：[身分證 Manager 記憶體和效能](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Xbox One UWP
* TCUI Api 加入 Xbox One UWP 應用程式的支援多使用者。  XSAPI c + + 加入新的 Windows::System::User ^ 參數會指定使用者，以及 XSAPI WinRT API 加入 ForUserAsync Api。
* 若要在 Xbox 上支援多使用者的更新的 UWP 範例

### <a name="other"></a>其他

* C + + /cli WinRT 新增的支援。   您可以找到更多詳細資料[這裡](../introduction-to-xbox-live-apis.md)
* 新功能中新增和移除您自己的記錄回呼。  診斷層級會傳遞至回呼，因此您可以微調您的行為。  請參閱`add_logging_handler`並`remove_logging_handler`在`microsoft::xbox::services::system::xbox_live_services_settings`命名空間

## <a name="documentation"></a>文件
* 為新的文件上[多人遊戲管理員](../multiplayer/multiplayer-manager.md)， [XIM](../multiplayer/xbox-integrated-multiplayer.md)，並[多人遊戲的概念](../multiplayer/multiplayer-concepts.md)Xbox live。
* [Xbox Live 簡介](../get-started-with-partner/get-started-with-xbox-live-partner.md)區段都已重寫。  如果您要建立新的 Xbox Live 啟用標題，或了解其他 Xbox Live 功能納入您的遊戲，您可以看到新的 docs[此處](../get-started-with-partner/get-started-with-xbox-live-partner.md)。

## <a name="tools"></a>工具
* Xbox Live 追蹤分析師，您可以在中找到[ https://aka.ms/XboxLiveTools ](https://aka.ms/XboxLiveTools)現在具有憑證的模式。  
* 也在 Xbox Live 追蹤分析器是多主控台支援。  如果您傳入 Fiddler 追蹤，其中包含多個主控台的 HTTP 要求時，便會針對每個產生個別的報告。
