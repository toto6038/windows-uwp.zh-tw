---
ms.assetid: 66a9cfe2-b212-4c73-8a36-963c33270099
description: 本文列出適用於 UWP 應用程式支援的 HTTP 即時資料流 (HLS) 通訊協定標記。
title: HTTP 即時資料流 (HLS) 標記支援
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7c6664d13e76a5774172094d632de9db25109fdc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613083"
---
# <a name="http-live-streaming-hls-tag-support"></a>HTTP 即時資料流 (HLS) 標記支援
下表列出適用於 UWP 應用程式支援的 HLS 標記。

> [!NOTE] 
> 以 "X-" 開頭的自訂標記可做為定時中繼資料來存取，如[媒體項目、播放清單與曲目](media-playback-with-mediasource.md)文章中所述。

|Tag |HLS 通訊協定版本所引進|HLS 通訊協定文件草稿版本|用戶端上的必要項|7 月發行的 Windows 10|Windows 10，版本 1511|Windows 10，版本 1607 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  基本標記                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|必要|支援|支援|支援|
| 4.3.1.2.  EXT-X-VERSION |2|3|必要|支援|支援|支援
|4.3.2.  媒體區段標記                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|必要|支援|支援|支援
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|選用|支援|支援|支援|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|選用|支援|支援|支援|
| 4.3.2.4.  EXT-X-KEY |1|0|選用|支援|支援|支援|
|&nbsp;&nbsp;&nbsp; 方法|1|0|屬性|"NONE, AES-128"|"NONE, AES-128"|"NONE, AES-128, SAMPLE-AES"|
|&nbsp;&nbsp;&nbsp; URI|1|0|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp; IV|2|3|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|屬性|不支援|不支援|不支援|
| 4.3.2.5.  EXT-X-MAP |5|9|選用|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp; URI|5|9|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|屬性|不支援|不支援|不支援|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|選用|不支援|不支援|不支援|
|4.3.3.  媒體播放清單標記                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|必要|支援|支援|支援|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|選用|支援|支援|支援|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|選用|不支援|不支援|不支援|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|選用|支援|支援|支援|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|選用|支援|支援|支援|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|選用|不支援|不支援|不支援|
|4.3.4.  主要播放清單標記                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|選用|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  型別|4|7|屬性|"AUDIO, VIDEO"|"AUDIO, VIDEO"|"AUDIO, VIDEO, SUBTITLES"|
|&nbsp;&nbsp;&nbsp;  URI|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  群組識別碼|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  語言|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  ASSOC 語言|6|13|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  名稱|4|7|屬性|不支援|不支援|支援|
|&nbsp;&nbsp;&nbsp;  預設值|4|7|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  自動選擇|4|7|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  強制|5|9|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  針對識別碼|6|12|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  特性|5|9|屬性|不支援|不支援|不支援|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|必要|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  頻寬|1|0|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  程式識別碼|1|0|屬性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  平均頻寬|7|14|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  轉碼器|1|0|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  解決方式|2|3|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  畫面播放速率|7|15|屬性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  音訊|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  影片|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  翻譯字幕|5|9|屬性|不支援|不支援|支援|
|&nbsp;&nbsp;&nbsp;  隱藏式字幕|6|12|屬性|不支援|不支援|不支援|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|選用|不支援|不支援|不支援|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|選用|不支援|不支援|不支援|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|選用|不支援|不支援|不支援|
|4.3.5.  媒體或主要播放清單標記                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|選用|不支援|支援|支援|
| 4.3.5.2.  EXT-X-START  |6|12|選用|不支援|部分支援|部分支援|
|&nbsp;&nbsp;&nbsp;  時間位移|6|12|屬性|不支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  精確|6|12|屬性|不支援|支援預設 "NO"|支援預設 "NO"|



## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [調適性串流處理](adaptive-streaming.md)
 

 




