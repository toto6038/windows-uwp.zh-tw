---
author: mtoepke
title: DirectX 11 移植常見問題集
description: 將遊戲移植到通用 Windows 平台 (UWP) 的常見問題集解答。
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: 06a4c9b434afedabc17a48e9929da8dc4460fe03
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5696889"
---
# <a name="directx-11-porting-faq"></a>DirectX 11 移植常見問題集




將遊戲移植到通用 Windows 平台 (UWP) 的常見問題集解答。

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>移植遊戲的程序是在 API 方法上進行搜尋與取代的操作嗎？還是我需要規劃更詳盡的移植程序？


Direct3D 11 是 Direct3D 9 的重要升級。 當中有很多範例改變，包含供視覺化圖形介面卡與其內容使用的個別 API，以及供裝置資源使用的全新多型層。 基本上您的遊戲仍然可以用相同的方式使用圖形硬體，但您需要了解新的 Direct3D 11 API 結構並更新圖形程式碼的每個部分，以使用正確的 API 元件。 請參閱[移植概念與考量](porting-considerations.md)。

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>新裝置上下文的用途為何？ 我是否應該將 Direct3D 9 裝置更換為 Direct3D 11 裝置、更換裝置上下文或兩者都要更換？


現在 Direct3D 裝置用來建立視訊記憶體中的資源，而裝置上下文則用來設定管線狀態與產生轉譯命令。 如需詳細資訊，請參閱：[Direct3D 9 之後最重要的變更有哪些？](understand-direct3d-11-1-concepts.md)

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>我需要針對 UWP 更新我的遊戲計時器嗎？


