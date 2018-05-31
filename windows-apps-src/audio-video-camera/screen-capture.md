---
author: eliotcowley
title: 螢幕擷取
description: Windows.Graphics.Capture 命名空間提供 API，從顯示畫面或應用程式視窗取得畫面格來建立要建置共同作業及互動體驗的視訊串流或快照。
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 3/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 螢幕擷取
ms.localizationpriority: medium
ms.openlocfilehash: 2b7883acd351c721b4539141cd46e3c199a8d8a1
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2018
ms.locfileid: "1639679"
---
# <a name="screen-capture"></a>螢幕擷取

從 Windows 10 版本 1803 起，[Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) 命名空間提供 API，從顯示畫面或應用程式視窗取得畫面格來建立要建置共同作業及互動體驗的視訊串流或快照。

使用螢幕擷取，開發人員可以為終端使用者叫用安全的系統 UI，選取要擷取的顯示或應用程式視窗，系統會在主動擷取項目周圍繪製黃色通知邊框。 在多個同時擷取工作階段中，會在每個擷取項目周圍繪製黃色邊框。

## <a name="add-the-screen-capture-capability"></a>新增螢幕擷取功能

位於 **Windows.Graphics.Capture** 命名空間的 API 需要在應用程式資訊清單中宣告一般的功能。 您必須將此項目直接新增到檔案中：
    
1. 在 **\[方案總管\]** 的 **\[Package.appxmanifest\]** 中按一下滑鼠右鍵。 
2. 選取 **\[開啟方式\]**。 
3. 選擇 **\[XML (文字) 編輯器\]**。 
4. 選取 **\[確定\]**。
5. 在 **\[封裝\]** 節點中，新增下列屬性：`xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"`
6. 另外也在 **\[封裝\]** 節點中，新增下列命令至 **IgnorableNamespaces** 屬性：`uap6`
7. 在 **\[功能\]** 節點中，新增下列元素：`<uap6:Capability Name="graphicsCapture"/>`

## <a name="launch-the-system-ui-to-start-screen-capture"></a>啟動系統 UI 以開始螢幕擷取

啟動系統 UI 之前，您可以查看您的應用程式目前是否可以進行螢幕擷取。 您的應用程式目前是否可以進行螢幕擷取可能有數個原因，包括裝置不符合硬體需求，或擷取目標的應用程式封鎖螢幕擷取。 使用 [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) 類別中的 **IsSupported** 方法來判斷是否支援 UWP 螢幕擷取︰

```cs
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported()) 
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed; 
    }    
}
```

確認支援螢幕擷取後，使用 [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) 類別來叫用系統選擇器 UI。 終端使用者使用這個 UI 來選擇要進行螢幕擷取的顯示器或應用程式視窗。 選擇器會傳回 [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem)，這將用來建立 **GraphicsCaptureSession**：

