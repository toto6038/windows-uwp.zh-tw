---
title: 建立 DirectX 通用 Windows 平台 (UWP) 遊戲
description: 在這組教學課程中，您會了解如何使用 DirectX 和 C++ 建立基本的通用 Windows 平台 (UWP) 遊戲。
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX 遊戲範例, 遊戲範例, 通用 Windows 平台 (UWP), Direct3D 11 遊戲
ms.date: 12/01/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dc602e2dd29231c1e6554d7ef55e9666a373fa31
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8326048"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>使用 DirectX 建立簡單的通用 Windows 平台 (UWP) 遊戲

在這組教學課程中，您會了解如何使用 DirectX 和 C++ 建立基本的通用 Windows 平台 (UWP) 遊戲。 我們涵蓋遊戲的所有主要部分，包括載入圖案和網格等資產的程序、建立主遊戲迴圈、實作簡單轉譯管線，以及新增音效和控制項。

我們為您說明 UWP 遊戲開發技術和考量。 我們沒有提供完整流程的遊戲。 而是將焦點放在開發 UWP DirectX 遊戲的重要概念，並根據這些概念提出 Windows 執行階段的具體考量。

## <a name="objective"></a>目標

使用 UWP DirectX 遊戲的基本概念和元件，並且能夠更得心應手地設計使用 DirectX 的 UWP 遊戲。

## <a name="what-you-need-to-know-before-starting"></a>開始前您必須知道的事


在我們開始這個教學課程前，您應該熟悉 下列主題。

-   Microsoft C++ 與 Windows 執行階段語言擴充功能 (C++/CX)。 這是 Microsoft C++ 的更新內容，納入了自動參考計數，而且這也是使用 DirectX 11.1 或更新版本 開發 UWP 遊戲的語言。
-   基本線性代數和牛頓物理概念。
-   基本圖形程式設計詞彙。
-   基本 Windows 程式設計概念。
-   必須對 [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) 和 [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569) API 有基本的認識。

##  <a name="direct3d-uwp-shooting-game-sample"></a>Direct3D UWP 射擊遊戲範例


這個範例實作簡單的第一人稱射擊場，玩家要用球射擊移動的目標。 擊中每個目標就會得到特定的分數，而且玩家可以晉級 6 個關卡，逐漸增加挑戰難度。 關卡結束後，會記錄得分，並提供玩家最後的分數。

範例示範的遊戲概念包括：

-   DirectX 11.1 與 Windows 執行階段的互通性
-   第一人稱 3D 遠近景深和相機
-   3D 立體視覺效果
-   3D 中物件之間的衝突偵測
-   處理滑鼠、觸控以及 Xbox 控制器控制項的使用者輸入
-   音訊混合和播放
-   基本的遊戲狀態電腦

![進行中的遊戲範例](images/simple-dx-game-overview.png)

| 主題 | 說明 |
|-------|-------------|
|[設定遊戲專案](tutorial--setting-up-the-games-infrastructure.md) | 組合遊戲的第一步是在 Microsoft Visual Studio 中設定一個專案，透過這種方式可以將所需的程式碼基礎結構數量減到最少。 使用正確的範本並設定專用於遊戲開發的專案，可以為您節省很多時間和麻煩。 我們會逐步引導您如何準備和設定簡單的遊戲專案。 |
| [定義遊戲的 UWP app 架構](tutorial--building-the-games-uwp-app-framework.md) | 建置一個可讓 UWP DirectX 遊戲物件與 Windows 互動的架構。 這包括下列 Windows 執行階段屬性：暫停/繼續事件處理、視窗焦點以及貼齊。  |
| [管理遊戲流程](tutorial-game-flow-management.md) | 定義高階狀態電腦，讓玩家與系統互動。 了解 UI 如何與整體遊戲的狀態電腦互動，以及如何建立 UWP 遊戲事件處理常式。 |
| [定義主要遊戲物件](tutorial--defining-the-main-game-loop.md) | 定義遊戲如何透過建立規則來進行。 |
| [轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md) | 組合轉譯架構以顯示圖形。 本主題分為兩個部分。 轉譯簡介說明如何在螢幕上顯示顯示器的場景物件。 |
| [轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md) | 在轉譯主題的第二個部分中，了解如何準備發生轉譯之前所需的資料。 |
| [新增使用者介面](tutorial--adding-a-user-interface.md) | 新增簡易選單選項，提醒顯示元件，然後提供意見反應給玩家。 |
| [新增控制項](tutorial--adding-controls.md) | 新增移動視角控制項到遊戲&mdash;基本觸控、滑鼠及遊戲控制器控制項。 |
| [新增音效](tutorial--adding-sound.md) | 了解如何使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) API 建立遊戲的音效。 |
| [延伸遊戲範例](tutorial-resources.md) | 使您進一步擁有 DirectX 遊戲開發的知識的資源，包括使用 XAML 建立重疊。 |