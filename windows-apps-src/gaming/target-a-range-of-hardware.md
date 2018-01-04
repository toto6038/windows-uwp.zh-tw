---
author: mtoepke
title: "支援各種硬體上的陰影圖"
description: "在更快速的裝置上轉譯逼真度更高的陰影，在功能較弱的裝置上轉譯更快速的陰影。"
ms.assetid: d97c0544-44f2-4e29-5e02-54c45e0dff4e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 遊戲, 陰影圖, directx"
ms.openlocfilehash: e4cffcf1e9655d5bc5dacbfc17cb64b5671d7551
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: zh-TW
---
# <a name="support-shadow-maps-on-a-range-of-hardware"></a>支援各種硬體上的陰影圖


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


在更快速的裝置上轉譯逼真度更高的陰影，在功能較弱的裝置上轉譯更快速的陰影。 [逐步解說：使用 Direct3D 11 中的深度緩衝區實作陰影體](implementing-depth-buffers-for-shadow-mapping.md)的第四部分。

## <a name="comparison-filter-types"></a>比較篩選器類型


如果裝置能夠承擔效能損失，就只使用線性篩選。 一般來說，Direct3D 功能層級 9\_1 裝置沒有足夠的能力，能夠為陰影中的線性篩選進行備援。 請在這些裝置上改用點篩選。 當您使用線性篩選時，請調整像素著色器，讓它能混合陰影邊緣。

建立適用於點篩選的比較取樣器：

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

接著針對線性篩選建立取樣器：

```cpp
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR;
DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_linear
        )
    );
```

選擇取樣器：

```cpp
ID3D11PixelShader* pixelShader;
ID3D11SamplerState** comparisonSampler;
if (m_useLinear)
{
    pixelShader = m_shadowPixelShader_linear.Get();
    comparisonSampler = m_comparisonSampler_linear.GetAddressOf();
}
else
{
    pixelShader = m_shadowPixelShader_point.Get();
    comparisonSampler = m_comparisonSampler_point.GetAddressOf();
}

// Attach our pixel shader.
context->PSSetShader(
    pixelShader,
    nullptr,
    0
    );

context->PSSetSamplers(0, 1, comparisonSampler);
context->PSSetShaderResources(0, 1, m_shadowResourceView.GetAddressOf());
```

利用線性篩選混合陰影邊緣：

```cpp
// Blends the shadow area into the lit area.
float3 light = lighting * (ambient + DplusS(N, L, NdotL, input.view));
float3 shadow = (1.0f - lighting) * ambient;
return float4(input.color * (light + shadow), 1.f);
```

## <a name="shadow-buffer-size"></a>陰影緩衝區大小


較大的陰影圖不會看起來濃淡不均勻，但是它們會在圖形記憶體中佔據更多空間。 在遊戲中試驗不同的陰影圖大小，並觀察不同裝置類型與不同顯示大小的結果。 請考量像是階層式陰影圖的最佳化，以使用較少的圖形記憶體來取得更好的結果。 請參閱[改善陰影深度圖的常見技術](https://msdn.microsoft.com/library/windows/desktop/ee416324)。

## <a name="shadow-buffer-depth"></a>陰影緩衝區深度


陰影緩衝區中較高的精確度將能提供更準確的深度測試結果，這可協助避免發生像是 z 緩衝區衝突的問題。 但是，與較大的陰影圖一樣，較高的精確度會佔用較多的記憶體。 在遊戲中使用不同的深度精確度類型來進行試驗 (DXGI\_FORMAT\_R24G8\_TYPELESS 與 DXGI\_FORMAT\_R16\_TYPELESS)，然後觀察不同功能層級上的速度與品質。

## <a name="optimizing-precompiled-shaders"></a>將預先編譯的著色器最佳化


雖然通用 Windows 平台 (UWP) 應用程式能夠使用動態著色器編譯，但是使用動態著色器連結更為快速。 您也可以使用編譯器指示詞和 `#ifdef` 區塊來建立不同版本的著色器。 您可以在文字編輯器中開啟 Visual Studio 專案檔，然後為 HLSL 新增多個 `<FxcCompiler>` 項目 (每個都有適當的前置處理器定義) 來這樣做。 請注意，這樣會使用不同的檔案名稱；在此例中，Visual Studio 會將 \_point 和 \_linear 附加到不同版本的著色器。

適用於著色器線性篩選版本的專案檔項目會定義 LINEAR：

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|x64'">LINEAR</PreprocessorDefinitions>
</FxCompile>
```

適用於著色器線性篩選版本的專案檔項目不會包含前置處理器定義：

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
</FxCompile>
```

 

 




