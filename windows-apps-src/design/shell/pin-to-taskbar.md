---
author: mijacobs
Description: You can programmatically pin your app to the taskbar,  bnd you can check if it's currently pinned.
title: 將應用程式釘選到工作列
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, taskbar, taskbar manager, pin to taskbar, primary tile, 工作列, 工作列管理員, 釘選到工作列, 主要磚
ms.localizationpriority: medium
ms.openlocfilehash: 47fcd1f9d090c49ecbd49e05696b33f789973160
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6458473"
---
# <a name="pin-your-app-to-the-taskbar"></a>將應用程式釘選到工作列

您可以寫程式將自己的應用程式釘選到工作列上，就如同您可以[將應用程式釘選到開始功能表](tiles-and-notifications/primary-tile-apis.md)。 您可以檢查您的應用程式目前是否已釘選，以及工作列是否允許釘選。 

![工作列](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **需要 Fall Creators Update**：您的目標必須是 SDK 16299 並執行組建 16299 或更新版本，才能使用工作列 API。

> **重要 API**：[TaskbarManager 類別](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>何時您應要求使用者將您的應用程式釘選在工作列上？ 

[TaskbarManager 類別](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)可讓您要求使用者將應用程式釘選到工作列上；使用者必須核准此要求。 您大費周章地建構了主要應用程式，現在您有機會可以要求使用者將它釘選到工作列。 但在我們深入了解程式碼之前，應先知道以下一些設計您在設計體驗時要注意的事項：

* **務必**在應用程式中以明確的「釘選至工作列」動作製作不具破壞性且可輕鬆關閉的 UX。 避免將對話方塊與飛出視窗用於此用途。 
* **務必**先清楚說明您應用程式的真實意義，再要求使用者釘選。
* **請勿**在該磚已釘選或裝置不支援該磚時，要求使用者釘選您的應用程式。 (本文說明如何判斷是否支援釘選。)
* **請勿**重複要求使用者釘選您的應用程式 (這樣可能會惹煩使用者)。
* **請勿**在沒有明確使用者互動情況下，或在應用程式已最小化/未開啟時呼叫釘選 API。


## <a name="1-check-whether-the-required-apis-exist"></a>1. 檢查所需的 API 是否存在

如果您的應用程式支援舊版 Windows 10，則必須檢查 TaskbarManager 類別是否可用。 您可以使用 [ApiInformation.IsTypePresent 方法](https://docs.microsoft.com/en-us/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_)執行這項檢查。 如果無可用的 TaskbarManager 類別，請避免執行任何 API 呼叫。

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. 檢查工作列是否存在並允許釘選

UWP 應用程式可在各種裝置上執行；並非所有的裝置皆支援工作列。 目前只有傳統型裝置支援工作列。 

即使工作列可供使用，使用者電腦上的群組原則仍可停用工作列釘選。 因此，在您嘗試釘選應用程式之前，您必須檢查是否支援釘選到工作列。 如果工作列存在並允許釘選，[TaskbarManager.IsPinningAllowed 屬性](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed)會傳回 true。 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> 如果您不想要將應用程式釘選到工作列，而且只想要了解工作列是否可供使用，請使用 [TaskbarManager.IsSupported 屬性](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported)。


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. 檢查應用程式目前是否已釘選到工作列

顯然地，如果應用程式已釘選在工作列，則沒有必要去要求使用者或讓您將應用程式釘選到工作列。 您可以使用 [TaskbarManager.IsCurrentAppPinnedAsync 方法](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync)先檢查應用程式是否已釘選，再要求使用者。

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4. 釘選您的應用程式

如果工作列存在並允許釘選，且您的應用程式目前尚未釘選，您可能會想要顯示精巧的秘訣讓使用者知道他們可以釘選您的應用程式。 例如，您可能會在 UI 中使用者可以點擊的某一處顯示釘選圖示。 

如果使用者按一下您的釘選建議 UI，您可以呼叫 [TaskbarManager.RequestPinCurrentAppAsync method](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync)。 此方法會顯示對話方塊要求使用者確認他們要將您的應用程式釘選到工作列上。

> [!IMPORTANT]
> 這必須從前景 UI 執行緒呼叫，否則將會擲回例外狀況。

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![釘選對話方塊](images/taskbar/pin-dialog.png)

此方法會傳回布林值，指出您的應用程式現在是否已釘選到工作列。 如果您的應用程式已釘選，此方法會立即傳回 true，而不會向使用者顯示對話方塊。 如果使用者按一下對話方塊中的 \[否\]，或不允許將您的應用程式釘選到工作列，此方法會傳回 false。 否則，使用者若已按一下 \[是\] 且應用程式已釘選時，API 將會傳回 true。


## <a name="resources"></a>資源

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [TaskbarManager 類別](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [將應用程式釘選到開始功能表](tiles-and-notifications/primary-tile-apis.md)