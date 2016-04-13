---
title: 處理 app 暫停
description: 了解如何在系統暫停您的 app 時，儲存重要的應用程式資料。
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
---

# 處理 app 暫停


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**暫停**](https://msdn.microsoft.com/library/windows/apps/br242341)

了解如何在系統暫停您的 app 時，儲存重要的應用程式資料。 這個範例會為 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件登錄一個事件處理常式，而且會將一個字串儲存至檔案。

## 登錄暫停事件處理常式


登錄以處理 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件，它會指示 app 必須在系統將其暫停之前，儲存自己的應用程式資料。

> [!div class="tabbedCodeSnippets"]
```cs
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
       ref new SuspendingEventHandler(this, &amp;MainPage::App_Suspending);
}
```

## 暫停之前，先儲存應用程式資料


當您的 app 處理 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件時，它有機會在處理常式函式中儲存自己的重要應用程式資料。 App 必須使用 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 儲存 API 來同步儲存簡單的應用程式資料。

> [!div class="tabbedCodeSnippets"]
```cs
partial class MainPage
{
    async void App_Suspending(
        Object sender, 
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated
    }
}
```
```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object, 
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated
    End Sub

End Class
```
```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated
}
```

## 備註


當使用者切換至另一個 app、桌面或 [開始] 畫面時，系統會暫停您的 app。 當使用者切換回您的 app 時，系統就會繼續執行 app。 當系統繼續執行您的 app 時，您的變數和資料結構內容和系統暫停 app 之前一樣，沒有變化。 系統會將 app 回復成暫停之前的相同狀態，如此使用者會以為 app 一直在背景中執行。

當 app 暫停時，系統會嘗試讓 app 及其資料保留在記憶體中。 不過，如果系統沒有資源可將 app 保存在記憶體中，系統將終止您的 app。 當使用者切換回已被終止的暫停 app 時，系統會傳送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，且必須在它的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中復原應用程式資料。

系統不會在 app 終止時提供通知，所以 app 必須在暫停時儲存應用程式資料並釋放獨占資源及檔案控制代碼，並在終止狀態結束後重新啟用時還原這些項目。

> **注意** 如果您需要在應用程式暫停時進行非同步工作，就必須延遲完成暫停，直到工作完成為止。 您可以將 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 方法用於 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 物件 (透過事件引數提供) 來延遲完成暫停，直到您在傳回的 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 物件上呼叫 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法為止。

> **注意** 為了改善 Windows 8.1 的系統回應性，當 app 暫停之後，存取資源的優先順序會變低。 為了支援這個新的優先順序，會延長暫停作業逾時，在 Windows 上讓 app 與標準優先順序一樣擁有 5 秒逾時，或在 Windows Phone 上有介於 1 到 10 秒之間的逾時。 您無法延長或改變這個逾時長度。

> **有關使用 Visual Studio 進行偵錯的注意事項：**Visual Studio 會防止 Windows 暫停已連接至偵錯工具的 app。 這是為了讓使用者在 app 執行時可以檢視 Visual Studio 偵錯 UI。 當您正在對某個 app 偵錯時，您可以使用 Visual Studio 傳送一個暫停事件給該 app。 確定 [偵錯位置]**** 工具列已經顯示，然後按一下 [暫停]**** 圖示。

## 相關主題


* [處理 app 啟用](activate-an-app.md)
* [處理 app 繼續執行](resume-an-app.md)
* [啟動、暫停和繼續的 UX 指導方針](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [App 週期](app-lifecycle.md)

 

 





<!--HONumber=Mar16_HO1-->


