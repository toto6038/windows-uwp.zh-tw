---
title: 遊戲和 DirectX
description: 通用 Windows 平台 (UWP) 能提供建立遊戲、發佈遊戲以及從遊戲中獲利的嶄新商機。 了解開始新的遊戲或移植現有的遊戲。
ms.assetid: 4073b835-c900-4ff2-9fc5-da52f9432a1f
---

# 遊戲和 DirectX


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

通用 Windows 平台 (UWP) 能提供建立遊戲、發佈遊戲以及從遊戲中獲利的嶄新商機。 了解開始新的遊戲或移植現有的遊戲。

| 主題 | 描述 |
|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Windows 10 遊戲開發指南](e2e.md) | 開發 UWP 遊戲的資源與資訊的端對端指南。 |
| [適用於通用 Windows 平台 app 的遊戲技術](game-development-platform-guide.md) | 在本指南中，您將了解可用於開發 UWP 遊戲的技術。 |
| [遊戲的專案範本與工具](prepare-your-dev-environment-for-windows-store-directx-game-development.md) | 示範您需要哪些項目，才能開始進行 UWP 的 DirectX 遊戲程式設計。 |
| [app 物件和 DirectX](about-the-metro-style-user-interface-and-directx.md) | 使用 DirectX 的 UWP 遊戲不會使用很多 Windows UI 使用者介面元素和物件。 相反地，由於它們在 Windows 執行階段堆疊的較低層次執行，因此必須以更基礎的方法來與使用者介面架構進行互通：方法為直接與 app 物件進行存取和互通。 了解這個互通發生的時間和方式，以及身為 DirectX 開發人員如何在開發 UWP app 時有效使用這個模型。 |
| [啟動和繼續 app](launching-and-resuming-apps-directx-and-cpp.md) | 了解如何啟動、暫停和繼續您的 UWP DirectX app。 |
| [適用於 DirectX 遊戲的 2D 圖形](working-with-2d-graphics-in-your-directx-game.md) | 我們將討論 2D 點陣圖圖形和效果的用法，以及如何在遊戲中使用它們。 |
| [適用於 DirectX 遊戲的基本 3D 圖形](an-introduction-to-3d-graphics-with-directx.md) | 我們將示範如何使用 DirectX 程式設計來實作 3D 圖形的基本概念。 |
| [在 DirectX 遊戲中載入資源](load-a-game-asset.md) | 大多數的遊戲會在某個時間點從本機存放區或一些其他資料串流中載入資源和資產 (如著色器、紋理、預先定義的網格或其他圖形資料)。 本文會引導您了解在 UWP 遊戲中載入這些檔案來使用時必須考量的整體事項。 |
| [使用 DirectX 建立簡單的 UWP 遊戲](tutorial--create-your-first-metro-style-directx-game.md) | 在這組教學課程中，您了解如何使用 DirectX 和 C++ 建立基本的 UWP 遊戲。 我們涵蓋遊戲的所有主要部分，包括載入圖案和網格等資產的程序、建立主遊戲迴圈、實作簡單轉譯管線，以及新增音效和控制項。 |
| [使用 C++ 和 DirectX 開發 Marble Maze (通用 Windows 平台遊戲)](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md) | 本節說明如何使用 DirectX 和 Visual C++ 來建立 3D UWP 遊戲。 本文件示範如何建立名為 Marble Maze 的 3D 遊戲，此遊戲支援新的板型規格 (例如平板電腦)，也可以在傳統的桌上型和膝上型電腦上執行。 |
| [支援螢幕方向](supporting-screen-rotation-directx-and-cpp.md) | 這裡將討論在 UWP DirectX app 中處理螢幕旋轉的最佳做法，以便能夠有效率及有效地使用 Windows 10 裝置的圖形硬體。 |
| [遊戲的音訊](working-with-audio-in-your-directx-game.md) | 了解如何開發音樂和聲音，並將它們納入您的 DirectX 遊戲，以及如何處理音訊訊號來建立動態和聲音定位。 |
| [適用於遊戲的觸控控制項](tutorial--adding-touch-controls-to-your-directx-game.md) | 了解如何在使用 DirectX 的 UWP C++ 遊戲中新增基本的觸控控制項。 我們將示範如何在 Direct3D 環境中加入觸控控制項來移動固定面相機，透過拖曳手指或手寫筆來移動相機的視角。 |
| [適用於遊戲的移動視角控制項](tutorial--adding-move-look-controls-to-your-directx-game.md) | 了解如何將傳統的滑鼠和鍵盤移動視角控制項 (也稱為用滑鼠視角 (mouselook) 控制項) 加入到您的 DirectX 遊戲。 |
| [最佳化輸入和轉譯迴圈](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) | 輸入延遲可能大幅影響遊戲的體驗，因此，最佳化輸入延遲可以使遊戲的感覺更完美。 此外，適當的輸入事件最佳化可以增加電池使用時間。 了解如何選擇正確的 [CoreDispatcher](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) 輸入事件處理選項，可確保您的遊戲可以盡可能順暢地處理輸入。 |
| [交換鏈結縮放和重疊](multisampling--scaling--and-overlay-swap-chains.md) | 了解如何在行動裝置上建立縮放的交換鏈結以加快轉譯速度，以及使用重疊交換鏈結 (可供使用時) 來提高視覺品質。 |
| [透過 DXGI 1.3 交換鏈結減少延遲](reduce-latency-with-dxgi-1-3-swap-chains.md) | 使用 DXGI 1.3 可減少有效的框架延遲，方法是等候交換鏈結在適當時機發出訊號來開始轉譯新畫面。 |
| [UWP app 中的多重取樣](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md) | 了解如何在以 Direct3D 建立的 UWP app 中使用多重取樣。 |
| [處理 Direct3D 11 中的裝置已移除案例](handling-device-lost-scenarios.md) | 本主題說明在移除或重新初始化圖形卡之後，應如何重建 Direct3D 與 DXGI 裝置介面鏈結。 |
| [遊戲的非同步程式設計](asynchronous-programming-directx-and-cpp.md) | 本主題包含搭配 DirectX 使用非同步程式設計與執行緒處理時應考量的各個重點。 |
| [遊戲的網路功能](work-with-networking-in-your-directx-game.md) | 了解如何在您的 DirectX 遊戲中開發與納入網路功能。 |
| [DirectX 與 XAML 互通性](directx-and-xaml-interop.md) | 您可以在 UWP 遊戲中，同時使用 Extensible Application Markup Language (XAML) 與 Microsoft DirectX。 |
| [封裝您的遊戲](package-your-windows-store-directx-game.md) | 大型 UWP 遊戲很容易會膨脹成更大型的遊戲，尤其是支援多國語言，並具有特定區域資產或功能選用高解析度資產的遊戲。 在這個主題中，您將了解如何使用 app 套件與 app 套件組合來自訂 app，讓客戶只收到他們實際所需的資源。 |
| [遊戲移植指南](porting-guides.md) | 提供將現有的遊戲移植到 Direct3D 11、UWP 和 Windows 10 的指南。 |
| [遊戲程式設計資源](additional-directx-game-programming-resources.md) | 如需 Windows 遊戲程式設計的詳細資訊，請查看下列資源。 |

 

> **注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發， 請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

若要充分利用遊戲開發概觀和教學課程，您應該熟悉下列主題：

-   Microsoft C++ 與元件延伸 (C++/CX)。 這是 Microsoft C++ 的更新內容，納入了自動參考計數，而且這也是使用 DirectX 11.1 或更新版本開發 UWP 遊戲的語言。
-   基本圖形程式設計詞彙。
-   基本 Windows 程式設計概念。
-   必須對 Direct3D 9 或 11 API 有基本的認識。

 

 






<!--HONumber=Mar16_HO1-->


