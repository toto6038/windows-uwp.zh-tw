---
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
組合原生 DirectX 和 Direct2D 與 BeginDraw 和 EndDraw 的交互操作
Windows.UI.Composition API 提供可將內容直接移到撰寫器中的原生交互操作介面。
---
# 組合原生 DirectX 和 Direct2D 與 BeginDraw 和 EndDraw 的交互操作

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows.UI.Composition API 提供可將內容直接移到撰寫器中的 [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068)、[**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058) 及 [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065) 原生交互操作介面。

原生交互操作是以 DirectX 紋理所支撐的表面物件為中心建構的。 這些表面建立自名為 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 的處理站物件。 這個物件是以基礎 Direct2D 或 Direct3D 裝置物件為後盾，它使用這些物件來配置表面的視訊記憶體。 組合 API 一律不會建立基礎 DirectX 裝置。 應用程式必須負責建立一個裝置，然後將它傳送給 **CompositionGraphicsDevice** 物件。 應用程式可以一次建立多個 **CompositionGraphicsDevice** 物件，並且可以使用相同的 DirectX 裝置做為多個 **CompositionGraphicsDevice** 物件的轉譯裝置。

## 建立表面

每個 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 都是做為一個表面處理站。 每個表面建立時都會有初始大小 (可能是 0,0)，但沒有有效像素。 處於初始狀態的表面可能會立即用於視覺化樹狀結構中 (例如透過 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 和 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433))，但是處於初始狀態的表面對螢幕輸出並沒有任何作用。 就各方面來說，它是完全透明的，即使指定的 Alpha 模式為「不透明」亦是如此。

有時 DirectX 裝置可能會轉譯成無法使用。 除其他原因之外，如果應用程式傳遞無效的引數給特定的 DirectX API、圖形卡被系統重設，或驅動程式已更新，也可能發生這種情況。 Direct3D 有一個 API 可供應用程式在裝置因任何原因遺失時，以非同步方式進行探索。 當 DirectX 裝置遺失時，應用程式必須將其捨棄、建立一個新裝置，然後將該裝置傳送給先前與錯誤 DirectX 裝置關聯的任何 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 物件。

## 將像素載入表面

為了將像素載入表面，應用程式必須呼叫 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) 方法， 視應用程式所要求的內容而定，這會傳回呈現紋理或 Direct2D 內容的 DirectX 介面。 應用程式必須接著將像素轉譯或上傳到該紋理。 當應用程式完成工作時，它必須呼叫 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 方法。 只有到這個時候，新像素才可供組合，但是它們仍然不會顯示在螢幕上，而是必須等到下一次認可對視覺化樹狀結構所做的一切變更時，才會顯示在螢幕上。 如果在呼叫 **EndDraw** 之前就已認可視覺化樹狀結構，則螢幕上將不會顯示進行中的更新，而表面也會繼續顯示它在 **BeginDraw** 之前擁有的內容。 呼叫 **EndDraw** 時，BeginDraw 所傳回的紋理或 Direct2D 內容指標會失效。 應用程式一律不應該在 **EndDraw** 呼叫之後快取該指標。

應用程式針對任何指定的 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)，一次只能在一個表面上呼叫 BeginDraw。 呼叫 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) 之後，應用程式必須在該表面呼叫 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060)，然後才能再於另一個表面呼叫 **BeginDraw**。 由於 API 相當靈活，因此如果應用程式想要從多個背景工作緒執行轉譯，就必須負責同步處理這些呼叫。 如果應用程式想要中斷一個表面的轉譯，並暫時切換到另一個表面，則應用程式可以使用 [**SuspendDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620064.aspx) 方法。 這可讓另一個 **BeginDraw** 順利完成，但不會讓第一個表面更新可供在螢幕上進行組合。 這可讓應用程式以交易方式執行多個更新。 暫停表面之後，應用程式可以呼叫 [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 方法來繼續進行該更新，或是也可以呼叫 **EndDraw** 來宣告更新已完成。 這表示針對任何指定的 **CompositionGraphicsDevice**，一次只能有一個正在更新的表面。 每個圖形裝置都獨立保有此狀態，因此如果兩個表面分屬不同的圖形裝置，應用程式便可同時轉譯到兩個表面。 不過，這會導致無法將這兩個表面的視訊記憶體放在一個集區中，因此較不具記憶體效益。

如果應用程式執行的操作不正確 (例如傳遞無效的引數，或先在一個表面上呼叫 **BeginDraw**， 然後才在另一個表面上呼叫 **EndDraw**)，[**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx)、[**SuspendDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620064.aspx)、[**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 及 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 方法就會傳回失敗。 這些類型的失敗代表應用程式錯誤，因此預期的處理方式是以立即失敗來處理它們。 當基礎 DirectX 裝置遺失時，**BeginDraw** 也可能會傳回失敗。 這個失敗並不嚴重，因為應用程式可以重新建立其 DirectX 裝置並再試一次。 因此，應用程式應該以直接跳過轉譯的方式來處理裝置遺失。 如果 **BeginDraw** 因任何原因發生失敗，則應用程式也不應該呼叫 **EndDraw**，因為首先 begin 就永遠不會成功。

## 捲動

基於效能考量，當應用程式呼叫 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) 時，所傳回的紋理內容並不保證是先前的表面內容。 應用程式必須假設內容是隨機的，因此，應用程式必須藉由在轉譯之前先清除表面，或藉由繪製足夠的不透明內容來涵蓋整個更新的矩形，以確保觸及所有像素。 藉由這樣的做法，再結合紋理指標只有在 **BeginDraw** 與 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 呼叫之間才有效的事實，應用程式便無法從表面複製先前的內容。 基於這個理由，我們提供了 [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063) 方法，可讓應用程式執行相同表面像素複製。

## 用法範例

下列範例說明非常一個簡單的案例，其中應用程式會建立繪圖表面，並使用 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) 和 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 在表面填入文字。 應用程式會使用 DirectWrite 來配置文字 (不顯示詳細資料)，然後使用 Direct2D 來轉譯它。 組合圖形裝置會在初始化階段直接接受 Direct2D 裝置。 這可讓 **BeginDraw** 傳回 ID2D1DeviceContext 介面指標，與讓應用程式在每次進行繪圖操作時建立 Direct2D 內容來包裝傳回的 ID3D11Texture2D 介面相比，效率高出許多。

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```

 

 






<!--HONumber=Mar16_HO1-->


