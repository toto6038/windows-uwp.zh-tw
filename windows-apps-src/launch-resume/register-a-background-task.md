---
title: 登錄背景工作
description: 了解如何建立可重複用來安全登錄大多數背景工作的函式。
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
---

# 登錄背景工作


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**BackgroundTaskRegistration 類別**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskBuilder 類別**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition 類別**](https://msdn.microsoft.com/library/windows/apps/br224834)

了解如何建立可重複用來安全登錄大多數背景工作的函式。

本主題假設您已有需要登錄的背景工作。 (請參閱[建立並登錄背景工作](create-and-register-a-background-task.md)，以取得如何撰寫背景工作的相關資訊)。

這個主題會逐步解說可登錄背景工作的公用程式函式。 這個公用程式函式會先檢查現有登錄，以避免多次登錄工作時可能產生的問題；也可以將系統條件套用到背景工作。 本逐步解說包括這個公用程式函式的完整工作範例。

**注意**  

通用 Windows app 在登錄任何背景觸發程序類型之前，必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

## 定義方法簽章和傳回類型


這個方法會採用背景工作的工作進入點、工作名稱、預先建構的背景工作觸發程序，以及 (選用) [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)。 這個方法會傳回 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 物件。

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint, 
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     
>     // We’ll add code to this function in subsequent steps.
> 
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     
>     // We’ll add code to this function in subsequent steps.
> 
> }
> ```

## 檢查現有登錄


檢查工作是否已登錄。 這是檢查的重點，因為如果多次登錄工作，則觸發該工作時，它就會多次執行；這樣可能會過量使用 CPU，也可能造成未預期的行為。

您可以查詢 [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 屬性並逐一查看結果，以檢查現有登錄。 檢查每個執行個體的名稱 - 如果它符合您要登錄的工作名稱，則中斷迴圈並設定旗標變數，讓您的程式碼能夠在下一個步驟中選擇不同路徑。

> **注意** 使用您 app 專用的背景工作名稱。 確認每個背景工作都有唯一的名稱。

下列程式碼會使用我們在上一個步驟中建立的 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 來登錄背景工作：

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint, 
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
> 
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
> 
>         if (cur.Value.Name == name)
>         {
>             // 
>             // The task is already registered.
>             // 
> 
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     // We’ll register the task in the next step.
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             // 
>             // The task is registered.
>             // 
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     // We’ll register the task in the next step.
> }
> ```

## 登錄背景工作 (或傳回現有登錄)


檢查現有背景工作登錄清單中是否已有該工作。 如果有，則傳回工作的該執行個體。

然後使用新的 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件登錄工作。 這段程式碼應該會檢查條件參數是否為 Null；如果不是，則將條件新增到登錄物件。 傳回由 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法傳回的 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)。

> **注意** 背景工作登錄參數會在登錄時受到驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 確認您的 app 能夠妥善處理背景工作登錄失敗的狀況；反之，如果 app 依賴有效的驗證物件，嘗試登錄工作之後，可能會當機。

下列範例會傳回現有工作，或新增可登錄背景工作的程式碼 (如果有選擇性的系統條件，則也包括在內)：

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint, 
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
> 
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
> 
>         if (cur.Value.Name == taskName)
>         {
>             // 
>             // The task is already registered.
>             // 
> 
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     //
>     // Register the background task.
>     //
> 
>     var builder = new BackgroundTaskBuilder();
> 
>     builder.Name = name;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
> 
>     if (condition != null)
>     {
> 
>         builder.AddCondition(condition);
>     }
> 
>     BackgroundTaskRegistration task = builder.Register();
> 
>     return task;
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
> 
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             // 
>             // The task is registered.
>             // 
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     //
>     // Register the background task.
>     //
> 
>     auto builder = ref new BackgroundTaskBuilder();
> 
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
> 
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
> 
>     BackgroundTaskRegistration ^ task = builder->Register();
> 
>     return task;
> }
> ```

## 完成背景工作登錄公用程式函式


這個範例顯示已完成的背景工作登錄函式。 這個函式可用來登錄大多數的背景工作 (除了網路背景工作之外)。

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> public static BackgroundTaskRegistration RegisterBackgroundTask(string taskEntryPoint,
>                                                                 string taskName,
>                                                                 IBackgroundTrigger trigger,
>                                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
> 
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
> 
>         if (cur.Value.Name == taskName)
>         {
>             // 
>             // The task is already registered.
>             // 
> 
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     //
>     // Register the background task.
>     //
> 
>     var builder = new BackgroundTaskBuilder();
> 
>     builder.Name = taskName;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
> 
>     if (condition != null)
>     {
> 
>         builder.AddCondition(condition);
>     }
> 
>     BackgroundTaskRegistration task = builder.Register();
> 
>     return task;
> }
> ```
> ```cpp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(Platform::String ^ taskEntryPoint,
>                                                              Platform::String ^ taskName,
>                                                              IBackgroundTrigger ^ trigger,
>                                                              IBackgroundCondition ^ condition)
> {
> 
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             // 
>             // The task is registered.
>             // 
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
> 
> 
>     //
>     // Register the background task.
>     //
> 
>     auto builder = ref new BackgroundTaskBuilder();
> 
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
> 
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
> 
>     BackgroundTaskRegistration ^ task = builder->Register();
> 
>     return task;
> }
> ```

> **注意**：本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 
## 相關主題


****

* [建立並登錄背景工作](create-and-register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)

****

* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


