---
title: 群組背景工作註冊
description: 將背景工作當做群組成員來註冊或取消註冊，以隔離這些註冊。
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, 背景工作
ms.openlocfilehash: 61419ac45acd27758e3c874ac4b03510561ccaf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155862"
---
# <a name="group-background-task-registration"></a>群組背景工作註冊

**重要 API**

[BackgroundTaskRegistrationGroup 類別](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

背景工作現在可以形成群組來進行註冊，您可以將此群組視為邏輯命名空間。 這種隔離方式有助於確保不同的 App 元件 (或不同的程式庫) 不會干擾彼此的背景工作註冊。

當 App 及其所用架構 (或程式庫) 以相同名稱來註冊背景工作時，App 可能會意外移除架構的背景工作註冊。 App 作者也可能會意外移除架構及程式庫的背景工作註冊，因為他們可以使用 [BackgroundTaskRegistration.AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) 將所有已註冊的背景工作取消註冊。  但是您可以使用群組來隔離背景工作註冊，所以不致於發生這種情況。

## <a name="features-of-groups"></a>群組的功能

* 個別群組可以依據 GUID 進行唯一識別。 這些群組也有相關聯的易記名稱字串，更方便在偵錯時讀取。
* 使用群組可以註冊多個背景工作。
* 在群組中註冊的背景工作不會在 [BackgroundTaskRegistration.AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) 中出現。 因此目前使用 **BackgroundTaskRegistration.AllTasks** 註冊其工作的 App，並不會意外取消註冊群組中的背景工作。 請參閱下列 [群組中的取消註冊背景工作](#unregister-background-tasks-in-a-group) ，以瞭解如何取消註冊所有已註冊為群組一部分的背景觸發程式。
* 每個「背景工作註冊」都有 Group 屬性，可用來判斷與其相關聯的是哪一個群組。
* 使用群組註冊同進程的背景工作，會導致啟用 [BackgroundTaskRegistrationGroup BackgroundActivated](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) 事件，而不是 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_)。

## <a name="register-a-background-task-in-a-group"></a>註冊群組中的背景工作

以下顯示如何將背景工作 (在此範例中，依據時區變更觸發) 註冊為群組成員。

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>取消註冊群組中的背景工作

以下示範如何取消註冊已註冊為群組成員的背景工作。
由於以群組註冊的背景工作不會在 [BackgroundTaskRegistration.AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) 中出現，因此您必須逐一查看群組、尋找註冊到每個群組的背景工作，然後才能將其取消註冊。

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>註冊持續事件

使用同處理序背景工作中的「背景工作註冊群組」時，您會將背景啟用作業導向群組的事件，而不是 Application 或 CoreApplication 物件上的事件。 這可讓 App 中的多個元件來處理啟用作業，而不是將所有啟用代碼路徑都放在 Application 物件中。 以下示範如何針對群組的背景啟用事件來註冊。 首先檢查 [BackgroundTaskRegistration.GetTaskGroup](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) 以判斷群組是否已經註冊。 如果沒有，則使用您的識別碼和易記名稱來建立新的群組。 然後在群組上註冊 BackgroundActivated 事件的事件處理常式。

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```