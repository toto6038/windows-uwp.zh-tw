---
author: drewbatgit
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: 本文說明如何使用 MediaProcessingTrigger 和背景工作，在背景處理媒體檔案。
title: 在背景處理媒體檔案
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 866fedf35aa6f1f585825195b18cdd1fed4bad11
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5812462"
---
# <a name="process-media-files-in-the-background"></a>在背景處理媒體檔案



本文說明如何使用 [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) 和背景工作，在背景處理媒體檔案。

本文中所描述的 App 範例可讓使用者選取要轉碼的輸入媒體檔案，以及指定轉碼結果的輸出檔案。 接著，就會啟動背景工作來執行轉碼作業。 
            [
              **MediaProcessingTrigger**
            ](https://msdn.microsoft.com/library/windows/apps/dn806005) 支援除了轉碼以外許多不同的媒體處理案例，包括轉譯媒體組合到磁碟，以及處理完成後上傳已處理的媒體檔案。

如需此範例中各種通用 Windows 應用程式功能的詳細資訊，請參閱：

-   [轉碼媒體檔案](transcode-media-files.md)
-   [啟動、繼續和背景工作](https://msdn.microsoft.com/library/windows/apps/mt227652)
-   [磚、徽章及通知](https://msdn.microsoft.com/library/windows/apps/mt185606)

## <a name="create-a-media-processing-background-task"></a>建立媒體處理背景工作

若要在 Microsoft Visual Studio 中將背景工作新增到您現有的方案，請輸入您元件的名稱

1.  從 **\[檔案\]** 功能表選取 **\[新增\]**，然後選取 **\[新增專案\]**。
2.  選取 **\[Windows 執行階段元件 (通用 Windows)\]** 專案類型。
3.  為新的元件專案輸入名稱。 這個範例使用 **MediaProcessingBackgroundTask** 專案名稱。
4.  按一下 [確定]。

在 **\[方案總管\]** 中，以滑鼠右鍵按一下預設建立的 "Class1.cs" 檔案的圖示，然後選取 **\[重新命名\]**。 將檔案重新命名為 "MediaProcessingTask.cs"。 當 Visual Studio 詢問您是否要重新命名這個類別的所有參考時，按一下 **\[是\]**。

在重新命名的類別檔案中，新增下列 **using** 指示詞，在專案中包含這些命名空間。
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

更新您的類別宣告，讓您的類別繼承自 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)。

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

將下列成員變數新增到您的類別：

-   
            [
              **IBackgroundTaskInstance**
            ](https://msdn.microsoft.com/library/windows/apps/br224797)，將用來以背景工作的進度更新前景 App。
-   
            [
              **BackgroundTaskDeferral**
            ](https://msdn.microsoft.com/library/windows/apps/hh700499)，非同步執行媒體轉碼時，讓系統不要關閉您的背景工作。
-   
            **CancellationTokenSource** 物件，可用來取消非同步轉碼作業。
-   
            [
              **MediaTranscoder**
            ](https://msdn.microsoft.com/library/windows/apps/br207080) 物件，用來轉碼媒體檔案。

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

當背景工作啟動時，系統會呼叫背景工作的 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法。 將傳入方法的 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 物件設定為對應的成員變數。 登錄 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 事件的處理常式，這會在系統需要關閉背景工作時引發。 然後，將 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) 屬性設定為零。

接著，呼叫背景工作物件的 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700507) 方法以取得延遲。 這會告訴系統不要關閉您的工作，因為您正在執行非同步作業。

接著，呼叫協助程式方法 **TranscodeFileAsync**，這會在下一節中定義。 如果成功完成，則會呼叫協助程式方法以啟動快顯通知，提醒使用者該轉碼已完成。

在 **Run** 方法的最後，在延遲的物件上呼叫 [**Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)，讓系統知道您的背景工作已完成且可終止。

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

在 **TranscodeFileAsync** 協助程式方法中，轉碼作業的輸入檔案與輸出檔案的檔案名稱會從您 app 的 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 擷取。 這些值將由您的前景 app 設定。 建立輸入檔案與輸出檔案的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件，然後建立要用於轉碼的編碼設定檔。

呼叫 [**PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936)，傳入輸入檔案、輸出檔案和編碼設定檔。 從這個呼叫傳回的 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 物件可讓您知道是否可以執行轉碼。 如果 [**CanTranscode**](https://msdn.microsoft.com/library/windows/apps/hh700942) 屬性為 true，請呼叫 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 來執行轉碼作業。


            **AsTask** 方法可讓您追蹤非同步作業的進度或取消作業。 建立新的 **Progress** 物件，指定您想要的進度單位，以及要呼叫來通知您目前工作進度的方法名稱。 將 **Progress** 物件連同可讓您取消工作的取消權杖傳入 **AsTask** 方法。

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

在您於上一個步驟中用來建立 Progress 物件的方法 **Progress** 中，設定背景工作執行個體的進度。 這會將進度傳入前景 app (如果正在執行)。

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]


            **SendToastNotification** 協助程式方法會取得只有文字內容的快顯通知的範本 XML 文件，建立新的快顯通知。 此時會設定快顯通知 XML 的文字項目，然後從 XML 文件建立新的 [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) 物件。 最後，呼叫 [**ToastNotifier.Show**](https://msdn.microsoft.com/library/windows/apps/br208659) 對使用者顯示快顯通知。

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

在 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.IBackgroundTaskInstance.Canceled) 事件 (這會在系統取消您背景工作時呼叫) 的處理常式中，您可以針對遙測用途記錄錯誤。

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>註冊和啟動背景工作

您必須先更新您前景 App 的 Package.appmanifest 檔案讓系統知道您的 App 使用背景工作，您才能從前景 App 啟動背景工作。

1.  在 **\[方案總管\]** 中按兩下 Package.appmanifest 檔案圖示，開啟資訊清單編輯器。
2.  選取 **\[宣告\]** 索引標籤。
3.  從 [**可用宣告**]，選取 [**背景工作**]，然後按一下 [**新增**]。
4.  在 [**支援的宣告**] 下，確認已選取 [**背景工作**] 項目。 在 [**屬性**] 下，選取 [**媒體處理**] 的核取方塊。
5.  在 [**進入點**] 文字方塊中，為您的背景測試指定命名空間與類別名稱，以句點分隔。 對於這個範例，則是：
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
接著，您必須將背景工作參考新增到前景 app。
1.  在 **\[方案總管\]** 中的前景 App 專案下，以滑鼠右鍵按一下 **\[參考\]** 資料夾，然後選取 **\[加入參考...\]**。
2.  展開 **\[專案\]** 節點，然後選取 **\[方案\]**。
3.  核取您背景工作專案旁邊的方塊，並按一下 **\[確定\]**。

此範例中的其餘程式碼，應該新增到您的前景 App。 首先，您必須將下列命名空間新增到專案。

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

接著，新增登錄背景工作所需的下列成員變數。

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]


            **PickFilesToTranscode** 協助程式方法使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)，開啟轉碼的輸入檔案與輸出檔案。 使用者可能會選取您的 app 無法存取的位置中的檔案。 若要確定您的背景工作可以開啟檔案，請將這些檔案新增到您的 App 的 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)。

