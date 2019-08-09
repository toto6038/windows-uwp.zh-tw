---
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: 本文列出 UWP app 支援的透過 HTTP 的動態彈性資料流 (DASH) 設定檔。
title: 透過 HTTP 的動態彈性資料流 (DASH) 設定檔支援
ms.date: 02/15/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 268020c06be57d8ac300f1202046e52b6e1d2507
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867388"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>透過 HTTP 的動態彈性資料流 (DASH) 設定檔支援


## <a name="supported-dash-profiles"></a>支援的 DASH 設定檔
下表列出 UWP app 所支援的 DASH 設定檔。

|Tag | 資訊清單類型 | 注意|7 月發行的 Windows 10|Windows 10，版本 1511|Windows 10，版本 1607 |Windows 10，版本 1607 |Windows 10，版本 1703| Windows 10 版本1809
|----------------|------|-------|-----------|--------------|---------|-------|--------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Static |     |支援            |  支援              | 支援        |支援| 支援| 支援|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | 盡力而為 | 支援            |  支援              | 支援        |支援| 支援| 支援|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 動態 | 區隔範本支援 $Time$ 但不支援 $Number$ | 不支援            | 不支援              | 不支援        |不支援| 支援| 支援|
|urn:mpeg&#58;dash:profile:isoff-on-demand:2011 |        |  | 不支援            |  不支援              | 不支援        |不支援| 不支援| 支援|


## <a name="unsupported-dash-profiles"></a>不支援的 DASH 設定檔
不支援上表未列出的設定檔，包括但不限於下列項目︰

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [彈性資料流程](adaptive-streaming.md)
 

 




