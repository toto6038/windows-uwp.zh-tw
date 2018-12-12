---
title: 將 DirectX 9 功能對應到 DirectX 11 API
description: 了解 Direct3D 9 遊戲使用的功能如何轉譯到 Direct3D 11 與通用 Windows 平台 (UWP)。
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX 9, DirectX 11, 移植
ms.localizationpriority: medium
ms.openlocfilehash: 56bb86706795e773d21e45263f640f9fc0aa596a
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8936065"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>將 DirectX 9 功能對應到 DirectX 11 API



**摘要**

-   [計劃 DirectX 移植](plan-your-directx-port.md)
-   [從 Direct3D 9 到 Direct3D 11 的重要變更](understand-direct3d-11-1-concepts.md)
-   功能對應


了解 Direct3D 9 遊戲使用的功能如何轉譯到 Direct3D 11 與通用 Windows 平台 (UWP)。

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>將 Direct3D 9 對應到 DirectX 11 API


[Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) 仍然是 DirectX 圖形的基礎，但從 DirectX 9 開始，API 已經有所變更：

-   Microsoft DirectX Graphics Infrastructure (DXGI) 用於設定圖形介面卡。 使用 [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534) 來選取緩衝區格式、建立交換鏈結、呈現框架以及建立共用資源。 請參閱 [DXGI 概觀](https://msdn.microsoft.com/library/windows/desktop/bb205075)。
-   Direct3D 裝置內容是用於設定管線狀態和產生轉譯命令。 我們大部分的範例都使用即時內容直接轉譯到裝置；Direct3D 11 也支援多執行緒轉譯，這種情況下會使用延遲內容。 請參閱 [Direct3D 11 中的裝置簡介](https://msdn.microsoft.com/library/windows/desktop/ff476880)。
-   某些功能已過時，其中最值得注意的是固定函式管線。 請參閱[過時的功能](https://msdn.microsoft.com/library/windows/desktop/cc308047)。

如需 Direct3D 11 功能的完整清單，請參閱 [Direct3D 11 功能](https://msdn.microsoft.com/library/windows/desktop/ff476342)與 [Direct3D 11 功能](https://msdn.microsoft.com/library/windows/desktop/hh404562)。

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>從 Direct2D 9 移至 Direct2D 11


[Direct2D (Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990) 仍然是 DirectX 圖形與 Windows 的重要部分。 您還是可以使用 Direct2D 繪製 2D 遊戲，並在 Direct3D 上繪製重疊 (HUD)。

Direct2D 在 Direct3D 之上執行；可使用任一種 API 來實作 2D 遊戲。 例如，使用 Direct3D 實作的 2D 遊戲可使用正視圖投影、設定 Z 值以控制基本型別的繪製順序，以及使用像素著色器新增特殊效果。

因為 Direct2D 是以 Direct3D 為基礎，因此它也使用 DXGI 與裝置內容。 請參閱 [Direct2D API 概觀](https://msdn.microsoft.com/library/windows/desktop/dd317121)。

[DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) API 透過 Direct2D 新增對於格式化文字的支援。 請參閱 [DirectWrite 簡介](https://msdn.microsoft.com/library/windows/desktop/dd371554)。

## <a name="replace-deprecated-helper-libraries"></a>取代過時的協助程式程式庫


D3DX 與 DXUT 已過時，UWP 遊戲無法使用。 這些協助程式程式庫提供紋理載入與網格載入等工作的資源。

-   [從 Direct3D 9 到 UWP 的簡易移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)逐步解說示範如何設定視窗、初始化 Direct3D 與執行基本 3D 轉譯。
-   [使用 DirectX 的簡易 UWP 遊戲](tutorial--create-your-first-uwp-directx-game.md)逐步解說示範常見的遊戲程式設計工作，包含圖形、載入檔案、UI、控制項與音效。
-   [DirectX 工具組](http://go.microsoft.com/fwlink/p/?LinkID=248929)社群專案提供可搭配 Direct3D 11 與 UWP app 使用的協助程式類別。

## <a name="move-shader-programs-from-fx-to-hlsl"></a>將著色器程式從 FX 移到 HLSL


在 UWP，D3DX 公用程式庫 (D3DX 9、D3DX 10 與 D3DX 11)，包含 Effects 在內都已過時。 UWP 的所有 DirectX 遊戲都使用 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) 來驅動圖形管線，而不使用 Effects。

Visual Studio 仍然使用 FXC 編譯著色器物件。 UWP 遊戲著色器會提前編譯。 位元組程式碼於執行階段載入，然後在適當的轉譯階段， 將每個著色器資源繫結至圖形管線。 著色器應該移至各自的獨立 .HLSL 檔案，而且應該在您的 C++ 程式碼中實作轉譯技術。

若要快速瀏覽載入著色器資源，請參閱[從 Direct3D 9 到 UWP 的簡易移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)。

Direct3D 11 引進了著色器模型 5，此模型需要 Direct3D 功能層級 11\_0 (或更高層級)。 請參閱 [Direct3D 11 的 HLSL 著色器模型 5 功能](https://msdn.microsoft.com/library/windows/desktop/ff471419)。

## <a name="replace-xnamath-and-d3dxmath"></a>取代 XNAMath 與 D3DXMath


使用 XNAMath (或 D3DXMath) 的程式碼應該移轉至 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)。 DirectXMath 包含可在 x86、x64 與 ARM 上進行移植的類型。 請參閱[從 XNA Math 程式庫移轉程式碼](https://msdn.microsoft.com/library/windows/desktop/ee418730)。

請注意，DirectXMath 浮點數類型方便搭配著色器使用。 例如，[**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) 與 [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) 便於對齊常數緩衝區的資料。

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>將 DirectSound 取代為 XAudio2 (與背景音訊)


UWP 不支援 DirectSound：

-   使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) 將音效新增至您的遊戲。

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>將 DirectInput 取代為 XInput 與 UWP API


UWP 不支援 DirectInput：

-   針對滑鼠、鍵盤與觸控輸入使用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 輸入事件回呼。
-   針對遊戲控制器支援 (與遊戲控制器耳機支援) 使用 [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001) 1.4。 若您在桌面與 UWP 使用共用的程式碼基底，請參閱 [XInput 版本](https://msdn.microsoft.com/library/windows/desktop/hh405051)，取得回溯相容性的詳細資訊。
-   如果您的遊戲需要使用應用程式列，請登錄 [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600) 事件。

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>使用 Microsoft 媒體基礎代替 DirectShow


DirectShow 不再是 DirectX API (或 Windows API) 的一部分。 [Microsoft 媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms694197)為使用共用表面的 Direct3D 提供視訊內容。 請參閱 [Direct3D 11 視訊 API](https://msdn.microsoft.com/library/windows/desktop/hh447677)。

## <a name="replace-directplay-with-networking-code"></a>將 DirectPlay 取代為網路程式碼


Microsoft DirectPlay 已過時。 如果您的遊戲使用網路服務，您必須提供符合 UWP 需求的網路程式碼。 請使用下列 API：

-   [適用於 UWP app (網路功能) (Windows) 的 Win32 和 COM](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Windows.Networking 命名空間 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Windows.Networking.Sockets 命名空間 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Windows.Networking.Connectivity 命名空間 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Windows.ApplicationModel.Background 命名空間 (Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

下列文章可協助您為 app 封裝資訊清單中的網路新增網路功能與宣告支援。

-   [使用通訊端進行連線 (使用 C#/VB/C++ 和 XAML 的 UWP app) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [使用 WebSocket 進行連線 (使用 C#/VB/C++ 和 XAML 的 UWP app) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [連線到 Web 服務 (使用 C#/VB/C++ 和 XAML 的 UWP app) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [網路功能基本知識](https://msdn.microsoft.com/library/windows/apps/mt280233)

請注意，所有 UWP App (包含遊戲) 都使用特定的背景工作類型，以維持 App 暫停時的連線功能。 如果您的遊戲需要在暫停時維持連線狀態，請參閱[網路功能基本知識](https://msdn.microsoft.com/library/windows/apps/mt280233)。

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174336">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280493">ID3D11Device2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280498">ID3D11DeviceContext2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476882">圖形管線</a>中會說明圖形管線階段。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174300">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404556">IDXGIFactory2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404537">IDXGIAdapter2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280345">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174423">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174472">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>使用 DXGI_PRESENT_TEST 旗標組呼叫 <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174322">IDirect3DBaseTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205909">IDirect3DTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174329">IDirect3DCubeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205941">IDirect3DVolumeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205865">IDirect3DIndexBuffer9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205915">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476351">ID3D11Buffer</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476633">ID3D11Texture1D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476635">ID3D11Texture2D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476637">ID3D11Texture3D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476628">ID3D11ShaderResourceView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476582">ID3D11RenderTargetView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476377">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205922">IDirect3DVertexShader9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205869">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476641">ID3D11VertexShader</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476576">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205919">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476575">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205805">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205806">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404571">ID3D11BlendState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476375">ID3D11DepthStencilState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446828">ID3D11RasterizerState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476588">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174369">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174371">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476407">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476409">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173566">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173567">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173590">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173564">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174350">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174375">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174372">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174370">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>沒有直接的對等項目</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174470">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174429">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174430">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>使用標準游標 API。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174425">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>LOST 裝置與 POOL_MANAGED 已不存在。 <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a> 可能會因 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509553">DXGI_ERROR_DEVICE_REMOVED</a> 傳回值。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174373">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174374">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174421">IDirect3DDevice9: LightEnable</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174422">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205798">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174437">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174438">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174463">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174433">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174462">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>固定函式管線已過時。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174308">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174309">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174320">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205859">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>功能層級已取代功能位元。 針對任何指定的功能層級，只有少數格式與功能用法案例是選用的。 這些可以 <a href="https://msdn.microsoft.com/library/windows/desktop/ff476497">ID3D11Device::CheckFeatureSupport</a> 和 <a href="https://msdn.microsoft.com/library/windows/desktop/bb173536">ID3D11Device::CheckFormatSupport</a> 進行檢查。</p></td>
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
<strong>注意：</strong> 使用器.r swizzle 將紅色複製到其他元件，以取得 Direct3D 9 行為的著色器中。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>注意：</strong> 使用著色器中的 swizzle.rrrg 將紅色複製，並將綠色移至 alpha 元件，以取得 Direct3D 9 行為。
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
<strong>注意：</strong> 在 Direct3D 9 中，資料已放大為 255.0 f，但這可在著色器中處理。
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
<strong>注意：</strong> 在 Direct3D 9 中，資料已放大為 255.0 f，但這可在著色器中處理。
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
<strong>注意：</strong> 來說，DXT1 與 DXT2 是相同的從 API/硬體的角度。 唯一的差異在於是否使用已預乘的 alpha，這可由應用程式追蹤，不需要個別格式。
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
<strong>注意：</strong> 來說，DXT3 與 DXT4 是相同的從 API/硬體的角度。 唯一的差異在於是否使用已預乘的 alpha，這可由應用程式追蹤，不需要個別格式。
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
<strong>注意：</strong> 使用器.r swizzle 將紅色複製到其他元件，以取得 D3D9 行為的著色器中。
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
<strong>注意：</strong> 著色器會取得 UINT 值，但需要如果 Direct3D 9 樣式整數浮點數 （0.0 f、 1.0 f...255.f)，UINT 可以只轉換為 float32 即可在著色器中。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>注意：</strong> 著色器會取得 SINT 值，但如果需要 Direct3D 9 樣式整數浮點數，SINT 可以只轉換為 float32 即可在著色器中。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>注意：</strong> 著色器會取得 SINT 值，但如果需要 Direct3D 9 樣式整數浮點數，SINT 可以只轉換為 float32 即可在著色器中。
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
<strong>注意：</strong> 需要功能層級 10.0 或更高
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>注意：</strong> 需要功能層級 10.0 或更高
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




