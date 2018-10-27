---
author: mtoepke
title: 逐步解說 - Direct3D 11 陰影體深度緩衝區
description: 此逐步解說示範如何在所有 Direct3D 功能層級的裝置上使用 Direct3D 11，轉譯使用深度圖的陰影體。
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX, 陰影體, 深度緩衝區, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: 269f8423795d7c2515ce555cf8e65f14a8c63431
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5701603"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>逐步解說：使用 Direct3D 11 中的深度緩衝區實作陰影體



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
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">建立深度緩衝區裝置資源</a></p></td>
<td align="left"><p>了解如何建立必要的 Direct3D 裝置資源，以支援陰影體的深度測試。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">將陰影圖轉譯為深度緩衝區</a></p></td>
<td align="left"><p>從光線的視角轉譯，以建立代表陰影體的二維深度圖。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">使用深度測試轉譯場景</a></p></td>
<td align="left"><p>將深度測試新增到您的頂點 (或幾何) 著色器與像素著色器，以建立陰影效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">支援各種硬體上的陰影圖</a></p></td>
<td align="left"><p>在更快速的裝置上轉譯逼真度更高的陰影，在功能較弱的裝置上轉譯更快速的陰影。</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>從陰影貼圖應用程式移植到 Direct3D 9 傳統型應用程式


Windows8 已到功能層級 9 \_1 與 9 \_3 深度比較功能。 現在，您可以將轉譯程式碼與陰影體移轉至 DirectX 11，而 Direct3D 11 轉譯器將降級以相容於功能層級 9 的裝置。 此逐步解說示範任何 Direct3D 11 應用程式或遊戲如何使用深度測試來實作傳統的陰影體。 程式碼包含下列程序：

1.  建立陰影對應的 Direct3D 裝置資源。
2.  新增轉譯階段以建立深度對應。
3.  將深度測試新增至主要轉譯階段。
4.  實作必要的著色器程式碼。
5.  舊版硬體上的快速轉譯選項。

完成此逐步解說後，您應該就會熟悉如何在與功能層級 9\_1 與更高版本相容的 Direct3D 11 中，實作基本的相容陰影體技術。

## <a name="prerequisites"></a>先決條件


您應該[為通用 Windows 平台 (UWP) DirectX 遊戲開發準備開發環境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)。 您還不需要範本，但您將需要 Microsoft Visual Studio2015 來建置此逐步解說的程式碼範例。

## <a name="related-topics"></a>相關主題


**Direct3D**

* [在 Direct3D 9 撰寫 HLSL 著色器](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [建立適用於 UWP 的新 DirectX 11 專案](user-interface.md)

**陰影貼圖技術文章**

* [改善陰影深度圖的常見技術](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [重疊的陰影圖](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 




