---
title: 多人遊戲附錄
description: 提供連結以深入了解 Xbox Live 多人遊戲 2015年服務。
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 703e48c0f200c59fa6f9181905756ef834aabe4a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643383"
---
# <a name="multiplayer-appendix"></a>多人遊戲附錄

玩遊戲，遊戲玩家分組的組件，可讓多人遊戲系統在 Xbox One。 系統是安全、 容易使用，且有彈性，讓您不僅快速，建置簡單的功能，但也能夠建置更複雜的功能，並插入您自己的服務。

> **注意**  
這篇文章是針對進階 API 使用方式。  做為起點，請看看[多人遊戲管理員 API](../multiplayer-manager.md)可大幅簡化開發。  請讓您知道是否您發現不支援的案例在多人遊戲管理員 中的補西牆。

目前版本的多人遊戲系統是 2015年多人遊戲。 這個版本會擷取 「 遊戲的合作對象 」 概念，並用以控制遊戲的工作階段的多人連線的工作階段目錄 (MPSD)。

> **注意**  
舊版的多人遊戲系統是 2014年多人遊戲。 此版本為基礎的概念，遊戲的合作對象以及透過合作對象的遊戲中的參與。 此版本是目前已被取代，雖然它的原始碼仍隨附 XDK。 2014 多人遊戲文件集不再隨附的 XDK。 如果您需要這份文件，請使用 2014年版本的 XDK。


## <a name="in-this-section"></a>本節內容

[簡介](introduction-to-the-multiplayer-system.md)  
導入了多人連線的系統。

[多人遊戲工作階段目錄 (mpsd)](multiplayer-session-directory.md)  
說明多人連線的工作階段目錄 (MPSD)，將遊戲的多人遊戲 API 中繼資料集中跨所有標題，並管理遊戲工作階段。

[MPSD 工作階段詳細資料](mpsd-session-details.md)  
提供 MPSD 工作階段的詳細資料。

[常見的問題時調整您的標題為 2015年多人遊戲](common-issues-when-adapting-multiplayer.md)  
描述當您調整 2015年多人一起運作的項目時，請考慮常見的問題。

[SmartMatch 配對](smartmatch-matchmaking.md)  
描述使用 Xbox Live 的配對系統。

[移轉仲裁程式](migrating-an-arbiter.md)  
描述 MPSD 的移轉程序仲裁程式。

[使用 SmartMatch 配對](using-smartmatch-matchmaking.md)  
描述如何使用 SmartMatch 配對。

[多人遊戲如何的](multiplayer-how-tos.md)  
提供使用 2015年多人在標題中的程序。

[多人連線的工作階段狀態碼](multiplayer-session-status-codes.md)  
Xbox one 定義多人連線的工作階段的狀態碼。

[2015 多人遊戲常見問題和疑難排解](multiplayer-2015-faq.md)  
定義常見問題和疑難排解的多人遊戲。

[Xbox One 多人連線的工作階段目錄](xbox-one-multiplayer-session-directory.md)提供多人連線的工作階段建立使用新的 Xbox 一個多人遊戲工作階段目錄 (MPSD) 服務的概觀。

[流程的多人連線遊戲邀請](flows-for-multiplayer-game-invites.md)說明體驗參與邀請多人遊戲的其他播放程式的流程。

[可見性和 joinability 遊戲工作階段和遊戲合作對象](game-session-and-game-party-visibility-and-joinability.md)描述可見性和 joinability 相關多人連線遊戲工作階段之間的差異。
