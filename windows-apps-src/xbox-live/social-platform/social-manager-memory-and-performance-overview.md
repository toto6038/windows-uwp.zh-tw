---
title: 社交管理員記憶體和效能
description: 說明使用 Xbox Live 社交 API manager 時的記憶體和效能考量。
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 社交 manager、 人員
ms.localizationpriority: medium
ms.openlocfilehash: 7c6a0a31c8fe82faa644a59147060f9323d42eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618453"
---
# <a name="social-manager-memory-and-performance-overview"></a>身分證 Manager 記憶體和效能概觀

## <a name="memory"></a>記憶體
身分證管理員現在可讓它有已配置的持續性記憶體保留在標題空間。 呼叫也可以指定自訂的記憶體配置器，以供社交 manager `xbox_live_services_settings::set_memory_allocation_hooks()`。 此記憶體配置器攔截程序將可能也會用於 Xbox Live API (XSAPI) 使用的未來的大型記憶體配置。

預設的社交管理員的記憶體配置最壞的情況應該大約 4.75 mb (1)。 還有一些額外負擔，社交 manager 可能會配置的 RTA 和 HTTP 的更新。 如果建立`xbox_social_user_group`從清單中，每個加入成員會佔用額外的 2.43 kb。 如果將使用者新增透過`create_social_social_user_group_from_list`， `update_social_user_group`，或新增外部標題朋友的使用者，這可能會導致 realloc，若要尋找的連續記憶體空間。

（1) 4.75 mb 來自 = 1000年`xbox_social_user`2.43 kb * 2。 2 是該社交管理員保留 double 的記憶體緩衝區。

## <a name="additional-user-limits"></a>其他的使用者限制
身分證管理員目前有圖形的每 100 位增加使用者的限制。 這是因為兩個問題：

1. RTA 訂用帳戶，使用者可以擁有最大數目為 1100年。 如果本機使用者有 1000年朋友，只剩下的其他訂用帳戶的 100。
2. 在 100 個的目前的使用者可以傳送給 PeopleHub 的批次大小的最大數量。

身分證管理員不允許從清單中社交使用者群組，以包含 100 個以上的使用者進行的通訊。 透過隨時都可以是系統中的每 100 位總其他使用者的全域限制`create_social_user_group_from_list`。

## <a name="processing-time"></a>處理時間
身分證管理員會有最糟的情況的案例 1100年使用者。 Xbox One 上社交 manager 的效能特性有壞的情況下，為 0.3 到 0.5 毫秒的 ms`do_work`根據建立的社交圖形數目。 典型的案例是 0.01 毫秒與 1000 位使用者的群組沒有群組已建立並最多 0.2 毫秒。
