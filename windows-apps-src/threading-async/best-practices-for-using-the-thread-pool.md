---
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: 使用執行緒集區的最佳做法
description: 本主題描述使用執行緒集區的最佳做法。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 執行緒, 執行緒集區
ms.localizationpriority: medium
ms.openlocfilehash: a498f685e7a810d19e2f1eb63ae112dd02587b84
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370689"
---
# <a name="best-practices-for-using-the-thread-pool"></a>使用執行緒集區的最佳做法

本主題描述使用執行緒集區的最佳做法。

## <a name="dos"></a>可行事項


-   使用執行緒集區在 app 以並行方式執行工作。

-   使用工作項目完成延伸工作，而且不會封鎖 UI 執行緒。

-   建立短期且獨立的工作項目。 工作項目會以非同步方式執行，而且可從佇列中以任何順序提交至集區。

-   使用 [**Windows.UI.Core.CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 將更新分派至 UI 執行緒。

-   使用 [**ThreadPoolTimer.CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) 代替 **Sleep** 函式。

-   使用執行緒集區，而不是建立自己的執行緒管理系統。 執行緒集區可在作業系統層級使用進階功能執行，且已經根據處理程序和整個系統中的裝置資源及活動最佳化以動態調整。

-   在 C++ 中，確定工作項目委派使用 agile 執行緒模型 (C++ 委派預設是 agile)。

-   當無法容許使用中的資源配置失敗時，請使用預先配置的工作項目。

## <a name="donts"></a>禁止事項


-   不要建立 *period* 值小於 &lt;1 (包括 0) 毫秒的定時計時器。 這會讓工作項目變得像是單次計時器。

-   不要提交完成時間比您使用 *period* 參數指定的時間更長的定期工作項目。

-   不要嘗試從在背景工作分派的工作項目傳送 UI 更新 (除了快顯通知及通知之外)。 請改為使用背景工作進度和完成處理常式 - 例如，[**IBackgroundTaskInstance.Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress)。

-   當您使用利用 **async** 關鍵字的工作項目處理常式時，請注意在處理常式中所有的程式碼執行之前，執行緒集區工作項目可能會設為完整狀態。 處理常式中 **await** 關鍵字之後的程式碼可能會在工作項目已經設為完整狀態之後才會執行。

-   不要未重新起始預先配置的工作項目，就嘗試重複執行它。 [建立定期工作項目](create-a-periodic-work-item.md)

## <a name="related-topics"></a>相關主題


* [建立定期工作項目](create-a-periodic-work-item.md)
* [將工作項目提交至執行緒集區](submit-a-work-item-to-the-thread-pool.md)
* [使用計時器提交工作項目](use-a-timer-to-submit-a-work-item.md)
