---
title: 新功能-2016 年 8 月 Xbox Live SDK
description: 新功能-2016 年 8 月 Xbox Live SDK
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2a498fea1ed0974935a273c9ee72ba2c95d15959
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627903"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>新功能-2016 年 8 月 Xbox Live SDK

請參閱[的新功能-2016 年 6 月](1606-whats-new.md)文章新增了哪些功能在 2016 年 6 月版本。

## <a name="os-and-tool-support"></a>作業系統和工具支援
Xbox Live SDK 支援 Windows 10 RTM [Version 10.0.10240] 與 Visual Studio 2015 RTM [Version 14.0.23107.0]。

## <a name="documentation"></a>文件
- 如果您要撰寫的 UWP 應用程式，並實作能夠邀請使用者在遊戲中上, 有指示```.appxmanifest```中所需的變更[設定您 AppXManifest 的多人遊戲](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md)。  這先前所討論上[論壇](https://forums.xboxlive.com)並[移植 xbox live 的程式碼紀元 uwp](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)文章
- [社交 Manager 簡介](../social-platform/intro-to-social-manager.md)文章已更新以反映最新的 API 變更，並提供一些函式的傳回碼的詳細資訊。

## <a name="unity-samples"></a>Unity 範例
適用於 Unity 開發人員撰寫的 UWP 應用程式已新增一些新的範例。
- 現在沒有社交範例 Unity 版本，您可以找到此範例/社交/Unity 目錄下。
- 另外還有說明如何使用連接儲存體的範例。  請如需範例，參閱範例/GameSave/Unity。
底下範例/AchievementsLeaderboard/Unity AchievementsLeaderboard Unity 版本

## <a name="social-manager"></a>身分證管理員
除了上面提到的文件更新，還有一些新的 Api 已新增。  以下分別說明，而且您可以看到更詳細地 social_manager.h

- 已新增的新 API，可讓沒有重新建立社交群組的更新：

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- 社交群組的已完成的更新以```social_user_group_updated```事件


## <a name="multiplayer"></a>多人遊戲
改善的工作階段瀏覽現已推出，您可以使用新的多人遊戲 Api 來利用它。

使用新的 Api，您可以篩選標記、 字串和其他豐富的資料，可讓使用者更輕鬆地找到他們想要播放的工作階段。

我們刊登更完整的文件，接下來的幾個月，但簡短您可以現在將"的搜尋控制代碼 」 與 MPSD 工作階段使用```set_search_handle```然後使用者可以搜尋標題呼叫中使用了完整的篩選機制的工作階段 ```get_search_handles```

新的 Api 如下所示。  請嘗試出，然後如果您遇到任何問題時，張貼支援的執行緒[論壇](https://forums.xboxlive.com)或連絡您的補西牆。  我們將會有如何使用這些 Api 的範例。

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> set_search_handle(
    _In_ multiplayer_search_handle_request searchHandleRequest
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<std::vector<multiplayer_search_handle_details>>> get_search_handles(
    _In_ const string_t& serviceConfigurationId,
    _In_ const string_t& sessionTemplateName,
    _In_ const string_t& orderBy,
    _In_ bool orderAscending,
    _In_ const string_t& searchQuery
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> clear_search_handle(_In_ const string_t& handleId);
```

### <a name="xbox-integrated-multiplayer"></a>Xbox 整合多人遊戲

我們已包含文件的 Xbox 整合式多人遊戲 (XIM) API。  API 本身會在 Xbox Live SDK 的後續版本中提供，但文件和標頭會進行可供預覽。

XIM 是獨立的介面，輕鬆地將多人遊戲的即時網路和聊天通訊新增至您的遊戲，透過 Xbox Live 服務的強大功能。

此預覽版 API 的文件的此處共用鼓勵客戶的意見反應和查詢。 我們談到稍早在 Xfest 2016，此 API，以及您所見封存[受管理的協力廠商開發人員網站上的簡報材料](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx)從 「 周全的多人網路和聊天 」 演講。 請注意，此預覽文件僅適用於 c + + API。 WinRT 對等項目的C#並將在年後發行其他語言。

如果您有興趣 XIM 的功能，有任何意見或其他問題關於此專案中，請隨意張貼[Xbox 開發人員論壇](https://forums.xboxlive.com/)或透過您的開發人員客戶經理連絡。

您可以看到 Xbox Live SDK xbox_integrated_multiplayer.chm Docs 目錄中這個新文件。  使用預覽 \include\xim\XboxIntegratedMultiplayer.h include 檔。  
