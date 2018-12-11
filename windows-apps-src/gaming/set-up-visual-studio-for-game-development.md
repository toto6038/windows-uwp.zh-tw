---
title: 用來進行遊戲程式設計的 Visual Studio 工具
description: Visual Studio 中提供的 DirectX 特定工具概觀。
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, visual studio, 工具, directx
ms.localizationpriority: medium
ms.openlocfilehash: 5a3938f486d52942031944b1184a711ddbc579db
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8924642"
---
# <a name="visual-studio-tools-for-game-programming"></a>用來進行遊戲程式設計的 Visual Studio 工具



**摘要**

-   [從範本建立 DirectX 遊戲專案](user-interface.md)
-   適用於 DirectX 遊戲程式設計的 Visual Studio 工具


如果您使用 Visual Studio Ultimate 來開發 DirectX app，則有額外工具可用來建立、編輯、預覽及匯出影像、模型及著色器資源。 同時還有工具可讓您在建置期間用來轉換資源和偵錯 DirectX 圖形程式碼。

本主題將提供這些圖形工具的概觀。

## <a name="image-editor"></a>影像編輯器


使用影像編輯器來利用 DirectX 所使用的豐富紋理與影像格式。 影像編輯器支援下列格式：

-   .png
-   .jpg、.jpeg、.jpe、.jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif、.tiff
-   .tga

建立[組建自訂檔](#build-customizations-for-3d-assets)，在建置期間將這些檔案新增到 .dds 檔案。

如需詳細資訊，請參閱[使用紋理與影像](https://msdn.microsoft.com/library/windows/apps/hh873119.aspx)。

> **注意：** 影像編輯器並非用來取代功能完整的影像編輯 app，但適合用於許多簡單的檢視與編輯案例。

 

## <a name="model-editor"></a>模型編輯器


您可以使用模型編輯器，從頭開始建立基本的 3D 模型，或者從功能完整的 3D 模型工具中檢視和修改更複雜的 3D 模型。 模型編輯器支援數個在 DirectX 應用程式開發時使用的 3D 模型格式。 您可以建立[組建自訂檔](#build-customizations-for-3d-assets)，在建置期間將這些檔案轉換成 .cmo 檔案。

-   .fbx
-   .dae
-   .obj

以下為編輯器中已套用光源的模型螢幕擷取畫面。

![茶壺](images/modeleditor.png)

如需詳細資訊，請參閱[使用 3D 模型](https://msdn.microsoft.com/library/windows/apps/hh873114.aspx)。

> **注意：** 模型編輯器並非用來取代功能完整的模型編輯 app，但適合用於許多簡單的檢視與編輯案例。

 

## <a name="shader-designer"></a>著色器設計工具


即使您不了解 HLSL 程式設計，也可以使用著色器設計工具為您的遊戲或應用程式建立自訂的視覺效果。

您以視覺化方式建立著色器來做為圖形。 每個節點都會根據該操作顯示輸出的預覽。 這裡提供的範例會使用球體預覽來套用 Lambert 光源。

![視覺著色器圖形](images/shaderdesigner.png)

使用著色器編輯器，以 .dgsl 格式來設計、編輯及儲存著色器。 它也會匯出下列格式：

-   .hlsl (原始碼)
-   .cso (位元組程式碼)
-   .h (HLSL 位元組程式碼陣列)

建立[組建自訂檔](#build-customizations-for-3d-assets)，以在建置期間將這其中的所有格式轉換成 .cso 檔案。

以下為著色器編輯器所匯出的部分 HLSL 程式碼。 這只是 Lambert 光源節點的程式碼。

```hlsl
//
// Lambert lighting function
//
float3 LambertLighting(
    float3 lightNormal,
    float3 surfaceNormal,
    float3 materialAmbient,
    float3 lightAmbient,
    float3 lightColor,
    float3 pixelColor
    )
{
    // Compute the amount of contribution per light.
    float diffuseAmount = saturate(dot(lightNormal, surfaceNormal));
    float3 diffuse = diffuseAmount * lightColor * pixelColor;

    // Combine ambient with diffuse.
    return saturate((materialAmbient * lightAmbient) + diffuse);
}
```

如需相關資訊，請參閱[使用著色器](https://msdn.microsoft.com/library/windows/apps/hh873117.aspx)。

## <a name="build-customizations-for-3d-assets"></a>適用於 3D 資產的組建自訂


您可以將組建自訂新增到專案，如此一來，Visual Studio 便能將資源轉換成可使用的格式。 在此之後，您就可以將資產載入 App， 並藉由建立並填入 DirectX 資源 (就像您在任何其他 DirectX App 中所做的動作) 來使用它們。

若要新增組建自訂，您可以在**方案總管]** 中的專案上按一下滑鼠右鍵，然後選取**建置自訂設定**。您可以將下列類型的組建自訂新增到您的專案。

-   影像內容管線會取得影像檔做為輸入並輸出 DirectDraw 表面 (.dds) 檔案。
-   網格內容管線會取得網格檔 (例如 .fbx) 並輸出 .cmo 網格檔。
-   著色器內容管線會從 Visual Studio 著色器編輯器中取得視覺著色器圖形 (.dgsl)，並輸出編譯過的著色器輸出 (.cso) 檔案。

如需詳細資訊，請參閱[在遊戲或應用程式中使用 3D 資產](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)。

## <a name="debugging-directx-graphics"></a>偵錯 DirectX 圖形


Visual Studio 提供圖形特定的偵錯工具。 使用這些工具進行諸如下列項目的偵錯：

-   圖形管線。
-   事件呼叫堆疊。
-   物件表格。
-   裝置狀態。
-   著色器錯誤。
-   未初始化或不正確的常數緩衝區與參數。
-   DirectX 版本相容性。
-   有限的 Direct2D 支援。
-   作業系統與 SDK 需求。

如需詳細資訊，請參閱[偵錯 DirectX 圖形](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx)。


 

 

 




