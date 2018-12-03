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
ms.openlocfilehash: ac6dd17f31dab1898aa394f901613d268c159b06
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458499"
---
# <a name="set-conditions-for-running-a-background-task"></a>設定執行背景工作的條件

**重要 API**

- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
- [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

了解如何設定條件以控制背景工作的執行時間。

有時，背景工作還需要背景工作成功符合特定條件。 登錄背景工作時，您可以指定一或多個由 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 指定的條件。 在引發觸發程序之後，將會檢查條件。 背景工作將會接著會排入佇列，但是它不會執行所有必要的條件必須等到。

將條件放在背景工作會以節省延長電池壽命和 CPU 可以避免不必要地執行。 例如，如果背景工作在計時器上執行，並且需要網際網路連線，則在登錄工作之前，請先將 **InternetAvailable** 條件新增到 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)。 這樣可以藉由讓工作只有在計時器時間已經過 *「並且」* 網際網路可用時才執行，協助防止工作不必要地使用系統資源與電池電力。

它也是可能由相同[**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)上多次呼叫**AddCondition**結合多個條件。 請小心，不要新增衝突的條件，例如 **UserPresent** 和 **UserNotPresent**。

## <a name="create-a-systemcondition-object"></a>建立 SystemCondition 物件

本主題假設您有一個已經與 App 關聯的背景工作，而且 App 包含的程式碼已經建立名為 **taskBuilder** 的 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件。  如果您必須先建立背景工作，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

本主題既適用於與前景 App 在不同處理序中執行的背景工作，也適用於與前景 App 在相同處理序中執行的背景工作。

新增條件之前, 建立[**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)物件來代表作用中必須是執行背景工作的條件。 在建構函式中，指定必須滿足[**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)列舉值的條件。

下列程式碼會建立[**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)物件，指定**InternetAvailable**條件：

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

如果要新增條件，請呼叫 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224769) 物件上的 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224768) 方法，並將 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 物件傳給它。

下列程式碼使用**taskBuilder**來新增**InternetAvailable**條件。

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

現在您可以登錄您的背景工作[**註冊**](https://msdn.microsoft.com/library/windows/apps/br224772)的方法，並指定的條件符合時才會啟動背景工作。

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
> 通用 Windows app 必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，才能登錄任何背景觸發程序類型。

為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

> [!NOTE]
> 背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的 App 能夠妥善處理背景工作註冊失敗的狀況；反之，如果 App 需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

## <a name="place-multiple-conditions-on-your-background-task"></a>在背景工作上放置多個條件

若要新增多個條件，您的應用程式必須多次呼叫 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 方法。 這些呼叫必須在工作登錄生效之前發生。

> [!NOTE]
> 採取小心，不要將衝突的條件新增到背景工作。

下列程式碼片段顯示多個條件，建立並登錄背景工作的內容中。

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
> 選擇您的背景工作的條件，讓它僅能執行時，它需要而且不會執行它不應時。 如需不同背景工作條件的說明，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

## <a name="related-topics"></a>相關主題

* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [建立及註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)
