---
title: 圖形診斷工具
description: 了解如何取得與使用圖形診斷功能，包括 Visual Studio 中的圖形偵錯、圖形畫面格分析，以及 GPU 使用量。
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.date: 11/20/2019
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 圖形, 診斷工具, directx
ms.localizationpriority: medium
ms.openlocfilehash: 14711a356f00ac027bf990554c90547017b9cc4d
ms.sourcegitcommit: f464e5157ca22cfe31075f2f1219b8227587bf03
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263092"
---
# <a name="graphics-diagnostics-tools"></a>圖形診斷工具

圖形診斷工具可從 Windows 10 內以選用功能的形式提供。 若要使用圖形診斷功能（在執行時間和 Visual Studio 中提供）來開發 DirectX 應用程式或遊戲，請安裝選用的圖形工具功能。

1. 移至 [**設定**] > **應用程式** > **應用程式 & 功能/選用功能**。
2. 如果 [**已安裝的功能**] 底下已列出 [**圖形工具**]，則您已經完成。 否則，請按一下 [**新增功能**]。
3. 搜尋和/或選取 [**圖形工具**]，然後按一下 [**安裝**]。

圖形診斷功能包括在 DirectX 執行階段中建立 Direct3D 偵錯裝置 (透過 Direct3D SDK 層) 的能力，再加上圖形偵錯、圖形畫面格分析與 GPU 使用量。

-   圖形偵錯可讓您追蹤您應用程式所進行的 Direct3D 呼叫。 然後您就能重新執行這些呼叫、檢查參數、偵錯與實驗著色器，並且將圖形資產視覺化，以診斷轉譯問題。 您可以在 Windows 電腦、模擬器或裝置上拍照，然後在不同的硬體上播放記錄。
-   Visual Studio 中的圖形畫面格分析會在圖形偵錯工具記錄檔上執行，並收集 Direct3D 繪製呼叫的基準計時。 然後它會藉由修改各種圖形設定來執行一組實驗，並產生計時結果的資料表。 您可以使用此資料了解您應用程式中的圖形效能問題並檢閱各項實驗的結果，以找出可能改善效能的環節。
-   Visual Studio 中的 GPU 使用量可讓您即時監視 GPU 使用狀況。 它會收集並分析 CPU 和 GPU 所處理之工作負載的計時資料，讓您可以判斷任何瓶頸所在的位置。

## <a name="related-topics"></a>相關主題

[Visual Studio 中的圖形診斷總覽](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)