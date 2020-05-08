---
title: 將 DirectX 9 功能對應到 DirectX 11 API
description: 了解 Direct3D 9 遊戲使用的功能如何轉譯到 Direct3D 11 與通用 Windows 平台 (UWP)。
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX 9, DirectX 11, 移植
ms.localizationpriority: medium
ms.openlocfilehash: e0643773ca529540284f0749de6e91349f4e9c47
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730237"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>將 DirectX 9 功能對應到 DirectX 11 API



**摘要**

-   [計劃 DirectX 移植](plan-your-directx-port.md)
-   [從 Direct3D 9 到 Direct3D 11 的重要變更](understand-direct3d-11-1-concepts.md)
-   功能對應


了解 Direct3D 9 遊戲使用的功能如何轉譯到 Direct3D 11 與通用 Windows 平台 (UWP)。

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>將 Direct3D 9 對應到 DirectX 11 API


[Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) 仍然是 DirectX 圖形的基礎，但從 DirectX 9 開始，API 已經有所變更：

-   Microsoft DirectX Graphics Infrastructure (DXGI) 用於設定圖形介面卡。 使用 [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi) 來選取緩衝區格式、建立交換鏈結、呈現框架以及建立共用資源。 請參閱 [DXGI 概觀](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi)。
-   Direct3D 裝置內容是用於設定管線狀態和產生轉譯命令。 我們大部分的範例都使用即時內容直接轉譯到裝置；Direct3D 11 也支援多執行緒轉譯，這種情況下會使用延遲內容。 請參閱 [Direct3D 11 中的裝置簡介](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro)。
-   某些功能已過時，其中最值得注意的是固定函式管線。 請參閱[過時的功能](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)。

如需 Direct3D 11 功能的完整清單，請參閱 [Direct3D 11 功能](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-features)與 [Direct3D 11 功能](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-1-features)。

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>從 Direct2D 9 移至 Direct2D 11


[Direct2D (Windows)](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) 仍然是 DirectX 圖形與 Windows 的重要部分。 您還是可以使用 Direct2D 繪製 2D 遊戲，並在 Direct3D 上繪製重疊 (HUD)。

Direct2D 在 Direct3D 之上執行；可使用任一種 API 來實作 2D 遊戲。 例如，使用 Direct3D 實作的 2D 遊戲可使用正視圖投影、設定 Z 值以控制基本型別的繪製順序，以及使用像素著色器新增特殊效果。

