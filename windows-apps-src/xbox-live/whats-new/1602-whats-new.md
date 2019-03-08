---
title: 新功能-2016 年 2 月 Xbox Live SDK
description: 新功能-2016 年 2 月 Xbox Live SDK
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c13b2893397a0d84e8919146435ece338bc1afde
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594733"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>新功能-2016 年 2 月 Xbox Live SDK

請參閱[的新功能-2015 年 10 月](1510-whats-new.md)1510年中提供新功能文章

## <a name="os-and-tool-support"></a>作業系統和工具支援
Xbox Live SDK 支援 Windows 10 RTM [Version 10.0.10240] 與 Visual Studio 2015 RTM [Version 14.0.23107.0]。

## <a name="throttling"></a>節流設定
- 更細緻的節流將即將推出大部分的 Xbox Live 服務。  Xbox 服務 API (XSAPI) 會自動處理重試，並通知您在開發期間會進行節流的呼叫。  更多詳細資料可在[最佳做法呼叫 Xbox Live](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md)文件中的文章。

## <a name="leaderboards"></a>排行榜
- 多重資料行的排行榜現在可以存取由 GetLeaderboard API。 如果您提供的額外資料行名稱的向量，結果的資料行的向量會填妥，如果有這些資料行。

## <a name="documentation"></a>文件
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights)文件在此處。  您可以使用 Application Insights 透過免費的 Azure 帳戶，來檢視中近乎即時的資料平台事件。  這項功能目前僅適用於 UWP 應用程式在桌面上執行的 Windows 10。
- 更新文件上的 Xbox 的常見事件工具適用於 UWP 開發人員討論如何產生包裝函式用於將資料平台事件。  請注意，這是選擇性的如果您偏好使用 WriteInGameEvent API 時，您可以繼續。
- 使用 Fiddler，偵錯資料平台事件，以確保正確地傳送。  這是僅適用於 UWP 的事件。
- 會提供有關如何收集即時追蹤分析師 工具的記錄檔的資訊。  請參閱[分析 Xbox Live 服務呼叫](../tools/analyze-service-calls.md)文章。
