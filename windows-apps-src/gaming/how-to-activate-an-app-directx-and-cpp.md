---
title: 如何啟用應用程式 (DirectX 和 C++)
description: 這個主題示範如何定義通用 Windows 平台 (UWP) DirectX app 的啟用經驗。
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX, 啟用
ms.localizationpriority: medium
ms.openlocfilehash: 4aeba58af61cffa33626c64cebcbade272af109b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368607"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>如何啟用應用程式 (DirectX 和 C++)



這個主題示範如何定義通用 Windows 平台 (UWP) DirectX app 的啟用經驗。

## <a name="register-the-app-activation-event-handler"></a>登錄應用程式啟用事件處理常式


首先，登錄以處理 [**CoreApplicationView::Activated**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 事件，這個事件會在作業系統啟動並初始化您的 app 時引發。

將這個程式碼新增到檢視提供者 (在這個範例中為 **MyViewProvider**) 的 [**IFrameworkView::Initialize**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) 方法實作中：

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

## <a name="activate-the-corewindow-instance-for-the-app"></a>啟用 app 的 CoreWindow 執行個體


當您的 app 啟動時，您必須取得 app 的 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 參照。 **CoreWindow** 包含您的 app 用來處理視窗事件的視窗事件訊息發送器。 請呼叫 [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 以在您 app 啟用事件的回呼中取得這項參照。 一旦取得這項參照之後，呼叫 [**CoreWindow::Activate**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.activate) 啟用主 app 視窗。

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>開始處理主 app 視窗的事件訊息


您的回呼會以 app [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 的 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 所處理之事件訊息的形式發生。 如果您未從 app 的主迴圈 (實作於檢視提供者的 [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) 方法中) 呼叫 [**CoreDispatcher::ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents)，就不會叫用這個回呼。

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

## <a name="related-topics"></a>相關主題


* [如何暫停應用程式 (DirectX 和C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [如何繼續應用程式 (DirectX 和C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 




