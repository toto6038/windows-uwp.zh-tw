---
author: Karl-Bridge-Microsoft
Description: Learn how to manage issues with speech-recognition accuracy caused by audio-input quality.
title: 管理音訊輸入的問題
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: speech, voice, speech recognition, natural language, dictation, input, user interaction, 語音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 094acdbcb5c5b3bf45bad757344be5187ae37cbc
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "7154470"
---
# <a name="manage-issues-with-audio-input"></a>管理音訊輸入的問題


了解如何管理因為音訊輸入品質而造成的語音辨識準確度問題。

> **重要 API**：[**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)、[**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)、[**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)


## <a name="assess-audio-input-quality"></a>評定音訊輸入品質


在啟用語音辨識功能的情況下，使用您語音辨識器的 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 事件，以判斷是否有一或多個音訊問題可能在干擾語音輸入。 事件引數 ([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430)) 會提供 [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) 屬性，用來描述偵測到的音訊輸入問題。

太多背景噪音、麥克風靜音，以及說話者的音量或速度，都可能會影響辨識。

我們將在此處設定語音辨識器，並開始接聽 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 事件。

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = "Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="manage-the-speech-recognition-experience"></a>管理語音辨識體驗


使用 [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) 屬性所提供的說明，來協助使用者改善辨識的條件。

我們將在此處建立 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 事件的處理常式，這個處理常式會檢查音量大小過低的情況。 然後使用 [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152) 物件，建議使用者嘗試調高說話音量。

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <a name="related-articles"></a>相關文章


* [語音互動](speech-interactions.md)

**範例**
* [語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




