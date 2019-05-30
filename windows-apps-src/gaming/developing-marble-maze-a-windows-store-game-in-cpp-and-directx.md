---
title: 使用 C++ 和 DirectX 開發 Marble Maze (UWP 遊戲)
description: 本節說明如何使用 DirectX 和 Visual C++ 來建立 3D 通用 Windows 平台 (UWP) 遊戲。
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 範例, DirectX, 3D
ms.localizationpriority: medium
ms.openlocfilehash: f287110584c02a5d7822f6b1d84cd332d4d6199d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368908"
---
# <a name="developing-marble-maze-a-uwp-game-in-c-and-directx"></a>使用 C++ 和 DirectX 開發 Marble Maze (UWP 遊戲)




本主題說明如何使用 DirectX 和 Visual C++ 來建立 3D 通用 Windows 平台 (UWP) 遊戲。 名為 Marble Maze 的遊戲適用於多種裝置，例如平板電腦及傳統的桌上型電腦與筆記型電腦。

> [!NOTE]
> 若要下載 Marble Maze 原始程式碼。請參閱[GitHub 上的範例](https://go.microsoft.com/fwlink/?LinkId=624011) (英文)。

> [!IMPORTANT]
> Marble Maze 說明的設計模式是我們認為建立 UWP 遊戲的最佳做法。 您可以調整許多實作細節，以符合您自己的做法和您所開發的遊戲的獨特需求。 請自行選擇使用最符合需求的各種技術或程式庫。 (不過，務必確定您的程式碼會傳遞[Windows 應用程式認證套件](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)。)當我們考慮實作是成功的遊戲開發關鍵這裡使用時，我們會強調這份文件中。

 

## <a name="introducing-marble-maze"></a>介紹 Marble Maze


我們選擇 Marble Maze 因為它相對基本，但仍展現大多數遊戲中的各種功能。 它示範如何使用圖形、輸入處理和音訊。 它也展現規則和目標等遊戲機制。

Marble Maze 類似桌面迷宮遊戲，這種遊戲通常是用盒子做成，盒子上有球洞及鋼珠或玻璃珠。 Marble Maze 的目標與桌面迷宮遊戲相同：藉由傾斜迷宮，讓彈珠在最短時間內從迷宮入口滾到出口，但不能讓彈珠掉到任何洞裡。 Marble Maze 增加了關卡的概念。 如果彈珠掉進洞裡，遊戲會從彈珠上次通過的關卡位置重新開始。

Marble Maze 提供許多方式讓使用者與遊戲板互動。 如果您有觸控功能或加速計功能的裝置，則可以利用這些裝置來移動遊戲板。 您也可以使用 Xbox One 控制器或滑鼠來控制遊戲玩法。

![Marble Maze 遊戲的螢幕擷取畫面。](images/marblemaze-2.png)

## <a name="prerequisites"></a>先決條件


-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://www.visualstudio.com/downloads/)
-   C++ 程式設計知識
-   認識 DirectX 和 DirectX 詞彙
-   COM 基本知識

## <a name="who-should-read-this"></a>本文件的對象


如果您想要建立 3D 遊戲或其他使用大量圖形的應用程式，適用於 Windows 10，這很適合您。 我們希望您善用本文件概述的原則和做法，來建立您自己的 UWP 遊戲。 如果您具備 C++ 和 DirectX 程式設計背景或有強烈的興趣，將可從這份文件中得到最多的收穫。 即使您沒有使用 DirectX 的經驗，只要有使用過類似的 3D 圖形程式設計環境的經驗，仍然可以受益。

[逐步解說：使用 DirectX 建立簡單的 UWP 遊戲](tutorial--create-your-first-uwp-directx-game.md)文件說明另一個範例，該範例使用 DirectX 和 C++ 來實作基本的 3D 射擊遊戲。

## <a name="what-this-documentation-covers"></a>本文件涵蓋的內容


本文件教導您如何：

-   使用 Windows 執行階段 API 和 DirectX 來建立 UWP 遊戲。
-   使用 [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) 和 [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) 來處理視覺化內容，例如模型、紋理、頂點和像素著色器，以及 2D 重疊。
-   整合輸入機制，例如觸控、加速計及 Xbox One 控制器。
-   使用 [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) 納入音樂和音效。

## <a name="what-this-documentation-does-not-cover"></a>本文件未涵蓋的內容


本文件不包含下列遊戲開發層面。 這些層面會由其他與之相關的資源說明。

-   3D 遊戲設計原則。
-   C++ 或 DirectX 程式設計基礎。
-   如何設計資源，例如紋理、模型或音訊。
-   如何對遊戲中發生的行為或效能問題進行疑難排解。
-   如何讓遊戲可在全球其他區域使用。
-   如何讓遊戲通過認證並發佈到 Microsoft Store。

Marble Maze 也會使用 [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) 程式庫來處理 3D 幾何和執行物理運算，例如碰撞。 本節不深入探討 DirectXMath。 如需 Marble Maze 如何使用 DirectXMath 的詳細資訊，請參閱原始程式碼。

雖然 Marble Maze 提供許多可重複使用的元件，但這並非完整的遊戲開發架構。 當我們認為某個 Marble Maze 元件可在遊戲中重複使用時，會在文件中加以強調。

## <a name="next-steps"></a>後續步驟


建議您從 [Marble Maze 範例基礎觀念](marble-maze-sample-fundamentals.md)入門，以了解 Marble Maze 結構，以及 Marble Maze 原始程式碼所遵循的一些編碼和樣式指導方針。 下表概述本節提及的文件，方便您參考。

## <a name="in-this-section"></a>本節內容


| 標題                                                                                                                    | 描述                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [滾珠迷宮範例基本概念](marble-maze-sample-fundamentals.md)                                                   | 提供遊戲結構的概觀，以及原始程式碼所遵循的一些編碼和樣式指導方針。                                                                                                                                 |
| [滾珠迷宮應用程式結構](marble-maze-application-structure.md)                                               | 說明如何建構 Marble Maze 應用程式的程式碼，以及 DirectX UWP App 的結構與傳統型應用程式的結構有何不同。                                                                                    |
| [將視覺內容加入至滾珠迷宮範例](adding-visual-content-to-the-marble-maze-sample.md)                   | 說明您在使用 Direct3D 和 Direct2D 時應該牢記的一些重要做法。 另外，說明 Marble Maze 如何在視覺化內容上運用這些做法。                                                                           |
| [加入輸入和互動功能至滾珠迷宮範例](adding-input-and-interactivity-to-the-marble-maze-sample.md) | 說明 Marble Maze 如何使用加速計、觸控及 Xbox One 控制器輸入，讓使用者能夠瀏覽功能表並與遊戲板互動。 另外，描述您在處理輸入時應該牢記的一些最佳做法。 |
| [將音訊加入至滾珠迷宮範例](adding-audio-to-the-marble-maze-sample.md)                                     | 說明 Marble Maze 如何利用音訊將音樂和音效加到遊戲體驗中。                                                                                                                                                  |

 

 

 