```cs
public async Task StartCaptureAsync() 
{ 
    // The GraphicsCapturePicker follows the same pattern the 
    // file pickers do. 
    var picker = new GraphicsCapturePicker(); 
    GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 

    // The item may be null if the user dismissed the 
    // control without making a selection or hit Cancel. 
    if (item != null) 
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item); 
    } 
}
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>建立擷取畫面集區與擷取工作階段

使用 **GraphicsCaptureItem**，您將會使用 D3D 裝置、支援的像素格式 (**DXGI\_FORMAT\_B8G8R8A8\_UNORM**)、所需畫面數 (可以是任何整數) 以及畫面大小，來建立 [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool)。 **GraphicsCaptureItem** 類別的 **ContentSize** 屬性可以當做畫面的大小：

```cs
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item) 
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create( 
        _canvasDevice, // D3D device 
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
        2, // Number of frames 
        _item.Size); // Size of the buffers   
} 
```

接下來，透過傳遞 **GraphicsCaptureItem** 到 **CreateCaptureSession** 方法，取得您 **Direct3D11CaptureFramePool** 之 **GraphicsCaptureSession** 類別的執行個體：

```cs
_session = _framePool.CreateCaptureSession(_item);
```

使用者在系統 UI 中明確地同意擷取應用程式視窗或顯示器後，**GraphicsCaptureItem** 可以關聯至多個 **CaptureSession** 物件。 如此一來，您的應用程式可以選擇擷取相同項目以用於各種體驗。

若要同時擷取多個項目，您的應用程式必須為每個要擷取項目建立擷取工作階段擷取，這需要為每個要擷取項目叫用選擇器 UI。

## <a name="acquire-capture-frames"></a>取得擷取畫面

使用所建立的畫面集區和擷取工作階段，在您的 **GraphicsCaptureSession** 執行個體上呼叫 **StartCapture** 方法，通知系統開始傳送擷取畫面到您的應用程式：

```cs
_session.StartCapture();
```

若要取得這些擷取畫面 (這是 [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe) 物件)，您可以使用 **Direct3D11CaptureFramePool.FrameArrived** 事件：

```cs
_framePool.FrameArrived += (s, a) => 
{ 
    // The FrameArrived event fires for every frame on the thread that         
    // created the Direct3D11CaptureFramePool. This means we don't have to 
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame()) 
    {
        // We'll define this method later in the document.
        ProcessFrame(frame); 
    }  
}; 
```

建議您盡量避免對 **FrameArrived** 使用 UI 執行緒，因為每次有可用的新畫面時就會引發這個事件，這將會很頻繁。 如果您選擇在 UI 執行緒上接聽 **FrameArrived**，請留意每次引發事件時您需執行多少工作。

或者，您也可以使用 **Direct3D11CaptureFramePool.TryGetNextFrame** 方法手動提取畫面，直到您取得所有所需畫面。

**Direct3D11CaptureFrame** 物件包含 **ContentSize**、**Surface** 和 **SystemRelativeTime** 的屬性。 **SystemRelativeTime** 是 QPC ([QueryPerformanceCounter](https://msdn.microsoft.com/library/windows/desktop/ms644904)) 時間，可用於同步處理其他媒體元素。

## <a name="processing-capture-frames"></a>處理擷取畫面

呼叫 **TryGetNextFrame** 時會從 **Direct3D11CaptureFramePool** 簽出每個畫面，並根據 **Direct3D11CaptureFrame** 物件的存留期將其簽入。 對於原生應用程式，釋放 **Direct3D11CaptureFrame** 物件就足以將畫面簽入回畫面集區。 對於受管理應用程式，建議使用 **Direct3D11CaptureFrame.Dispose** (C++ 中的 **Close**) 方法。 **Direct3D11CaptureFrame** 會實作 [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable) 介面，這會為 C# 呼叫者投射為 [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)。

在畫面簽入之後，應用程式不應該儲存 **Direct3D11CaptureFrame** 物件的參考，也不應該儲存基礎 Direct3D 表面的參考。

在處理畫面時，建議應用程式在與 **Direct3D11CaptureFramePool** 物件相關聯的相同裝置上採用 [ID3D11Multithread](https://msdn.microsoft.com/library/windows/desktop/mt644886) 鎖定。

基礎 Direct3D 表面一定是在建立 (或重新建立) **Direct3D11CaptureFramePool** 時指定的大小。 如果內容大於畫面，內容會被裁剪到畫面的大小。 如果內容小於畫面，則畫面的其餘部分會包含未定義的資料。 建議應用程式使用 **ContentSize** 屬性為 **Direct3D11CaptureFrame** 複製子矩形，以避免顯示未定義的內容。

## <a name="react-to-capture-item-resizing-or-device-lost"></a>對於調整擷取項目大小或遺失裝置的反應

在擷取過程中，應用程式可能會想要變更其 **Direct3D11CaptureFramePool** 的層面。 這包括提供新的 Direct3D 裝置、變更框架緩衝區的大小，或甚至變更集區中緩衝區的數目。 在這些案例中，**Direct3D11CaptureFramePool** 物件上的 **Recreate** 方法是建議使用的工具。

呼叫 **Recreate** 時，會捨棄所有現有的畫面。 這是為了防止給出其基礎 Direct3D 表面屬於應用程式可能無法再存取的裝置。 基於這個原因，建議在呼叫 **Recreate** 前先處理完所有擱置中畫面。

## <a name="putting-it-all-together"></a>總結

下列程式碼片段是在 UWP 應用程式中實作擷取螢幕的的端對端範例：

```cs
using Microsoft.Graphics.Canvas; 
using System; 
using System.Threading.Tasks; 
using Windows.Graphics.Capture; 
using Windows.Graphics.DirectX; 
using Windows.UI.Composition; 
 