[**QueryPerformanceCounter**](https://msdn.microsoft.com/library/windows/desktop/ms644904) 與 [**QueryPerformanceFrequency**](https://msdn.microsoft.com/library/windows/desktop/ms644905) 仍然是實作 UWP app 遊戲計時器最好的方式。

您應該注意計時器與 UWP app 週期的細微差別。 在玩家重新啟動傳統型遊戲時，暫停/繼續是不同的，因為您的遊戲 會從上次的遊戲進度即時繼續快照。 如果已經過一段長時間 (例如數個星期)，某些遊戲計時器實作可能無法正確運作。 當遊戲繼續時，您可以使用 app 週期事件來重設計時器。

仍然使用 RDTSC 指示的遊戲必須升級。 請參閱[遊戲計時與多核心處理器](https://msdn.microsoft.com/library/windows/desktop/ee417693)。

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>我的遊戲程式碼是以 D3DX 與 DXUT 為基礎。 有任何方法可幫助我移轉程式碼嗎？


[DirectX 工具組 (DirectXTK)](http://go.microsoft.com/fwlink/p/?LinkID=248929) 社群專案提供可搭配 Direct3D 11 使用的協助程式類別。

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>我要如何維護傳統型與 Microsoft Store 的程式碼路徑？


Chuck Walbourn 的系列文章[適用於遊戲的雙用途撰寫程式碼技術](http://go.microsoft.com/fwlink/p/?LinkID=286210)提供在傳統型與 Microsoft Store 的程式碼路徑間共用程式碼的指導方針。

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>如何在 DirectX UWP app 中載入影像資源？


有兩個 API 路徑可用來載入影像：

-   內容管線可將影像轉換為當作 Direct3D 紋理資源使用的 DDS 檔案。 請參閱[在您的遊戲或應用程式中使用 3D 資產](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)。
-   可使用 [Windows 影像處理元件](https://msdn.microsoft.com/library/windows/desktop/ee719902)載入各種格式的影像，並可用於 Direct2D 點陣圖與 Direct3D 紋理資源。

您也可使用 [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) 或 [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926) 中的 DDSTextureLoader 與 WICTextureLoader。

## <a name="where-is-the-directx-sdk"></a>DirectX SDK 在哪裡？


DirectX SDK 已內含在 Windows SDK 中。 不在 Windows SDK 內的最新 DirectX SDK 是在 2010 年 6 月。 Direct3D 範例位於「程式碼庫」，其中包含其他的 Windows 應用程式範例。

## <a name="what-about-directx-redistributables"></a>對於 DirectX 可轉散發檔案的做法？


Windows SDK 中的大部分元件都已包含在支援的作業系統版本內，否則就是沒有 DLL 元件 (例如 DirectXMath)。 UWP app 可使用的所有 Direct3D API 元件都已提供給您的遊戲使用；您不需要轉散發這些元件。

Win32 傳統型 app 仍然使用 DirectSetup，因此如果您也要升級遊戲的傳統版本， 請參閱[遊戲開發人員的 Direct3D 11 部署](https://msdn.microsoft.com/library/windows/desktop/ee416644)。

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>從 Effects 移轉前，有任何方法可將傳統型程式碼更新為 DirectX 11 嗎？


請參閱[適用於 Direct3D 11 更新的 Effects](http://go.microsoft.com/fwlink/p/?LinkId=271568)。 Effects 11 可協助移除傳統 DirectX SDK 標頭上的相依性；它可用於協助移植，並且只能用在傳統型應用程式上。

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>有任何方法可將 DirectX 8 遊戲移植到 UWP 嗎？


是：

-   閱讀[轉換為 Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb204851)。
-   確定您的遊戲沒有修正管線的剩餘部分 - 請參閱[過時的功能](https://msdn.microsoft.com/library/windows/desktop/cc308047)。
-   然後採取 DirectX 9 移植方法：[從 D3D 9 移植到 UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md)。

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>我可以將 DirectX 10 或 11 遊戲移植到 UWP 嗎？


DirectX 10.x 與 11 傳統型遊戲能夠輕易地移植到 UWP。 請參閱 [移轉至 Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476190)。

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>如何在多個監視器系統中選擇正確的顯示裝置？


使用者會選擇要顯示應用程式的監視器。 呼叫 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)，並將第一個參數設為 **nullptr**，讓 Windows 提供正確的介面卡。 然後取得裝置的 [**IDXGIDevice interface**](https://msdn.microsoft.com/library/windows/desktop/bb174527)，呼叫 [**GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) 並使用 DXGI 介面卡建立交換鏈結。

## <a name="how-do-i-turn-on-antialiasing"></a>如何開啟消除鋸齒？


當您建立 Direct3D 裝置時，即會啟用消除鋸齒 (多重取樣)。 呼叫 [**CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499) 列舉多重取樣支援，然後在呼叫 [**CreateSurface**](https://msdn.microsoft.com/library/windows/desktop/bb174530) 時於 [**DXGI\_SAMPLE\_DESC structure**](https://msdn.microsoft.com/library/windows/desktop/bb173072) 設定多重取樣選項。

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>我的遊戲使用多執行緒及/或延遲轉譯進行轉譯。 我需要知道的 Direct3D 11 事項有哪些？


請造訪 [Direct3D 11 中的多執行緒簡介](https://msdn.microsoft.com/library/windows/desktop/ff476891)開始進行。 如需主要差異的清單，請參閱 [Direct3D 版本間的執行緒差異](https://msdn.microsoft.com/library/windows/desktop/ff476890)。 請注意，延遲的轉譯使用裝置 *「延遲的內容」*，而不使用 *「即時內容」*。

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>哪裡可以閱讀更多有關 Direct3D 9 之後的可程式設計管線？


請瀏覽下列主題：

-   [HLSL 的程式設計指南](https://msdn.microsoft.com/library/windows/desktop/bb509635)
-   [Direct3D 10 常見問題集](https://msdn.microsoft.com/library/windows/desktop/ee416643)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>我的模型應該使用哪種格式代替 .x 檔案格式？


雖然我們並未提供 .x 檔案格式的正式替代格式，但是許多範例都使用 SDKMesh 格式。 Visual Studio 也有[內容管線](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)，可將數個常用格式編譯至 CMO 檔案，並可使用 Visual Studio 3D 初學者套件的程式碼或使用 [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) 載入。

## <a name="how-do-i-debug-my-shaders"></a>如何偵錯著色器？


Microsoft Visual Studio2015 包含 DirectX 圖形診斷工具。 請參閱[偵錯 DirectX 圖形](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx)。

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>*x* 函式的 Direct3D 11 對應功能是什麼？


請參閱＜將 DirectX 9 功能對應到 DirectX 11 API＞中的[函式對應](feature-mapping.md#function-mapping)。

##  <a name="what-is-the-dxgiformat-equivalent-of-y-surface-format"></a>*y* 表面格式的 DXGI\_FORMAT 對應功能是什麼？


請參閱＜將 DirectX 9 功能對應到 DirectX 11 API＞中的[表面格式對應](feature-mapping.md#surface-format-mapping)。

 

 




