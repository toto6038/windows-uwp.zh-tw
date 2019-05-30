---
title: 逐步解說 - Direct3D 11 陰影體深度緩衝區
description: 此逐步解說示範如何在所有 Direct3D 功能層級的裝置上使用 Direct3D 11，轉譯使用深度圖的陰影體。
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX, 陰影體, 深度緩衝區, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: 2ce0cbd310ea89c5fa7b5c68033402f559768a24
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368515"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>逐步解說：實作使用 Direct3D 11 中的深度緩衝區的陰影磁碟區



此逐步解說示範如何在所有 Direct3D 功能層級的裝置上使用 Direct3D 11，轉譯使用深度圖的陰影體。
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">建立深度緩衝區裝置的資源</a></p></td>
<td align="left"><p>了解如何建立必要的 Direct3D 裝置資源，以支援陰影體的深度測試。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">呈現陰影地圖深度緩衝區</a></p></td>
<td align="left"><p>從光線的視角轉譯，以建立代表陰影體的二維深度圖。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">轉譯場景中的填入深度測試</a></p></td>
<td align="left"><p>將深度測試新增到您的頂點 (或幾何) 著色器與像素著色器，以建立陰影效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">支援一組硬體上的陰影 maps</a></p></td>
<td align="left"><p>在更快速的裝置上轉譯逼真度更高的陰影，在功能較弱的裝置上轉譯更快速的陰影。</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>從陰影貼圖應用程式移植到 Direct3D 9 傳統型應用程式


Windows 8 adde d 深度比較功能，功能層級 9\_1 和 9\_3。 現在，您可以將轉譯程式碼與陰影體移轉至 DirectX 11，而 Direct3D 11 轉譯器將降級以相容於功能層級 9 的裝置。 此逐步解說示範任何 Direct3D 11 應用程式或遊戲如何使用深度測試來實作傳統的陰影體。 程式碼包含下列程序：

1.  建立陰影對應的 Direct3D 裝置資源。
2.  新增轉譯階段以建立深度對應。
3.  將深度測試新增至主要轉譯階段。
4.  實作必要的著色器程式碼。
5.  舊版硬體上的快速轉譯選項。

完成本逐步解說中，您應該熟悉如何在 Direct3D 11 功能層級 9 與相容實作的基本相容的陰影磁碟區技術\_1 和更新版本。

## <a name="prerequisites"></a>先決條件


您應該[為通用 Windows 平台 (UWP) DirectX 遊戲開發準備開發環境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)。 您不需要的範本，但您將需要 Microsoft Visual Studio 2015 建置此逐步解說的程式碼範例。

## <a name="related-topics"></a>相關主題


**Direct3D**

* [在 Direct3D 中撰寫 HLSL 著色器 9](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [建立新的 DirectX 11 專案適用於 UWP](user-interface.md)

**對應技術文件的陰影**

* [常見的技術來改善陰影深度對應](https://docs.microsoft.com/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)
* [串聯的陰影對應](https://docs.microsoft.com/windows/desktop/DxTechArts/cascaded-shadow-maps)

 

 




