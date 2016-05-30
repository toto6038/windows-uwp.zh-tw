---
author: Karl-Bridge-Microsoft
Description: 設定語音辨識器忽略靜音或無法辨識的聲音 (Babble) 並繼續聆聽語音輸入的時間長度。
title: 設定語音辨識逾時
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
---

# 設定語音辨識逾時
設定語音辨識器忽略靜音或無法辨識的聲音 (Babble) 並繼續聆聽語音輸入的時間長度。

**重要 API**

-   [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)
-   [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)


## <span id="Set_a_timeout"></span><span id="set_a_timeout"></span><span id="SET_A_TIMEOUT"></span>設定逾時


在這裡，我們會指定各種 [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253) 值：

-   InitialSilenceTimeout - SpeechRecognizer 偵測靜音 (在產生任何辨識結果之前) 並且假設語音輸入不是即將進行的時間長度。
-   BabbleTimeout - SpeechRecognizer 在假設語音輸入已結束並且完成辨識作業之前，繼續聆聽無法辨識音效 (babble) 的時間長度。
-   EndSilenceTimeout - SpeechRecognizer 偵測靜音 (在產生辨識結果之前) 並且假設語音輸入已結束的時間長度。

**注意：**每個辨識器都可以設定逾時。

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <span id="related_topics"></span>相關文章


* [語音互動](speech-interactions.md) 
           **範例**
* [語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 






<!--HONumber=May16_HO2-->


