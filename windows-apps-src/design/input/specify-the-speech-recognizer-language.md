---
Description: 了解如何選取已安裝的語言以用於語音辨識。
title: 指定語音辨識器語言
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: speech, voice, speech recognition, natural language, dictation, input, user interaction, 語音, 聲音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7d4eb10bff1bbbda3bc10a8a1d770fecf6b4042e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173312"
---
# <a name="specify-the-speech-recognizer-language"></a>指定語音辨識器語言


了解如何選取已安裝的語言以用於語音辨識。

> **重要 API**：[**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)、[**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)、[**Language**](/uwp/api/Windows.Globalization.Language)


我們將在此處列舉已安裝於系統上的語言、識別哪一個是預設語言，並選取不同的辨識語言。

**必要條件：**

本主題以[語音辨識](speech-recognition.md)為基礎。

您對於語音辨識和辨識限制式應該有基本的了解。

如果您是開發 Windows 應用程式的新手，請參閱這些主題，以瞭解此處所討論的技術。

-   [建立您的第一個應用程式](../../get-started/your-first-app.md)
-   請參閱[事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)，以了解事件相關資訊

**使用者經驗指導方針：**

如需有關設計既實用又吸引人且支援語音之應用程式的有用提示，請參閱[語音設計指導方針](./speech-interactions.md)。

## <a name="identify-the-default-language"></a>識別預設語言


語音辨識器會使用系統語音功能的語言，做為其預設的辨識語言。 這個語言是由使用者在裝置的 [設定] &gt; [系統] &gt; [語音] &gt; [語音功能的語言] 畫面上所設定。

我們可藉由檢查 [**SystemSpeechLanguage**](/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage) 靜態屬性來識別預設語言。

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>確認已安裝的語言


已安裝的語言會隨著裝置而不同。 如果您要針對特定限制式來使用它，就應該確認該語言是否存在。

**注意**   安裝新的語言套件之後，需要重新開機。 \_如果指定的語言不受支援或尚未完成安裝，則會引發錯誤碼 SPERR 找不到的例外狀況， \_ (0x8004503a) 。

 

勾選 [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 類別的這兩個靜態屬性之一，來判斷裝置上支援的語言：

-   [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)：[**Language**](/uwp/api/Windows.Globalization.Language) 物件的集合，可以與預先定義的聽寫與網頁搜尋文法搭配使用。

-   [**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)：[**Language**](/uwp/api/Windows.Globalization.Language) 物件的集合，可以與清單限制式或語音辨識文法規格 (SRGS) 檔案搭配使用。

## <a name="specify-a-language"></a>指定語言


若要指定語言，請傳遞 [**SpeechRecognizer**](/uwp/api/Windows.Globalization.Language) 建構函式中的 [**Language**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 物件。

我們將在此處指定 "en-US" 做為辨識語言。


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>備註


您可以藉由將 [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) 新增到 [**SpeechRecognizer**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 的 [**Constraints**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 集合，然後呼叫 [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)，來設定主題限制式。 如果辨識器不是使用支援的主題語言來初始化，即會傳回 **TopicLanguageNotSupported** 的 [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)。

您可以藉由將 [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) 新增到 [**SpeechRecognizer**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 的 [**Constraints**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 集合，然後呼叫 [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)，來設定清單限制式。 您無法直接指定自訂清單的語言， 而是改用辨識器的語言來處理清單。

SRGS 文法是一種可透過 [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint) 類別呈現的開放式標準 XML 格式。 與自訂清單不同，您可以在 SRGS 標記中指定文法的語言。 如果辨識器不會初始化為與 SRGS 標記相同的語言，則 [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) 會失敗並產生 **TopicLanguageNotSupported** 的 [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)。

## <a name="related-articles"></a>相關文章

* [語音互動](speech-interactions.md)

**範例**

* [語音辨識和語音合成範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 