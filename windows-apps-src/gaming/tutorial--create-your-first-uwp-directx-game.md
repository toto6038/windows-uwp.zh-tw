---
title: 建立 DirectX 通用 Windows 平台 (UWP) 遊戲
description: 在這組教學課程中，您將瞭解如何使用 DirectX 和 [c + +/WinRT](../cpp-and-winrt-apis/index.md) 來建立基本通用 WINDOWS 平臺 (UWP) 名為 **Simple3DGameDX**的範例遊戲。
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX 範例遊戲，範例遊戲，通用 Windows 平臺 (UWP) ，Direct3D 11 遊戲
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 284aa821cc58a49f45bed3b0d7e28c20f9d19ba1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163022"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>使用 DirectX 建立簡單的通用 Windows 平台 (UWP) 遊戲

在這組教學課程中，您將瞭解如何使用 DirectX 和 [c + +/WinRT](../cpp-and-winrt-apis/index.md) 來建立基本通用 WINDOWS 平臺 (UWP) 名為 **Simple3DGameDX**的範例遊戲。 遊戲會在簡單的第一個第一員的第一次製作資源庫中進行。

> [!NOTE]
> 您可以從這裡下載 **Simple3DGameDX** 範例遊戲本身的連結是 [Direct3D 範例遊戲](/samples/microsoft/windows-universal-samples/simple3dgamedx/)。 C + +/WinRT 來源程式碼位於名為的資料夾中 `cppwinrt` 。 如需其他 UWP 範例應用程式的詳細資訊，請參閱 [取得 uwp 應用程式範例](../get-started/get-app-samples.md)。

這些教學課程涵蓋遊戲的所有主要部分，包括載入像是藝術和網格等資產的程式、建立主要遊戲迴圈、實行簡單的轉譯管線，以及新增音效和控制項。

您也會看到 UWP 遊戲開發技巧和考慮。 我們將著重于重要的 UWP DirectX 遊戲開發概念，並針對這些概念呼叫 Windows 執行時間專屬的考慮。

## <a name="objective"></a>目標

瞭解 UWP DirectX 遊戲的基本概念和元件，並更熟悉使用 DirectX 設計 UWP 遊戲。

## <a name="what-you-need-to-know"></a>您必須知道的事項

在本教學課程中，您必須熟悉這些主題。

- [C + +/WinRT](../cpp-and-winrt-apis/index.md)。 C + +/WinRT 是標準的現代 c + + 17 語言投射，適用于 Windows 執行階段 (WinRT) Api，實作為以標頭檔案為基礎的程式庫，並設計為提供您最先進的 Windows Api 存取權。
- 基本線性代數和牛頓物理概念。
- 基本圖形程式設計詞彙。
- 基本 Windows 程式設計概念。
- 必須對 [Direct2D](/windows/desktop/Direct2D/direct2d-portal) 和 [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11) API 有基本的認識。

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Direct3D UWP 診斷資源庫範例

**Simple3DGameDX**範例遊戲會實行簡單的第一個第一次的3d 拍攝資源庫，讓播放者在移動目標上引發球。 擊中每個目標就會得到特定的分數，而且玩家可以晉級 6 個關卡，逐漸增加挑戰難度。 等級結束後，會計算得分，並提供玩家最後的分數。

此範例會示範這些遊戲概念。

- DirectX 11.1 與 Windows 執行階段的交互操作
- 第一人稱 3D 遠近景深和相機
- 3D 立體視覺效果
- 衝突-3D 中的物件之間的偵測
- 處理滑鼠、觸控以及 Xbox 控制器控制項的使用者輸入
- 音訊混合和播放
- 基本遊戲狀態-電腦

![作用中的遊戲範例](images/simple-dx-game-overview.png)

|主題|說明|
|-------|-------------|
|[設定遊戲專案](tutorial--setting-up-the-games-infrastructure.md)|開發遊戲的第一個步驟是在 Microsoft Visual Studio 中設定專案。 當您設定專門用於遊戲開發的專案之後，您稍後可以將其重新使用為一種範本。|
|[定義遊戲的 UWP 應用程式架構](tutorial--building-the-games-uwp-app-framework.md)|編寫通用 Windows 平臺 (UWP) 遊戲的第一步，是建立可讓應用程式物件與 Windows 互動的架構。|
|[管理遊戲流程](tutorial-game-flow-management.md)|定義高階狀態電腦，讓玩家與系統互動。 了解 UI 如何與整體遊戲的狀態電腦互動，以及如何建立 UWP 遊戲事件處理常式。|
|[定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)|現在，我們來看看範例遊戲的主要物件的詳細資料，以及它所實行的規則如何轉譯成與遊戲世界的互動。|
|[轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)|瞭解如何開發呈現管線來顯示圖形。 轉譯簡介。|
|[轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)|了解如何組合轉譯管線來顯示圖形。 遊戲轉譯、設定及準備資料。|
|[新增使用者介面](tutorial--adding-a-user-interface.md)|瞭解如何將2D 使用者介面重迭新增至 DirectX UWP 遊戲。|
|[新增控制項](tutorial--adding-controls.md)|現在，我們來看看範例遊戲如何在3-d 遊戲中實行移動外觀控制項，以及如何開發基本的觸控、滑鼠和遊戲控制器控制項。|
|[加入聲音](tutorial--adding-sound.md)|使用 XAudio2 Api 開發簡單的音效引擎來播放遊戲音樂和音效效果。|
|[延伸遊戲範例](tutorial-resources.md)|了解如何實作 UWP DirectX 遊戲的 XAML 重疊。|