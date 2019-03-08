---
title: 成就 2017
description: 成就 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bfc67f6aca27abf095a89c451111e6429bca82e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590983"
---
# <a name="achievements-2017"></a>成就 2017

成就 2017年系統可讓遊戲開發人員使用直接呼叫模型來解除鎖定新 Xbox Live 的遊戲，Xbox One、 Windows 10，Windows 10 Phone、 Android 及 iOS 上的成就。

## <a name="introduction"></a>簡介

Xbox One，我們引進了新的 Cloud-Powered 成就系統，可讓遊戲開發人員只需傳送遊戲內遙測事件驅動其 Xbox Live 功能，例如使用者統計資料、 成就、 豐富的目前狀態和多人遊戲的資料。 這已開啟許多新權益-單一事件可以更新資料的多個的 Xbox Live 功能;Xbox Live 設定所在的伺服器，而不是在用戶端;另外還有更多功能。

年來，Xbox One 推出之後，我們已聽到密切遊戲的開發人員的意見反應，並且開發人員有一致的方式共用下列：

1.  **想要解除鎖定透過直接呼叫模式的成就。** 許多開發人員建置適用於各種平台，包括舊版 Xbox 的遊戲和類似的成就的系統，在這些平台上使用直接呼叫的方法。 支援直接解除鎖定在 Xbox One 上的呼叫和其他目前 gen Xbox 平台會簡化跨平台遊戲開發的需求，以及開發時間成本。

2.  **減少組態複雜度。** 與 Cloud-Powered 成就系統的成就解除鎖定的邏輯必須在 Xbox Live 設定，好讓服務知道如何解譯項目的統計資料，以及何時解除鎖定使用者的成就。 這是透過原本不存在的成就設定新的成就規則區段。 而需要解除鎖定在雲端中的邏輯可以是相當強大，這個額外的組態需求會增加至設計和建立項目的成就的複雜度。

3.  **難以進行疑難排解。** 雖然 Cloud-Powered 成就系統導入的各種實用的功能，也很難遊戲開發人員驗證，以及疑難排解其成就的問題，因為成就解除鎖定所間接觸發的規則live 服務上，而非直接受遊戲本身。

值得注意的是，遊戲開發人員也重複共用它們意識的意見反應和值所導入 Cloud-Powered 成就系統以及特定功能：

1.  新的使用者體驗功能，例如成就進展、 即時的更新、 概念封面獎勵和張貼到活動摘要會解除鎖定。

2.  組態的改進，例如服務組態，而不是必須包含在遊戲套件的本機設定 (也就是 gameconfig，XLAST，SPA，等等) 和能夠輕鬆地編輯成就字串 & 遊戲已出貨之後，映像。

使用成就 2017 時，我們要建置取代的現有 Cloud-Powered 成就系統未來的項目，若要使用可設定成積、 整合的 Xbox 遊戲開發人員更容易成就解除鎖定 & 到更新遊戲程式碼，並驗證成就會如預期般運作。

## <a name="whats-different-with-achievements-2017"></a>有何不同成就 2017

|                          | 成就 2017年系統        | 雲端架構的成就系統      |
|--------------------------|---------------------------------------|----------------------------------------|
| 解除鎖定觸發程序           | 直接透過 API 呼叫                 | 間接透過遙測事件        |
| 解除鎖定擁有者             | 標題                                 | Xbox Live                              |
| 設定            | 字串、 影像獎勵              | 字串、 影像獎勵，解除鎖定規則\[+ 統計資料，+ 事件\]                    |
| 進展              | 支援 <br>*直接透過 API 呼叫*                | 支援 <br> *間接透過遙測事件*       |
| 即時活動 (RTA) | 支援                             | 支援                              |
| 挑戰               | 不支援   | 支援                      |

## <a name="title-requirements"></a>標題需求

以下是任何標題，其中會使用成就 2017年系統需求。

