---
title: 從 Direct3D 9 到 Direct3D 11 的重要變更
description: 本主題說明 DirectX 9 和 DirectX 11 的概要差異。
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, directx, direct3d 9, direct3d 11, 變更
ms.localizationpriority: medium
ms.openlocfilehash: ecdd8591efb3920d2cfe333aa8ec02c65c1a1465
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8897553"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>從 Direct3D 9 到 Direct3D 11 的重要變更



**摘要**

-   [計劃 DirectX 移植](plan-your-directx-port.md)
-   從 Direct3D 9 到 Direct3D 11 的重要變更
-   [功能對應](feature-mapping.md)


本主題說明 DirectX 9 和 DirectX 11 的概要差異。

Direct3D 11 基本上與 Direct3D 9 使用相同的 API 類型，也就是圖形硬體中的低階虛擬化介面； 但它還是可以讓您在各種硬體實作上執行繪圖操作。 圖形 API 的配置從 Direct3D 9 開始就已經變更，不但擴充了裝置內容的概念，也特別針對圖形基礎結構新增 API。 儲存在 Direct3D 裝置上的資源有一個全新的資料多型機制，稱為資源檢視。

## <a name="core-api-functions"></a>核心 API 函式


過去在 Direct3D 9，您必須先建立 Direct3D API 的介面才能開始使用它。 而在 Direct3D 11 通用 Windows 平台 (UWP) 遊戲中，您呼叫名為 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 的靜態函式來建立裝置和裝置內容。

## <a name="devices-and-device-context"></a>裝置和裝置內容


一個 Direct3D 11 裝置代表一個虛擬化的圖形卡。 它是用在視訊記憶體中建立資源，例如，將紋理上傳到 GPU、在紋理資源和交換鏈結建立檢視，以及建立紋理取樣器。 如需 Direct3D 11 裝置介面用途的完整清單，請參閱 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 和 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)。

Direct3D 11 裝置內容是用於設定管線狀態和產生轉譯命令。 例如，Direct3D 11 轉譯鏈結會使用裝置內容來設定轉譯鏈結和繪製場景 (請參閱以下內容)。 裝置內容是用於存取 (對應) Direct3D 裝置資源使用的視訊記憶體；也用於更新子資源資料，例如常數緩衝區資料。 如需 Direct3D 11 裝置內容用途的完整清單，請參閱 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 和 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)。 請注意，我們大部分的範例都使用即時內容直接轉譯到裝置，但是 Direct3D 11 也支援延遲的裝置內容，這主要用於多執行緒處理。

在 Direct3D 11 中，裝置控制代碼和裝置內容控制代碼都是透過呼叫 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 取得。 您在圖形卡支援的 Direct3D 功能層級，要求一組特定的硬體功能和抓取資訊時，也是使用這個方法。 如需裝置、裝置內容和執行緒考量的詳細資訊，請參閱 [Direct3D 11 中的裝置簡介](https://msdn.microsoft.com/library/windows/desktop/ff476880)。

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>裝置基礎結構、框架緩衝區和轉譯目標檢視


在 Direct3D 11，裝置介面卡和硬體設定是以使用 [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) 和 [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) COM 介面的 DirectX Graphics Infrastructure (DXGI) API 進行設定。 緩衝區和其他視窗資源 (可見或螢幕上不可見) 都是由特定的 DXGI 介面建立和設定，[**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) Factory 模式實作會取得 DXGI 資源，例如框架緩衝區。 因為 DXGI 擁有交換鏈結，所以 DXGI 介面會用來將畫面呈現在螢幕上，請參閱 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)。

使用 [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) 建立與您的遊戲相容的交換鏈結。 您需要建立核心視窗或組合 (XAML 互通性) 的交換鏈結，而不是建立 HWND 的交換鏈結。

## <a name="device-resources-and-resource-views"></a>裝置資源和資源檢視


