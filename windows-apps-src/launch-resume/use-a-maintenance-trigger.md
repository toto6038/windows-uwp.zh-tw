---
title: 使用維護觸發程序
description: 了解如何在裝置使用 AC 電源時，使用 MaintenanceTrigger 類別於背景中執行輕量型程式碼。
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: c155a2b80826669693c3250282076d8a1b27ee83
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259398"
---
# <a name="use-a-maintenance-trigger"></a>使用維護觸發程序

**重要 API**

- [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)

了解如何在裝置使用 AC 電源時，使用 [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 類別於背景中執行輕量型程式碼。

## <a name="create-a-maintenance-trigger-object"></a>建立維護觸發程序物件

這個範例假設您已經有能夠在裝置使用 AC 電源時，於背景中執行以強化應用程式的輕量型程式碼。 本主題將焦點放在 [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) (與 [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 類似)。

如需有關撰寫背景工作類別的詳細資訊，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

建立新的 [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 物件。 第二個參數 *OneShot* 會指定維護工作將只執行一次，還是要繼續定期執行。 如果 *OneShot* 設定成 True，第一個參數 (*FreshnessTime*) 會指定排定背景工作之前要等待的時間 (以分鐘為單位)。 如果 *OneShot* 設定成 False，則 *FreshnessTime* 會指定背景工作的執行頻率。

> [!NOTE]
> 如果*FreshnessTime*設定為小於15分鐘，則嘗試註冊背景工作時，會擲回例外狀況（exception）。

這個範例程式碼會建立一個每小時執行一次的觸發程式。

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>(選用) 新增條件

- 如有需要，可建立背景工作條件以控制何時執行工作。 在符合條件之前，條件會防止背景工作執行，如需詳細資訊，請參閱[設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)。

這個範例中，條件是設成 **InternetAvailable**，讓維護只有在網際網路可用 (或是網際網路變成可用時) 時才會執行。 如需可能的背景工作條件的清單，請參閱 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

下列程式碼會將條件新增到維護工作建立器：

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>登錄背景工作

- 呼叫背景工作登錄函式以登錄背景工作。 如需有關登錄背景工作的詳細資訊，請參閱[登錄背景工作](register-a-background-task.md)。

下列程式碼會登錄維護工作。 請注意，它會假設您的背景工作與您的應用程式在個別的處理程序中執行，因為它指定了 `entryPoint`。 如果您的背景工作與您的應用程式在相同處理程序中執行，您就不需指定 `entryPoint`。

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> 針對桌上型電腦以外的所有裝置系列，如果裝置的記憶體變成不足，背景工作可能就會終止。 如果沒有顯示記憶體不足的例外狀況，或是應用程式沒有處理該狀況，背景工作將會在沒有警告也沒有引發 OnCanceled 事件的情況下終止。 這有助於確保前景應用程式的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

> [!NOTE]
> 通用 Windows 平臺應用程式必須先呼叫[**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) ，才能註冊任何背景觸發程式類型。

為了確保您的通用 Windows 應用程式在您對應用程式發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)，然後在應用程式於更新後啟動時呼叫 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

> [!NOTE]
> 背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的應用程式能夠妥善處理背景工作註冊失敗的狀況；反之，如果應用程式需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

## <a name="related-topics"></a>相關主題

* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [註冊背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式中觸發暫止、繼續和背景事件（在進行調試時）](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
