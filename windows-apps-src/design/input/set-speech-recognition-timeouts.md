---
author: Karl-Bridge-Microsoft
Description: Set how long a speech recognizer ignores silence or unrecognizable sounds (babble) and continues listening for speech input.
title: 設定語音辨識逾時
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: speech, voice, speech recognition, natural language, dictation, input, user interaction, 語音, 聲音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 00923b4448d96943cf00eade46c39c42e87c4f96
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6977918"
---
# <a name="set-speech-recognition-timeouts"></a>設定語音辨識逾時


設定語音辨識器忽略靜音或無法辨識的聲音 (Babble) 並繼續聆聽語音輸入的時間長度。

> **重要 API**：[**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)、[**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)

## <a name="set-a-timeout"></a>設定逾時


在這裡，我們會指定各種 [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253) 值：

-   InitialSilenceTimeout - SpeechRecognizer 偵測靜音 (在產生任何辨識結果之前) 並且假設語音輸入不是即將進行的時間長度。
-   BabbleTimeout - SpeechRecognizer 在假設語音輸入已結束並且完成辨識作業之前，繼續聆聽無法辨識音效 (babble) 的時間長度。
-   EndSilenceTimeout - SpeechRecognizer 偵測靜音 (在產生辨識結果之前) 並且假設語音輸入已結束的時間長度。

**注意：** 逾時可以設定每個辨識器為基礎。

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>相關文章


* [語音互動](speech-interactions.md)
**範例**
* [語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




