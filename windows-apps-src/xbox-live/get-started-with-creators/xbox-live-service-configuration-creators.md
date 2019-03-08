---
title: Xbox Live 創作者的服務組態
description: 深入了解 Xbox Live 創作者計劃的服務組態。
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.date: 10/03/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 76d25a48caadb908e30e6e1897c19178e2b837e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662233"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>創作者計畫的 Xbox Live 服務設定

## <a name="what-is-service-configuration"></a>什麼是服務組態？

您可能熟悉的一些 Xbox Live 功能這類[排行榜](../leaderboards-and-stats-2017/leaderboards.md)並[連接的儲存體](../storage-platform/connected-storage/connected-storage-technical-overview.md)。

如果您不這樣做，我們將簡短說明排行榜做為範例。 排行榜讓玩家能看到值，表示相較於其他玩家的成就。 比方說高分數的 arcade 遊戲、 單圈時間在賽車遊戲中或在第一人稱射擊的致勝。 但不同於 arcade 機器只顯示最高的分數，從該實體機器已播放的球員，在 Xbox live 就可以顯示從世界各地的最高得分。

但針對此動作，您需要執行一些一次性設定，以便 Xbox Live 知道您排行榜。 比方說的值是否應該以遞增或遞減值，排序和資料的哪個組件它應該會排序。

此設定會在[合作夥伴中心](https://partner.microsoft.com/dashboard)如 Xbox Live 創作者計劃，而且您可以閱讀[開始啟動與 Xbox Live](get-started-with-xbox-live-creators.md)以了解如何進行設定。

## <a name="get-your-ids"></a>取得您的識別碼

若要啟用 Xbox Live 服務，您必須取得數個設定您的開發環境和標題的識別碼。 這些可以藉由更新您的 Xbox Live 服務組態取得。

如果您目前沒有標題在合作夥伴中心，請參閱[建立及測試新的建立者標題](create-and-test-a-new-creators-title.md)指導方針。

### <a name="critical-ids"></a>重要識別碼

有三個識別碼是很重要的標題和 Xbox one 的應用程式開發： 沙箱識別碼、 標題識別碼和服務組態識別碼 (SCID)。

必須要有一個沙箱識別碼，以設定您的開發環境時，標題的識別碼和 SCID 不需要進行初始開發，但所需的任何使用 Xbox Live 服務。 因此，建議您取得所有的三次。 您可以檢視所有的識別碼"Xbox Live"根組態 頁面上，如下所示：

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>沙箱識別碼

沙箱提供內容隔離您的環境，在開發期間，確定它全新的開發和測試您的標題。 沙箱識別碼會識別您的沙箱。 雖然多個主控台可存取一個沙箱主控台只可以在任何時候，存取一個沙箱。

沙箱識別碼會區分大小寫。

#### <a name="service-configuration-id-scid"></a>服務設定識別碼 (SCID)

開發的過程中，您將建立統計資料、 排行榜及許多其他線上的功能。 這些都是屬於您的服務組態，並且需要存取 SCID。 SCIDs 會區分大小寫。

#### <a name="title-id"></a>標題的識別碼

標題的識別碼可唯一識別您的 Xbox Live 服務的標題。 它用在整個服務來讓使用者存取項目的即時內容，他們的使用者統計資料、 成積、 和等，並啟用即時多人遊戲的功能。

標題的識別碼可以是區分大小寫，根據使用方式和位置。

## <a name="publish-your-xbox-live-service-configuration"></a>發行您的 Xbox Live 服務組態

當您變更到 Xbox Live 設定您的遊戲，您需要在 Xbox Live 的其餘部分所見，並可以看到您的遊戲之前發行變更。 當您仍然使用您的遊戲時，您會發佈到您自己開發的沙箱。 開發沙箱可讓您能夠對您的遊戲在隔離的環境中的變更。 在公開發行您的遊戲時，Xbox Live 的組態將會自動發佈至零售沙箱。
根據預設，一個 Xbox 和 Windows 10 電腦是零售沙箱中。

在 Xbox Live 設定 頁面上，按一下 **測試** 按鈕，將目前的 Xbox Live 設定發佈到您開發的沙箱。

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)