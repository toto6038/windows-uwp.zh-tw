---
author: mtoepke
title: 如何暫停 app (DirectX 和 C++)
description: 這個主題示範如何在系統暫停通用 Windows 平台 (UWP) DirectX 應用程式時，儲存重要的系統狀態與應用程式資料。
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 暫停, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 204d61430f59c820e9ef9ef36832cd1c24ee7f9c
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6200335"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>如何暫停 app (DirectX 和 C++)



這個主題示範如何在系統暫停通用 Windows 平台 (UWP) DirectX app 時，儲存重要的系統狀態與 app 資料。

## <a name="register-the-suspending-event-handler"></a>登錄暫停事件處理常式


首先，登錄來處理 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件，這個事件會在您的應用程式被使用者或系統動作移到暫停狀態時引發。

將這個程式碼新增到檢視提供者的 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法實作中：

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>在暫停前先儲存所有應用程式資料


當您的 app 處理 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件時，它有機會在處理常式函式中儲存自己的重要應用程式資料。 App 必須使用 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 儲存 API 來同步儲存簡單的應用程式資料。 如果您正在開發遊戲，請儲存所有重要的遊戲狀態資訊。 請別忘了暫停音訊處理！

現在，實作回呼。 在這個方法中儲存應用程式資料。

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}
```

這個回呼必須在 5 秒內完成。 在這個回呼期間，您必須呼叫 [**SuspendingOperation::GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 來要求延遲，這會開始倒數計時。 當您的應用程式完成儲存操作時，請呼叫 [**SuspendingDeferral::Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 來告訴系統現在已經可以將您的應用程式暫停。 如果您沒有要求延遲，或是 app 儲存資料所花的時間超過 5 秒，系統就會自動將您的 app 暫停。

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

## <a name="call-trim"></a>呼叫 Trim()


從 Windows8.1 開始，所有 DirectX UWP 應用程式必須都呼叫[**idxgidevice3:: Trim**](https://msdn.microsoft.com/library/windows/desktop/dn280346)時暫停。 這個呼叫會通知圖形驅動程式釋放為應用程式配置的所有暫存緩衝區，當應用程式處於暫停狀態時，這樣做可減少為回收記憶體資源而導致應用程式被終止的機會。 這是 Windows8.1 的認證需求。

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}

// Call this method when the app suspends. It provides a hint to the driver that the app 
// is entering an idle state and that temporary buffers can be reclaimed for use by other apps.
void DX::DeviceResources::Trim()
{
    ComPtr<IDXGIDevice3> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    dxgiDevice->Trim();
}
```

## <a name="release-any-exclusive-resources-and-file-handles"></a>釋放所有獨占資源及檔案控制代碼


當您的應用程式處理 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件時，也可以釋放獨占資源及檔案控制代碼。 明確釋放獨占資源及檔案控制代碼，有助於確保當您的 app 不使用這些資源時，其他 app 仍然可以使用它們。 如果在終止後重新啟用應用程式，則應該會開啟獨占資源及檔案控制代碼。

## <a name="remarks"></a>備註


當使用者切換至另一個應用程式或桌面時，系統會暫停您的應用程式。 當使用者切換回您的 app 時，系統就會繼續執行 app。 當系統繼續執行您的 app 時，您的變數和資料結構內容和系統暫停 app 之前一樣，沒有變化。 系統會將 app 回復成暫停之前的相同狀態，如此使用者會以為 app 一直在背景中執行。

當 app 暫停時，系統會嘗試讓 app 及其資料保留在記憶體中。 不過，如果系統沒有資源可將 app 保存在記憶體中，系統將終止您的 app。 當使用者切換回已被終止的暫停 app 時，系統會傳送一個 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，並且應該會在它的 **CoreApplicationView::Activated** 事件處理常式中還原其應用程式資料。

系統不會在 app 終止時提供通知，所以 app 必須在暫停時儲存應用程式資料並釋放獨占資源及檔案控制代碼，並在終止狀態結束後重新啟用時還原這些項目。

## <a name="related-topics"></a>相關主題

* [如何繼續 app (DirectX 和 C++)](how-to-resume-an-app-directx-and-cpp.md)
* [如何啟用 app (DirectX 和 C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




