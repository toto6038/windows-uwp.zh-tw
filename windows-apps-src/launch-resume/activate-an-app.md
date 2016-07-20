---
author: TylerMSFT
title: "處理 app 啟用"
description: "了解如何透過覆寫 OnLaunched 方法來處理 app 啟用。"
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
translationtype: Human Translation
ms.sourcegitcommit: fb83213a4ce58285dae94da97fa20d397468bdc9
ms.openlocfilehash: f47a3b7fcb4bec4138e11a079c3d10e918c1eb95

---

# 處理 app 啟用


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

了解如何透過覆寫 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法來處理 app 啟用。

## 覆寫啟動處理常式

當 app 啟用後，不論任何理由，系統都會傳送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件。 如需啟用類型的清單，請參閱 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) 列舉。

[**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 類別定義了您可以覆寫來處理不同啟用類型的方法。 數種啟用類型有您可以覆寫的特定方法。 如需其他啟用類型，請覆寫 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 方法。

定義應用程式的類別。

```xml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AppName.App" >
```

覆寫 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法。 不論使用者何時啟動 app 都會呼叫這個方法。 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 參數包含 app 之前的狀態以及啟用引數。

**注意** 就「Windows Phone 市集」app 而言，每次使用者從 [開始] 磚或 app 清單啟動 app 時，都會呼叫此方法，即使 app 目前在記憶體中被暫停也是如此。 在 Windows 上，從 [開始] 磚或 app 清單啟動暫停的 app 並不會呼叫此方法。

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> namespace AppName
> {
>    public partial class App
>    {
>       async protected override void OnLaunched(LaunchActivatedEventArgs args)
>       {
>          EnsurePageCreatedAndActivate();
>       }
>
>       // Creates the MainPage if it isn't already created.  Also activates
>       // the window so it takes foreground and input focus.
>       private MainPage EnsurePageCreatedAndActivate()
>       {
>          if (Window.Current.Content == null)
>          {
>              Window.Current.Content = new MainPage();
>          }
>
>          Window.Current.Activate();
>          return Window.Current.Content as MainPage;
>       }
>    }
> }
> ```
> ```vb
> Class App
>    Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
>       Window.Current.Content = New MainPage()
>       Window.Current.Activate()
>    End Sub
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
> void App::OnLaunched(LaunchActivatedEventArgs^ args)
> {
>    EnsurePageCreatedAndActivate();
> }
>
> // Creates the MainPage if it isn't already created.  Also activates
> // the window so it takes foreground and input focus.
> void App::EnsurePageCreatedAndActivate()
> {
>     if (_mainPage == nullptr)
>     {
>         // Save the MainPage for use if we get activated later
>         _mainPage = ref new MainPage();
>     }
>     Window::Current->Content = _mainPage;
>     Window::Current->Activate();
> }
> ```

## 在 app 暫停然後終止時還原應用程式資料


當使用者切換到已終止的 app 時，系統會傳送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，並將 [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 設定成 **Launch**，以及將 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 設定成 **Terminated** 或 **ClosedByUser**。 App 必須載入它已儲存的應用程式資料，並且重新整理已顯示的內容。

> [!div class="tabbedCodeSnippets"]
> ```cs
> async protected override void OnLaunched(LaunchActivatedEventArgs args)
> {
>    if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
>        args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```
> ```vb
> Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
>    Dim restoreState As Boolean = False
>
>    Select Case args.PreviousExecutionState
>       Case ApplicationExecutionState.Terminated
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case ApplicationExecutionState.ClosedByUser
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case Else
>          ' TODO: Populate the UI with defaults
>    End Select
>
>    Window.Current.Content = New MainPage(restoreState)
>    Window.Current.Activate()
> End Sub
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
> {
>    if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
>        args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```

如果 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 的值是 **NotRunning**，app 將無法成功儲存它的應用程式資料，而且 app 必須從頭開始，如同它剛被初始啟動一般。

## 備註

> **注意**：就 Windows Phone 市集 App 而言，[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件的後面一律跟著 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)，即使在 App 目前已被暫停，而使用者從主要磚或 App 清單重新啟動 App 的情況下，也是如此。 如果目前的視窗中已有設定的內容，app 可以略過初始化程序。 您可以檢查 [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 屬性，以判斷 app 是從主要磚還是次要磚啟動，然後根據該資訊，決定您是要呈現全新的 app 體驗，還是繼續 app 體驗。

## 相關主題

* [處理 app 暫停](suspend-an-app.md)
* [處理 app 繼續執行](resume-an-app.md)
* [App 暫停和繼續執行的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [App 週期](app-lifecycle.md)

**參考資料**

* [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)

 

 



<!--HONumber=Jun16_HO5-->


