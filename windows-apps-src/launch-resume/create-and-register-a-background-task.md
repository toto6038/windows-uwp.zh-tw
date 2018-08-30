---
author: TylerMSFT
title: 建立及註冊跨處理序的背景工作
description: 建立跨處理序背景工作類別並加以註冊，讓它在應用程式不在前景時也能執行。
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.author: twhitney
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: a599fdef47bb681ef4909fe5bba2a01a1687ba66
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3114825"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>建立及註冊跨處理序的背景工作

**重要 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

建立背景工作類別並加以註冊，讓它在 App 不在前景時也能執行。 本主題示範如何建立及註冊與 App 處理序不同處理序中執行的背景工作。 若要在前景應用程式中直接進行背景工作，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)。

> [!NOTE]
> 如果您使用背景工作在背景播放媒體，請參閱[在背景播放媒體](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)，以了解有關 Windows10 版本 1607 中讓此操作更容易進行的改進功能詳細資訊。

## <a name="create-the-background-task-class"></a>建立背景工作類別

您可以撰寫實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 介面的類別，在背景執行程式碼。 藉由使用，例如， [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)或[**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)觸發特定事件時，就會執行此程式碼。

下列步驟示範如何撰寫實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 介面的新類別。

1.  為背景工作建立一個新專案，並將其新增到您的方案中。 若要這樣做，您在**方案總管] 中**的 [方案] 節點上按一下滑鼠右鍵並選取 \ [**加入** \> **新的專案**。 選取 [ **Windows 執行階段元件**專案類型名稱專案，然後按一下 [確定]。
2.  從您的通用 Windows 平台 (UWP) app 專案參考背景工作專案。 為 C# 或 c + + 應用程式，在您的應用程式專案中，**參考**上按一下滑鼠右鍵並選取 [**加入新參考**。 在 **\[方案\]** 下選取 **\[專案\]**，然後選取您背景工作專案的名稱並按一下 **\[確定\]**。
3.  若要在背景工作專案中，新增實作[**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)介面的新類別。 [**IBackgroundTask.Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)方法就是一個會在觸發指定的事件; 時將會呼叫的必要的進入點需要具備這個方法，每個背景工作。

> [!NOTE]
> 背景工作類別本身&mdash;及背景工作專案中的所有其他類別&mdash;一定要位於**密封**（或**最後一個**） 的**公用**類別。

下列範例程式碼示範一個非常基本的起點的背景工作類別。

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  如果您在背景工作中執行任何非同步程式碼，您的背景工作就需要使用延遲。 如果您沒有使用延遲，就背景工作處理程序可能意外終止如果**Run**方法傳回之前執行任何非同步工作完成。

要求**Run**方法中呼叫非同步方法之前的延遲。 將延遲儲存至類別資料成員，以便能夠從非同步方法存取它。 在非同步程式碼執行完成之後，請宣告延遲完成。

下列範例程式碼會取得延遲、 儲存它，以及完成非同步程式碼時放開。

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral()
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> 在 C# 中，可以使用 **async/await** 關鍵字呼叫背景工作的非同步方法。 在 C + + /CX，可以使用工作鏈結達成類似的結果。

如需有關非同步模式的詳細資訊，請參閱[非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187335)。 如需有關如何使用延遲防止背景工作提前停止的其他範例，請參閱[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)。

下列步驟是在您 app 的其中一個類別 (例如 MainPage.xaml.cs) 中完成。

> [!NOTE]
> 您也可以建立專門用來登錄背景工作的函式&mdash;請[註冊背景工作](register-a-background-task.md)。 在此情況下，而不是使用接下來三個步驟，您可以只需建構觸發程序及它提供給登錄函式，以及在工作名稱、 工作進入點，和 （選擇性） 條件。

## <a name="register-the-background-task-to-run"></a>註冊要執行的背景工作

1.  了解背景工作是否已登錄逐一[**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787)屬性。 這個步驟很重要；如果您的 app 不會檢查現有背景工作的註冊情況，就很容易多次註冊同一工作，因而造成效能問題，且將耗費大量 CPU 時間才能完成工作。

下列範例會逐一查看**AllTasks**屬性，並設定旗標變數設為 true 工作已登錄。

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  如果應用程式工作尚未登錄，則使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 建立您背景工作的執行個體。 工作進入點應該是您的背景工作類別名稱，並以命名空間為前置詞。

背景工作觸發程序會控制背景工作將在何時執行。 如需可用觸發程序的清單，請參閱 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)。

例如，此程式碼會建立新的背景工作，並將設定**TimeZoneChanged**觸發程序就會發生時也能執行：

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  您可以新增條件，以控制觸發程序事件發生後何時執行工作 (選用)。 例如，如果您希望等到使用者在場時才執行工作，可以使用 **UserPresent** 條件。 如需可用條件的清單，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

下列範例程式碼會指派條件，要求必須要有使用者：

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  透過呼叫 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件的 Register 方法來註冊背景工作。 儲存 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 結果以便在下一個步驟使用。

下列程式碼會註冊背景工作並儲存結果：

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> 通用 Windows app 必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，才能登錄任何背景觸發程序類型。

為了確保您的通用 Windows app 在您發行更新之後繼續正常執行，請使用 **ServicingComplete** (請參閱 [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) 觸發程序來執行任何更新後的組態變更，例如移轉 App 的資料庫及登錄背景工作。 此時，最佳做法是將與舊版 App 關聯的背景工作取消登錄 (請參閱 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471))，然後為新版 App 登錄背景工作 (請參閱 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485))。

