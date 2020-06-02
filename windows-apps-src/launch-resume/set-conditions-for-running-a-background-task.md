---
title: 設定執行背景工作的條件
description: 了解如何設定條件以控制背景工作的執行時間。
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 04f351a2eed5290e31a3f40c5421addf01422154
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262779"
---
# <a name="set-conditions-for-running-a-background-task"></a>設定執行背景工作的條件

**重要 API**

- [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)
- [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

了解如何設定條件以控制背景工作的執行時間。

有時，背景工作還需要在某些條件符合的情況下，才能順利執行。 登錄背景工作時，您可以指定一或多個由 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 指定的條件。 在引發觸發程序之後，會檢查條件。 接著將背景工作排入佇列，但是必 須等到符合所有必要的條件之後才會執行它。

將條件放在背景工件可以避免執行不必要的工作，以延長電池壽命和節省 CPU。 例如，如果背景工作在計時器上執行，並且需要網際網路連線，則在登錄工作之前，請先將 **InternetAvailable** 條件新增到 [**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)。 這有助於防止工作使用系統資源和電池壽命，只有在計時器已過*並*可使用網際網路時，才會執行背景工作。

您也可以在相同的[**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)上多次呼叫**AddCondition** ，以結合多個條件。 請小心，不要新增衝突的條件，例如 **UserPresent** 和 **UserNotPresent**。

## <a name="create-a-systemcondition-object"></a>建立 SystemCondition 物件

本主題假設您有一個已經與 App 關聯的背景工作，而且 App 包含的程式碼已經建立名為 **taskBuilder** 的 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 物件。  如果您必須先建立背景工作，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

本主題既適用於與前景 App 在不同處理序中執行的背景工作，也適用於與前景 App 在相同處理序中執行的背景工作。

在新增條件之前，請先建立 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 物件，來代表背景工作執行時必須生效的條件。 在建構函式中，透過提供 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 列舉數值的方式，指定必須滿足的條件。

下列程式碼會建立 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 物件，以指定 **InternetAvailable** 條件：

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>將 SystemCondition 物件新增到背景工作

如果要新增條件，請呼叫 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 物件上的 [**AddCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 方法，並將 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 物件傳給它。

下列程式碼使用 **taskBuilder** 來新增 **InternetAvailable** 條件。

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>註冊您的背景工作

現在您可以使用 [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) 方法來登錄背景工作，而且只有在滿足指定條件時，背景工作才會啟動。

下列程式碼會登錄工作並儲存產生的 BackgroundTaskRegistration 物件：

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> 背景工作註冊參數是在註冊之時進行驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的 App 能夠妥善處理背景工作註冊失敗的狀況；反之，如果 App 需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

## <a name="place-multiple-conditions-on-your-background-task"></a>在背景工作上放置多個條件

若要新增多個條件，您的應用程式必須多次呼叫 [**AddCondition**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 方法。 這些呼叫必須在工作登錄生效之前發生。

> [!NOTE]
> 請小心不要將衝突的條件新增至背景工作。

下列程式碼片段顯示建立和註冊背景工作的內容中的多個條件。

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>備註

> [!NOTE]
> 選擇您背景工作的條件，使其只在需要時才執行，而且不會在不應的情況下執行。 如需不同背景工作條件的說明，請參閱 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

## <a name="related-topics"></a>相關主題

* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [建立及註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [註冊背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
