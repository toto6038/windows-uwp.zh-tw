---
title: 非同步程式設計 (DirectX 和 C++)
description: 本主題包含搭配 DirectX 使用非同步程式設計與執行緒處理時應考量的各個重點。
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 非同步程式設計, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 8551a49512d4b17ab1bab704596d9e5389de3eb6
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8924854"
---
# <a name="asynchronous-programming-directx-and-c"></a>非同步程式設計 (DirectX 和 C++)



本主題包含搭配 DirectX 使用非同步程式設計與執行緒處理時應考量的各個重點。

## <a name="async-programming-and-directx"></a>非同步程式設計與 DirectX


如果您正在學習 DirectX，或者即使您已經有 DirectX 使用經驗，可以考慮將所有圖形處理管線放在一個執行緒中。 遊戲的任何指定場景中，都有常見的資源，例如點陣圖、著色器，以及需要獨佔存取的其他資產。 這些相同的資源需要您同步處理對平行執行緒間這些資源的任何存取。 轉譯很難在多個執行緒中平行處理。

不過，如果您的遊戲夠複雜，或者如果您希望改善效能，則可以使用非同步程式設計，平行處理某些非轉譯管線特定的元件。 現代的硬體都具備多核心與超執行緒的 CPU 功能，您的應用程式應該加以妥善利用！ 您可以針對不需要直接存取 Direct3D 裝置上下文的某些遊戲元件使用非同步程式設計，來確保達到這個目標，例如：

-   檔案 I/O
-   物理特性
-   AI
-   網路功能
-   音訊
-   控制項
-   XAML UI 元件

您的 App 可以在多個並行執行緒上處理這些元件。 檔案 I/O (特別是資產載入) 可從非同步載入中獲益甚大，因為您的遊戲或 app 可以保持互動狀態，並同時載入或串流處理數 MB (或數百 MB) 的資產。 建立與管理這些執行緒最簡單的方式是使用[平行模式程式庫](https://msdn.microsoft.com/library/dd492418.aspx)和 **task** 模式，如同 PPLTasks.h 中定義之 **concurrency** 命名空間中所包含的。 使用[平行模式程式庫](https://msdn.microsoft.com/library/dd492418.aspx)可以直接利用多核心與超執行緒 CPU，也可以改善因大量 CPU 計算或網路處理所造成的感覺載入時間過長及延遲等狀況。

> **注意：** 在通用 Windows 平台 (UWP) 應用程式中，使用者介面完全在單一執行緒 apartment (STA) 中執行。 如果您是為使用 [XAML 互通性](directx-and-xaml-interop.md)的 DirectX 遊戲建立 UI，則只能使用 STA 來存取控制項。

 

## <a name="multithreading-with-direct3d-devices"></a>使用 Direct3D 裝置的多執行緒


只有支援 Direct3D 功能層級 11\_0 或以上的圖形裝置才能使用裝置上下文的多執行緒。 不過，您可能希望在許多平台 (例如專用的遊戲平台) 上充分使用功能強大的 GPU。 在最簡單的情況下，您可能希望將抬頭顯示器 (HUD) 資訊圖表的轉譯與 3D 場景轉譯及投影區隔開來，而且讓兩個元件都使用個別平行的管線。 兩個執行緒都必須使用相同的 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 來建立與管理資源物件 (紋理、網格、著色器及其他資產)，不過，它是單一執行緒的，且需要實作某種同步處理機制 (例如重要區段) 才能安全地加以存取。 此外，當您可以在不同執行緒上 (針對延遲轉譯) 為裝置上下文建立個別的命令清單時，無法在同一個 **ID3D11DeviceContext** 執行個體上同時播放那些命令清單。

現在，您的 app 也可以使用 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (對多執行緒處理是安全的) 來建立資源物件。 所以，何不一律使用 **ID3D11Device** 而非 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 呢？ 沒錯，目前而言，有些圖形介面可能還無法使用多執行緒的驅動程式支援。 您可以查詢裝置並查看它是否支援多執行緒，不過，如果您希望觸及最廣泛的對象，最好還是使用單一執行緒的 **ID3D11DeviceContext** 來執行資源物件管理。 也就是說，當圖形裝置驅動程式不支援多執行緒或命令清單時，Direct3D 11 會嘗試以內部方式處理裝置上下文的同步存取；而如果不支援命令清單，則會提供軟體實作。 因此，對於使用圖形介面的平台，就算缺乏多執行緒裝置上下文存取的驅動程式支援，您仍然可以撰寫在其上執行的多執行緒程式碼。

如果您的 app 支援分別處理命令清單與處理顯示畫面的執行緒，您可能希望讓 GPU 保持使用中，在處理命令清單的同時，以未察覺不順暢或延遲的方式來即時顯示畫面。 在此情況下，您可以針對每個執行緒使用不同的 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)，並使用 D3D11_RESOURCE_MISC_SHARED 旗標來建立它們，以便共用資源 (例如紋理)。 在此案例中，必須在處理執行緒時呼叫 [**ID3D11DeviceContext::Flush**](https://msdn.microsoft.com/library/windows/desktop/ff476425)，以便在顯示執行緒中顯示資源物件處理結果之前，完成命令清單的執行。

## <a name="deferred-rendering"></a>延遲轉譯


延遲轉譯會記錄命令清單中的圖形命令，因此，可以在某些其他時間中進行播放，而且延遲轉譯的設計目的是為了支援單一執行緒上的轉譯，同時記錄命令以便在其他執行緒上轉譯。 當這些命令完成之後，可以在產生最終顯示物件 (框架緩衝區、紋理或其他圖形輸出) 的執行緒上執行。

使用 [**ID3D11Device::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/ff476505) (而非 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 或 [**D3D11CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083)，因為這兩者會建立即時內容) 來建立延遲內容。 如需詳細資訊，請參閱[即時與延遲轉譯](https://msdn.microsoft.com/library/windows/desktop/ff476892)。

## <a name="related-topics"></a>相關主題


* [Direct3D 11 中的多執行緒簡介](https://msdn.microsoft.com/library/windows/desktop/ff476891)

 

 




