---
author: mtoepke
title: 使用深度測試轉譯場景
description: 將深度測試新增到您的頂點 (或幾何) 著色器與像素著色器，以建立陰影效果。
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, rendering, scene, depth testing, direct3d, shadows, 遊戲, 轉譯, 場景, 深度測試, 陰影
ms.localizationpriority: medium
ms.openlocfilehash: dc776a60e771cc8d5961e8c7b9c67eb99fabea3a
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7304554"
---
# <a name="render-the-scene-with-depth-testing"></a>使用深度測試轉譯場景




將深度測試新增到您的頂點 (或幾何) 著色器與像素著色器，以建立陰影效果。 [逐步解說：使用 Direct3D 11 中的深度緩衝區實作陰影體](implementing-depth-buffers-for-shadow-mapping.md)的第三部分。

## <a name="include-transformation-for-light-frustum"></a>包含光線範圍的轉換


您的頂點著色器需要針對每個頂點計算光線空間的轉換位置。 使用常數緩衝區來提供光線空間模型、檢視及投影矩陣 您也可以使用這個常數緩衝區提供光線位置與標準，以進行光線計算。 光線空間的轉換位置將會在深度測試期間使用。

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    float4 modelPos = mul(pos, model);
    pos = mul(modelPos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    // Transform the vertex position into projected space from the POV of the light.
    float4 lightSpacePos = mul(modelPos, lView);
    lightSpacePos = mul(lightSpacePos, lProjection);
    output.lightSpacePos = lightSpacePos;

    // Light ray
    float3 lRay = lPos.xyz - modelPos.xyz;
    output.lRay = lRay;
    
    // Camera ray
    output.view = eyePos.xyz - modelPos.xyz;

    // Transform the vertex normal into world space.
    float4 norm = float4(input.norm, 1.0f);
    norm = mul(norm, model);
    output.norm = norm.xyz;
    
    // Pass through the color and texture coordinates without modification.
    output.color = input.color;
    output.tex = input.tex;

    return output;
}
```

接著，像素著色器將使用頂點著色器提供的插補光線空間位置來測試像素是否位於陰影中。

## <a name="test-whether-the-position-is-in-the-light-frustum"></a>測試位置是否位於光線範圍中


首先，透過將 X 與 Y 座標標準化，以檢查像素是否位於光線的檢視範圍中。 如果它們均位於範圍 \[0, 1\] 內，則像素可能就位於陰影中。 否則，您可以略過深度測試。 著色器可以呼叫 [Saturate](https://msdn.microsoft.com/library/windows/desktop/hh447231) 並根據原始值來比較結果，藉以快速測試此項目。

```cpp
// Compute texture coordinates for the current point's location on the shadow map.
float2 shadowTexCoords;
shadowTexCoords.x = 0.5f + (input.lightSpacePos.x / input.lightSpacePos.w * 0.5f);
shadowTexCoords.y = 0.5f - (input.lightSpacePos.y / input.lightSpacePos.w * 0.5f);
float pixelDepth = input.lightSpacePos.z / input.lightSpacePos.w;

float lighting = 1;

// Check if the pixel texture coordinate is in the view frustum of the 
// light before doing any shadow work.
if ((saturate(shadowTexCoords.x) == shadowTexCoords.x) &&
    (saturate(shadowTexCoords.y) == shadowTexCoords.y) &&
    (pixelDepth > 0))
{
```

## <a name="depth-test-against-the-shadow-map"></a>根據陰影圖進行的深度測試


使用一個取樣比較函式 ([SampleCmp](https://msdn.microsoft.com/library/windows/desktop/bb509696) 或 [SampleCmpLevelZero](https://msdn.microsoft.com/library/windows/desktop/bb509697))，根據深度圖來測試像素在光線空間中的深度。 計算標準化的光線空間深度值 (也就是 `z / w`)，並將值傳遞到比較函式。 因為我們針對取樣器使用 LessOrEqual 比較測試，因此內建函式會在通過比較測試時傳回零；這表示像素位於陰影中。

```cpp
// Use an offset value to mitigate shadow artifacts due to imprecise 
// floating-point values (shadow acne).
//
// This is an approximation of epsilon * tan(acos(saturate(NdotL))):
float margin = acos(saturate(NdotL));
#ifdef LINEAR
// The offset can be slightly smaller with smoother shadow edges.
float epsilon = 0.0005 / margin;
#else
float epsilon = 0.001 / margin;
#endif
// Clamp epsilon to a fixed range so it doesn't go overboard.
epsilon = clamp(epsilon, 0, 0.1);

// Use the SampleCmpLevelZero Texture2D method (or SampleCmp) to sample from 
// the shadow map, just as you would with Direct3D feature level 10_0 and
// higher.  Feature level 9_1 only supports LessOrEqual, which returns 0 if
// the pixel is in the shadow.
lighting = float(shadowMap.SampleCmpLevelZero(
    shadowSampler,
    shadowTexCoords,
    pixelDepth + epsilon
    )
    );
```

## <a name="compute-lighting-in-or-out-of-shadow"></a>計算陰影的光源方向


如果像素並未位於陰影中，則像素著色器應該會計算直接光源，並將它新增到像素值。

```cpp
return float4(input.color * (ambient + DplusS(N, L, NdotL, input.view)), 1.f);
```

```cpp
float3 DplusS(float3 N, float3 L, float NdotL, float3 view)
{
    const float3 Kdiffuse = float3(.5f, .5f, .4f);
    const float3 Kspecular = float3(.2f, .2f, .3f);
    const float exponent = 3.f;

    // Compute the diffuse coefficient.
    float diffuseConst = saturate(NdotL);

    // Compute the diffuse lighting value.
    float3 diffuse = Kdiffuse * diffuseConst;

    // Compute the specular highlight.
    float3 R = reflect(-L, N);
    float3 V = normalize(view);
    float3 RdotV = dot(R, V);
    float3 specular = Kspecular * pow(saturate(RdotV), exponent);

    return (diffuse + specular);
}
```

否則，像素著色器應該使用周遭環境光源來計算像素值。

```cpp
return float4(input.color * ambient, 1.f);
```

在這個逐步解說的下一個部分，您將了解如何[支援各種硬體上的陰影圖](target-a-range-of-hardware.md)。

 

 




