---
title: 計劃 DirectX 移植
description: 計劃從 DirectX 9 到 DirectX 11 與通用 Windows 平台 (UWP) 的遊戲移植專案 -- 升級您的圖形程式碼，並將遊戲放置於 Windows 執行階段環境中。
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, directx, port, 連接埠
ms.localizationpriority: medium
ms.openlocfilehash: abbcd688df01b779a1cb3ab9e30bd13709926be4
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8785880"
---
# <a name="plan-your-directx-port"></a>計劃 DirectX 移植



**摘要**

-   計劃 DirectX 移植
-   [從 Direct3D 9 到 Direct3D 11 的重要變更](understand-direct3d-11-1-concepts.md)
-   [功能對應](feature-mapping.md)


計劃從 DirectX 9 到 DirectX 11 與通用 Windows 平台 (UWP) 的遊戲移植專案：升級您的圖形程式碼，並將遊戲放置於 Windows 執行階段環境中。

## <a name="plan-to-port-graphics-code"></a>計劃移植圖形程式碼


在您開始將遊戲移植到 UWP 之前，請務必確認您的遊戲不含任何來自 Direct3D 8 的保留功能。 請確定您的遊戲未殘留任何固定函式管線。 如需過時功能的完整清單 (包含固定管線功能)，請參閱[過時的功能](https://msdn.microsoft.com/library/windows/desktop/cc308047)。

從 Direct3D 9 升級到 Direct3D 11 不只是搜尋與取代的變更。 您需要知道 Direct3D 裝置、裝置上下文及圖形基礎結構之間的差異，並了解 Direct3D 9 之後的其他重要變更。 您可以藉由閱讀本節的其他主題來開始進行這個程序。

您必須使用自己的協助程式庫或社群工具來取代 D3DX 與 DXUT 協助程式庫。 如需詳細資訊，請參閱[功能對應](feature-mapping.md)一節。

> **注意：** 您可以使用[DirectX 工具組](http://go.microsoft.com/fwlink/p/?LinkID=248929)或[DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926)來取代先前由 D3DX 與 DXUT 所提供的部分功能。

 

以組合語言撰寫的著色器應該使用著色器模型 4 層級 9\_1 或 9\_3 功能升級到 HLSL，而針對效果程式庫所撰寫的著色器將需要升級到更新版本的 HLSL 語法。 如需詳細資訊，請參閱[功能對應](feature-mapping.md)一節。

熟悉不同的 [Direct3D 功能層級](https://msdn.microsoft.com/library/windows/desktop/ff476876)。 功能層級可以藉由定義已知的功能組合，將範圍廣泛的視訊硬體分類。 每一組大致上都會對應到 Direct3D 版本 (從 9.1 到 11.2)。 所有功能層級都使用 DirectX 11 API。

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>計劃將 Win32 UI 程式碼移植到 CoreWindow


UWP app 會在針對名稱為 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 的 app 容器所建立的視窗中執行。 您的遊戲可以藉由從 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 繼承以控制視窗，這樣所需的實作細節會比桌面視窗來得少。 遊戲的主迴圈將位於 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法內。

UWP app 的週期與傳統型 app 的週期有很大的差別。 您將需要經常儲存遊戲，因為在發生暫停事件時，app 能夠用來停止執行程式碼的時間有限，而且您想要確認當 app 繼續執行時，玩家能夠立即回到他們原先所在的位置。 遊戲的儲存頻率必須足以維持繼續進行遊戲之後有持續進行遊戲的使用經驗，但不要讓遊戲儲存頻率高到影響畫面播放速率或導致遊戲間斷。 當遊戲從終止狀態繼續執行時，該遊戲可能需要載入遊戲狀態。

[DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415571) 可以用來做為 D3DXMath 與 XNAMath 的替代項目，而且如果您需要數學程式庫，它就可以派上用場。 DirectXMath 含有快速的可攜式資料類型，以及已對齊且封裝來與著色器搭配使用的類型。

已將像是[連鎖 API](https://msdn.microsoft.com/library/windows/desktop/dd405529) 的原生程式庫加以延伸，以支援 ARM 內建函式。 如果遊戲使用連鎖 API，則可持續在 DirectX 11 與 UWP 中使用它們。

我們的範本和程式碼範例會使用您可能還不熟悉的 C++ 新功能。 例如，將非同步方法搭配 [**lambda expressions**](https://msdn.microsoft.com/library/windows/apps/dd293608.aspx) 用來載入 Direct3D 資源，而不需封鎖 UI 執行緒。

您通常會使用的概念有兩種：

-   Managed 參考 ([**^ 運算子**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)) 和 [**Managed 類別**](https://msdn.microsoft.com/library/windows/apps/6w96b5h7.aspx) (ref 類別) 都是 Windows 執行階段的基本部分。 您需要為含有 Windows 執行階段元件的介面使用 Managed ref 類別，例如 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) (逐步解說中將提供更多相關資訊)。
-   使用 Direct3D 11 COM 介面時，請使用 [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) 範本類型，讓 COM 指標更容易使用。

 

 