如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

## <a name="handle-background-task-completion-using-event-handlers"></a>使用事件處理常式來處理背景工作完成

您應該向 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 註冊方法，以便讓 app 可以從背景工作取得結果。 當應用程式會啟動或繼續執行時，如果背景工作已自上次在前景的應用程式已完成，將會呼叫標記的方法。 (如果背景工作是在您 app 目前處於前景時完成，將會立即呼叫 OnCompleted 方法)。

1.  在 OnCompleted 方法中處理背景工作的完成。 例如，背景工作結果可能會導致 UI 更新。 OnCompleted 事件處理常式方法需要在此處顯示的方法配置，即使這個範例未使用 *args* 參數亦同。

下列範例程式碼會辨識背景工作完成並呼叫範例 UI 更新方法，以取得訊息字串。

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> UI 更新應該以非同步方式執行，以避免 UI 執行緒阻塞。 例如，請參閱[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)中的 UpdateUI 方法。

2.  返回登錄背景工作的位置。 在該行的程式碼後面，新增 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 物件。 提供您的 OnCompleted 方法做為 **BackgroundTaskCompletedEventHandler** 建構函式的參數。

下列範例程式碼會將 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 新增到 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)：

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>在應用程式資訊清單中宣告您的 app 使用背景工作

在 app 能執行背景工作之前，您必須在 app 資訊清單中宣告每一個背景工作。 如果您的應用程式嘗試登錄背景工作觸發程序而未列在資訊清單中，登錄的背景工作將會失敗與 「 執行階段類別未登錄 」 錯誤。

1.  透過開啟名為 Package.appxmanifest 的檔案來開啟封裝資訊清單設計工具。
2.  開啟 **\[宣告\]** 索引標籤。
3.  從 **\[可用宣告\]** 下拉式清單中選擇 **\[背景工作\]**，然後按一下 **\[新增\]**。
4.  選取 **\[系統事件\]** 核取方塊。
5.  在**進入點：** ] 文字方塊中，輸入您背景類別也就是此範例中為 Tasks.ExampleBackgroundTask 的名稱與命名空間。
6.  關閉資訊清單設計工具。

下列 Extensions 元素會新增至您的 Package.appxmanifest 檔案中以註冊背景工作：

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>摘要與後續步驟

您現在應該了解如何撰寫背景工作類別的基本概念，如何在 app 內註冊背景工作，以及如何讓 app 辨識背景工作已經完成。 您也應該了解如何更新 app 資訊清單，才能讓您的 app 成功登錄背景工作。

> [!NOTE]
> 下載[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)，以查看使用背景工作、完整且健全的 UWP app 內容中的類似程式碼範例。

請參閱下列 API 參考的相關主題、背景工作概念指引，以及撰寫使用背景工作之 app 的更詳細說明。

## <a name="related-topics"></a>相關主題

**詳細的背景工作教學主題**

* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [登錄背景工作](register-a-background-task.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)。
* [將跨處理序背景工作轉換成同處理序背景工作](convert-out-of-process-background-task.md)  

**背景工作指引**

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**背景工作 API 參考**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)
