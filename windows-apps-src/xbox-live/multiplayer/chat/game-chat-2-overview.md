---
title: 遊戲的對談 2 概觀
description: 了解如何將您的遊戲中的語音通訊，使用 Xbox Live 遊戲聊天 2，遊戲的對談的更新版本。
ms.date: 10/20/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 遊戲的對談、 遊戲聊天 2、 語音通訊
ms.localizationpriority: medium
ms.openlocfilehash: 9f013f8b80cc7bca367c3ef5cd2c0d1da86cc98c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615823"
---
# <a name="game-chat-2-overview"></a>遊戲的聊天 2 概觀

遊戲的聊天 2 可讓您將新增語音和文字的聊天通訊至您的應用程式時尊重您的玩家隱私權設定與一個 Xbox 的遊戲和中樞應用程式相關的語音和文字的聊天謻 Xbox。 播放程式已啟用透過輕鬆存取-遊戲的聊天文字記錄設定的語音轉換文字或文字轉換語音轉換遊戲聊天 2 會以透明的方式執行建立聊天文字訊息，表示內送的語音音訊及播放的翻譯分別合成語音音訊外寄的聊天文字訊息。

> [!NOTE]
> 如果您要尋找特定 API 的參考，可以找到可下載的 Xbox Live API 編譯 HTML 說明 (.chm) 檔案中[此處](https://aka.ms/xboxliveuwpdocs)。

- **通訊關聯性**-遊戲聊天 2 可讓您更細微的控制您的玩家可以與彼此通訊的方式。 除了指定小組或頻道，遊戲聊天 2 會要求必須定義明確的使用者每對之間的關聯性。 遊戲的對談 2 通訊關聯性支援 uni 和任何一對的播放程式之間的雙向通訊。 語音和文字通訊關聯性可以彼此獨立設定。

- **協助工具**-遊戲聊天 2 支援語音轉換文字和文字轉換語音。 啟用時，遊戲聊天 2 會尊重您的玩家的 「 遊戲交談文字記錄 「 喜好設定，並會以透明的方式執行建立聊天文字訊息，表示內送的語音音訊 」 和 「 play 合成語音音訊的外寄的聊天文字翻譯訊息，分別。

- **Xbox Live 整合**-遊戲聊天 2 使用 Xbox Live 服務可確保會遵守每位玩家的喜好設定和權限。

- **語音活動偵測**-遊戲聊天 2 執行語音活動偵測，以判斷當音訊資料包含語音活動。

- **自動取得控制**-遊戲聊天 2 執行自動取得的控制，將使用者的麥克風輸出中的變化降至最低。

- **轉碼器**-遊戲聊天 2 將編碼的音訊資料，必須傳遞至應用程式的遠端執行個體。 Xbox One 上這種編碼方式 （在接收端解碼） 是硬體加速。

- `chat_manager::start/finish_processing_state_changes` -對方法，由應用程式來執行非同步作業，以擷取的形式處理結果的每個 UI 畫面格呼叫`game_chat_state_change`結構，然後釋放相關聯的資源時完成。

- `chat_manager::start/finish_processing_data_frames` -一組方法用來插入應用程式的傳輸層的遊戲聊天 2。 這些方法會呼叫應用程式所擷取，並將每個網路畫面`game_chat_data_frame`遠端裝置上的應用程式的執行個體的物件，然後釋放相關聯的資源時完成。

- `chat_manager::process_incoming_data` -用來為遊戲聊天 2 已從遠端執行個體的遊戲聊天 2 傳遞透過應用程式的傳輸層提供資料的方法。

- **即時音訊操作**-遊戲聊天 2 可讓應用程式將本身插入聊天音訊管線中，使其可以檢查或操作聊天音訊資料。 請參閱[即時音訊操作](real-time-audio-manipulation.md)如需詳細資訊。

應用程式會通知程式庫的本機裝置上的使用者和應該一起交談的遠端裝置上的使用者。 應用程式接著會設定每個使用者之間的關聯性。

遊戲聊天 2 設定之後，輪詢從本機使用者的 microphone(s) 音訊、 執行自動獲得控制和語音活動偵測、 編碼資料，然後公開 （expose） 音訊資料傳遞給遠端執行個體的遊戲透過聊天 2 `chat_manager::start/finish_processing_data_frames`。 應用程式必須提供所包含的資料`game_chat_state_change`交談在相同物件中指定之 2 的遊戲的遠端執行個體的物件。 在收到資料時，應用程式的遠端執行個體必須將資料提交至自己的遊戲透過聊天 2 的執行個體`chat_manager::process_incoming_data`。 遊戲的對談 2 解碼內送資料，並以本機使用者的音訊裝置會接著呈現它。

遊戲的對談 2 會強制執行 Xbox Live 的權限及隱私權需求。 沒有通訊的權限的使用者將不會產生音訊資料，並透過隱私權設定封鎖的使用者將無法呈現音訊資料。

若要開始，請參閱[聊天 2 使用遊戲](using-game-chat-2.md)。 如果您使用C#，請參閱[使用遊戲聊天 2 WinRT 投影](using-game-chat-2-winrt.md)。 如果您已經有遊戲對談的實作，使用[遊戲聊天 2 移轉指南](game-chat-2-migration.md)將遊戲對談的概念與呼叫模式對應至遊戲聊天 2 雷同。

如需遊戲聊天 2 API 的資訊，請下載 Xbox API 參考：[XboxAPIs.zip](https://aka.ms/xboxliveuwpdocs)