因為 Direct2D 是以 Direct3D 為基礎，因此它也使用 DXGI 與裝置內容。 請參閱 [Direct2D API 概觀](https://docs.microsoft.com/windows/desktop/Direct2D/the-direct2d-api)。

[DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal) API 透過 Direct2D 新增對於格式化文字的支援。 請參閱 [DirectWrite 簡介](https://docs.microsoft.com/windows/desktop/DirectWrite/introducing-directwrite)。

## <a name="replace-deprecated-helper-libraries"></a>取代過時的協助程式程式庫


D3DX 與 DXUT 已過時，UWP 遊戲無法使用。 這些協助程式程式庫提供紋理載入與網格載入等工作的資源。

-   [從 Direct3D 9 到 UWP 的簡易移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)逐步解說示範如何設定視窗、初始化 Direct3D 與執行基本 3D 轉譯。
-   [使用 DirectX 的簡易 UWP 遊戲](tutorial--create-your-first-uwp-directx-game.md)逐步解說示範常見的遊戲程式設計工作，包含圖形、載入檔案、UI、控制項與音效。
-   [DirectX 工具組](https://github.com/Microsoft/DirectXTK)社群專案提供可搭配 Direct3D 11 與 UWP app 使用的協助程式類別。

## <a name="move-shader-programs-from-fx-to-hlsl"></a>將著色器程式從 FX 移到 HLSL


在 UWP，D3DX 公用程式庫 (D3DX 9、D3DX 10 與 D3DX 11)，包含 Effects 在內都已過時。 UWP 的所有 DirectX 遊戲都使用 [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl) 來驅動圖形管線，而不使用 Effects。

Visual Studio 仍然使用 FXC 編譯著色器物件。 UWP 遊戲著色器會提前編譯。 位元組程式碼於執行階段載入，然後在適當的轉譯階段， 將每個著色器資源繫結至圖形管線。 著色器應該移至各自的獨立 .HLSL 檔案，而且應該在您的 C++ 程式碼中實作轉譯技術。

若要快速瀏覽載入著色器資源，請參閱[從 Direct3D 9 到 UWP 的簡易移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)。

Direct3D 11 引進了著色器模型5，其需要 Direct3D 功能\_等級 11 0 （或更高版本）。 請參閱 [Direct3D 11 的 HLSL 著色器模型 5 功能](https://docs.microsoft.com/windows/desktop/direct3dhlsl/overviews-direct3d-11-hlsl)。

## <a name="replace-xnamath-and-d3dxmath"></a>取代 XNAMath 與 D3DXMath


使用 XNAMath (或 D3DXMath) 的程式碼應該移轉至 [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)。 DirectXMath 包含可在 x86、x64 與 ARM 上進行移植的類型。 請參閱[從 XNA Math 程式庫移轉程式碼](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration)。

請注意，DirectXMath 浮點數類型方便搭配著色器使用。 例如，[**XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) 與 [**XMFLOAT4X4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) 便於對齊常數緩衝區的資料。

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>將 DirectSound 取代為 XAudio2 (與背景音訊)


UWP 不支援 DirectSound：

-   使用 [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) 將音效新增至您的遊戲。

##  <a name="replace-directinput-with-xinput-and-windows-runtime-apis"></a>將 DirectInput 取代為 XInput 和 Windows 執行階段 Api


UWP 不支援 DirectInput：

-   針對滑鼠、鍵盤與觸控輸入使用 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 輸入事件回呼。
-   針對遊戲控制器支援 (與遊戲控制器耳機支援) 使用 [XInput](https://docs.microsoft.com/windows/desktop/xinput/getting-started-with-xinput) 1.4。 若您在桌面與 UWP 使用共用的程式碼基底，請參閱 [XInput 版本](https://docs.microsoft.com/windows/desktop/xinput/xinput-versions)，取得回溯相容性的詳細資訊。
-   如果您的遊戲需要使用應用程式列，請登錄 [**EdgeGesture**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.EdgeGesture) 事件。

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>使用 Microsoft 媒體基礎代替 DirectShow


DirectShow 不再是 DirectX API (或 Windows API) 的一部分。 [Microsoft 媒體基礎](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)為使用共用表面的 Direct3D 提供視訊內容。 請參閱 [Direct3D 11 視訊 API](https://docs.microsoft.com/windows/desktop/medfound/direct3d-11-video-apis)。

## <a name="replace-directplay-with-networking-code"></a>將 DirectPlay 取代為網路程式碼


Microsoft DirectPlay 已過時。 如果您的遊戲使用網路服務，您必須提供符合 UWP 需求的網路程式碼。 請使用下列 API：

-   [適用於 UWP app (網路功能) (Windows) 的 Win32 和 COM](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)
-   [**Windows.Networking 命名空間 (Windows)**](https://docs.microsoft.com/uwp/api/Windows.Networking)
-   [**Windows.Networking.Sockets 命名空間 (Windows)**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)
-   [**Windows.Networking.Connectivity 命名空間 (Windows)**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity)
-   [**Windows.ApplicationModel.Background 命名空間 (Windows)**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

下列文章可協助您為 app 封裝資訊清單中的網路新增網路功能與宣告支援。

-   [使用通訊端進行連線 (使用 C#/VB/C++ 和 XAML 的 UWP app) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh452976(v=win.10))
-   [使用 WebSocket 進行連線 (使用 C#/VB/C++ 和 XAML 的 UWP app) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh994396(v=win.10))
-   [連線到 Web 服務 (使用 C#/VB/C++ 和 XAML 的 UWP app) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
-   [網路功能基本知識](https://docs.microsoft.com/windows/uwp/networking/networking-basics)

請注意，所有 UWP App (包含遊戲) 都使用特定的背景工作類型，以維持 App 暫停時的連線功能。 如果您的遊戲需要在暫停時維持連線狀態，請參閱[網路功能基本知識](https://docs.microsoft.com/windows/uwp/networking/networking-basics)。

## <a name="function-mapping"></a>函式對應


使用下表協助將程式碼從 Direct3D 9 轉換至 Direct3D 11。 這也可幫助您區別裝置與裝置內容。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Direct3D 11 對等項目</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3ddevice9">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2">ID3D11Device2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11devicecontext2">ID3D11DeviceContext2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline">圖形管線</a>中會說明圖形管線階段。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2">IDXGIFactory2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2">IDXGIAdapter2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-present">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-testcooperativelevel">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>使用 DXGI_PRESENT_TEST 旗標組呼叫 <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dbasetexture9">IDirect3DBaseTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dtexture9">IDirect3DTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dcubetexture9">IDirect3DCubeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvolumetexture9">IDirect3DVolumeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dindexbuffer9">IDirect3DIndexBuffer9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexbuffer9">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer">ID3D11Buffer</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture1d">ID3D11Texture1D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d">ID3D11Texture2D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture3d">ID3D11Texture3D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview">ID3D11ShaderResourceView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview">ID3D11RenderTargetView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexshader9">IDirect3DVertexShader9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dpixelshader9">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader">ID3D11VertexShader</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexdeclaration9">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setrenderstate">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setsamplerstate">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11blendstate1">ID3D11BlendState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilstate">ID3D11DepthStencilState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11rasterizerstate1">ID3D11RasterizerState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11samplerstate">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitive">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitive">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexedinstanced">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawinstanced">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetprimitivetopology">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawauto">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-beginscene">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-endscene">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitiveup">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitiveup">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>沒有直接的對等項目</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-showcursor">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorposition">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorproperties">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>使用標準游標 API。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-reset">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>LOST 裝置與 POOL_MANAGED 已不存在。 <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a> 可能會因 <a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error">DXGI_ERROR_DEVICE_REMOVED</a> 傳回值。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawrectpatch">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawtripatch">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-lightenable">IDirect3DDevice9: LightEnable</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-multiplytransform">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setlight">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setmaterial">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setnpatchmode">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setfvf">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settexturestagestate">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>固定函式管線已過時。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdepthstencilmatch">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdeviceformat">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-getdevicecaps">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-validatedevice">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>功能層級已取代功能位元。 針對任何指定的功能層級，只有少數格式與功能用法案例是選用的。 這些可以 <a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport">ID3D11Device::CheckFeatureSupport</a> 和 <a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-checkformatsupport">ID3D11Device::CheckFormatSupport</a> 進行檢查。</p></td>
</tr>
</tbody>
</table>

 

## <a name="surface-format-mapping"></a>表面格式對應


使用下表將 Direct3D 9 格式轉換為 DXGI 格式。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D 9 格式</th>
<th align="left">Direct3D 11 格式</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>請注意</strong>   ，使用著色器中的 r 拌和可複製紅色到其他元件，以取得 Direct3D 9 的行為。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>注意：</strong>   使用著色器中的拌和來複製紅色，並將綠色移到 Alpha 元件，以取得 Direct3D 9 的行為。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>請注意</strong>   ，在 Direct3D 9 中，資料是由 255.0 f 進行調整，但這可以在著色器中處理。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>請注意</strong>   ，在 Direct3D 9 中，資料是由 255.0 f 進行調整，但這可以在著色器中處理。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM 與 DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM 與 DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>請注意</strong>   ，從 API/硬體的觀點來看，DXT1 和 DXT2 都相同。 唯一的差異在於是否使用已預乘的 alpha，這可由應用程式追蹤，不需要個別格式。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM 與 DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM 與 DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>請注意</strong>   ，從 API/硬體的觀點來看，DXT3 和 DXT4 都相同。 唯一的差異在於是否使用已預乘的 alpha，這可由應用程式追蹤，不需要個別格式。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM 與 DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 與 D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>請注意</strong>   ，使用著色器中的 r 拌和，可將紅色複製到其他元件，以取得 D3D9 行為。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>請注意</strong>   ，著色器會取得 UINT 值，但如果需要 Direct3D 9 樣式整數浮點數（0.0 f，1.0 f .。。255. f），UINT 只可以轉換成著色器中的 float32。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>請注意</strong>   ，著色器會取得聖馬丁值，但如果需要 Direct3D 9 樣式整數浮點，則最聖可以轉換成著色器中的 float32。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>請注意</strong>   ，著色器會取得聖馬丁值，但如果需要 Direct3D 9 樣式整數浮點，則最聖可以轉換成著色器中的 float32。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>無法使用</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>注意</strong>  需要功能層級10.0 或更新版本
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>注意</strong>  需要功能層級10.0 或更新版本
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




