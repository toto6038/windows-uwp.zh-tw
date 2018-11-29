---
title: DirectX 遊戲專案範本
description: 了解建立通用 Windows 平台 (UWP) 和 DirectX 遊戲的範本。
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, directx, 範本
ms.localizationpriority: medium
ms.openlocfilehash: 9a4491fe9a3bb97a73652c40a2968f2f53c377b5
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8215436"
---
# <a name="directx-game-project-templates"></a>DirectX 遊戲專案範本



DirectX 與通用 Windows 平台 (UWP) 範本可讓您快速建立做為遊戲起點的專案。

## <a name="prerequisites"></a>先決條件


若要建立專案，您需要：

-   [下載 Microsoft Visual Studio2015](https://www.visualstudio.com/vs-2015-product-editions)。 視覺 Studio2015 具備圖形程式設計，例如偵錯工具的工具。 如需 DirectX 圖形與遊戲功能及工具的概觀，請參閱[適用於 DirectX 遊戲開發的 Visual Studio 工具](set-up-visual-studio-for-game-development.md)。

## <a name="choosing-a-template"></a>選擇範本


視覺 Studio2015 包含三個 DirectX 和 UWP 範本：

-   DirectX 11 應用程式 (通用 Windows) - DirectX 11 應用程式 (通用 Windows) 範本會建立使用 DirectX 11 直接轉譯為應用程式視窗的 UWP 專案。
-   DirectX 12 應用程式 (通用 Windows) - DirectX 12 應用程式 (通用 Windows) 範本會建立使用 DirectX 12 直接轉譯為應用程式視窗的 UWP 專案。
-   DirectX 11 和 XAML App (通用 Windows) - DirectX 11 和 XAML App (通用 Windows) 範本會建立使用 DirectX 11 在 XAML 控制項內轉譯的 UWP 專案。 此範本採用 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)，因此您可以使用 XAML UI 控制項。 這讓新增使用者介面元素變得更容易，但是使用 XAML 範本可能會導致 效能變差。

您可以根據效能和想要使用的技術來選擇範本。

## <a name="template-structure"></a>範本結構


DirectX 通用 Windows 範本包含下列檔案：

-   pch.h 和 pch.cpp - 預先編譯的標頭支援。
-   Package.appxmanifest - app 部署套件的屬性。
-   \*.pfx - 應用程式的憑證。
-   外部相依性 - 專案所使用之外部檔案的連結。
-   \*Main.h 和 \*Main.cpp - 用來管理應用程式資產、更新應用程式狀態和轉譯畫面的方法。
-   App.h 和 App.cpp - 應用程式的主要進入點。 連接 App 與 Windows 殼層，並處理應用程式週期事件。 這些檔案只會出現在 DirectX 11 應用程式 (通用 Windows) 和 DirectX 12 應用程式 (通用 Windows) 範本中。
-   App.xaml、App.xaml.cpp 和 App.xaml.h - 應用程式的主要進入點。 連接 App 與 Windows 殼層，並處理應用程式週期事件。 這些檔案只會出現在 DirectX 11 和 XAML 應用程式 (通用 Windows) 範本中。
-   DirectXPage.xaml、DirectXPage.xaml.cpp 和 DirectXPage.xaml.h - 裝載 DirectX SwapChainPanel 的頁面。 這些檔案只會出現在 DirectX 11 和 XAML 應用程式 (通用 Windows) 範本中。
-   內容
    -   Sample3DSceneRenderer.h 和 Sample3DSceneRenderer.cpp - 具現化基本轉譯管線的範例轉譯器。
    -   SampleFpsTextRenderer.h 和 SampleFpsTextRenderer.cpp - 使用 Direct2D 和 DirectWrite 轉譯螢幕右下角目前的 FPS 值。 這些檔案只會出現在 DirectX 11 應用程式 (通用 Windows) 和 DirectX 11 與 XAML 應用程式 (通用 Windows) 範本中。
    -   SamplePixelShader.hlsl - 簡單範例像素著色器。
    -   SampleVertexShader.hlsl - 簡單範例頂點著色器。
    -   ShaderStructures.h - 用來將日期傳送到範例頂點著色器的結構。
-   通用
    -   StepTimer.h - 動畫和模擬計時的協助程式類別。
    -   DirectXHelper.h - 其他 Helper 函式。
    -   DeviceResources.h 和 Device Resources.cpp - 提供一個介面，供所擁有的 DeviceResources 要被通知裝置即將遺失或建立的應用程式使用。
    -   d3dx12.h - 包含 D3DX12 公用程式庫。 此檔案只會出現在 DirectX 12 應用程式 (通用 Windows) 中。
-   資產 - 應用程式所使用的標誌和 Splashscreen 影像。

## <a name="next-steps"></a>後續步驟


現在您已經有了起點，可以將它納入您的遊戲開發知識及 Microsoft Store 遊戲開發技能。

如果您要移植現有的遊戲，請參閱以下主題。

-   [從 OpenGL ES 2.0 移植到 Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [從 DirectX 9 移植到通用 Windows 平台](porting-your-directx-9-game-to-windows-store.md)

如果您要建立新的 DirectX 遊戲，請參閱以下主題。

-   [使用 DirectX 建立簡單的 UWP 遊戲](tutorial--create-your-first-uwp-directx-game.md)
-   [使用 C++ 和 DirectX 開發 Marble Maze (通用 Windows 平台遊戲)](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)