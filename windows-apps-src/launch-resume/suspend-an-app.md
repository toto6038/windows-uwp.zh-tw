---
author: TylerMSFT
title: 處理 app 暫停
description: 了解如何在系統暫停您的應用程式時，儲存重要的應用程式資料。
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 7cb93c410f583884f75f21d9beda03db87c024f9
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5696258"
---
# <a name="handle-app-suspend"></a>處理應用程式暫停

**重要 API**

- [**暫停**](https://msdn.microsoft.com/library/windows/apps/br242341)

了解如何在系統暫停您的 app 時，儲存重要的應用程式資料。 這個範例會為 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件登錄一個事件處理常式，並將一個字串儲存至檔案。

## <a name="register-the-suspending-event-handler"></a>登錄暫停事件處理常式

登錄以處理 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件，它會指示 app 必須在系統暫停 app 之前，儲存自己的應用程式資料。

```csharp
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```

```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Suspending({ this, &MainPage::App_Suspending });
}
```

```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending +=
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## <a name="save-application-data-before-suspension"></a>暫停之前，先儲存應用程式資料

當您的 app 處理 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件時，它有機會在處理常式函式中儲存自己的重要應用程式資料。 App 應該使用 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 儲存 API 來同步儲存簡單的應用程式資料。

```csharp
partial class MainPage
{
    async void App_Suspending(
        Object sender,
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object,
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated.
    End Sub

End Class
```

```cppwinrt
void MainPage::App_Suspending(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::ApplicationModel::SuspendingEventArgs const& /* e */)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

## <a name="release-resources"></a>釋出資源

您應該釋出獨佔資源及檔案控制代碼，以便讓其他 App 可以在您的 App 暫停時存取這些資源。 獨占資源的範例包括相機、I/O 裝置、外部裝置及網路資源。 明確釋放獨占資源及檔案控制代碼，有助於確保當您的 app 暫停時，其他 app 仍然可以使用它們。 當應用程式繼續執行時，應要重新取得獨占資源和檔案控制代碼。

## <a name="remarks"></a>備註

當使用者切換至另一個 app、桌面或 [開始] 畫面時，系統會暫停您的 app。 當使用者切換回您的 app 時，系統就會繼續執行 app。 當系統繼續執行您的 app 時，您的變數和資料結構內容和系統暫停 app 之前一樣，沒有變化。 系統會將 app 回復成暫停之前的相同狀態，如此使用者會以為 app 一直在背景中執行。

當 app 暫停時，系統會嘗試讓 app 及其資料保留在記憶體中。 不過，如果系統沒有資源可將 app 保存在記憶體中，系統將終止您的 app。 當使用者切換回已被終止的暫停 app 時，系統會傳送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，且必須在它的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中復原應用程式資料。

系統不會在終止 App 時對其發出通知，因此 App 必須在被暫停時，儲存其應用程式資料並釋出獨佔資源及檔案控制代碼，然後在其終止狀態結束後重新啟用時還原這些項目。

如果您在處理常式內進行非同步呼叫，控制權會立即從該非同步呼叫交回。 這表示執行可隨後從事件處理常式傳回，即使非同步呼叫尚未完成，app 也會移至下一個狀態。 針對會傳送給事件處理常式的 [**EnteredBackgroundEventArgs**](http://aka.ms/Ag2yh4) 物件使用 [**GetDeferral**](http://aka.ms/Kt66iv) 方法以延遲暫停，一直到您針對傳回的 [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 物件呼叫 [**Complete**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx) 方法之後。

延遲不會提高在應用程式終止之前所需執行的程式碼數量。 其只會延遲終止，直到呼叫延遲的 *Complete* 方法或期限到期 (*視何者先發生*) 為止。 若要延伸暫止狀態使用[**ExtendedExecutionSession**中的時間](run-minimized-with-extended-execution.md)

> [!NOTE]
> 為了改善的系統回應性 Windows8.1，應用程式存取會變低優先順序的資源它們被暫停之後。 為了支援這個新的優先順序，會延長暫停作業逾時，在 Windows 上讓 app 與標準優先順序一樣擁有 5 秒逾時，或在 Windows Phone 上有介於 1 到 10 秒之間的逾時。 您無法延長或改變這個逾時長度。

**有關使用 Visual Studio 進行偵錯的注意事項：** Visual Studio 會防止 Windows 暫停已連接至偵錯工具的 app。 這是為了讓使用者在 app 執行時可以檢視 Visual Studio 偵錯 UI。 當您正在對某個 app 偵錯時，您可以使用 Visual Studio 傳送一個暫停事件給該 app。 確定 **\[偵錯位置\]** 工具列已經顯示，然後按一下 **\[暫停\]** 圖示。

## <a name="related-topics"></a>相關主題

* [App 週期](app-lifecycle.md)
* [處理 app 啟用](activate-an-app.md)
* [處理 app 繼續執行](resume-an-app.md)
* [啟動、暫停和繼續的 UX 指導方針](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [延伸執行](run-minimized-with-extended-execution.md)

 

 
