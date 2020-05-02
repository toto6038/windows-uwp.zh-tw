---
title: 使用視覺層讓您的傳統型應用程式現代化
description: 使用視覺層來增強 .NET 或 Win32 傳統型應用程式的 UI。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215168"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>在傳統型應用程式中使用視覺層

您現在可以在非 UWP 傳統型應用程式中使用 UWP API，來增強您的 WPF、Windows Forms 和 C++ Win32 應用程式的外觀、風格及功能，並且利用只能透過 UWP 取用的最新 Windows 10 UI 功能。

在許多情況下，您可使用 [XAML Islands](xaml-islands.md)，將新式 XAML 控制項新增至您的應用程式。 不過，當您需要建立超越內建控制項的自訂體驗時，可以存取視覺層 API。

視覺層會針對圖形、效果和動畫提供高效能、保留模式的 API。 這是跨 Windows 10 裝置的 UI 基礎。 UWP XAML 控制項是以視覺層為基礎，可實現 [Fluent Design System](/windows/uwp/design/fluent-design-system/index) 的許多層面，例如光線、深度、動作、材質和規模。

![使用視覺層建立的使用者介面](images/visual-layer-interop/pull-to-animate.gif)

> _使用視覺層建立的使用者介面_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>在任何 Windows 應用程式中建立視覺上吸引人的使用者介面

視覺層可讓您使用自訂繪製內容 (視覺效果) 的輕量型合成，以及對應用程式中的物件套用強大的動畫、效果和操作，以建立吸引人的體驗。 視覺層不會取代任何現有的 UI 架構。相反地，其為這些架構的重要補充。

您可使用視覺層為您的應用程式提供獨特的外觀和風格，並建立身分識別，使您的應用程式與其他應用程式與眾不同。 其也會啟用 Fluent Design 原則，這類原則的設計訴求是為了讓您的應用程式更容易使用，並引起使用者更多參與。 例如，您可使用視覺層來建立視覺提示和動畫螢幕轉換，而後者可顯示螢幕上項目之間的關聯性。

## <a name="visual-layer-features"></a>視覺層功能

### <a name="brushes"></a>筆刷

[組合筆刷](/windows/uwp/composition/composition-brushes)可讓您以純色、漸層、影像、影片、複雜效果等繪製 UI 物件。

![以 Material Creator 建立的蛋](images/visual-layer-interop/egg.gif)

> _以 [Material Creator 示範應用程式](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)建立的蛋。_

### <a name="effects"></a>效果

[組合效果](/windows/uwp/composition/composition-effects)包括光線、陰影和篩選效果的清單。 這類效果可以動畫呈現、自訂和鍊結，然後直接套用至視覺效果。 SceneLightingEffect 可與組合光源結合，以營造氣氛、深度和材質。

![光線和材料](images/visual-layer-interop/light-interop.gif)

> _[Windows UI 組合範例庫](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)中所示範的光線和材質。_

### <a name="animations"></a>動畫

[組合動畫](/windows/uwp/composition/composition-animation)可直接在 Compositor 程序中執行，與 UI 執行緒無關。 這可確保平滑和規模，讓您可以執行大量的並行、明確動畫。 除了熟悉的 KeyFrame 動畫用來驅動一段時間的屬性變更之外，您還可使用運算式來設定不同屬性之間的數學關聯性，包括使用者輸入。 輸入驅動的動畫可讓您建立可動態和流暢地回應使用者輸入的 UI，進而導致更高的使用者參與。

![使用視覺層建立的使用者介面](images/visual-layer-interop/swipe-scroller.gif)

> _[Windows UI 組合範例庫](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)中所示範的動作。_

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>保留現有的程式碼庫並以累加方式採用

現有應用程式中的程式碼代表您不想遺失的大量投資。 您可遷移內容的 Island  以使用視覺層，並將其餘的 UI 保留在其現有的架構中。 這表示您可對您的應用程式 UI 進行重大更新和增強，而不需要對現有的程式碼庫進行大量變更。

## <a name="samples-and-tutorials"></a>範例和教學課程

藉由實驗我們的範例，了解如何在您的應用程式中使用視覺層。 這些範例和教學課程可協助您開始使用視覺層，並向您示範功能的運作方式。

### <a name="win32"></a>Win32

- [使用視覺層搭配 Win32](using-the-visual-layer-with-win32.md) 教學課程
  - [Hello 組合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello 向量範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [視覺表面範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [螢幕擷取範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [使用視覺層搭配 Windows Forms](using-the-visual-layer-with-windows-forms.md) 教學課程
  - [Hello 組合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [視覺層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [使用視覺層搭配 WPF](using-the-visual-layer-with-wpf.md) 教學課程
  - [Hello 組合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [視覺層整合範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [螢幕擷取範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>限制

雖然許多視覺層功能裝載於傳統型應用程式與 UWP 應用程式時的作用相同，但有些功能的確有所限制。 以下是一些需要留意的限制：

- 效果鏈依賴 [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) 進行效果描述。 傳統型應用程式中不支援 [Win2D NuGet 套件](https://www.nuget.org/packages/Win2D.uwp)，因此您必須從[原始程式碼](https://github.com/Microsoft/Win2D)將其重新編譯。
- 若要進行點擊測試，您必須自行瀏覽視覺化樹狀結構來進行界限計算。 這與 UWP 中的視覺層相同，但在此情況下例外，您沒有可輕鬆繫結至的 XAML 元素可進行點擊測試。
- 視覺層沒有可供轉譯文字的基元。
- 當兩種不同的 UI 技術一起使用時 (例如 WPF 和視覺層)，其各自負責在螢幕上繪製自己的像素，而且無法共用像素。 因此，視覺層內容一律會在其他 UI 內容上轉譯 (這就是所謂的「空間」  問題)。您可能需要進行額外的編碼和測試，以確保您的視覺層內容會隨著主機 UI 調整大小，而不會遮蔽其他內容。
- 傳統型應用程式中裝載的內容不會針對 DPI 自動調整大小或縮放。 您可能需要採取額外的步驟，以確保您的內容能處理 DPI 變更 (如需詳細資訊，請參閱平台專屬教學課程)。

## <a name="additional-resources"></a>其他資源

- [視覺層](/windows/uwp/composition/visual-layer)
- [組合視覺效果](/windows/uwp/composition/composition-visual-tree)
- [組合筆刷](/windows/uwp/composition/composition-brushes)
- [組合效果](/windows/uwp/composition/composition-effects)
- [組合動畫](/windows/uwp/composition/composition-animation)

API 參考資料

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)