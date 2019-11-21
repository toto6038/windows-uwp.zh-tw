---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: 視覺層
description: Windows.UI.Composition API 可讓您存取架構層 (XAML) 與圖形層 (DirectX) 之間的組合層。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ac41d461982a39a939e460b7a81b144e5a08fdb3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255518"
---
# <a name="visual-layer"></a>視覺層

視覺層提供高效能、圖形的保留模式 API、效果和動畫，以及是 Windows 裝置上所有的 UI 的基礎。 您會以宣告方式定義 UI，而視覺效果層會依賴圖形硬體加速，以確保您的內容、效果和動畫會以順暢且無問題的方式呈現，與應用程式的 UI 執行緒無關。

值得注意的重點︰

* 熟悉的 WinRT API
* 從其他動態 UI 和互動架構
* 與設計工具一致的概念
* 效能未突然下降的線性延展性

Windows UWP app 已經使用透過其中一個 UI 架構的視覺層。 您也可以直接利用視覺層極為輕鬆地進行自訂轉譯、效果和動畫。

![UI 架構分層︰架構層級 (Windows.UI.XAML) 建置於視覺層 (Windows.UI.Composition) 上，而視覺層建置於圖形層 (DirectX) 上](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>視覺層的內容為何？

視覺層的主要功能如下︰

1. **內容**︰自訂繪製內容的輕量型組合
1. **效果**︰其效果可以建立動畫、鏈結和自訂的即時 UI 效果系統
1. **動畫**：在與 UI 執行緒無關的情況下執行表達性、無從驗證的架構動畫

### <a name="content"></a>內容

使用視覺效果，可讓動畫和效果系統裝載、轉換和使用內容。 類別階層的基底是[**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual)類別，為組合器中可見狀態之應用程式處理程序中的輕量型、敏捷執行緒 Proxy。 視覺效果的子類別包含  [**system.windows.media.containervisual>** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) ，可讓子系建立包含內容的視覺效果和[**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)樹狀結構，並可使用純色、自訂繪製內容或視覺效果來進行繪製。 這些視覺效果類型可一起構成 2D UI 的視覺效果樹狀結構，並且支援大部分的可見 XAML FrameworkElement。

如需詳細資訊，請參閱[組合視覺效果](composition-visual-tree.md)概觀。

### <a name="effects"></a>效果

視覺層中的效果系統可讓您將一連串的篩選和透明效果套用至視覺效果或視覺效果樹狀結構。 這是 UI 效果系統，請不要與影像和媒體效果混淆。 效果可以與動畫系統搭配運作，讓使用者獲得效果屬性的平順和動態動畫，而其轉譯與 UI 執行緒無關。 視覺層中的效果提供創意建置組塊，而這些建置組塊可以搭配使用並建立動畫來建構量身打造與互動的體驗。

除了可動畫效果鏈結之外，視覺層也支援光源模式，可讓視覺效果透過回應可動畫光源來模擬材料屬性。 視覺效果也可以轉型陰影。 光源和陰影可以組合使用，以建立深度和真實性的感覺。

如需詳細資訊，請參閱[組合效果](composition-effects.md)概觀。

### <a name="animations"></a>動畫

視覺層中的動畫系統可讓您移動視覺效果、建立效果動畫，並驅動轉換、剪輯和其他屬性。  這是一種不受架構規範的系統，已從頭開始設計，並具有效能考慮。  它會與 UI 執行緒分開執行，以確保流暢程度和擴充性。  雖然它可讓您使用熟悉的主要畫面格動畫來驅動屬性變更一段時間，但也可讓您設定不同屬性之間的數學關聯性，包括使用者輸入，讓您能夠直接製作順暢的單純體驗。

如需詳細資訊，請參閱[組合動畫](composition-animation.md)概觀。

### <a name="working-with-your-xaml-uwp-app"></a>使用 XAML UWP app

您可以使用 XAML 架構所建立的視覺效果，並在[**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview)中使用[**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting)類別，以支援可見的 FrameworkElement。 請注意，架構為您建立的視覺效果會隨附自訂的一些限制。 這是因為架構將會管理位移、轉換和存留時間。 不過，您可以建立自己的視覺效果，並透過 ElementCompositionPreview，或將它新增到視覺效果樹狀結構中某個位置的現有 ContainerVisual，以將它們附加至現有 XAML 元素。

如需詳細資訊，請參閱[搭配使用視覺層與 XAML](using-the-visual-layer-with-xaml.md) 概觀。

### <a name="working-with-your-desktop-app"></a>使用您的桌面應用程式

您可以使用視覺效果層來增強 WPF、Windows Forms 和C++ Win32 桌面應用程式的外觀、風格和功能。 您可以遷移內容的孤島以使用視覺分層，並將其餘的 UI 保留在現有的架構中。 這表示您可以對您的應用程式 UI 進行重大更新和增強，而不需要對現有的程式碼基底進行大量變更。

如需詳細資訊，請參閱[使用視覺層讓您的傳統型應用程式現代化](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)。

## <a name="additional-resources"></a>其他資源

* [**API 的完整參考檔**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* [WindowsUIDevLabs GitHub](https://github.com/microsoft/WindowsCompositionSamples) 有進階的 UI 和組合範例
* [Windows. UI. 撰寫範例庫](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui Twitter 摘要](https://twitter.com/windowsui)
* 閱讀 Kenny Kerr 針對這個 API 撰寫的 MSDN 文章：[圖形與動畫 - Windows 組合邁向 10](https://msdn.microsoft.com/magazine/mt590968) (英文)
