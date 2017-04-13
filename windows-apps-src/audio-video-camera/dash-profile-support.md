---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: "本文列出 UWP app 支援的透過 HTTP 的動態彈性資料流 (DASH) 設定檔。"
title: "透過 HTTP 的動態彈性資料流 (DASH) 設定檔支援"
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9ca8f2d1783a73ae38fed1d3ee1e58b809ddd2aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>透過 HTTP 的動態彈性資料流 (DASH) 設定檔支援
下表列出 UWP app 支援的 DASH 設定檔。



|標記 | 資訊清單類型 | 附註|7 月發行的 Windows 10|Windows 10，版本 1511|Windows 10，版本 1607 |Windows 10，版本 1607 |Windows 10，版本 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg:dash:profile:isoff-live:2011 | 靜態 |     |支援            |  支援              | 支援        |支援| 支援|
|urn:mpeg:dash:profile:isoff-main:2011 |        | 盡力而為 | 支援            |  支援              | 支援        |支援| 支援|
|urn:mpeg:dash:profile:isoff-live:2011 | 動態 | 區隔範本支援 $Time$ 但不支援 $Number$ | 不支援            | 不支援              | 不支援        |不支援| 支援|


## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [彈性資料流](adaptive-streaming.md)
 

 