最後，在您 app 的 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 中設定輸入檔案與輸出檔案名稱的項目。 背景工作會擷取這個位置中的檔案名稱。

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

若要登錄背景工作，請建立新的 [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) 和新的 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)。 設定背景工作建立器的名稱，讓您可以在稍後識別。 將 [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) 設定為與資訊檔案清單中相同的命名空間和類別名稱字串。 將 [**Trigger**](https://msdn.microsoft.com/library/windows/apps/dn641725) 屬性設定為 **MediaProcessingTrigger** 執行個體。

登錄工作之前，請務必對整個 [**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 集合執行迴圈，並且對您在 [**BackgroundTaskBuilder.Name**](https://msdn.microsoft.com/library/windows/apps/br224771) 屬性中指定名稱的任何工作呼叫 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229870)，取消登錄任何之前已登錄的工作。

呼叫 [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 登錄背景工作。 註冊 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) 和 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224808) 事件的處理常式。

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

當應用程式是一開始就已啟動，如所示**OnNavigatedTo**事件，例如，一般的應用程式會註冊其背景工作。

呼叫 **MediaProcessingTrigger** 物件的 [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn765071) 方法來啟動背景工作。 這個方法所傳回的 [**MediaProcessingTriggerResult**](https://msdn.microsoft.com/library/windows/apps/dn806007) 物件可讓您知道背景工作是否已順利啟動，如果沒有，則可讓您知道背景工作為什麼沒有啟動。 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

一般的應用程式將會啟動背景工作以回應使用者互動，例如中的 UI 控制項的**Click**事件。

背景工作更新作業的進度時，會呼叫 **OnProgress** 事件處理常式。 您可以利用這個機會以進度資訊更新您的 UI。

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

背景工作執行完成時，會呼叫 **OnCompleted** 事件處理常式。 這是另一個可以更新 UI 以提供狀態資訊給使用者的機會。

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




