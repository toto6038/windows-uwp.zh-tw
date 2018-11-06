---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: 本文列出 UWP app 支援的透過 HTTP 的動態彈性資料流 (DASH) 設定檔。
title: 透過 HTTP 的動態彈性資料流 (DASH) 設定檔支援
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a4ec9f9e81010d39af496da156afa676f4b3714
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6042983"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>透過 HTTP 的動態彈性資料流 (DASH) 設定檔支援


## <a name="supported-dash-profiles"></a>支援的 DASH 設定檔
下表列出 UWP app 所支援的 DASH 設定檔。

|標記 | 資訊清單類型 | 附註|7 月發行的 Windows 10|Windows 10，版本 1511|Windows 10，版本 1607 |Windows 10，版本 1607 |Windows 10，版本 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 靜態 |     |支援            |  支援              | 支援        |支援| 支援|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | 盡力而為 | 支援            |  支援              | 支援        |支援| 支援|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 動態 | 區隔範本支援 $Time$ 但不支援 $Number$ | 不支援            | 不支援              | 不支援        |不支援| 支援|


## <a name="unsupported-dash-profiles"></a>不支援的 DASH 設定檔
不支援上表未列出的設定檔，包括但不限於下列項目︰

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:isoff-on-demand:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [彈性資料流](adaptive-streaming.md)
 

 




