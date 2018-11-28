---
Description: Learn how to create multi-step interactions in your notifications.
title: 具有擱置中更新啟用的快顯通知
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp, 快顯通知, 擱置中的更新, pendingupdate, 多步驟互動性, 多步驟互動
ms.localizationpriority: medium
ms.openlocfilehash: b1574ee2913bd2889af204aae1089dc170df95b8
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7839283"
---
# <a name="toast-with-pending-update-activation"></a>具有擱置中更新啟用的快顯通知

您可使用 **PendingUpdate** 在快顯通知中建立多步驟互動。 例如，如下所示，您可以建立一系列快顯通知，其中的後續快顯通知取決於先前快顯通知的回應。

![具有擱置更新的快顯通知](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **需要 Desktop Fall Creators Update 和 Notifications 程式庫 2.0.0**：您必須執行桌上型電腦組建 16299 或更新版本，才能看見擱置中更新運作。 您必須使用版本 2.0.0 或更高版本的 [UWP Community Toolkit Notifications NuGet 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，以便在按鈕上指派 **PendingUpdate**。 只有桌上型電腦才支援 **PendingUpdate**，其他裝置上將會忽略。


## <a name="prerequisites"></a>必要條件

本文假設您已熟悉下列知識...

- [建構快顯通知內容](adaptive-interactive-toasts.md)
- [傳送快顯通知及處理背景啟用](send-local-toast.md)


## <a name="overview"></a>概觀

若要實作在啟用行為後使用擱置中更新的快顯通知...

1. 在快顯通知背景啟用按鈕上，指定 **PendingUpdate** 的 **AfterActivationBehavior**

2. 指派傳送快顯通知時的 **Tag** (也可以是 **Group**)

3. 當使用者按下您的按鈕時，您的背景工作將會啟動，快顯通知將保持在螢幕上的擱置中更新狀態

4. 在您的背景工作中，使用相同的 **Tag** 和 **Group** 以新內容傳送新的快顯通知


## <a name="assign-pendingupdate"></a>指派 PendingUpdate

在快顯通知背景啟用按鈕上，設定 **AfterActivationBehavior** 為 **PendingUpdate**。 請注意，這只適用於具有 **Background** 之 **ActivationType** 的按鈕。

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>在通知上使用標記

為了稍後能夠更換通知，我們已在通知上指派 **Tag** (也可以使用 **Group**)。

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>使用新內容更換快顯通知

為了回應使用者按下按鈕，會觸發您的背景工作，您需要使用新內容更換快顯通知。 只需使用相同的 **Tag** 和 **Group** 傳送新內容，即可更換快顯通知。

因此，由於使用者已經與您的快顯通知互動，強烈建議在回應按鈕點按的更換項目上**將音訊設為靜音**。

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>相關主題

- [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [傳送本機快顯通知及處理啟用](send-local-toast.md)
- [快顯通知內容文件](adaptive-interactive-toasts.md)
- [快顯通知進度列](toast-progress-bar.md)