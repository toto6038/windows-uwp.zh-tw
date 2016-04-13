---
如何啟用 app (DirectX 和 C++)
這個主題示範如何定義通用 Windows 平台 (UWP) DirectX app 的啟用經驗。
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
---

# 如何啟用 app (DirectX 和 C++)


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

這個主題示範如何定義通用 Windows 平台 (UWP) DirectX app 的啟用經驗。

## 登錄應用程式啟用事件處理常式


首先，登錄以處理 [**CoreApplicationView::Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，這個事件會在作業系統啟動並初始化您的 app 時引發。

將這個程式碼新增到檢視提供者 (在這個範例中為 **MyViewProvider**) 的 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法實作中：

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## 啟用 app 的 CoreWindow 執行個體


當您的 app 啟動時，您必須取得 app 的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 參照。 **CoreWindow** 包含您的 app 用來處理視窗事件的視窗事件訊息發送器。 請呼叫 [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) 以在您 app 啟用事件的回呼中取得這項參照。 一旦取得這項參照之後，呼叫 [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254) 啟用主 app 視窗。

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## 開始處理主 app 視窗的事件訊息


您的回呼會以 app [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 的 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 所處理之事件訊息的形式發生。 如果您未從 app 的主迴圈 (實作於檢視提供者的 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法中) 呼叫 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)，就不會叫用這個回呼。

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## 相關主題


* [如何暫停 app (DirectX 和 C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [如何繼續 app (DirectX 和 C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 






<!--HONumber=Mar16_HO1-->


