---
title: 您使用視覺分層的桌面應用程式現代化
description: 您可以使用視覺圖層來增強.NET 或 Win32 桌面應用程式的 UI。
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 05793c023fa0a0b08d4487e859f31442c7c1d53a
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "58174067"
---
# <a name="modernize-your-desktop-app-using-the-visual-layer"></a>您使用視覺分層的桌面應用程式現代化

您現在可以使用在非 UWP 桌面應用程式中 UWP Api，以增強的外觀、 風格和您的 WPF、 Windows Form 的功能和C++Win32 應用程式，並利用最新的 Windows 10 UI 功能，只可透過 UWP。

對於許多案例中，您可以使用[XAML 群島](../xaml-platform/xaml-host-controls.md)將最新的 XAML 控制項新增至您的應用程式。 不過，當您需要建立自訂的體驗，超越內建控制項，您可以存取視覺分層 Api。

視覺分層提供高效能、 保留模式 API 圖形、 效果及動畫。 它是 UI 的基礎，在 Windows 10 裝置上。 UWP XAML 控制項為基礎視覺分層，並啟用的許多層面[Fluent Design System](../design/fluent-design-system/index.md)，例如光線、 深度、 影片、 Material 和小數位數。

![以視覺化的圖層建立的使用者介面](images/interop/pull-to-animate.gif)

> _以視覺化的圖層建立的使用者介面_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>在任何 Windows 應用程式中建立視覺效果變化豐富的使用者介面

視覺分層，可讓您使用輕量級撰寫自訂的繪製內容 （視覺效果），並套用功能強大的動畫、 效果及操作您的應用程式中這些物件，以建立更吸引人的體驗。 視覺分層不會取代任何現有的 UI 架構;相反地，很有價值的補充資訊，這些架構。

您可以使用視覺分層讓您的應用程式獨特的外觀與風格，並建立設定與其他應用程式的身分識別。 它也可讓專為讓您的應用程式更容易使用，從使用者繪製更具互動性的 Fluent 設計原則。 例如，使用建立的視覺提示和動畫的畫面轉換在螢幕顯示項目之間的關聯性。

## <a name="visual-layer-features"></a>視覺分層功能

### <a name="brushes"></a>筆刷

[組合的筆刷](composition-brushes.md)可讓您繪製 UI 物件以純色、 漸層、 影像、 影片及複雜的影響，等等。

![建立與內容建立者蛋](images/interop/egg.gif)

> _建立與蛋[內容建立者示範應用程式](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)。_

### <a name="effects"></a>效果

[撰寫效果](composition-effects.md)包括光線、 陰影和篩選效果的清單。 它們可以動畫顯示、 自訂，並鏈結，然後直接套用至視覺效果。 SceneLightingEffect 可以結合以建立大氣、 深度和資料的組合光源。

![光線和材質](images/interop/light-interop.gif)

> _中的光線和材質示範[Windows UI 複合應用程式範例庫](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)。_

### <a name="animations"></a>動畫

[撰寫動畫](composition-animation.md)直接在複合項程序，獨立的 UI 執行緒中執行。 這可確保平滑和小數位數，因此您可以執行大量的並行、 明確的動畫。 除了熟悉主要畫面格動畫的磁碟機的屬性變更經過一段時間，您可以使用運算式來設定不同的屬性，包括使用者輸入之間的數學關係。 輸入驅動的動畫可讓您建立動態，並流暢地回應使用者輸入，可能會導致較高的使用者參與的 UI。

![以視覺化的圖層建立的使用者介面](images/interop/swipe-scroller.gif)

> _影片所示[Windows UI 複合應用程式範例庫](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)。_

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>保留您現有的程式碼基底，並以累加方式採用

您現有的應用程式中的程式碼表示您不想遺失的大量投資。 您可以移轉_群島_的內容，以使用視覺分層，並將其餘的 UI 保存在其現有的架構。 這表示您可以進行重大更新和增強功能到您的應用程式 UI 而不需要進行廣泛的變更對您現有的程式碼基底。

## <a name="samples-and-tutorials"></a>範例和教學課程

了解如何在您的應用程式中使用視覺分層，藉由使用我們的範例實驗。 這些範例和教學課程會幫助您開始使用視覺效果圖層，並顯示功能的運作方式。

### <a name="win32"></a>Win32

- [視覺分層中使用 Win32](using-the-visual-layer-with-win32.md)教學課程
  - [Hello 組合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello 向量範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [虛擬介面範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [螢幕擷取範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [使用 Windows Form 中的視覺分層](using-the-visual-layer-with-windows-forms.md)教學課程
  - [Hello 組合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [視覺化的圖層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [使用 WPF 中的視覺分層](using-the-visual-layer-with-wpf.md)教學課程
  - [Hello 組合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [視覺化的圖層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [螢幕擷取範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>限制

雖然許多視覺分層功能的運作方式相同的 UWP 應用程式中所顯示的一樣，桌面應用程式中裝載時，某些功能沒有限制。 以下是一些要注意的限制：

- 影響鏈結依賴[來參照 Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)的效果描述。 [來參照 Win2D NuGet 套件](https://www.nuget.org/packages/Win2D.uwp)不支援在桌面應用程式，因此您必須重新編譯它從[原始程式碼](https://github.com/Microsoft/Win2D)。
- 若要進行點擊測試，您需要進行範圍的計算方式為帶您自己的視覺化樹狀結構。 這等同於視覺分層中 UWP，但在此情況下沒有任何 XAML 項目，您可以輕鬆地繫結至的點擊測試。
- 視覺分層沒有轉譯文字的基本類型。
- 當兩個不同的 UI 技術一起使用時，例如 WPF 和視覺化的圖層，其負責繪製其本身的像素為單位的畫面上，每個，它們不能共用像素為單位。 如此一來，在 UI 中的其他內容上一律呈現視覺分層的內容。 (這就所謂_空間_問題。)您可能需要執行額外的程式碼撰寫和測試，以確保您的視覺分層內容與主應用程式 UI 調整大小並不 occlude 其他內容。
- 桌面應用程式中裝載的內容不會自動調整大小，或適用於 DPI 調整。 確定您的內容會處理 DPI 變更，可能需要額外的步驟。 （請參閱平台特定的教學課程的詳細資訊）。

## <a name="additional-resources"></a>其他資源

- [視覺層](visual-layer.md)
- [視覺化撰寫](composition-visual-tree.md)
- [組合的筆刷](composition-brushes.md)
- [撰寫效果](composition-effects.md)
- [組合的動畫](composition-animation.md)

API 參考資料

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)