namespace CaptureSamples 
{ 
    class Sample
    {
        // Capture API objects.
        private Vector2 _lastSize; 
        private GraphicsCaptureItem _item; 
        private Direct3D11CaptureFramePool _framePool; 
        private GraphicsCaptureSession _session; 
 
        // Non-API related members.
        private CanvasDevice _canvasDevice; 
        private CompositionDrawingSurface _surface; 

        public async Task StartCaptureAsync() 
        { 
            // The GraphicsCapturePicker follows the same pattern the 
            // file pickers do. 
            var picker = new GraphicsCapturePicker(); 
            GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 
 
            // The item may be null if the user dismissed the 
            // control without making a selection or hit Cancel. 
            if (item != null) 
            { 
                StartCaptureInternal(item); 
            }
        } 
 
        private void StartCaptureInternal(GraphicsCaptureItem item) 
        { 
             // Stop the previous capture if we had one.
            StopCapture(); 
 
            _item = item; 
            _lastSize = _item.Size; 
 
             _framePool = Direct3D11CaptureFramePool.Create( 
                _canvasDevice, // D3D device 
                DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
                2, // Number of frames 
                _item.Size); // Size of the buffers 
 
            _framePool.FrameArrived += (s, a) => 
            { 
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we 
                // don't have to do a null-check here, as we know we're the only 
                // one dequeueing frames in our application.  
 
                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.
 
                using (var frame = _framePool.TryGetNextFrame()) 
                { 
                    ProcessFrame(frame); 
                }  
            }; 
 
            _item.Closed += (s, a) => 
            { 
                StopCapture(); 
            }; 
 
            _session = _framePool.CreateCaptureSession(_item); 
            _session.Start(); 
        } 
 
        public void StopCapture() 
        { 
            _session?.Dispose(); 
            _framePool?.Dispose(); 
            _item = null; 
            _session = null; 
            _framePool = null; 
        } 
 
        private void ProcessFrame(Direct3D11CaptureFrame frame) 
        { 
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids 
            // throwing in the catch block below (device creation could always 
            // fail) along with ensuring that resize completes successfully and 
            // isn’t vulnerable to device-lost.   
            bool needsReset = false; 
            bool recreateDevice = false; 
 
            if (frame.ContentSize != _lastSize) 
            { 
                needsReset = true; 
                _lastSize = frame.ContentSize; 
            } 
            
            try 
            { 
                // Take the D3D11 surface and draw it into a  
                // Composition surface.
 
                // Convert our D3D11 surface into a Win2D object.
                var canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface( 
                    _canvasDevice, 
                    frame.Surface); 
 
                // Helper that handles the drawing for us, not shown. 
                FillSurfaceWithBitmap(_surface, canvasBitmap); 
            } 
            // This is the device-lost convention for Win2D.
            catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
            { 
                // We lost our graphics device. Recreate it and reset 
                // our Direct3D11CaptureFramePool.  
                needsReset = true; 
                recreateDevice = true; 
            } 
 
            if (needsReset) 
            { 
                ResetFramePool(frame.ContentSize, recreateDevice); 
            }
        } 
 
        private void ResetFramePool(Vector2 size, bool recreateDevice) 
        { 
            do 
            { 
                try 
                { 
                    if (recreateDevice) 
                    { 
                        _canvasDevice = new CanvasDevice(); 
                    } 
 
                    _framePool.Recreate( 
                        _canvasDevice,  
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,  
                        2, 
                        size); 
                } 
                // This is the device-lost convention for Win2D.
                catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
                { 
                    _canvasDevice = null; 
                    recreateDevice = true; 
                } 
            } while (_canvasDevice == null); 
        } 
    } 
} 
```

## <a name="see-also"></a>請參閱

* [Windows.Graphics.Capture 命名空間](https://docs.microsoft.com/uwp/api/windows.graphics.capture)