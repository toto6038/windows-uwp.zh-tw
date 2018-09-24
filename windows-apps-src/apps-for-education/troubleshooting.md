---
Description: Troubleshoot Microsoft Take a Test events and errors with the event viewer.
title: 使用事件檢視器對「Microsoft 進行測驗」進行疑難排解。
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，教育版
ms.localizationpriority: medium
ms.openlocfilehash: 3193525316d085e56244d6f03da99e3e07c6539f
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "4148253"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>使用事件檢視器對「Microsoft 進行測驗」進行疑難排解

您可以使用事件檢視器來見識「進行測驗」的事件和錯誤。 在收到鎖定要求之後、裝置成功註冊、成功套用鎖定原則等情況下，「進行測驗」會記錄事件。

若要啟用以「事件檢視器」檢視事件：
1. 開啟 `Event Viewer`
2. 瀏覽至 `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. 以滑鼠右鍵按一下 `Operational`，並選取 `Enable Log`

若要儲存事件記錄檔︰
1. 以滑鼠右鍵按一下 `Operational`
2. 按一下 `Save All Events As…`
