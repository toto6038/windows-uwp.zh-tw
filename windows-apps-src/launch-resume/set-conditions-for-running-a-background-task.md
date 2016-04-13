---
title: 設定執行背景工作的條件
description: 了解如何設定條件以控制背景工作的執行時間。
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
---

# 設定執行背景工作的條件


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

了解如何設定條件以控制背景工作的執行時間。

有時，除了會觸發工作的事件之外，背景工作還需要符合某些條件才能順利執行。 您可以在登錄背景工作時由 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 指定一或多個條件。 在引發觸發程序之後，會檢查條件，將背景工作排入佇列，但是必 須等到符合所有必要的條件之後才會執行它。

將條件放在背景工件可以避免執行不必要的工作，以延長電池壽命和 CPU 執行階段。 例如，如果背景工作在計時器上執行，並且需要網際網路連線，則登錄工作之前，請將 **InternetAvailable** 條件新增到 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)。 這樣可以透過讓工作在計時器已經過且網際網路可用時執行，以協助防止工作不必要地使用系統資源與電池使用時間。

**注意** 藉由在相同的 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 上多次呼叫 AddCondition，可以結合多個條件。 請小心，不要新增衝突的條件，例如 **UserPresent** 和 **UserNotPresent**。

 

## 建立 SystemCondition 物件


本主題假設您已經有和 app 相關聯的背景工作，而且 app 也已經包含了建立名為 **taskBuilder** 的 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件。

新增條件之前，建立 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 物件，代表背景工作執行時必須生效的條件。 建構函式中，透過提供 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 列舉數值的方式，指定必須滿足的條件。

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

> **注意** 請小心，不要將衝突的條件新增到背景工作。
 

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
> ```
> ```cpp
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
> ```

## Remarks


> **Note**  Chose the right conditions for your background task so that it only runs when it's needed, and doesn't run when it won't work. See [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) for descriptions of the different background task conditions.

> **Note**  This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Related topics


****

* [Create and register a background task](create-and-register-a-background-task.md)
* [Declare background tasks in the application manifest](declare-background-tasks-in-the-application-manifest.md)
* [Handle a cancelled background task](handle-a-cancelled-background-task.md)
* [Monitor background task progress and completion](monitor-background-task-progress-and-completion.md)
* [Register a background task](register-a-background-task.md)
* [Respond to system events with background tasks](respond-to-system-events-with-background-tasks.md)
* [Update a live tile from a background task](update-a-live-tile-from-a-background-task.md)
* [Use a maintenance trigger](use-a-maintenance-trigger.md)
* [Run a background task on a timer](run-a-background-task-on-a-timer-.md)
* [Guidelines for background tasks](guidelines-for-background-tasks.md)

****

* [Debug a background task](debug-a-background-task.md)
* [How to trigger suspend, resume, and background events in Windows Store apps (when debugging)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


