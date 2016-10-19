---
author: TylerMSFT
title: "設定執行背景工作的條件"
description: "了解如何設定條件以控制背景工作的執行時間。"
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
translationtype: Human Translation
ms.sourcegitcommit: b877ec7a02082cbfeb7cdfd6c66490ec608d9a50
ms.openlocfilehash: 0d90511c9fcfd722dfcc51a8ff8e5163e31e9fdf

---

# 設定執行背景工作的條件

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

了解如何設定控制背景工作執行時機的條件。

有時，除了會觸發工作的事件之外，背景工作還需要在某些條件符合的情況下，才能順利執行。 登錄背景工作時，您可以指定一或多個由 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 指定的條件。 在引發觸發程序之後，會檢查條件，將背景工作排入佇列，但是必 須等到符合所有必要的條件之後才會執行它。

將條件放在背景工件可以避免執行不必要的工作，以延長電池壽命和 CPU 執行階段。 例如，如果背景工作在計時器上執行，並且需要網際網路連線，則在登錄工作之前，請先將 **InternetAvailable** 條件新增到 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)。 這樣可以藉由讓工作只有在計時器時間已經過「並且」**網際網路可用時才執行，協助防止工作不必要地使用系統資源與電池電力。

您也可以藉由在相同的 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 上多次呼叫 AddCondition 來結合多個條件。 請小心，不要新增衝突的條件，例如 **UserPresent** 和 **UserNotPresent**。

## 建立 SystemCondition 物件

本主題假設您有一個已經與 App 關聯的背景工作，而且 App 已經包含會建立名為 **taskBuilder** 之 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件的程式碼。  如果您需要先建立背景工作，請參閱[建立並登錄單一處理程序背景工作](create-and-register-a-singleprocess-background-task.md)或[建立並登錄在個別處理程序中執行的背景工作](create-and-register-a-background-task.md)。

本主題既適用於與前景 App 在相同處理程序中執行的背景工作，也適用於與前景 App 在個別處理程序中執行的背景工作。

在新增條件之前，請先建立 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 物件，此物件代表背景工作執行時必須生效的條件。 建構函式中，透過提供 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 列舉數值的方式，指定必須滿足的條件。

下列程式碼會建立 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 物件，以指定網際網路可用性做為條件需求：

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## 將 SystemCondition 物件新增到背景工作


如果要新增條件，請呼叫 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件上的 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 方法，並將 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 物件傳給它。

下列程式碼使用 TaskBuilder 登錄 InternetAvailable 背景工作條件：

> [!div class="tabbedCodeSnippets"]
> ```cs
> taskBuilder.AddCondition(internetCondition);
> ```
> ```cpp
> taskBuilder->AddCondition(internetCondition);
> ```

## 登錄背景工作


現在您可以使用 [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法來登錄背景工作，而且只有在滿足指定條件時，工作才會啟動。

下列程式碼會登錄工作並儲存產生的 BackgroundTaskRegistration 物件：

> [!div class="tabbedCodeSnippets"]
> ```cs
> BackgroundTaskRegistration task = taskBuilder.Register();
> ```
> ```cpp
> BackgroundTaskRegistration ^ task = taskBuilder->Register();
> ```

> **注意** 通用 Windows app 在登錄任何背景觸發程序類型之前，必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

> **注意** 背景工作登錄參數會在登錄時受到驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 確認您的 app 能夠妥善處理背景工作登錄失敗的狀況；反之，如果 app 依賴有效的驗證物件，嘗試登錄工作之後，可能會當機。

## 在背景工作上放置多個條件

若要新增多個條件，您的應用程式必須多次呼叫 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 方法。 這些呼叫必須在工作登錄生效之前發生。

> **注意：**請小心，不要將衝突的條件新增到背景工作。
 

下列程式碼片段會在建立並登錄背景工作的內容中顯示多個條件：

> [!div class="tabbedCodeSnippets"]
```cs
> //
> // Set up the background task.
> //
>
> TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
>
> var recurringTaskBuilder = new BackgroundTaskBuilder();
>
> recurringTaskBuilder.Name           = "Hourly background task";
> recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder.SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
>
> recurringTaskBuilder.AddCondition(userCondition);
> recurringTaskBuilder.AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```
```cpp
> //
> // Set up the background task.
> //
>
> TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
>
> auto recurringTaskBuilder = ref new BackgroundTaskBuilder();
>
> recurringTaskBuilder->Name           = "Hourly background task";
> recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder->SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
>
> recurringTaskBuilder->AddCondition(userCondition);
> recurringTaskBuilder->AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## 備註


> **注意**：為背景工作選擇正確條件，讓工作只在需要時才執行，而不會在不該執行時執行。 如需不同背景工作條件的說明，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

> **注意**：本文章適用於撰寫通用 Windows 平台 (UWP) App 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題

****

* [建立並登錄在個別處理程序中執行的背景工作](create-and-register-a-background-task.md)
* [建立並登錄單一處理程序背景工作](create-and-register-a-singleprocess-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)

****

* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Aug16_HO3-->


