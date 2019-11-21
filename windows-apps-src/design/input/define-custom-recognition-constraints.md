---
Description: 了解如何定義及使用自訂限制式來進行語音辨識。
title: 定義自訂辨識限制式
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: speech, voice, speech recognition, natural language, dictation, input, user interaction, 語音, 聲音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b94c946222f510c7f1b1f7619b67ee83e6c2256
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258008"
---
# <a name="define-custom-recognition-constraints"></a>定義自訂辨識限制式

了解如何定義及使用自訂限制式來進行語音辨識。

> **重要 API**：[**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)、[**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)、[**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)

語音辨識至少需要一個限制式來定義可辨識的詞彙。 如果沒有指定任何限制式，則會使用預先定義的通用 Windows app 聽寫文法。 請參閱[語音辨識](speech-recognition.md)。

## <a name="add-constraints"></a>新增限制式

使用 [**SpeechRecognizer.Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 屬性可以為語音辨識器新增限制式。

這裡涵蓋了三種從 app 內部使用的語音辨識限制式。 （如需 Cortana 語音命令條件約束，請參閱[在 cortana 中使用語音命令啟動前景應用程式](https://docs.microsoft.com/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana)）。

- [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)—以預先定義的文法（聽寫或 web 搜尋）為基礎的條件約束。
- [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)—根據單字或片語清單的條件約束。
- [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)-在語音辨識文法規格（SRGS）檔案中定義的條件約束。

每個語音辨識器可以有一個限制式集合。 只有下列限制式組合是有效的：

- 單一主題限制式 (口述或網頁搜尋)
- 針對 Windows 10 Fall Creators Update (10.0.16299.15) 與更新版本，單一主題限制式可和清單限制式組合使用
- 清單限制式和/或文法檔限制式的組合。

> [!Important]
> 先呼叫 **[SpeechRecognizer.CompileConstraintsAsync](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)** 方法編譯限制式，再開始辨識處理序。

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>指定網頁搜尋文法 (SpeechRecognitionTopicConstraint)

必須將主題限制式 (口述或網頁搜尋文法) 新增到語音辨識器的限制式集合。

> [!NOTE]
> 您可以使用 [SpeechRecognitionListConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) 搭配 [SpeechRecognitionTopicConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)，透過提供一組您認為可能在聽寫時用到的網域特定關鍵字，來提高聽寫正確性。

在這裡，我們將把網頁搜尋文法新增到限制式集合。

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
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
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

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>指定程式設計清單限制式 (SpeechRecognitionListConstraint)

必須將清單限制式新增到語音辨識器的限制式集合。

請記住下列重點：

- 您可以將多個清單限制式新增到限制式集合。
- 您可以將實作 **IIterable&lt;String&gt;** 的任何集合用於字串值。

在這裡，我們將透過程式設計方式指定一個字詞陣列做為清單限制式，並將它新增到語音辨識器的限制式集合。

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'yes', 'no'";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>指定 SRGS 文法限制式 (SpeechRecognitionGrammarFileConstraint)

必須將 SRGS 文法檔新增到語音辨識器的限制式集合。

SRGS 版本 1.0 是業界標準的標記語言，用於建立語音辨識適用的 XML 格式文法。 雖然通用 Windows 應用程式提供使用 SRGS 建立語音辨識文法的替代方案，但您會發現，使用 SRGS 建立文法可產生最佳結果，尤其是在更為複雜的語音辨識案例上。

SRGS 文法提供完整的功能集，可幫助您為應用程式建構複雜的語音互動。 例如，您可以使用 SRGS 文法：

- 指定字詞和片語的說出順序，以進行識別。
- 合併要辨識的多個清單和片語中的字詞。
- 連結至其他文法。
- 指派替代字詞或片語的加權，以增加或減少其用於比對語音輸入的可能性。
- 包含選擇性字詞或片語。
- 使用特殊規則以協助篩選未指定或非預期的輸入，例如，不符合文法的隨機語音或背景噪音。
- 使用語意，定義語音識別對您應用程式的意義。
- 以內嵌於文法或透過語彙連結的方式指定發音。

如需 SRGS 元素與屬性的詳細資料，請參閱 [SRGS 文法 XML 參考](https://msdn.microsoft.com/library/hh361653)。 如果要開始建立 SRGS 文法，請參閱[如何建立基本 XML 文法](https://msdn.microsoft.com/library/hh361658)。

請記住下列重點：

- 您可以將多個文法檔限制式新增到限制式集合。
- 針對符合 SRGS 規則的 XML 型文法文件，請使用 .grxml 副檔名。

這個範例會使用名為 srgs.grxml (稍後會有說明) 的檔案中定義的 SRGS 文法。 在檔案屬性中， **[封裝動作]** 是設定為 **[內容]** ，而 **[複製到輸出目錄]** 是設定為 **[ 永遠複製 ]** ：

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'blue background', 'green text'";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

這個 SRGS 檔案 (srgs.grxml) 包含語意轉譯標記。 這些標記提供一個將文法相符資料傳回給 app 的機制。 文法必須符合 [語音辨識（SISR） 1.0](https://www.w3.org/TR/semantic-interpretation/)規格的全球資訊網協會（W3C）語義轉譯。

如下，我們將接聽各種不同形式的 "yes" 和 "no"。

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## <a name="manage-constraints"></a>管理限制式

載入限制集合以執行辨識後，透過將限制的 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) 屬性設為 **true** 或 **false**，您的 app 可以管理要啟用哪些限制以執行辨識操作。 預設設定是 **true**。

與針對每項辨識操作載入、卸載及編譯限制式相比，先一次載入限制式，再視需要予以啟用及停用，通常是較有效率的方式。 請視需要使用 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) 屬性。

對限制式的數目加以限制可以限制語音辨識器針對語音輸入所需搜尋及比對的資料量。 這樣可以同時改善語音辨識的效能和準確度。

請根據應用程式在目前辨識操作的內容中可預期的片語，決定要啟用的限制式。 例如，如果目前的應用程式內容是要顯示色彩，您可能就不需要啟用辨識動物名稱的限制式。

若要提示使用者可以說出什麼內容，請使用 [**SpeechRecognizerUIOptions.AudiblePrompt**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.audibleprompt) 與 [**SpeechRecognizerUIOptions.ExampleText**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.exampletext) 屬性，這兩個屬性是藉由 [**SpeechRecognizer.UIOptions**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions) 屬性來設定。 在進行辨識操作時，幫使用者準備好可以說出的內容，可以讓使用者更可能說出符合使用中限制式的片語。

## <a name="related-articles"></a>相關文章

- [語音互動](speech-interactions.md)

### <a name="samples"></a>範例

- [語音辨識和語音合成範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)