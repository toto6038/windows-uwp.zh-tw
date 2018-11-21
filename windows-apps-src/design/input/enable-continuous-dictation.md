---
author: Karl-Bridge-Microsoft
Description: Learn how to capture and recognize long-form, continuous dictation speech input.
title: 啟用連續聽寫
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: 語音, 語音辨識, 自然語言, 聽寫, 輸入, 使用者互動
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ea7c0b92c5900e468023dd5b972942a89c2833c3
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7557734"
---
# <a name="continuous-dictation"></a>連續聽寫

了解如何擷取及辨識較長且連續的聽寫語音輸入。

> **重要 API**：[**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896)、[**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)

在[語音辨識](speech-recognition.md)中，您可以學習如何使用 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653244) 物件的 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 或 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653226) 方法抓取或辨識相對簡短的語音輸入，例如，在撰寫簡短的簡訊服務 (SMS) 訊息或問問題時。

針對較長、連續語音辨識的工作階段 (例如聽寫或電子郵件) 時，請使用 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn913913) 的 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn653226) 屬性來取得 [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) 物件。

> [!NOTE]
> 聽寫語言支援取決於您的應用程式執行所在的[裝置](https://docs.microsoft.com/windows/uwp/design/devices/)。 適用於電腦和膝上型電腦，只 EN-US 辨識出，雖然 Xbox 和手機可以辨識語音辨識所支援的所有語言。 如需詳細資訊，請參閱[指定語音辨識器的語言](specify-the-speech-recognizer-language.md)。

## <a name="set-up"></a>設定

您的應用程式需要幾項物件來管理連續聽寫工作階段：

- [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 物件的執行個體。
- 對 UI 發送器的參照以在聽寫時更新 UI。
- 追蹤使用者說出之累計文字的方式。

我們在此處宣告 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 執行個體為程式碼後置類別的私用欄位。 如果您想要連續聽寫持續超過單一可延伸應用程式標記語言 (XAML) 頁面，您的應用程式需要將參照儲存在其他位置。

```CSharp
private SpeechRecognizer speechRecognizer;
```

在聽寫期間，辨識器會從背景執行緒引發事件。 因為在 XAML 中，背景執行緒無法直接更新 UI，所以您的應用程式必須使用發送器來更新 UI 以回應辨識事件。

我們在此處宣告將在稍候使用 UI 發送器初始化的私用欄位。

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

若要追蹤者說了哪些內容，您必須處理語音辨識器所引發的識別事件。 這些事件會提供使用者口述內容片段的辨識結果。

我們在此處使用 [**StringBuilder**](https://msdn.microsoft.com/library/system.text.stringbuilder.aspx) 物件來保留在工作階段期間取得的所有辨識結果。 處理後的新結果會附加到 **StringBuilder**。

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>初始化

初始化連續語音辨識時，您必須：

- 擷取 UI 執行緒的發送器 (如果您在連續辨識事件處理常式中更新您應用程式的 UI)。
- 初始化語音辨識器。
- 編譯內建聽寫文法。
    **注意：** 語音辨識需要至少需要一個限制式來定義可辨識的詞彙。 如果沒有指定任何限制式，則會使用預先定義的聽寫文法。 請參閱[語音辨識](speech-recognition.md)。
- 設定辨識事件的事件接聽程式。

在這個範例中，我們會在 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 頁面事件中初始化語音辨識。

1. 因為語音辨識器引發的事件是在背景執行緒中發生，所以請建立對發送器的參照以更新 UI 執行緒。 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 一律會在 UI 執行緒上叫用。
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  然後我們會初始化 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 執行個體。
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  之後我們會新增及編譯定義 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 能夠辨識之所有文字和片語的文法。

    如果您沒有明確指定文法，預設會使用預先定義的聽寫文法。 通常，預設文法是一般聽寫的最佳選擇。

    在這裡，我們立即呼叫 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) 而不新增文法。

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>處理辨識事件

您可以透過呼叫 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) 或 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 來擷取單一、簡短的發音或片語。 

但是，若要擷取較長、連續的辨識工作階段，我們指定事件接聽程式來於使用者說話時在背景執行，並定義處理常式來建立聽寫字串。

然後我們使用辨識器的 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) 屬性來取得提供用於管理連續辨識工作階段之方法與事件的 [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) 物件。

有兩個事件特別重要：

- [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)，會在辨識器已經產生某些結果時發生。
- [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899)，會在連續辨識工作階段已經結束時發生。

[**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件會在使用者說話時引發。 辨識器會連續聆聽使用者說話，並定時引發傳遞語音輸入片段的事件。 您必須使用事件引數的 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) 屬性檢查語音輸入，然後在事件處理常式中採取適當動作，例如附加文字到 StringBuilder 物件。

做為 [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) 的執行個體，[**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) 屬性非常適合用來判斷您是否要接受語音輸入： [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) 為此提供兩個屬性：

