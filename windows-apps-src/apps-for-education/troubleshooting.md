---
description: 使用事件檢視器對「Microsoft 進行測驗」的事件和錯誤進行疑難排解。
title: 使用事件檢視器對「Microsoft 進行測驗」進行疑難排解。
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, 教育
ms.localizationpriority: medium
ms.openlocfilehash: cd30d54f1bff5fd43fbeb6e286e327fed9f8a585
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031481"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>使用事件檢視器對「Microsoft 進行測驗」進行疑難排解

您可以使用事件檢視器來見識「進行測驗」的事件和錯誤。 在收到鎖定要求之後、裝置成功註冊、成功套用鎖定原則等情況下，「進行測驗」會記錄事件。

若要啟用以「事件檢視器」檢視事件：
1. 開啟 `Event Viewer`
2. 巡覽到 `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. 按一下滑鼠右鍵 `Operational` 並選取 `Enable Log`

若要儲存事件記錄檔︰
1. 以滑鼠右鍵按一下 `Operational`
2. 按一下 `Save All Events As…`
