---
title: 新功能的 Xbox Live SDK-2015 年 6 月
description: 新功能的 Xbox Live SDK-2015 年 6 月
ms.assetid: 354bcd47-2564-4dd5-89e3-242bca462b35
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a42d0fb0a3cb457a60a0542bfc5966893d00f18b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627873"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2015"></a>新功能的 Xbox Live SDK-2015 年 6 月

Xbox Live SDK 的年 6 月版本包含下列更新：

## <a name="os-and-tool-support"></a>作業系統和工具支援 ##
Xbox Live SDK 現在支援最新的測試人員組建的 Windows 10 和 Visual Studio 2015 RC。

## <a name="title-callable-ui-apis"></a>標題可呼叫的 UI Api

| 注意 |
|------|
| 本節僅適用於 UWP 標題 XDK 標題應該如，請參閱 Windows.Xbox.UI.SystemUI 類別 TCUI  |

Xbox Live SDK 現在包含包裝函式支援 Windows 10 電腦/行動裝置上顯示股票 UI 的標題可呼叫 UI (TCUI) 的 Api。

這些 Api 僅適用於 UWP 應用程式。

您可以從 TitleCallableUI (WinRT) 和 title_callable_ui （c + +） 類別來呼叫 TCUI 包裝函式。

股票 Ui 包括：
* 播放程式選擇器 UI
* 遊戲邀請選擇器 UI
* 播放程式設定檔卡片 UI
* 新增/移除 friend UI
* 顯示遊戲標題成就 UI

請參閱新*TCUI*範例，例如新的 api 的使用方式。 您可以找到下的範例 {*SDK 來源根目錄*} \Samples\TCUI。

## <a name="new-authentication-model-for-uwp-apps"></a>新的 UWP 應用程式的驗證模型
Xbox Live SDK 現在支援使用 Microsoft 帳戶 (MSA)，用來識別 Windows 10 電腦/行動裝置上的 Xbox Live 的播放程式。

您現在可以使用 XboxLiveUser (WinRT) 和 xbox_live_user （c + +） 類別以登入使用者。

## <a name="new-api-for-writing-events-in-uwp-apps"></a>適用於 UWP 應用程式中寫入事件的新 API

| 注意 |
|------|
| 本節僅適用於 UWP 的標題。  XDK 的開發人員應該參閱[遊戲事件](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/game-events)XDK 的特定資訊的發行項  |

將新 EventsService (WinRT) 和 events_service （c + +） 的類別可讓您撰寫遊戲內活動可更新的使用者統計資料、 成就、 排行榜、 等等。這些新的類別是僅適用於 UWP app。

## <a name="breaking-change-to-event-handlers"></a>事件處理常式的重大變更 ##
已從單一變更 c + + SDK 中的所有事件處理常式`void set_*_handler()`方法，以一組`function_context add_*_handler()`和`void remove_*_handler(function_context context)`方法。

每個`add_*_handler()`方法現在會傳回`function_context`物件。 當您移除事件處理常式時，您必須傳入`function_context`物件。

例如：
```
function_context subscriptionLostContext = xbox_live_context()->multiplayer_service().add_multiplayer_subscription_lost_handler(...);

localUser->xbox_live_context()->multiplayer_service().remove_multiplayer_subscription_lost_handler(subscriptionLostContext);
```

## <a name="new-apis-for-managing-multiplayer-scenarios"></a>新的 Api，以管理多人遊戲情節
Xbox Live SDK 現在會包含一組 c + + Api 來管理常見的多人遊戲案例。 Api 會包含在 xbox.services.experimental.multiplayer 命名空間。

這些 Api 會在它們可能會變更的方式根據意見反應的實驗性命名空間中。  我們鼓勵開發人員使用它們，並提供意見反應。

請參閱新*MultiplayerManager*範例，例如這些新 api 的使用方式。 您可以找到下的範例 {*SDK 來源根目錄*} \Samples\MultiplayerManager。
