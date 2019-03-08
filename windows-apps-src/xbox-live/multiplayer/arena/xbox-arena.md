---
title: Xbox 競技場
description: 了解如何使用 Xbox 競技場執行您的遊戲的比賽。
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 競技場，聯賽、 ux
ms.localizationpriority: medium
ms.openlocfilehash: b08da01323d05c961005d562b70667dbbdf85437
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643763"
---
# <a name="xbox-arena"></a>Xbox 競技場

Xbox Arena 是遊戲的建立和其目標是遊戲的最有趣的競爭帶入廣大的群眾透過 Xbox Live、 執行線上聯賽性 Xbox One 和 Windows 10 平台。
談到具競爭力的遊戲，而有我們想要盡可能有彈性的競技場，每一個發行者會有不同的需求。 因此有三種不同的方式，競技場上執行您的遊戲的比賽：

* 使用加盟執行聯賽 Xbox Live 為您提供的工具和服務來設定並執行您自己的比賽。

* Xbox 已與領先業界的聯賽召集人 (TOs)，代表標題的比賽跑競技場，您可以讓人員合作。 （如需支援的聯賽召集人的清單，請連絡 Microsoft 客戶經理）。

* 播放程式可以建立並使用我們的使用者產生的比賽或 UGTs 執行他們自己的競技場比賽。

最重要的是，將競技場的支援新增至您的標題可讓您充分利用所有這三種。 我們提供健全的 Api，可將升級聯賽中的遊戲，並促進參與者的轉換和比對的項目。 Xbox 競技場中樞支援聯賽管理工作，例如註冊、 通知、 方括號和植入，並報告結果。

> [!IMPORTANT]  
> Xbox 競技場只會提供給受管理的合作夥伴和ID@Xbox開發人員。 它不適用於 Xbox Live 創作者計劃。

## <a name="a-titles-baseline-tournament-experience"></a>項目的基準聯賽體驗

如果您的標題會與一或多個聯賽召集人整合，它必須提供基準聯賽體驗。 您也可以與特定聯賽組合管理更深入整合，如果您選擇，提供更豐富的體驗競賽。 但您仍然必須提供基準體驗進行其他的聯賽召集人，以及 UGTs 和加盟執行聯賽如果您選擇執行它們。

### <a name="baseline-requirements-for-a-title"></a>標題的基本需求

* 如需完整的需求清單，請連絡 Microsoft 客戶經理。

### <a name="ui-recommendations"></a>UI 的建議

* 找出相符項目是在 UI 中的聯賽。

* 大廳 ui 中包含的 UI 項目，將使用者連結到您聯賽的中樞和/或 Xbox 競技場 shell UI 聯賽詳細資料頁面或聯賽組合管理 應用程式。



必須提供您的標題的基準使用者體驗是簡單且有足夠的彈性，來處理許多可能的競爭對手格式。 您可以隨意調整使用者經驗指導方針和需求，以符合您的標題 UI 流程，並確保流暢的使用者體驗。

例如：

* 所需的資訊不一定會顯示在主畫面上，前提是某處，例如詳細資料頁面上所提供或快顯。

* 可能有許多版本的每個畫面中，或可能會將它們結合或使用現有的遊戲畫面。 比方說，許多遊戲都包含後續相符項目 「 遊戲報告 」 畫面上，這可能是調整以符合這兩個 「 等候結果 」 與 「 準備 」 畫面的需求。

* 不需要變更階段做為螢幕。 比方說，如果小組的階段從切換 「 準備 」 到 「 遊戲 」 但使用者卻在 「 準備 」 畫面上，您的標題不需要立即跳到遊戲。 它可以 （且不應該） 切換 「 等候相符的項目...」 按鈕的指標，比方說，「 準備好要遊戲 」 —，讓使用者在控制流程，並因此可能需要進一步了解它。 它也沒關係延後處理，直到使用者確認轉換 「 遊戲 」 階段的需求。


## <a name="arena-vs-title-roles"></a>與標題 「 角色 」 競技場

隨著整個聯賽階段進度進出遊戲時，會變得複雜的使用者。 每種遊戲中扮演不同的程序時，沒有更少機會記下的位置和發生的情況。

> [!TIP]
> **UX 建議**  
>
> 簡化遊戲與 Xbox 競技場 UI 之間的功能角色仍可維持清楚分辨。 比方說，競技場，在完成所有管理相關的工作，而遊戲內都完成所有遊戲的 play 相關工作。

Xbox 競技場角色 （設定聯賽）   | 項目的角色 （遊戲）
--- | ---
<ul><li>註冊和簽入</li><li>通知</li><li>植入及方括號</li><li>團隊資訊</li></ul> |     <ul><li>Arena UI 的參與者的轉換</li><li>識別多人遊戲大廳 UI 中的特定聯賽的相符項目</li><li>升級和/或瀏覽的比賽中的遊戲</li></ul>

## <a name="engineering-guidance"></a>工程設計指引

文章 | description
--- | ---
[Arena 標題整合](arena-title-integration.md) | 了解如何將 Xbox 競技場支援整合到您的標題。

## <a name="operations-guidance"></a>操作指南

文章 | description
--- | ---
[Xbox 競技場作業入口網站](operations-portal.md) | 描述您可用來建立和管理項目的 Xbox 競技場與整合的官方比賽作業入口網站。

## <a name="user-experience-guidance"></a>使用者體驗指南

文章 | description
--- | ---
[探索 Xbox 比賽](discovering-xbox-tournaments.md) | 提供秘訣和建議，以製作絕佳的使用者體驗來探索現有的比賽。
[加入聯賽](arena-ux-join-tournament.md)  |  提供的註冊和加入聯賽秘訣和建議，以製作絕佳的使用者經驗。
[比對 engagement](arena-ux-match-engagement.md) | 描述播放程式進行聯賽的階段的使用者體驗。
[Arena API UI 中繼資料](arena-apis-metadata.md)  | 描述您可用來顯示聯賽的目前狀態的相關資訊中的遊戲競技場 Api 所傳回的中繼資料。
[Arena 通知](arena-notifications.md)  | 當 Xbox 競技場聯賽的參與者，以傳送通知時，請描述的條件。
[Arena 使用者案例](arena-user-scenarios.md)  | 描述玩家以其最常見的動機競技場案例。
