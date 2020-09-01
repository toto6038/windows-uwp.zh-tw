---
title: 適用於 DirectX 遊戲的基本 3D 圖形
description: 我們將示範如何使用 DirectX 程式設計來實作 3D 圖形的基本概念。
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX, 圖形
ms.localizationpriority: medium
ms.openlocfilehash: 68ee694c036d4bc92f76ea75f7c5e5e530e26334
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173192"
---
# <a name="basic-3d-graphics-for-directx-games"></a>適用於 DirectX 遊戲的基本 3D 圖形



我們將示範如何使用 DirectX 程式設計來實作 3D 圖形的基本概念。

**目標：** 學習進行 3D 圖形 app 的程式設計。

## <a name="prerequisites"></a>先決條件


我們假設您熟悉 C++。 您還需要圖形程式設計概念的基本經驗。

**完成所需的總時間：** 30 分鐘。

## <a name="where-to-go-from-here"></a>現在該如何開始


在這裡，我們會討論如何使用 DirectX 和 c + + Cx 開發3D 圖形 \\ 。 這個包含五個部分的教學課程將為您介紹 [Direct3D](/windows/desktop/direct3d) API 及許多其他 DirectX 範例中也會用到的概念和程式碼。 這些部分彼此互相關聯，涵蓋範圍從為您的 UWP C++ app 設定 DirectX，到在基本型別上添加紋理和新增效果。

> **注意**   本教學課程使用右手座標系統與資料行向量。 許多 DirectX 範例和 app 都使用左手座標系統搭配列向量。 如需更完整的圖形數學方案和支援左手座標系統搭配列向量的方案，請考慮使用 [DirectXMath](/windows/desktop/dxmath/directxmath-portal)。 如需詳細資訊，請參閱[使用 DirectXMath 搭配 Direct3D](/windows/desktop/dxmath/pg-xnamath-migration-d3dx)。

 

我們將示範如何：

-   使用 Windows 執行階段將 [Direct3D](/windows/desktop/direct3d) 介面初始化
-   套用每一頂點著色器作業
-   設定幾何
-   將場景點陣化 (將 3D 場景平面化成 2D 投影)
-   消除隱藏的表面

> **注意**  

 

接著，我們會建立一個 Direct3D 裝置、交換鏈結和轉譯目標檢視，並將轉譯的影像呈現到顯示器。

[快速入門：設定 DirectX 資源及顯示影像](setting-up-directx-resources.md)

## <a name="related-topics"></a>相關主題


* [Direct3D 11 圖形](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 