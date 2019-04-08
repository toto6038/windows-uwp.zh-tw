---
title: 處理應用程式暫停
description: 了解如何在系統暫停您的應用程式時，儲存重要的應用程式資料。
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: e440812861cf853810f9fee597c807b439dda426
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599043"
---
# <a name="handle-app-suspend"></a>處理應用程式暫停

**重要的 Api**

- [**暫止**](https://msdn.microsoft.com/library/windows/apps/br242341)

了解如何在系統暫停您的應用程式時，儲存重要的應用程式資料。 這個範例會為 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件登錄一個事件處理常式，並將一個字串儲存至檔案。

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

當您的 app 處理 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件時，它有機會在處理常式函式中儲存自己的重要應用程式資料。 App 必須使用 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 儲存 API 來同步儲存簡單的應用程式資料。

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

## <a name="release-resources"></a>釋放資源

您應該釋放獨占資源及檔案控制代碼，這樣當您的 app 暫停時，其他 app 仍然可以使用它們。 獨占資源的範例包括相機、I/O 裝置、外部裝置及網路資源。 明確釋放獨占資源及檔案控制代碼，有助於確保當您的 app 暫停時，其他 app 仍然可以使用它們。 當應用程式繼續執行時，應要重新取得獨占資源和檔案控制代碼。

## <a name="remarks"></a>備註

當使用者切換至另一個 app、桌面或 [開始] 畫面時，系統會暫停您的 app。 當使用者切換回您的 app 時，系統就會繼續執行 app。 當系統繼續執行您的 app 時，您的變數和資料結構內容和系統暫停 app 之前一樣，沒有變化。 系統會將 app 回復成暫停之前的相同狀態，如此使用者會以為 app 一直在背景中執行。

當 app 暫停時，系統會嘗試讓 app 及其資料保留在記憶體中。 不過，如果系統沒有資源可將 app 保存在記憶體中，系統將終止您的 app。 當使用者切換回已被終止的暫停 app 時，系統會傳送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，且必須在它的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中復原應用程式資料。

系統不會在 app 終止時提供通知，所以 app 必須在暫停時儲存應用程式資料並釋放獨占資源及檔案控制代碼，並在終止狀態結束後重新啟用時還原這些項目。

如果您在處理常式內進行非同步呼叫，控制權會立即從該非同步呼叫交回。 這表示執行可隨後從事件處理常式傳回，即使非同步呼叫尚未完成，app 也會移至下一個狀態。 針對會傳送給事件處理常式的 [**EnteredBackgroundEventArgs**](https://aka.ms/Ag2yh4) 物件使用 [**GetDeferral**](https://aka.ms/Kt66iv) 方法以延遲暫停，一直到您針對傳回的 [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 物件呼叫 [**Complete**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx) 方法之後。

延遲不會提高在應用程式終止之前所需執行的程式碼數量。 其只會延遲終止，直到呼叫延遲的 *Complete* 方法或期限到期 (*視何者先發生*) 為止。 若要延長暫停狀態的時間，請使用 [**ExtendedExecutionSession**](run-minimized-with-extended-execution.md)

> [!NOTE]
> 若要改善系統回應能力，在 Windows 8.1 中的，應用程式會提供低優先順序服務存取權給資源之後他們就會暫停。 為了支援這個新的優先順序，會延長暫停作業逾時，在 Windows 上讓 app 與標準優先順序一樣擁有 5 秒逾時，或在 Windows Phone 上有介於 1 到 10 秒之間的逾時。 您無法延長或改變這個逾時長度。

**關於使用 Visual Studio 偵錯注意事項：** Visual Studio 可防止 Windows 暫止附加至偵錯工具的應用程式。 這是為了讓使用者在 app 執行時可以檢視 Visual Studio 偵錯 UI。 當您正在對某個 app 偵錯時，您可以使用 Visual Studio 傳送一個暫停事件給該 app。 確定 **\[偵錯位置\]** 工具列已經顯示，然後按一下 **\[暫停\]** 圖示。

## <a name="related-topics"></a>相關主題

* [應用程式生命週期](app-lifecycle.md)
* [處理應用程式啟用](activate-an-app.md)
* [處理應用程式繼續執行](resume-an-app.md)
* [UX 指導方針，以便啟動，暫止和繼續](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [延伸的執行](run-minimized-with-extended-execution.md)

 

 
