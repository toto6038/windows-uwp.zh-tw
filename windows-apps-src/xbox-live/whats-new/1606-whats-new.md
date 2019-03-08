---
title: 新功能的 Xbox Live SDK-2016 年 6 月
description: 新功能的 Xbox Live SDK-2016 年 6 月
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d984d054d9e5fd7f9d34b42c1a224d53632e719
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595283"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>新功能的 Xbox Live SDK-2016 年 6 月

請參閱[的新功能-2016 年 4 月](1604-whats-new.md)文章新增了哪些功能在 2016 年 4 月版本。

> **注意：** 如果您使用 Xbox 開發人員套件 」 (XDK)，則*的新功能-2016 年 4 月*文章涵蓋年 3 月 XDK 釋出因為所加入的其他新功能。

## <a name="os-and-tool-support"></a>作業系統和工具支援
Xbox Live SDK 支援 Windows 10 RTM [Version 10.0.10240] 與 Visual Studio 2015 RTM [Version 14.0.23107.0]。

## <a name="contextual-search"></a>內容相關式搜尋
已新增下列新的類別`contextual_search`API 擷取遊戲的剪輯：

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

請參閱參考文件，如需詳細資訊。

## <a name="networking"></a>網路功能
Xbox Live SDK 現在包含新判斷提示，偵測到 5 個或多個 websockets 如果會建立標題的單一執行個體中每位使用者。

您可以停用此判斷提示，藉由呼叫`disable_asserts_for_maximum_number_of_websockets_activated()`。

> **注意：** 身分證管理員及多人遊戲管理員使用單一的結合的 websocket，如果您使用這些功能在您的標題。

## <a name="tools"></a>工具
* Xbox Live 復原外掛程式的 Fiddler 現在已包含在 Xbox Live SDK。  
它是可讓開發人員，選擇性地封鎖呼叫 Xbox Live 的 Fiddler 的附加元件。
使用此外掛程式，開發人員可以跨遊戲標題中的部分服務中斷中測試復原。
此工具包含許多要測試 Xbox Live 服務端點內建和不同的錯誤類型。
支援的所有 Xbox One 和 UWP 的標題。
