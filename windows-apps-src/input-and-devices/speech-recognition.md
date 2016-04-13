---
Description: 使用語音辨識以提供輸入、指定動作或命令，以及完成工作。
title: 語音辨識
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
---

# 語音辨識


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

使用語音辨識以提供輸入、指定動作或命令，以及完成工作。

**重要 API**

-   [**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)



語音辨識包含了語音執行階段、用於設計執行階段程式的辨識 API、現成的口述和網頁搜尋文法，以及可幫助使用者探索和使用語音辨識功能的預設系統 UI。


## <span id="Set_up_the_audio_feed"> </span> <span id="set_up_the_audio_feed"> </span> <span id="SET_UP_THE_AUDIO_FEED"> </span>設定音訊饋送


確定您的裝置有麥克風或對等的功能。

在 [app 套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474) (**package.appxmanifest** 檔案) 中設定 [**麥克風**] 裝置功能 ([**DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/br211430))，以存取麥克風的音訊饋送。 這樣可讓 app 從連接的麥克風錄音。

請參閱 [app 功能宣告](https://msdn.microsoft.com/library/windows/apps/mt270968)。

## <span id="Recognize_speech_input"> </span> <span id="recognize_speech_input"> </span> <span id="RECOGNIZE_SPEECH_INPUT"> </span>辨識語音輸入


*限制式*會定義 app 在語音輸入中辨識的字詞和片語 (詞彙)。 限制式是語音辨識的核心，可大幅提升您應用程式的語音辨識準確度。

您可以在執行語音辨識時使用各種類型的限制式：

1.  **預先定義的文法** ([**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446))。

    預先定義的聽寫和網頁搜尋文法可為您的 app 提供語音辨識，而不需要您編寫文法。 使用這些文法時，語音辨識是由遠端 Web 服務所執行，並將結果傳回裝置。

    預設的任意文字聽寫文法能夠辨識使用者可利用特定語言說出的大部分字詞與片語，並已最佳化而能辨識簡短的片語。 如果您沒有為 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 物件指定任何限制式，就會使用預先定義的聽寫文法。 當您不想限制使用者可以說出的內容種類時，任意文字聽寫就很實用。 典型的用法包括建立記事或聽寫訊息內容。

    網頁搜尋文法類似聽寫文法，包含大量使用者可能說出的字詞與片語。 不過，已將它最佳化，可辨識使用者在搜尋 Web 時常用的詞彙。

    **注意** 由於預先定義的聽寫和網頁搜尋文法可能相當龐大，且因為是在線上 (並非在裝置上)，因此，效能可能不及安裝在裝置上的自訂文法快速。

     

    These predefined grammars can be used to recognize up to 10 seconds of speech input and require no authoring effort on your part. However, they do require a connection to a network.

    To use web-service constraints, speech input and dictation support must be enabled in **Settings** by turning on the "Get to know me" option in the Settings -&gt; Privacy -&gt; Speech, inking, and typing page.

    Here, we show how to test whether speech input is enabled and open the Settings -&gt; Privacy -&gt; Speech, inking, and typing page, if not.

    First, we initialize a global variable (HResultPrivacyStatementDeclined) to the HResult value of 0x80045509. See [Exception handling for in C\# or Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194).

```    CSharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

    We then catch any standard exceptions during recogntion and test if the [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) value is equal to the value of the HResultPrivacyStatementDeclined variable. If so, we display a warning and call `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` to open the Settings page.

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
catch (Exception exception)
    {
      // Handle the speech privacy policy error.
      if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
      {
        resultTextBlock.Visibility = Visibility.Visible;
        resultTextBlock.Text = "The privacy statement was declined. 
          Go to Settings -> Privacy -> Speech, inking and typing, and ensure you 
          have viewed the privacy policy, and &#39;Get To Know You&#39; is enabled.";
        // Open the privacy/speech, inking, and typing settings page.
        await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
      }
      else
      {
        var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
        await messageDialog.ShowAsync();
      }
    }
```

2.  **程式設計的清單限制式** ([**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421))。

    程式設計的清單限制式提供使用字詞或片語清單來建立簡易文法的輕量型方法。 清單限制式非常適合用來辨識簡短的明確片語。 明確指定所有符合文法的文字也可以提高辨識準確度，因為語音辨識引擎只需處理確認相符的語音。 清單也可透過程式設計方式來更新。

    清單限制式包含一個字串陣列，代表您應用程式的辨識操作將接受的語音輸入。 您可以藉由建立一個語音辨識清單限制式物件並傳遞字串陣列，在您的應用程式中建立清單限制式。 然後，將該物件新增到辨識器的限制式集合。 當語音辨識器辨識到清單中的任何一個字串時，即代表辨識成功。

3.  **SRGS 文法** ([**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412))。

    語音辨識文法規格 (SRGS) 文法是一份靜態文件，與程式設計的清單限制式不同，您可以使用 [SRGS 版本 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302) 所定義的 XML 格式來編輯該文件。 SRGS 文法透過讓您可在單一辨識中擷取多個語意意義，來提供控制整個語音辨識的最佳體驗。

4.  **語音命令限制式** ([**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220))

    使用語音命令定義 (VCD) XML 檔案，定義使用者在啟用您 app 時可以說出以起始動作的命令。 如需詳細資訊，請參閱[利用 Cortana 語音命令啟動前景 app](launch-a-foreground-app-with-voice-commands-in-cortana.md)。

**注意** 要使用哪種類型的限制式，取決於您想建立的辨識體驗的複雜度。 任一種都可能是特定辨識工作的最佳選擇，您也許會找到所有限制式類型在您 app 中的用途。
若要開始使用條件約束，請參閱[定義自訂辨識條件約束](define-custom-recognition-constraints.md)。

 

預先定義的通用 Windows app 聽寫文法可以辨識一個語言中大部分的字詞和簡短片語。 將語音辨識器物件具現化而沒有搭配自訂限制式時，預設會啟用該預先定義的聽寫文法。

在這個範例中，我們將示範如何：

-   建立語音辨識器。
-   編譯預設的 Universal Windows app 限制式 (尚未將任何文法新增到語音辨識器的文法集)。
-   使用 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 方法提供的基本辨識 UI 和 TTS 回饋開始接聽語音。 如果不需要預設 UI，請使用 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) 方法。

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <span id="Customize_the_recognition_UI"> </span> <span id="customize_the_recognition_ui"> </span> <span id="CUSTOMIZE_THE_RECOGNITION_UI"> </span>自訂辨識 UI


當您的應用程式呼叫 [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 來嘗試進行語音辨識時，會依下列順序顯示數個畫面。

如果您使用的是以預先定義的文法 (口述或網頁搜尋) 為基礎的限制式：

-   [正在聆聽]**** 畫面。
-   [正在思考]**** 畫面。
-   [聽到您說]**** 畫面或錯誤畫面。

如果您使用的是以字詞或片語清單為基礎的限制式，或是以 SRGS 文法檔為基礎的限制式：

-   [正在聆聽]**** 畫面。
-   [您說的是]**** 畫面 (如果使用者所說的內容可以解譯成多個可能的結果)。
-   [聽到您說]**** 畫面或錯誤畫面。

下列影像針對使用以 SGRS 文法檔為基礎之條件約束的語音辨識器，顯示其畫面之間流程的範例。 在這個範例中，語音辨識是成功的。

![以 SGRS 文法檔為基礎之限制的初始辨識畫面](images/speech-listening-initial.png)

![以 SGRS 文法檔為基礎之限制的中繼辨識畫面](images/speech-listening-intermediate.png)

![以 SGRS 文法檔為基礎之限制的最終辨識畫面](images/speech-listening-complete.png)

[正在聆聽]**** 畫面可以提供 app 能夠辨識的字詞或片語的範例。 以下示範如何使用 [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234) 類別的屬性 (透過呼叫 [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254) 屬性來取得) 來自訂 [正在聆聽]**** 畫面上的內容。

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
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
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

## <span id="related_topics"> </span>相關文章


**開發人員**
* [語音互動](speech-interactions.md)
**設計人員**
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)
**範例**
* [語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 






<!--HONumber=Mar16_HO4-->


