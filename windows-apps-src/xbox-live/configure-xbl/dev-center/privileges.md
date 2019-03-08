---
title: 在合作夥伴中心內的權限設定
description: 了解如何在合作夥伴中心內設定權限
ms.date: 04/09/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live、 Xbox、 遊戲、 uwp、 windows 10，其中一個 Xbox、 權限，合作夥伴中心
ms.openlocfilehash: 2036be2a6184909799236b013c52ff4614a158aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612473"
---
# <a name="configure-privileges-in-partner-center"></a>在合作夥伴中心內設定權限

權限組態頁面即表示玩家進行串流處理至資料流服務，例如標題的限制是否[Mixer](https://mixer.com/)。 根據預設您的遊戲不會限制任何串流平台上的廣播，變更此頁面只需要您想要限制的廣播。 您可以限制廣播，有兩種。 您可能會停用廣播 everywhere 核取方塊，**預設**區段中，或者您可以藉由新增在沙箱限制廣播的沙箱**沙箱會覆寫**一節。

核取方塊，**預設**區段會限制這個項目上的所有服務與沙箱的廣播。

![預設限制的廣播](../../images/dev-center/privileges/default-privileges-check.JPG)

若要限制按一下特定的沙箱廣播**新增**按鈕**沙箱會覆寫**一節。 從下拉式清單中選擇目標沙箱並核取方塊下方來限制所選的沙箱上該標題的廣播。 取消核取或刪除來移除廣播的限制，可以是沙箱覆寫。

![受限制的沙箱廣播](../../images/dev-center/privileges/sandbox-privileges-check.JPG)

按一下 [**儲存**来保留這些設定進行任何組態變更] 按鈕。

> [!NOTE]
> 核取方塊，若要停用廣播，將只禁止 streaming 透過 Xbox 主控台或電腦上的 Windows 遊戲列完成。 在此頁面上的方塊不會防止使用擷取卡或其他外部擷取或串流服務。