Direct3D 11 在視訊記憶體資源支援一個額外的多型層級稱為檢視。 實際上，您原本有一個 Direct3D 9 紋理物件，而您現在會有兩個物件：一個是保存資料的紋理資源，另一個是指出如何使用檢視進行轉譯的資源檢視。 以資源為基礎的檢視可讓該資源用於特定的目的。 例如，2D 紋理資源建立為 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)，然後在上面建立著色器資源檢視 ([**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628))，便可用來做為著色器中的紋理。 也可以在相同的 2D 紋理資源上建立轉譯目標檢視 ([**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582))，便可用來做為繪圖表面。 另一個範例中，在單一紋理資源上使用兩個個別的檢視，以兩種不同的像素格式來表示相同的像素資料。

基礎資源必須以與建立來源檢視類型相容的屬性建立。 例如，如果 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 是套用到以 [**D3D11\_BIND\_RENDER\_TARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476085) 旗標建立的表面。 該表面也必須具備與轉譯相容的 DXGI 表面格式 (請參閱 [**DXGI_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059))。

用於轉譯的大多數資源都是繼承自 [**ID3D11Resource**](https://msdn.microsoft.com/library/windows/desktop/ff476584) 介面，而該介面是從 [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380) 繼承而來。 頂點緩衝區、索引緩衝區、常數緩衝區和著色器都是 Direct3D 11 資源。 輸入配置和取樣器狀態直接繼承自 [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380)。

資源檢視使用 DXGI\_FORMAT 列舉值來指示像素格式。 並不是每個 D3DFMT 都支援做為 DXGI\_FORMAT。 例如，DXGI 中沒有等同於 D3DFMT\_R8G8B8 的 24bpp RGB 格式。 也不是每一個 RGB 格式都有 BGR 對應項 (DXGI\_FORMAT\_R10G10B10A2\_UNORM 等同於 D3DFMT\_A2B10G10R10，但是 D3DFMT\_A2R10G10B10 沒有直接的對應項)。 您應該在建置階段先做規劃，將這些舊版格式的內容轉換為支援的格式。 如需 DXGI 格式的完整清單，請參閱 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059) 列舉。

Direct3D 裝置資源 (及資源檢視) 會在場景轉譯之前建立。 裝置內容是用於設定轉譯鏈結，說明如下。

## <a name="device-context-and-the-rendering-chain"></a>裝置內容和轉譯鏈結


在 Direct3D 9 和 Direct3D 10.x，曾有一個管理資源建立、狀態和繪圖的單一 Direct3D 裝置物件。 在 Direct3D 11，Direct3D 裝置介面仍然會管理資源建立，但所有狀態和繪圖操作都使用 Direct3D 裝置內容來處理。 以下是如何使用裝置內容 ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 介面) 設定轉譯鏈結的範例：

-   設定和清除轉譯目標檢視 (以及深度樣板檢視)
-   設定輸入組合語言階段 (IA 階段) 的頂點緩衝區、索引緩衝區及輸入配置
-   將頂點和像素著色器繫結到管線
-   將常數緩衝區繫結到著色器
-   將紋理檢視和取樣器繫結到像素著色器
-   繪製場景

呼叫其中一個 [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) 方法時，會在轉譯目標檢視繪製場景。 當您完成所有繪圖時，DXGI 介面卡會透過呼叫 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 用來呈現完整的畫面。

## <a name="state-management"></a>狀態管理


具備一組大型個別切換的 Direct3D 9 Managed 狀態設定是以 SetRenderState、SetSamplerState 和 SetTextureStageState 方法進行設定。 因為 Direct3D 11 不支援舊版固定函式管線，所以透過撰寫像素著色器 (PS) 來取代 SetTextureStageState。 Direct3D 9 狀態區塊沒有對應項目。 Direct3D 11 代之使用 4 種狀態物件來管理狀態，這些物件為轉譯狀態的編組提供更簡單的方式。

例如，您不使用 SetRenderState 搭配 D3DRS\_ZENABLE，而是使用這一個和其他相關的狀態設定來建立 DepthStencilState 物件，然後在轉譯時用它來變更狀態。

將 Direct3D 9 應用程式移植到狀態物件時，請注意，您的各種狀態組合會表示為不可變的狀態物件。 它們只應該建立一次，只要有效就可以重複使用。

## <a name="direct3d-feature-levels"></a>Direct3D 功能層級


