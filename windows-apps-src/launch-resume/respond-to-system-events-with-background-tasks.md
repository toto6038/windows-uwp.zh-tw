---
author: TylerMSFT
title: 使用背景工作回應系統事件
description: 了解如何建立回應 SystemTrigger 事件的背景工作。
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
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
ms.openlocfilehash: 45f6e10bc355e3a2dc054d54fef35fbeb1095dc7
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "3231856"
---
# <a name="respond-to-system-events-with-background-tasks"></a>使用背景工作回應系統事件

**重要 API**

- [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
- [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

了解如何建立回應 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 事件的背景工作。

本主題假設您有為 App 撰寫的背景工作類別，且這個工作需要執行以回應系統所觸發的事件，例如當網際網路可用性發生變更或是使用者登入時。 本主題將焦點放在 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 類別。 如需有關撰寫背景工作類別的詳細資訊，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

## <a name="create-a-systemtrigger-object"></a>建立 SystemTrigger 物件

在應用程式程式碼中，建立新的 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 物件。 第一個參數 *triggerType* 指定將啟用這個背景工作的系統事件觸發程序類型。 如需事件類型清單，請參閱 [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)。

第二個參數 *OneShot* 會指定背景工作將只在下一次系統事件發生時執行一次，還是每次系統事件發生時都執行，直到工作解除登錄為止。

下列程式碼會指定背景工作在每當網際網路變成可用時就執行：

```csharp
SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemTrigger internetTrigger{
    Windows::ApplicationModel::Background::SystemTriggerType::InternetAvailable, false};
```

```cpp
SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
```

## <a name="register-the-background-task"></a>登錄背景工作

呼叫背景工作登錄函式以登錄背景工作。 如需有關註冊背景工作的詳細資訊，請參閱[註冊背景工作](register-a-background-task.md)。

下列程式碼會為在跨處理序中執行的背景處理序註冊背景工作。 如果您呼叫的是與主控 App 在相同處理序中執行的背景工作，您就不會設定 `entrypoint`：

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass"; // Namespace name, '.', and the name of the class containing the background task
string taskName   = "Internet-based background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" }; // don't set for in-process background tasks.
std::wstring taskName{ L"Internet-based background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass"; // don't set for in-process background tasks
String ^ taskName   = "Internet-based background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

> [!NOTE]
> 通用 Windows 平台應用程式必須呼叫[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) ，才能登錄任何背景觸發程序類型。

為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

> [!NOTE]
> 背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的 App 能夠妥善處理背景工作註冊失敗的狀況；反之，如果 App 需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。
 
## <a name="remarks"></a>備註

若要查看背景工作登錄的執行方式，請下載[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)。

背景工作可以執行來回應 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 和 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 事件，但是您還是必須 [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。 您也必須在登錄任何背景工作類型之前先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

App 可以登錄會回應 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)、[**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 和 [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831) 事件的背景工作，以便在即使 app 不在前景的情況下，也能夠與使用者進行即時通訊。 如需詳細資訊，請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)。

## <a name="related-topics"></a>相關主題

* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [建立及註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)
