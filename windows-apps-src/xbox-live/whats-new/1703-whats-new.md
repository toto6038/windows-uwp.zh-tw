---
title: 新功能的 Xbox Live SDK-2017 年 3 月
description: 新功能的 Xbox Live SDK-2017 年 3 月
ms.assetid: 03180585-6f87-4929-acfc-750bd78988a0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: be8127e01d8eaae96a1d71f71967a653c00b0280
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595343"
---
# <a name="whats-new-for-the-xbox-live-sdk---march-2017"></a>新功能的 Xbox Live SDK-2017 年 3 月

請參閱[的新功能-2016 年 12 月](1612-whats-new.md)文章新增了哪些功能在 2016 年 12 月版本。

## <a name="xbox-services-api"></a>Xbox 服務 API

### <a name="data-platform-2017"></a>資料平台 2017

我們引進了一個簡化的統計資料 API。  過去您必須傳送事件對應至 stat XDP 或合作夥伴中心上所定義的規則，這些會更新統計資料的值，在雲端中。  我們將此模型稱為 Stats 2013。

使用統計資料 2017 時，您的標題隨即出現在控制項狀態的值。  您只要呼叫 API 的最新的統計資料值，並可取得傳送至服務直接而不需要的事件。  這會使用新`StatsManager`API，您可以深入了解[播放程式統計資料](../leaderboards-and-stats-2017/player-stats.md)

### <a name="github"></a>GitHub

Xbox Live API (XSAPI) 現在是在 GitHub 上的可用[ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api)。  隨附的二進位檔的 XDK，或是做為 NuGet 套件仍建議使用，不過您可以自由地使用來源，並歡迎原始碼程式碼參與。  

## <a name="xbox-live-creators-program"></a>Xbox Live 創作者計畫

Xbox Live 創作者計劃是開發人員計劃，提供開發人員接觸到的 Xbox Live 功能的子集。  如果您已在ID@Xbox程式，這不會對您造成任何影響。

您可以深入了解中的程式[開發人員計劃概觀](../developer-program-overview.md)。

## <a name="documentation"></a>文件

有下列的新文件

| 文章 | 描述 |
|---------|-------------|
|[Xbox Live 服務組態](../xbox-live-service-configuration.md) | 執行您的 Xbox Live 標題的服務組態的更新的資訊
| [設定 Xbox Live 中 Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | 在 Unity 設定適用於 Xbox Live 創作者計劃開發人員的新資訊 |
| [Xbox Live 沙箱](../xbox-live-sandboxes.md) | Xbox Live 的沙箱和內容隔離的簡化的指引 |
| [Xbox Live 的測試帳戶](../xbox-live-test-accounts.md) | 有關測試帳戶的工作，以及如何建立它們在合作夥伴中心 |
