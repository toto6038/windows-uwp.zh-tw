---
title: 單一存在點 (SPOP)
description: 了解如何使用 Xbox Live 單一的存在點 (SPOP)，以確保標題，就會一次只有單一裝置上播放。
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 spop、 單點的目前狀態
ms.localizationpriority: medium
ms.openlocfilehash: f1503a6168a50175e861e17027e8b642d1b7834d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595003"
---
# <a name="single-point-of-presence-spop"></a>單一存在點 (SPOP)

## <a name="overview"></a>概觀
單一點的目前狀態 (SPOP)，是 Xbox Live 強制執行條件，其中一個標題可以只能播放一個裝置上一次。 這會強制執行 Xbox One XDK 及任何裝置上的 UWP 標題。
Xbox Live 標題，無論裝置，可以開始進行使用者登入另一部 Xbox One 或 Windows 10 裝置上的標題。

## <a name="how-to-handle-spop"></a>如何處理 SPOP
與任何其他類型的登出事件相同的方式時，SPOP 應該是項目所處理。 這樣做會起始驗證，他們想在另一部裝置上會造成中斷標題 SPOP 動作時，使用者永遠被通知透過 UI。

* XDK 標題`User::SignOutCompleted`發生這種情況時，會觸發事件。
* UWP 項目的，他們會通知的登出透過`sign_out_complete`處理常式從`xbox_live_user`類別。 請參閱[UWP 專案的驗證](authentication-for-UWP-projects.md)如需詳細資訊
