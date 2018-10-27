---
author: Karl-Bridge-Microsoft
Description: Learn how to select an installed language to use for speech recognition.
title: 指定語音辨識器語言
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: speech, voice, speech recognition, natural language, dictation, input, user interaction, 語音, 聲音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e042a9bbedee3ded0601eda06da8e349c4b788c
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5683096"
---
# <a name="specify-the-speech-recognizer-language"></a>指定語音辨識器語言


了解如何選取已安裝的語言以用於語音辨識。

> **重要 API**：[**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251)、[**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250)、[**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)


我們將在此處列舉已安裝於系統上的語言、識別哪一個是預設語言，並選取不同的辨識語言。

**先決條件：**

本主題以[語音辨識](speech-recognition.md)為基礎。

您對於語音辨識和辨識限制式應該有基本的了解。

如果您是開發通用 Windows 平台 (UWP) App 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個 App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：**

如需有關設計既實用又吸引人且支援語音之應用程式的有用提示，請參閱[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <a name="identify-the-default-language"></a>識別預設語言


語音辨識器會使用系統語音功能的語言，做為其預設的辨識語言。 這個語言是由使用者在裝置的 [設定] &gt; [系統] &gt; [語音] &gt; [語音功能的語言] 畫面上所設定。

我們可藉由檢查 [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252) 靜態屬性來識別預設語言。

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>確認已安裝的語言


已安裝的語言會隨著裝置而不同。 如果您要針對特定限制式來使用它，就應該確認該語言是否存在。

**注意：** 安裝新的語言套件之後，是需要重新開機。 如果不支援或無法完成安裝指定的語言，就會引發例外狀況且錯誤碼為 SPERR\_NOT\_FOUND (0x8004503a)。

 

勾選 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 類別的這兩個靜態屬性之一，來判斷裝置上支援的語言：

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251)：[**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 物件的集合，可以與預先定義的聽寫與網頁搜尋文法搭配使用。

-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250)：[**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 物件的集合，可以與清單限制式或語音辨識文法規格 (SRGS) 檔案搭配使用。

## <a name="specify-a-language"></a>指定語言


若要指定語言，請傳遞 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/br206804) 建構函式中的 [**Language**](https://msdn.microsoft.com/library/windows/apps/dn653226) 物件。

我們將在此處指定 "en-US" 做為辨識語言。


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>備註


您可以藉由將 [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) 新增到 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653241) 的 [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653226) 集合，然後呼叫 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)，來設定主題限制式。 如果辨識器不是使用支援的主題語言來初始化，即會傳回 **TopicLanguageNotSupported** 的 [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)。

您可以藉由將 [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) 新增到 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653241) 的 [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653226) 集合，然後呼叫 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)，來設定清單限制式。 您無法直接指定自訂清單的語言， 而是改用辨識器的語言來處理清單。

SRGS 文法是一種可透過 [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412) 類別呈現的開放式標準 XML 格式。 與自訂清單不同，您可以在 SRGS 標記中指定文法的語言。 如果辨識器不會初始化為與 SRGS 標記相同的語言，則 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) 會失敗並產生 **TopicLanguageNotSupported** 的 [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)。

## <a name="related-articles"></a>相關文章

**開發人員**

* [語音互動](speech-interactions.md)

**設計人員**

* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)

**範例**

* [語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