- [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) 會指示辨識是否成功。 可能造成辨識失敗的原因有很多。
- [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) 會指示辨識器了解正確文字的相對信賴等級。

以下是支援連續辨識的基本步驟：  

1. 在這裡，我們在 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/dn913900) 頁面事件中註冊 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/br227508) 連續辨識事件的處理常式。
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  然後我們檢查 [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) 屬性。 如果「信賴等級」的值是 [**Medium**](https://msdn.microsoft.com/library/windows/apps/dn631409) 或更高，我們就會將文字附加到 StringBuilder。 我們也會在收集輸入時更新 UI 。

    **注意：** [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)事件會在無法直接更新 UI 的背景執行緒上引發。 如果處理常式需要更新 UI (就像 \[語音和 TTS 範例\] 一樣)，您就必須透過發送器的 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法發送更新至 UI。
```csharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  然後我們會處理 [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899) 事件，該事件會指示連續聽寫的結束點。

    工作階段會在您呼叫 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) 或 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) 方法 (在下一節中說明) 時結束。 工作階段也可以在發生錯誤時結束，或在使用者停止說話時結束。 檢查事件引數的 [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) 屬性來判斷工作階段的結束原因 ([**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433))。

    在這裡，我們在 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/dn913899) 頁面事件中註冊 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br227508) 連續辨識事件的處理常式。
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  事件處理常式會檢查 Status 數性以判斷辨識是否成功。 它也會處理使用者已經停止說話的情況。 通常，[**TimeoutExceeded**](https://msdn.microsoft.com/library/windows/apps/dn631433) 會被視為成功辨識，因為它表示使用者已經結束說話。 您應該在您的程式碼中處理此情況，以提供良好體驗。

    **注意：** [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)事件會在無法直接更新 UI 的背景執行緒上引發。 如果處理常式需要更新 UI (就像 \[語音和 TTS 範例\] 一樣)，您就必須透過發送器的 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法發送更新至 UI。
```csharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## <a name="provide-ongoing-recognition-feedback"></a>提供進行中的辨識回應


當人們交談時，他們通常會依賴前後相關內容來完整了解說話內容。 語音辨識也一樣，通常需要前後相關內容來提供高信賴等級的辨識結果。 例如，只有能夠在前後文字中收集更多相關內容時才能區分 "weight" 與 "wait" 這兩個字。 辨識器在能夠正確辨識某個字或某幾個字之後，才會引發 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件。

這可能會導致使用者體驗不是那麼理想，因為使用者會持續說話，且辨識器在達到夠高的信賴等級以引發 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件之前不會提供任何結果。

處理 [**HypothesisGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913914) 事件以改善此明顯的回應性不足問題。 無論辨識器是否會針對正在處理的文字產生一組新的可能相符項目，都會引發此事件。 事件引數會提供包含目前相符項目的 [**Hypothesis**](https://msdn.microsoft.com/library/windows/apps/dn913911) 屬性。 在使用者連續說話時向使用者顯示這些項目，並讓他們知道處理作業仍在作用中。 在信賴等級提高並決定辨識結果之後，以[**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913895) 事件中提供的最終 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913900) 來取代臨時的 **Hypothesis** 結果。

我們在這裡附加假設文字和省略符號 (…) 至輸出 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 的目前的值。 在新的假設內容產生時，以及在從 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 事件取得最終結果之後，文字方塊的內容就會更新。

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## <a name="start-and-stop-recognition"></a>開始及停止辨識


在開始辨識工作階段之前，請檢查語音辨識器 [**State**](https://msdn.microsoft.com/library/windows/apps/dn913915) 屬性的值。 語音辨識器必須處於 [**Idle**](https://msdn.microsoft.com/library/windows/apps/dn653227) 狀態。

在檢查語音辨識器的狀態之後，我們會透過呼叫語音辨識器之 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913901) 屬性的 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn913913) 方法來開始工作階段。

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

有兩種方式可以停止辨識：

-   [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) 會讓任何擱置中辨識事件完成 ([**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 會繼續被引發，直到所有擱置中辨識作業完成為止)。
-   [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) 會立即終止辨識工作階段，並捨棄任何擱置中結果。

在檢查語音辨識器的狀態之後，我們會透過呼叫語音辨識器之 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913898) 屬性的 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913913) 方法來停止工作階段。

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> 在呼叫 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913900) 之後，可能會發生 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913898) 事件。  
> 因為多執行緒的關係，可能會在呼叫 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913900) 時仍於堆疊中保留 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913898) 事件。 如果是這樣，就仍會引發 **ResultGenerated** 事件。  
> 如果您在取消辨識工作階段時設定任何私用欄位，請一律確認它們在 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 處理常式中的值。 例如，如果您在取消工作階段時將欄位設定為 null，請勿假設您處理常式中的欄位已經初始化。

 

## <a name="related-articles"></a>相關文章


* [語音互動](speech-interactions.md)

**範例**
* [語音辨識和語音合成範例](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




