---
title: 開發*Marble Maze* &mdash; 以 c + + for DirectX 建立的大理石迷宮 a 通用 Windows 平臺（UWP）遊戲
description: 檔集的這一節將說明如何使用 DirectX 和 c + + 來建立3D 通用 Windows 平臺（UWP）遊戲。
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 範例, DirectX, 3D
ms.localizationpriority: medium
ms.openlocfilehash: d2c0c630090c178a54a0452ab3cc430ffee4a176
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409497"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>開發*Marble Maze* &mdash; 以 c + + for DirectX 建立的大理石迷宮 a 通用 Windows 平臺（UWP）遊戲

本主題說明如何使用 DirectX 和 c + + 來建立3D 通用 Windows 平臺（UWP）遊戲。 這個遊戲稱為「*大理石迷宮*」，它會採用多種外型規格，例如平板電腦、傳統桌上型電腦和膝上型電腦。

> [!NOTE]
> 若要下載*大理石迷宮*原始碼，請參閱[GitHub 上的範例](https://github.com/microsoft/Windows-appsample-marble-maze)。

> [!IMPORTANT]
> *大理石迷宮*說明的設計模式，是我們認為是建立 UWP 遊戲的最佳做法。 您可以調整許多實作細節，以符合您自己的做法和您所開發的遊戲的獨特需求。 請自行選擇使用最符合需求的各種技術或程式庫。 (不過，一定要確定您的程式碼通過 [Windows 應用程式認證套件](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit))。當我們認為某個實作是成功開發遊戲的關鍵時，我們會在此文件中強調。

## <a name="introducing-marble-maze"></a>*大理石迷宮*簡介

我們選擇了*大理石迷宮*，因為它是相當基本的，但仍會示範大部分遊戲中所發現的功能廣度。 它示範如何使用圖形、輸入處理和音訊。 它也展現規則和目標等遊戲機制。

*大理石迷宮*類似于 [資料表-top labyrinth] 遊戲，通常是從包含孔和鋼或半透明大理石的方塊來建立。 *大理石迷宮*的目標與第一版的資料表相同：傾斜迷宮可在最少的時間內，從一開始到迷宮的結尾處引導大理石，而不會讓大理石落在任何洞上。 *大理石迷宮*會加入檢查點的概念。 如果彈珠掉進洞裡，遊戲會從彈珠上次通過的關卡位置重新開始。

*大理石迷宮*提供多種方式讓使用者與遊戲面板互動。 如果您有觸控功能或加速計功能的裝置，則可以利用這些裝置來移動遊戲板。 您也可以使用 Xbox One 控制器或滑鼠來控制遊戲玩法。

![Marble Maze 遊戲的螢幕擷取畫面。](images/marblemaze-2.png)

## <a name="prerequisites"></a>必要條件

-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   C++ 程式設計知識
-   認識 DirectX 和 DirectX 詞彙
-   COM 基本知識

## <a name="who-should-read-this"></a>哪些人應閱讀本文？

如果您想要建立適用于 Windows 10 的3D 遊戲或其他圖形密集型應用程式，這就是您的需求。 我們希望您善用本文件概述的原則和做法，來建立您自己的 UWP 遊戲。 如果您具備 C++ 和 DirectX 程式設計背景或有強烈的興趣，將可從這份文件中得到最多的收穫。 即使您沒有使用 DirectX 的經驗，只要有使用過類似的 3D 圖形程式設計環境的經驗，仍然可以受益。

[逐步解說：使用 DirectX 建立簡單的 UWP 遊戲](tutorial--create-your-first-uwp-directx-game.md)文件說明另一個範例，該範例使用 DirectX 和 C++ 來實作基本的 3D 射擊遊戲。

## <a name="what-this-documentation-covers"></a>本文件涵蓋的內容

本文件教導您如何：

-   使用 Windows 執行階段 API 和 DirectX 來建立 UWP 遊戲。
-   使用[Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)和[Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal)來處理視覺內容，例如模型、紋理、頂點和圖元著色器，以及2d 重迭。
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

*大理石迷宮*也會使用[DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)程式庫來處理3d 幾何，並執行物理計算，例如衝突。 本節不深入探討 DirectXMath。 如需有關*大理石迷宮*如何使用 DirectXMath 的詳細資訊，請參閱原始程式碼。

雖然*大理石迷宮*提供許多可重複使用的元件，但它並不是完整的遊戲開發架構。 當我們考慮讓*大理石迷宮*元件可在遊戲中重複使用時，我們會在檔中強調。

## <a name="next-steps"></a>後續步驟

我們建議您從大理石迷宮[範例基本](marble-maze-sample-fundamentals.md)概念開始，以瞭解大理石*迷宮*結構以及*大理石迷宮*原始程式碼遵循的一些編碼和樣式指導方針。 下表概述本節提及的文件，方便您參考。

## <a name="in-this-section"></a>本節內容

| Title                                                                                                                    | 描述                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Marble Maze 範例基礎觀念](marble-maze-sample-fundamentals.md)                                                   | 提供遊戲結構的概觀，以及原始程式碼所遵循的一些編碼和樣式指導方針。                                                                                                                                 |
| [Marble Maze 應用程式結構](marble-maze-application-structure.md)                                               | 描述*大理石迷宮*應用程式代碼的結構，以及 DirectX UWP 應用程式的結構與傳統桌面應用程式的不同之處。                                                                                    |
| [在 Marble Maze 範例中加入視覺化內容](adding-visual-content-to-the-marble-maze-sample.md)                   | 說明您在使用 Direct3D 和 Direct2D 時應該牢記的一些重要做法。 同時描述*大理石迷宮*如何套用這些適用于視覺內容的做法。                                                                           |
| [在 Marble Maze 範例中加入輸入和互動](adding-input-and-interactivity-to-the-marble-maze-sample.md) | 描述*大理石迷宮*如何與加速計、觸控和 Xbox one 控制器輸入搭配運作，讓使用者可以流覽功能表並與遊戲面板互動。 另外，描述您在處理輸入時應該牢記的一些最佳做法。 |
| [在 Marble Maze 範例中加入音訊](adding-audio-to-the-marble-maze-sample.md)                                     | 描述*大理石迷宮*如何與音訊搭配使用，以將音樂和音效效果新增至遊戲體驗。                                                                                                                                                  |
