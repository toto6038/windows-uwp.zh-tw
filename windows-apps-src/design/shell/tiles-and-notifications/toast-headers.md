---
author: andrewleader
Description: Learn how to use headers to visually group your toast notifications in Action Center.
title: 快顯通知標頭
label: Toast headers
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
keywords: windows 10, uwp, 快顯通知, 標頭, 快顯通知標頭, 通知, 群組快顯通知, 控制中心
ms.localizationpriority: medium
ms.openlocfilehash: fcc515b811a11be045ce80ed816708d230720a29
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5690055"
---
# <a name="toast-headers"></a>快顯通知標頭

您可以在通知上使用快顯通知標頭，來視覺分組控制中心內的相關通知。

> [!IMPORTANT]
> **需要 Desktop Creators Update 和 Notifications 程式庫 1.4.0**：您必須執行桌上型電腦組建 15063 或更新版本，才能看見快顯通知標頭。 您必須使用版本 1.4.0 或更高版本的 [UWP Community Toolkit Notifications NuGet 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，以便在快顯通知內容中建構標頭。 只有桌上型電腦才支援標頭。

如下所示，群組交談已整合至單一標頭「露營!!」下方。 交談中的每個個別訊息都是不同的快顯通知，但共用相同的快顯通知標頭。

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

您也可以選擇依照類別來視覺分組您的通知，例如航班提醒、包裹追蹤等。

## <a name="add-a-header-to-a-toast"></a>新增標頭到快顯通知

以下是將標頭加入快顯通知的方式。

> [!NOTE]
> 只有桌上型電腦才支援標頭。 不支援標頭的裝置會直接忽略標頭。

```csharp
ToastContent toastContent = new ToastContent()
{
    Header = new ToastHeader()
    {
        Id = "6289",
        Title = "Camping!!",
        Arguments = "action=openConversation&id=6289",
    },

    Visual = new ToastVisual() { ... }
};
```

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

總結來說...

1. 新增 **Header** 到 **ToastContent**
2. 指派必要的 **Id**、**Title** 和 **Arguments** 屬性
3. 傳送您的通知 ([深入了解](send-local-toast.md))
4. 在另一個通知上，使用相同標頭 **Id** 將它們整合在同一個標頭下。 **Id** 是用來判斷是否應將通知分組在一起的唯一屬性，這表示 **Title** 和 **Arguments** 可以不同。 會使用來自群組中最新通知的 **Title** 和 **Arguments**。 如果該通知被移除，則 **Title** 和 **Arguments** 會退回下一個最新的通知。


## <a name="handle-activation-from-a-header"></a>處理來自標頭的啟用

標頭可供使用者點按，因此使用者可以按下標頭以從您的應用程式了解更多資訊。

因此，應用程式可以在標頭上提供 **Arguments**，類似於快顯本身的啟動引數。

啟用的處理方式與[一般快顯通知啟用](send-local-toast.md#handling-activation-1)相同，這表示您可以在 `App.xaml.cs` 的 **OnActivated** 方法中擷取這些引數，如同當使用者按下快顯通知的主體或快顯通知的按鈕時您所執行的動作。

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>其他資訊

標頭會在視覺上分隔與分組通知。 這不會變更其他任何邏輯，包括應用程式可以擁有的通知數目上限 (20) 以及通知清單的先進先出行為。

標頭中的通知順序如下所示... 對於特定的應用程式，會首先出現來自應用程式的最新通知 (以及整個標頭群組，如果屬於標題的一部分)。

**Id** 可以是您選擇的任何字串。 **ToastHeader** 中的任何屬性沒有長度或字元限制。 唯一限制是您的整個 XML 快顯通知內容不可大於 5 KB。

建立標頭不會變更 [查看更多] 按鈕出現之前顯示在控制中心的通知數目 (這個數字預設為 3，使用者可以在通知的系統設定中針對每個應用程式進行設定)。

按一下標頭，如同按一下應用程式標題，並不會清除屬於此標頭的任何通知 (您的應用程式應該使用快顯通知 API 來清除相關通知)。


## <a name="related-topics"></a>相關主題

- [傳送本機快顯通知及處理啟用](send-local-toast.md)
- [快顯通知內容文件](adaptive-interactive-toasts.md)