Direct3D 有一個用於判斷硬體支援的新機制，稱為「功能層級」。 功能層級透過允許您要求一組定義完整的 GPU 功能，簡化了判斷圖形卡作用的工作。 例如，9\_1 功能層級會實作由 Direct3D 9 圖形卡所提供的功能，包含著色器模型 2.x。 因為 9\_1 是最低的功能層級，所以您可以預期，處於相同 Direct3D 9 可程式化著色器模型支援階段的所有裝置，都支援頂點著色器和像素著色器。

您的遊戲將使用 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 建立 Direct3D 裝置和裝置內容。 當您呼叫這個函式時，要提供您的遊戲可支援的功能層級清單， 它會傳回該清單中可支援的最高功能層級。 例如，如果您的遊戲可以使用 BC4/BC5 紋理 (DirectX 10 硬體的一項功能)，支援的功能層級清單中至少要包含 9\_1 和 10\_0。 如果遊戲是在 DirectX 9 硬體上執行，而無法使用 BC4/BC5 紋理，[**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 會傳回 9\_1。 然後，您的遊戲可以切換回不同的紋理格式 (和較小的紋理)。

如果您決定擴充 Direct3D 9 遊戲以支援更高的 Direct3D 功能層級，最好先完成現有 Direct3D 9 圖形程式碼的移植。 等到您的遊戲可以在 Direct3D 11 運作之後，比較容易以增強的圖形新增其他轉譯路徑。

如需功能層級支援的詳細說明，請參閱 [Direct3D 功能層級](https://msdn.microsoft.com/library/windows/desktop/ff476876)。 如需 Direct3D 11 功能的完整清單，請參閱 [Direct3D 11 功能](https://msdn.microsoft.com/library/windows/desktop/ff476342)和 [Direct3D 11.1 功能](https://msdn.microsoft.com/library/windows/desktop/hh404562)。

## <a name="feature-levels-and-the-programmable-pipeline"></a>功能層級和可程式化的管線


Direct3D 9 推出之後，硬體仍持續不斷地演進，而可程式化的圖形管線也增加了多個新的選擇性階段。 您所擁有的圖形管線選項組，隨 Direct3D 功能層級不同而異。 功能層級 10.0 包含幾何著色器階段，以及在 GPU 上物件多重轉譯的選擇性資料流輸出。 功能層級 11\_0 包含輪廓著色器和網域著色器，可與硬體鑲嵌搭配使用。 功能層級 11\_0 也包含 DirectCompute 著色器的完整支援，但功能層級 10.x 只包含有限形式的 DirectCompute 支援。

所有著色器都是使用對應到 Direct3D 功能層級的著色器設定檔，以 HLSL 撰寫而成。 著色器設定檔是向上相容，所以使用 vs\_4\_0\_level\_9\_1 或 ps\_4\_0\_level\_9\_1 編譯的 HLSL 著色器可以在所有裝置上運作。 著色器設定檔無法向下相容，所以使用 vs\_4\_1 編譯的著色器只能在功能層級 10\_1、11\_0 或 11\_1 的裝置上運作。

著色器的 Direct3D 9 管理常數使用共用陣列搭配 SetVertexShaderConstant 和 SetPixelShaderConstant。 Direct3D 11 則使用常數緩衝區，這些是像頂點緩衝區或索引緩衝區等的資源。 常數緩衝區的設計目的是要有效率地更新。 這個做法不是將所有著色器常數組織成單一全域陣列，而是將您的常數組織為邏輯群組，並透過一或多個常數緩衝區加以管理。 當您將 Direct3D 9 遊戲移植到 Direct3D 11 時，需要先規劃如何組織您的常數緩衝區，才能適當地更新常數。 例如，將每個畫面未更新的著色器常數編組成個別常數緩衝區，所以您不必經常將該資料以及更多動態著色器常數上傳到圖形卡。

> **注意：** 多數 Direct3D 9 應用程式都大量使用著色器，但偶爾會混用舊版固定函式行為。 請注意，Direct3D 11 只使用可程式化的著色模型。 Direct3D 9 的舊版固定函式功能已過時。

 

 

 




