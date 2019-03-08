---
title: XDK 專案的驗證
description: 了解如何在 Xbox Development Kit (XDK) 標題中的 Xbox Live 使用者登入。
ms.assetid: 713bb2e3-80c5-4ac9-8697-257525f243d3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 597b3becfa2083955d8bd4e0adc91e4ae9b827a1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613493"
---
# <a name="authentication-for-xdk-projects"></a>XDK 專案的驗證

若要利用 Xbox Live 功能的遊戲中，使用者必須建立 Xbox Live 設定檔來識別自行在 Xbox Live 社群。  追蹤的 Xbox Live 服務相關的遊戲活動使用該 Xbox Live 設定檔，例如使用者的玩家代號和玩家的圖片，身為使用者的遊戲朋友，什麼遊戲使用者扮演了非常，哪些使用者已解除鎖定，其中使用者代表的成就特定的遊戲，等排行榜。

概括而言，您可以使用 Xbox Live Api 遵循下列步驟：
1. 識別互動的使用者
2. 建立根據使用者的 Xbox Live 內容
3. 使用 Xbox Live 的內容來存取 Xbox Live 服務
4. 當在遊戲結束 」 或 「 使用者符號外、 釋出 XboxLiveContext 物件設定為 null

### <a name="creating-an-xboxliveuser-object"></a>建立 XboxLiveUser 物件
大部分的 Xbox Live 的活動相關的 Xbox Live 使用者。  身為遊戲開發人員，您必須先建立 XboxLiveUser 物件來代表本機使用者。

C++：
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>( user );
```

WinRT:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
Microsoft::Xbox::Services::XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext( user );
```
