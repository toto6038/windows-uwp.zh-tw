---
title: 如何繼續應用程式 (DirectX 和 C++)
description: 這個主題示範如何在系統恢復通用 Windows 平台 (UWP) DirectX 應用程式時，還原重要的應用程式資料。
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 繼續, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: f0aa60061ae9fc14392bfe4beb0693ba50fda0df
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740536"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>如何繼續應用程式 (DirectX 和 C++)



這個主題示範如何在系統恢復通用 Windows 平台 (UWP) DirectX app 時，還原重要的應用程式資料。

## <a name="register-the-resuming-event-handler"></a>登錄繼續事件處理常式


登錄以處理 [**CoreApplication::Resuming**](https://msdn.microsoft.com/library/windows/apps/br205859) 事件，它指示使用者跳出然後又返回您的應用程式。

將這個程式碼新增到檢視提供者的 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法實作中：

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## <a name="refresh-displayed-content-after-suspension"></a>暫停之後重新整理顯示的內容


當您的應用程式處理繼續事件時，就會有機會重新整理它自己的已顯示內容。 請還原任何您已經使用 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 的處理常式儲存的應用程式，然後重新啟動處理。 遊戲裝置：如果您已經暫停音訊引擎，現在就是重新啟動它的時候。

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

這個回呼會以 app [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 的 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 所處理之事件訊息的形式發生。 如果您未從 app 的主迴圈 (實作於檢視提供者的 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法中) 呼叫 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)，就不會叫用這個回呼。

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

## <a name="remarks"></a>備註


當使用者切換至另一個應用程式或桌面時，系統會暫停您的應用程式。 當使用者切換回您的 app 時，系統就會繼續執行 app。 當系統繼續執行您的 app 時，您的變數和資料結構內容和系統暫停 app 之前一樣，沒有變化。 系統會將 app 回復成暫停之前的相同狀態，如此使用者會以為 app 一直在背景中執行。 不過，應用程式可能已經暫停一段相當長的時間，所以它應該重新整理在應用程式暫停期間可能已經變更的任何顯示內容，並且重新啟動任何轉譯或音訊處理執行緒。 如果您在先前的暫停事件期間儲存了任何遊戲狀態資料，請現在還原它。

## <a name="related-topics"></a>相關主題

* [如何暫停 app (DirectX 和 C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [如何啟用 app (DirectX 和 C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




