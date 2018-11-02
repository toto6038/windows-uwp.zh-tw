---
author: eliotcowley
title: 螢幕擷取
description: Windows.Graphics.Capture 命名空間提供 API，從顯示畫面或應用程式視窗取得畫面格來建立要建置共同作業及互動體驗的視訊串流或快照。
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 螢幕擷取
ms.localizationpriority: medium
ms.openlocfilehash: d28ed1fce79a815155180ab8a3c708e2c8bf8916
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5970921"
---
# <a name="screen-capture"></a>螢幕擷取

從 Windows 10 版本 1803 起，[Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) 命名空間提供 API，從顯示畫面或應用程式視窗取得畫面格來建立要建置共同作業及互動體驗的視訊串流或快照。

使用螢幕擷取，開發人員可以為終端使用者叫用安全的系統 UI，選取要擷取的顯示或應用程式視窗，系統會在主動擷取項目周圍繪製黃色通知邊框。 在多個同時擷取工作階段中，會在每個擷取項目周圍繪製黃色邊框。

> [!NOTE]
> 在傳統型應用程式和 Windows Mixed Reality 沉浸式頭戴式裝置上才支援螢幕擷取 Api。

## <a name="add-the-screen-capture-capability"></a>新增螢幕擷取功能

**Windows.Graphics.Capture**命名空間中找到的 Api 需要您的應用程式資訊清單中宣告一般的功能：
    
1. 在**方案總管] 中**開啟**Package.appxmanifest** 。
2. 選取 **\[功能\]** 索引標籤。
3. 檢查**圖形擷取**。

![圖形擷取](images/screen-capture-1.png)

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

這是 UI 程式碼，所以它必須在 UI 執行緒上呼叫。 如果您正在從程式碼後置中呼叫它，您的應用程式 （例如**MainPage.xaml.cs**) 的頁面這是為您自動完成，但如果不是，您可以強迫在下列程式碼在 UI 執行緒上執行：

```cs
CoreWindow window = CoreApplication.MainView.CoreWindow;
           
await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
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

下列程式碼片段是如何在 UWP 應用程式中實作擷取螢幕的端對端範例。 在此範例中，我們有在前端按鈕，按下，會呼叫**Button_ClickAsync**方法。

> [!NOTE]
> 此程式碼片段會使用[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)，2D 圖形轉譯的程式庫。 如需有關如何將它設定您的專案進行資訊其文件，請參閱。

```cs
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace WindowsGraphicsCapture
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Capture API objects.
        private SizeInt32 _lastSize;
        private GraphicsCaptureItem _item;
        private Direct3D11CaptureFramePool _framePool;
        private GraphicsCaptureSession _session;

        // Non-API related members.
        private CanvasDevice _canvasDevice;
        private CompositionGraphicsDevice _compositionGraphicsDevice;
        private Compositor _compositor;
        private CompositionDrawingSurface _surface;

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();
            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice);
            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with the composition APIs.

            var visual = _compositor.CreateSpriteVisual();
            visual.RelativeSizeAdjustment = Vector2.One;
            var brush = _compositor.CreateSurfaceBrush(_surface);
            brush.HorizontalAlignmentRatio = 0.5f;
            brush.VerticalAlignmentRatio = 0.5f;
            brush.Stretch = CompositionStretch.Uniform;
            visual.Brush = brush;
            ElementCompositionPreview.SetElementChildVisual(this, visual);
        }

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
            _session.StartCapture();
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

            if ((frame.ContentSize.Width != _lastSize.Width) ||
                (frame.ContentSize.Height != _lastSize.Height))
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

                // Helper that handles the drawing for us.
                FillSurfaceWithBitmap(canvasBitmap);
            }

            // This is the device-lost convention for Win2D.
            catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
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

        private void FillSurfaceWithBitmap(CanvasBitmap canvasBitmap)
        {
            CanvasComposition.Resize(_surface, canvasBitmap.Size);

            using (var session = CanvasComposition.CreateDrawingSession(_surface))
            {
                session.Clear(Colors.Transparent);
                session.DrawImage(canvasBitmap);
            }
        }

        private void ResetFramePool(SizeInt32 size, bool recreateDevice)
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
                catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
                {
                    _canvasDevice = null;
                    recreateDevice = true;
                }
            } while (_canvasDevice == null);
        }

        private async void Button_ClickAsync(object sender, RoutedEventArgs e)
        {
            await StartCaptureAsync();
        }
    }
}
```

## <a name="see-also"></a>請參閱

* [Windows.Graphics.Capture 命名空間](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
