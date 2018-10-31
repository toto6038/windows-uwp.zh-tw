---
author: Karl-Bridge-Microsoft
Description: Use speech recognition to provide input, specify an action or command, and accomplish tasks.
title: 語音辨識
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: speech, voice, speech recognition, natural language, dictation, input, user interaction, 語音, 聲音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.author: kbridge
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b9148b2d57c55bdff09be9a9d6bb8a6b65d93f12
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5811829"
---
# <a name="speech-recognition"></a>語音辨識


使用語音辨識以提供輸入、指定動作或命令，以及完成工作。

> **重要 API**：[**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)

語音辨識包含了語音執行階段、用於設計執行階段程式的辨識 API、現成的口述和網頁搜尋文法，以及可幫助使用者探索和使用語音辨識功能的預設系統 UI。

## <a name="configure-speech-recognition"></a>設定語音辨識

若要支援使用您的應用程式的語音辨識，使用者必須連線和啟用的麥克風他們在裝置上，並接受 Microsoft 隱私權原則授與您的應用程式的權限才能使用。

自動與系統對話方塊，要求存取和使用麥克風的權限提示使用者的音訊饋送 （範例中的[語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)如下所示），只是設定**的麥克風**[裝置功能](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability)中的[應用程式套件資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)。 如需詳細資訊，請參閱[應用程式功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。

![用於存取麥克風的隱私權原則](images/speech/privacy.png)

如果使用者按一下 [是] 來授與存取權麥克風，您的應用程式會新增到清單，核准的應用程式在 \ [設定]-> [隱私權]-> [麥克風] 頁面。 不過，因為使用者可以選擇在任何時候關閉這項設定，您應該確認您的 app 有存取麥克風，再嘗試使用它。

如果您也想要支援聽寫，Cortana，或其他語音辨識服務 （例如[預先定義的文法](#predefined-grammars)定義中的主題限制式），您必須也確認**線上語音辨識**(設定]-> [隱私權]-> [語音) 是啟用。

此程式碼片段顯示如何您的應用程式可以檢查麥克風是否存在，以及如果已使用它的權限。

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>辨識語音輸入

*「限制式」* 定義了應用程式可在語音輸入中辨識的字詞和片語 (詞彙)。 限制式是語音辨識的核心，可大幅提升您應用程式的語音辨識準確度。

您可以使用下列類型的限制式辨識語音輸入。

### <a name="predefined-grammars"></a>預先定義的文法

預先定義的聽寫和網頁搜尋文法可為您的 app 提供語音辨識，而不需要您編寫文法。 使用這些文法時，語音辨識是由遠端 Web 服務所執行，並將結果傳回裝置。

預設的任意文字聽寫文法能夠辨識使用者可利用特定語言說出的大部分字詞與片語，並已最佳化而能辨識簡短的片語。 如果您沒有為 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 物件指定任何限制式，就會使用預先定義的聽寫文法。 當您不想限制使用者可以說出的內容種類時，任意文字聽寫就很實用。 典型的用法包括建立記事或聽寫訊息內容。

網頁搜尋文法類似聽寫文法，包含大量使用者可能說出的字詞與片語。 不過，已將它最佳化，可辨識使用者在搜尋 Web 時常用的詞彙。

**注意：** 因為預先定義的聽寫和網頁搜尋文法可能很大，且因為是在線上 （不在裝置上），效能可能不一樣的裝置上安裝自訂文法快速。     

這些預先定義的文法可用來辨識最多 10 秒鐘的語音輸入，而您不需要花費任何編寫的精力。 但是，它們需要連線到網路。

若要使用 Web 服務的限制，必須在 **\[設定\] -> \[隱私權\] -> \[語音、筆跡與輸入\]** 頁面的 **\[設定\]** 中開啟 \[了解我\] 選項以啟用語音輸入與聽寫支援。

以下說明如何測試是否已啟用語音輸入，並開啟 \[設定\] -> \[隱私權\] -> \[語音、筆跡與輸入\] 頁面 (如果未啟用)。

首先，我們會將全域變數 (HResultPrivacyStatementDeclined) 初始化為 0x80045509 的 HResult 值。 請參閱[以 C\# 或 Visual Basic 撰寫的例外狀況處理](https://msdn.microsoft.com/library/windows/apps/dn532194)。

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

然後我們會在辨識期間捕捉任何標準例外，並測試 [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) 值是否等於 HResultPrivacyStatementDeclined 變數的值。 如果相等，我們會顯示警告並呼叫 `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` 以開啟 [設定] 頁面。

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
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

請參閱[**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)。

### <a name="programmatic-list-constraints"></a>程式設計的清單限制式 

程式設計的清單限制式提供使用字詞或片語清單來建立簡易文法的輕量型方法。 清單限制式非常適合用來辨識簡短的明確片語。 明確指定所有符合文法的文字也可以提高辨識準確度，因為語音辨識引擎只需處理確認相符的語音。 清單也可透過程式設計方式來更新。

清單限制式包含一個字串陣列，代表您應用程式的辨識操作將接受的語音輸入。 您可以藉由建立一個語音辨識清單限制式物件並傳遞字串陣列，在您的應用程式中建立清單限制式。 然後，將該物件新增到辨識器的限制式集合。 當語音辨識器辨識到清單中的任何一個字串時，即代表辨識成功。

請參閱[**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)。

### <a name="srgs-grammars"></a>SRGS 文法

語音辨識文法規格 (SRGS) 文法是一份靜態文件，與程式設計的清單限制式不同，您可以使用 [SRGS 版本 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302) 所定義的 XML 格式來編輯該文件。 SRGS 文法透過讓您可在單一辨識中擷取多個語意意義，來提供控制整個語音辨識的最佳體驗。

 請參閱[**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)。

### <a name="voice-command-constraints"></a>語音命令限制式

使用語音命令定義 (VCD) XML 檔案，定義使用者在啟用您 app 時可以說出以起始動作的命令。 如需詳細資訊，請參閱[利用 Cortana 語音命令啟動前景 app](https://msdn.microsoft.com/cortana/voicecommands/launch-a-foreground-app-with-voice-commands-in-cortana)。

請參閱[**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220)/

**注意：** 類型，您使用的限制式，取決於您想要建立的辨識體驗的複雜度。 任一種都可能是特定辨識工作的最佳選擇，您也許會找到所有限制式類型在您 app 中的用途。
若要開始使用條件約束，請參閱[定義自訂辨識條件約束](define-custom-recognition-constraints.md)。

預先定義的通用 Windows app 聽寫文法可以辨識一個語言中大部分的字詞和簡短片語。 將語音辨識器物件具現化而沒有搭配自訂限制式時，預設會啟用該預先定義的聽寫文法。

在這個範例中，我們將示範如何：

- 建立語音辨識器。
- 編譯預設的 Universal Windows app 限制式 (尚未將任何文法新增到語音辨識器的文法集)。
- 使用 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 方法提供的基本辨識 UI 和 TTS 回饋開始接聽語音。 如果不需要預設 UI，請使用 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) 方法。

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

## <a name="customize-the-recognition-ui"></a>自訂辨識 UI


當您的應用程式呼叫 [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 來嘗試進行語音辨識時，會依下列順序顯示數個畫面。

如果您使用的是以預先定義的文法 (口述或網頁搜尋) 為基礎的限制式：

-   **[正在聆聽]** 畫面。
-   **[正在思考]** 畫面。
-   **[聽到您說]** 畫面或錯誤畫面。

如果您使用的是以字詞或片語清單為基礎的限制式，或是以 SRGS 文法檔為基礎的限制式：

-   **[正在聆聽]** 畫面。
-   **[您說的是]** 畫面 (如果使用者所說的內容可以解譯成多個可能的結果)。
-   **[聽到您說]** 畫面或錯誤畫面。

下列影像針對使用以 SGRS 文法檔為基礎之條件約束的語音辨識器，顯示其畫面之間流程的範例。 在這個範例中，語音辨識是成功的。

![以 SGRS 文法檔為基礎之限制的初始辨識畫面](images/speech-listening-initial.png)

![以 SGRS 文法檔為基礎之限制的中繼辨識畫面](images/speech-listening-intermediate.png)

![以 SGRS 文法檔為基礎之限制的最終辨識畫面](images/speech-listening-complete.png)

**\[正在聆聽\]** 畫面可以提供 app 能夠辨識的字詞或片語的範例。 以下示範如何使用 [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234) 類別的屬性 (透過呼叫 [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254) 屬性來取得) 來自訂 **\[正在聆聽\]** 畫面上的內容。

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

## <a name="related-articles"></a>相關文章


**開發人員**
* [語音互動](speech-interactions.md)
**設計人員**
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)
**範例**
* [語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




