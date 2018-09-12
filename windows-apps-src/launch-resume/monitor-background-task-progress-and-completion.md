---
author: TylerMSFT
title: 監視背景工作進度和完成
description: 了解應用程式如何辨識背景工作所報告的進度與完成。
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: ef57c6293b37f91653b5f825881b1446e38a824b
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2018
ms.locfileid: "3930852"
---
# <a name="monitor-background-task-progress-and-completion"></a>監視背景工作進度和完成

**重要 API**

- [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
- [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
- [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

了解 App 如何辨識在跨處理序中執行之背景工作所報告的進度與完成。 (針對同處理序背景工作，您可以設定共用變數來表示進度和完成)。

背景工作的進度和完成可以透過 App 程式碼監視。 若要執行這項作業，App 要訂閱已向系統註冊之背景工作的事件。

- 這個主題假設您有一個註冊背景工作的 App。 若要快速開始建立背景工作，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。 如需條件與觸發程序的深入資訊，請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)。

## <a name="create-an-event-handler-to-handle-completed-background-tasks"></a>建立事件處理常式，以處理完成的背景工作。

### <a name="step-1"></a>步驟 1
建立事件處理常式函式，以處理完成的背景工作。 這個程式碼需要遵循特定的配置， [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803)物件和[**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778)物件。

針對**OnCompleted**背景工作事件處理常式方法使用下列配置。

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    // TODO: Add code that deals with background task completion.
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task completion.
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    // TODO: Add code that deals with background task completion.
};
```

### <a name="step-2"></a>步驟 2
將程式碼新增至會處理背景工作完成的事件處理常式。

例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會更新 UI。

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    UpdateUI();
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    UpdateUI();
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{    
    UpdateUI();
};
```

## <a name="create-an-event-handler-function-to-handle-background-task-progress"></a>建立事件處理常式函式，以處理背景工作進度。

### <a name="step-1"></a>步驟 1
建立事件處理常式函式，以處理完成的背景工作。 這個程式碼需要遵循特定的配置，其中包括 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) 物件與 [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 物件：

針對 OnProgress 背景工作事件處理常式方式使用下列配置：

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    // TODO: Add code that deals with background task progress.
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task progress.
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    // TODO: Add code that deals with background task progress.
};
```

### <a name="step-2"></a>步驟 2
將程式碼新增至會處理背景工作完成的事件處理常式。

例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會使用透過 *args* 參數所傳遞的進度狀態來更新 UI：

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    var progress = "Progress: " + args.Progress + "%";
    BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    UpdateUI();
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
{
    winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    auto progress = "Progress: " + args->Progress + "%";
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
};
```

## <a name="register-the-event-handler-functions-with-new-and-existing-background-tasks"></a>使用新的和現有的背景工作來登錄事件處理函式

### <a name="step-1"></a>步驟 1
當應用程式第一次登錄背景工作時，它應該登錄以接收其進度和完成更新，以防應用程式在前景仍然為使用中時執行工作。

例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會在它登錄的每個背景工作上呼叫下列函式：

```csharp
private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
{
    task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
}
```

```cppwinrt
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(Windows::ApplicationModel::Background::IBackgroundTaskRegistration const& task)
{
    auto progress{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
    {
        winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    } };

    task.Progress(progress);

    auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
    {
        UpdateUI();
    } };

    task.Completed(completed);
}
```

```cpp
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
{
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        auto progress = "Progress: " + args->Progress + "%";
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    };

    task->Progress += ref new BackgroundTaskProgressEventHandler(progress);

    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {
        UpdateUI();
    };

    task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
}
```

### <a name="step-2"></a>步驟 2
當應用程式啟動或是瀏覽至背景工作狀態是相關的新頁面時，它應該取得目前登錄的背景工作清單，並將它們與進度和完成事件處理函式關聯。 應用程式目前登錄的背景工作清單保留在 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 屬性中。

例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會使用下列程式碼來附加瀏覽 SampleBackgroundTask 頁面時要執行的事件處理常式：

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    foreach (var task in BackgroundTaskRegistration.AllTasks)
    {
        if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value);
            BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
        }
    }

    UpdateUI();
}
```

```cppwinrt
void SampleBackgroundTask::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& /* e */)
{
    // A pointer back to the main page. This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    m_rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

    for (auto const& task : allTasks)
    {
        if (task.Value().Name() == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value());
            break;
        }
    }

    UpdateUI();
}
```

```cpp
void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
{
    // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto iter = BackgroundTaskRegistration::AllTasks->First();
    auto hascur = iter->HasCurrent;
    while (hascur)
    {
        auto cur = iter->Current->Value;

        if (cur->Name == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(cur);
            break;
        }

        hascur = iter->MoveNext();
    }

    UpdateUI();
}
```

## <a name="related-topics"></a>相關主題

* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)
