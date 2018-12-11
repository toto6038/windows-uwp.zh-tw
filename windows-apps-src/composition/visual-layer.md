---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: 視覺層
description: Windows.UI.Composition API 可讓您存取架構層 (XAML) 與圖形層 (DirectX) 之間的組合層。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 599c2625bffff40a30f26bfb40f7cce9c97acdd1
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8927599"
---
# <a name="visual-layer"></a>視覺層

視覺層提供高效能、圖形的保留模式 API、效果和動畫，以及是 Windows 裝置上所有的 UI 的基礎。您可以透過宣告方式定義 UI，而且視覺層依賴圖形硬體加速，確保以平滑且無干擾的方式轉譯您的內容、效果和動畫，並且與應用程式的 UI 執行緒無關。

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

使用視覺效果，可讓動畫和效果系統裝載、轉換和使用內容。 類別階層的基底是[**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)類別，為組合器中可見狀態之應用程式處理程序中的輕量型、敏捷執行緒 Proxy。 視覺效果的子類別包含 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) ，以允許子項建立視覺效果和[**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) ，其中包含的樹狀結構的內容，而且可以使用純色、 自訂繪製內容或視覺效果進行繪製。 這些視覺效果類型可一起構成 2D UI 的視覺效果樹狀結構，並且支援大部分的可見 XAML FrameworkElement。

如需詳細資訊，請參閱[組合視覺效果](composition-visual-tree.md)概觀。

### <a name="effects"></a>效果

視覺層中的效果系統可讓您將一連串的篩選和透明效果套用至視覺效果或視覺效果樹狀結構。 這是 UI 效果系統，請不要與影像和媒體效果混淆。 效果可以與動畫系統搭配運作，讓使用者獲得效果屬性的平順和動態動畫，而其轉譯與 UI 執行緒無關。 視覺層中的效果提供創意建置組塊，而這些建置組塊可以搭配使用並建立動畫來建構量身打造與互動的體驗。

除了可動畫效果鏈結之外，視覺層也支援光源模式，可讓視覺效果透過回應可動畫光源來模擬材料屬性。 視覺效果也可以轉型陰影。 光源和陰影可以組合使用，以建立深度和真實性的感覺。

如需詳細資訊，請參閱[組合效果](composition-effects.md)概觀。

### <a name="animations"></a>動畫

視覺層中的動畫系統可讓您移動視覺效果、建立效果動畫，並驅動轉換、剪輯和其他屬性。它是無從驗證架構系統，而且在設計時都會考量效能。它會從 UI 執行緒獨立執行，確保順暢和延展性。雖然它可讓您使用熟悉的 KeyFrame 動畫來驅動一段時間的屬性變更，但也可讓您設定不同屬性之間的數學關係，包括使用者輸入，讓您直接製作順暢的編排體驗。

如需詳細資訊，請參閱[組合動畫](composition-animation.md)概觀。

### <a name="working-with-your-xaml-uwp-app"></a>使用 XAML UWP app

您可以使用 XAML 架構所建立的視覺效果，並在[**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908)中使用[**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976)類別，以支援可見的 FrameworkElement。 請注意，架構為您建立的視覺效果會隨附自訂的一些限制。 這是因為架構將會管理位移、轉換和存留時間。 不過，您可以建立自己的視覺效果，並透過 ElementCompositionPreview，或將它新增到視覺效果樹狀結構中某個位置的現有 ContainerVisual，以將它們附加至現有 XAML 元素。

如需詳細資訊，請參閱[搭配使用視覺層與 XAML](using-the-visual-layer-with-xaml.md) 概觀。

## <a name="additional-resources"></a>其他資源

* [**API 的完整參考文件**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
* [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs) 中的進階 UI 和組合範例
* [Windows.UI.Composition 範例庫](https://aka.ms/winuiapp)
* [@windowsui Twitter 摘要 ](https://twitter.com/windowsui)
* 閱讀 Kenny Kerr 針對這個 API 撰寫的 MSDN 文章：[圖形與動畫 - Windows Composition 邁向 10](https://msdn.microsoft.com/magazine/mt590968) (英文)