1.  **必須是新的 （未發行） 標題。** 已發行和使用 Cloud-Powered 成就系統的項目不符合資格。 如需詳細資訊，請參閱[為什麼無法現有標題 「 移轉 」 到新的成就 2017年系統？](#_Why_can’t_existing)

2.  **必須使用年 8 月 2016 XDK 或更新版本。** 8 月 2016 XDK 發行 Update_Achievement API。

3.  **必須是 XDK 或 UWP 的標題。** 成就 2017 system 不適用於舊版的平台，包括 Xbox 360、 Windows 8.x 或更舊版本，或 Windows Phone 8 或更舊版本。

## <a name="updateachievement-api"></a>Update_Achievement API

一旦您的成就透過 XDP 來設定或[UDC](../configure-xbl/dev-center/achievements-in-udc.md)並發行至您的開發人員沙箱時，您的標題可以解除鎖定方法是呼叫 Update_Achievement API。

XDK 和 Xbox Live SDK 中可用的 API。

### <a name="api-signature"></a>API 簽章

API 簽章如下所示：

```c++
/// <summary>
    /// Allow achievement progress to be updated and achievements to be unlocked.  
    /// This API will work even when offline. On PC and Xbox One, updates will be posted by the system when connection is re-established even if the title isn't running
    /// </summary>
    /// <param name="xboxUserId">The Xbox User ID of the player.</param>
    /// <param name="titleId">The title ID.</param>
    /// <param name="serviceConfigurationId">The service configuration ID (SCID) for the title.</param>
    /// <param name="achievementId">The achievement ID as defined by XDP or Partner Center.</param>
    /// <param name="percentComplete">The completion percentage of the achievement to indicate progress.
    /// Valid values are from 1 to 100. Set to 100 to unlock the achievement.  
    /// Progress will be set by the server to the highest value sent</param>
    /// <remarks>
    /// Returns a task<T> object that represents the state of the asynchronous operation.
    ///
    /// This method calls V2 GET /users/xuid({xuid})/achievements/{scid}/update
    /// </remarks>
    _XSAPIIMP pplx::task<xbox::services::xbox_live_result<void>> update_achievement(
        _In_ const string_t& xboxUserId,
        _In_ uint32_t titleId,
        _In_ const string_t& serviceConfigurationId,
        _In_ const string_t& achievementId,
        _In_ uint32_t percentComplete
        );
```

`xbox::services::xbox_live_result<T>` 是針對所有的 c + + 的 Xbox Live 服務 API 呼叫傳回的呼叫。

如需詳細資訊，請參閱 Xfest 2015 演講，"XSAPI:C + +，任何例外狀況 ！ 」<br>
[video](https://go.microsoft.com/?linkid=9888207) |  [slides](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>透過 Update_Achievement API 解除鎖定

若要解除鎖定的成就，設定*percentComplete*到 100 之間。

如果使用者在線上時，要求會立即傳送到 Xbox Live 的成就服務，並會觸發下列的使用者體驗：

-   使用者會收到的成就解除鎖定的通知;

-   指定的成就標題; 使用者的成就清單中，會出現為已解除鎖定

-   解除鎖定的成就會新增至使用者的活動摘要。

> *注意：會使用成就 2017年系統的成就和 Cloud-Powered 成就的使用者體驗中不可見的差異。*

如果使用者已離線，解除鎖定要求都會排入本機使用者的裝置上。 當使用者的裝置已重新建立網路連線，則要求會自動傳送至成就服務，請注意： 不須執行動作來觸發這 – 遊戲，而且所述，會發生上述的使用者體驗。

### <a name="updating-completion-progress-via-updateachievement-api"></a>透過 Update_Achievement API 更新的完成進度

若要更新使用者的進度解除鎖定的成就，設定*percentComplete*為最適當的整數 1 到 100 之間。

只能增加成就的進度。 如果*percentComplete*設定為小於數字比成就的最後*percentComplete*值，更新將會被忽略。 例如，如果成就*percentComplete*先前已設定為 75，傳送更新 25 的值將會忽略和成就仍會顯示為完整的 75%。

如果*percentComplete*設為 100，將會解除鎖定成就。

如果*percentComplete*設為大於 100 的數字，API 就會如同您將它設定為剛好為 100。

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>我是否可以寄送我尚未使用成就 2017年系統的標題？

沒錯 ！ 所有新的標題是歡迎並鼓勵使用成就 2017年代替 Cloud-Powered 成就系統的系統。

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>為什麼挑戰中不支援的成就 2017年系統？

挑戰的呈現方式與目前的實作不滿足大多數的遊戲開發人員需要顯示跨 Xbox 遊戲的使用方式資料。 我們將繼續收集開發人員輸入與此空間中的意見反應，並儘可能提供未來會有開發人員需求的點會有更多的功能。 新發行的 Xbox Arena 是具競爭力的新功能介紹 Xbox 功能的範例遊戲新，但類似，方向。

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>我仍然可以新增新的成就每個日曆季如果我的標題使用成就 2017年系統嗎？

是。 成就原則維持不變。

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>為什麼無法現有標題 「 移轉 」 到新的成就 2017年系統？

絕大多數的現有項目中，「 移轉 」 至成就 2017年系統不會限制在只更新其服務組態和成就的事件寫入交換解除鎖定呼叫 –，雖然這些變更只會有非常成本高昂且會包含錯誤和非預期的行為，可能會無法挽回地損毀的成就中會造成相當重大的風險。 相反地，大部分現有的項目也會有使用者使用現有的資料。 嘗試轉換已在使用系統會 Cloud-Powered 成就的即時遊戲是成本相當高的投入時間，開發人員和 Xbox 不僅會大幅危及，現有的使用者設定檔和/或遊戲體驗。

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>如果我的標題發行使用 Cloud-Powered 成就系統，可以任何未來 DLC 標題切換到成就 2017年嗎？

所有的成就，標題必須使用相同的成就系統。 基底的遊戲成就等使用任何成就系統是必須針對所有未來的成就，標題使用的系統。

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>同時我開發沙箱中測試的成就，我可以混合和搭配使用成就 2017年系統和 Cloud-Powered 成就系統之間嗎？

不。 所有的成就，標題必須使用相同的成就系統。

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>成就 2017年也包含離線解除鎖定？

如果裝置離線時，標題會解除鎖定的成就，Update_Achievement API 會自動離線解除鎖定要求排入佇列，並當裝置已重新建立類似於目前的網路連線時，會自動同步處理到 Xbox Live雲端架構的成就系統的離線的體驗。 成就解除鎖定將不會發生使用者處於離線狀態。

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>我看到 XDP 中新的 「 AchievementUpdate 」 事件。 如果我的標題會使用該事件，表示它有成就 2017年嗎？

*AchievementUpdate*基底事件進行後端時，才由 Xbox Live。 您可以放心忽略它。 如果您的標題設定使用此基底的事件類型的事件，這些事件寫入將會忽略 Xbox Live。 建置在 Cloud-Powered 成就系統的標題應該繼續使用其他的基底事件類型來設定它們的事件。 不需要設定成就 2017年系統為基礎的標題*任何*成就用途的事件。
