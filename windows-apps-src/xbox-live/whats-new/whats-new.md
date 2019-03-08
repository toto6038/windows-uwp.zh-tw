---
title: Xbox Live 的最新消息
description: Xbox Live SDK 的最新消息
ms.date: 10/23/2018
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33e5b6afbf0d60679bfce1789be2d965fd881f1c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614113"
---
# <a name="whats-new-for-xbox-live"></a>Xbox Live 最新動向
您也可以檢查[Xbox Live API GitHub 認可記錄](https://github.com/Microsoft/xbox-live-api/commits/master)若要查看所有最新的程式碼變更，到 Xbox Live Api。

## <a name="in-this-article"></a>本文內容

* [2018 年 6 月](#june-2018)
* [2017 年 8 月](#august-2017)
* [2017 年 7 月](#july-2017)
* [2017 年 6 月](#june-2017)
* [2017 年 5 月](#may-2017)
* [2017 年 4 月](#april-2017)
* [2017 年 3 月](#march-2017)
* [封存](#archived)

## <a name="june-2018"></a>2018 年 6 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="c-api-layer-for-xsapi"></a>XSAPI C API 層

C Api 現在可供某些 Xbox Live 的功能。 新的 API 層會提供許多優點，如需支援的功能，包括自訂記憶體管理、 手動執行緒管理非同步工作，以及新的 HTTP 程式庫。

如需詳細資訊，請參閱 < [Xbox Live C Api](../xsapi-flat-c.md)。

## <a name="august-2017"></a>2017 年 8 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="in-game-clubs"></a>在遊戲俱樂部

開發人員現在可以建立 「 遊戲中俱樂部 」。 與標準 Xbox 俱樂部的遊戲中梅花差異，在於它們是由開發人員可完全自訂，而且可用內部和外部的遊戲。 身為遊戲開發人員，您可以快速建立任何類型的持續性的群組符合您獨特的需求，例如 teams、 這條通道、 squads、 公會 」 等遊戲內的案例中使用它們。

Xbox live 的成員可以存取外部遊戲的遊戲中梅花能保持任何 Xbox 體驗連線以及對您的遊戲使用 club 的功能，例如聊天、 摘要、 LFG 和 Mixer 免費 Xbox 主控台、 電腦或 iOS/Android 裝置上。

Api 可用於建立及管理直接從您的遊戲中的遊戲中俱樂部。 這些 Api 會存在於 xbox::services::clubs 命名空間。

## <a name="july-2017"></a>2017 年 7 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="tournaments"></a>比賽

新的 Api 已新增以支援比賽。 您現在可以使用 xbox::services::tournaments::tournament_service 類別來存取比賽服務從您的標題。
這些新聯賽 Api 適用於下列案例：

* 查詢服務，以尋找目前的標題中的所有現有的比賽。
* 從服務擷取聯賽的相關詳細資料。
* 查詢服務，以擷取一份聯賽的小組。
* 從服務擷取相關的小組聯賽。 詳細資料。
* 追蹤變更比賽與小組使用即時活動 (RTA) 的訂用帳戶。

## <a name="june-2017"></a>2017 年 6 月

### <a name="xbox-live-features"></a>Xbox Live 功能

#### <a name="game-chat-2"></a>遊戲交談 2

現在可使用的更新和改進的版本，遊戲談。 如需詳細資訊，請參閱 <<c0> [ 遊戲聊天 2 概觀](../multiplayer/chat/game-chat-2-overview.md)。

### <a name="xbox-live-tools"></a>Xbox Live 的工具

#### <a name="xbox-live-powershell-module"></a>Xbox Live 的 PowerShell 模組

* 若要輕鬆地切換沙箱的開發電腦上已加入 PowerShell 模組。 如需詳細資訊，請參閱[工具](../tools/tools.md)

#### <a name="bug-fixes"></a>Bug 修正

* 各種 bug 修正。 請檢查[GitHub 認可記錄](https://github.com/Microsoft/xbox-live-api/commits/master)如需完整清單。

## <a name="may-2017"></a>2017 年 5 月

### <a name="xbox-services-apis"></a>Xbox 服務 Api

#### <a name="multiplayer"></a>多人遊戲

* 現在查詢搜尋功能在處理在回應中包含自訂的工作階段屬性。

#### <a name="bug-fixes"></a>Bug 修正

* 已修正 「 不正確 json 」，而不是有效的 HTTP 錯誤碼所傳回。

## <a name="april-2017"></a>2017 年 4 月

### <a name="xbox-services-apis"></a>Xbox 服務 Api

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Xbox Live Api 已更新為支援 Visual Studio 2017 中，針對通用 Windows 平台 (UWP) 及 Xbox One 的標題。

#### <a name="tournaments"></a>比賽

新的 Api 已新增以支援比賽。 您現在可以使用`xbox::services::tournaments::tournament_service`類別來存取比賽服務從您的標題。

這些新聯賽 Api 適用於下列案例：

* 查詢服務，以尋找目前的標題中的所有現有的比賽。
* 從服務擷取聯賽的相關詳細資料。
* 查詢服務，以擷取一份聯賽的小組。
* 從服務擷取相關的小組聯賽。 詳細資料。
* 追蹤變更比賽與小組使用即時活動 (RTA) 的訂用帳戶。

## <a name="march-2017"></a>2017 年 3 月

### <a name="xbox-services-api"></a>Xbox 服務 API

#### <a name="data-platform-2017"></a>資料平台 2017

我們引進了一個簡化的統計資料 API。  過去您必須傳送事件對應至 stat XDP 或合作夥伴中心上所定義的規則，這些會更新統計資料的值，在雲端中。  我們將此模型稱為 Stats 2013。

使用統計資料 2017 時，您的標題隨即出現在控制項狀態的值。  您只要呼叫 API 的最新的統計資料值，並可取得傳送至服務直接而不需要的事件。  這會使用新`StatsManager`API，您可以深入了解[播放程式統計資料](../leaderboards-and-stats-2017/player-stats.md)

#### <a name="github"></a>GitHub

Xbox Live API (XSAPI) 現在是在 GitHub 上的可用[ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api)。  隨附的二進位檔的 XDK，或是做為 NuGet 套件仍建議使用，不過您可以自由地使用來源，並歡迎原始碼程式碼參與。  

### <a name="xbox-live-creators-program"></a>Xbox Live 創作者計畫

Xbox Live 創作者計劃是開發人員計劃，提供開發人員接觸到的 Xbox Live 功能的子集。  如果您已在ID@Xbox程式，這不會對您造成任何影響。

您可以深入了解中的程式[開發人員計劃概觀](../developer-program-overview.md)。

### <a name="documentation"></a>文件

有下列新的文章：

| 文章 | 描述 |
|---------|-------------|
|[Xbox Live 服務組態](../xbox-live-service-configuration.md) | 執行您的 Xbox Live 標題的服務組態的更新的資訊
| [設定 Xbox Live 中 Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | 在 Unity 設定適用於 Xbox Live 創作者計劃開發人員的新資訊 |
| [Xbox Live 沙箱](../xbox-live-sandboxes.md) | Xbox Live 的沙箱和內容隔離的簡化的指引 |
| [Xbox Live 的測試帳戶](../xbox-live-test-accounts.md) | 有關測試帳戶的工作，以及如何建立它們在合作夥伴中心 |

## <a name="archived"></a>封存

* [2016 年 12 月](1612-whats-new.md)
* [2016 年 11 月](1611-whats-new.md)
* [2016 年 8 月](1608-whats-new.md)
* [2016 年 6 月](1606-whats-new.md)
* [2016 年 4 月](1603-whats-new.md)
* [2016 年 3 月](1603-whats-new.md)
* [2016 年 2 月](1602-whats-new.md)
* [2015 年 10 月](1510-whats-new.md)
* [2015 年 9 月](1509-whats-new.md)
* [2015 年 8 月](1508-whats-new.md)
* [2015 年 6 月](1506-whats-new.md)
