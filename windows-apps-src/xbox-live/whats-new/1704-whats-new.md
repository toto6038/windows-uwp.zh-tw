---
title: 新功能的 Xbox Live api-2017 年 4 月
description: 新功能的 Xbox Live api-2017 年 4 月
ms.assetid: ''
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 競技場、 比賽
ms.localizationpriority: medium
ms.openlocfilehash: a9256bfce05c14a0bfa63c7ec656dd899648d844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651303"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>新功能的 Xbox Live api-2017 年 4 月

請參閱[的新功能-2017 年 3 月](1703-whats-new.md)的新增功能的文章在 2017 年 3 月版本。

## <a name="xbox-services-apis"></a>Xbox 服務 Api

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xbox Live Api 已更新為支援 Visual Studio 2017 中，針對通用 Windows 平台 (UWP) 及 Xbox One 的標題。

### <a name="tournaments"></a>比賽

新的 Api 已新增以支援比賽。 您現在可以使用`xbox::services::tournaments::tournament_service`類別來存取比賽服務從您的標題。

這些新聯賽 Api 適用於下列案例：

* 查詢服務，以尋找目前的標題中的所有現有的比賽。
* 從服務擷取聯賽的相關詳細資料。
* 查詢服務，以擷取一份聯賽的小組。
* 從服務擷取相關的小組聯賽。 詳細資料。
* 追蹤變更比賽與小組使用即時活動 (RTA) 的訂用帳戶。
