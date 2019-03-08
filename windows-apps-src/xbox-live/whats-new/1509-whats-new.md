---
title: 新功能的 Xbox Live SDK-2015 年 9 月
description: 新功能的 Xbox Live SDK-2015 年 9 月
ms.assetid: 84b82fde-f6f3-4dc2-b2df-c7c7313a2cc3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c618a738dc2670d3d3de1fa2f4c4108c24130eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602273"
---
# <a name="whats-new-for-the-xbox-live-sdk---september-2015"></a>新功能的 Xbox Live SDK-2015 年 9 月

請參閱[的新功能-2015 年 8 月](1508-whats-new.md)的新增功能的文章在 2015 年 8 月版本。

Xbox Live SDK 的 9 月版本包含下列更新：

## <a name="os-and-tool-support"></a>作業系統和工具支援 ##
Xbox Live SDK 支援 Windows 10 RTM [Version 10.0.10240] 與 Visual Studio 2015 RTM [Version 14.0.23107.0]。

## <a name="contextual-search-apis"></a>內容相關式搜尋 Api
* 可讓您的標題或搜尋您所選擇的即時統計資料與您 game(s) 從廣播的應用程式。
* 請參閱 Microsoft::Xbox::Services::ContextualSearch 中新的 Api

## <a name="app-insights-for-events"></a>App Insights 事件

| 注意 |
|------|
| App Insights 僅適用於 UWP 的標題。  如果您正在開發的 XDK 標題，本節不適用於您 |

<p/>

* 使用 AppInsights 就可以檢視使用 write_in_game_event() 撰寫的事件
* 文件即將這在未來，同時請洽詢您補西牆，即可存取

## <a name="logging"></a>記錄
* service_call_logging_config in xbox::services::experimental
* 若要啟動和停止追蹤透過 xbTrace.exe 主控台上，您必須呼叫 register_for_protocol_activation service_call_logging_config 類別上。  進行這個呼叫一次在您的遊戲初始化期間。

## <a name="resync-for-rta"></a>RTA 的重新同步
* RTA 服務可讓您認為可能過期的使用者資訊時，可能會發生重新同步處理
* 標題應該呼叫對應的 HTTP 呼叫，他們已訂閱的訂用帳戶
* 若要重新訂閱沒有標題
* 已新增的 xbox::services::real_time_activity_service::add_resync_handler
* 已移除的 xbox::services::real_time_activity_service::remove_resync_handler
* Added http_status_429_too_many_requests
* 標題傳送太多的 http 要求進行節流時，就會看到這個錯誤狀況

## <a name="documentation"></a>文件
* 移轉到 Xbox Live 服務 API 2.0
* 錯誤處理
* 在 Windows 10 的 Xbox Live 驗證
* 內容相關式搜尋
