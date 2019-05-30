---
Description: 設定語音辨識器忽略靜音或無法辨識的聲音 (Babble) 並繼續聆聽語音輸入的時間長度。
title: 設定語音辨識逾時
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: speech, voice, speech recognition, natural language, dictation, input, user interaction, 語音, 聲音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f321b9ec43f2c844854600b8260a7fdc189c0446
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365384"
---
# <a name="set-speech-recognition-timeouts"></a>設定語音辨識逾時


設定語音辨識器忽略靜音或無法辨識的聲音 (Babble) 並繼續聆聽語音輸入的時間長度。

> **重要的 Api**:[**逾時**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts)， [ **SpeechRecognizerTimeouts**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>設定逾時


在這裡，我們會指定各種 [**Timeouts**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts) 值：

-   InitialSilenceTimeout - SpeechRecognizer 偵測靜音 (在產生任何辨識結果之前) 並且假設語音輸入不是即將進行的時間長度。
-   BabbleTimeout - SpeechRecognizer 在假設語音輸入已結束並且完成辨識作業之前，繼續聆聽無法辨識音效 (babble) 的時間長度。
-   EndSilenceTimeout - SpeechRecognizer 偵測靜音 (在產生辨識結果之前) 並且假設語音輸入已結束的時間長度。

**附註**  逾時可以設定每個辨識器為基礎。

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>相關文章


* [語音互動](speech-interactions.md)
**範例**
* [語音辨識和語音合成器範例](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




