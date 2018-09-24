---
author: normesta
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 傳統型應用程式中的 UWP 控制項
ms.author: normesta
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: d5a4865f403685752225a729bf68abb15237dd90
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "4156581"
---
# <a name="uwp-controls-in-desktop-applications"></a>傳統型應用程式中的 UWP 控制項

> [!NOTE]
> Api 與此文章中討論的控制項是目前可為開發人員預覽。 雖然我們鼓勵您試用您自己的原型程式碼中現在，我們不建議您使用它們在實際執行程式碼中這一次。 這些 Api 和控制項將會繼續成熟並穩定在未來的 Windows 版本。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

在下一個版本的 Windows 10 中我們正在將 UWP 控制項放到非 UWP 傳統型應用程式，讓您可以提升外觀、 感覺和您現有的傳統型應用程式與最新的 Windows 10 UI 功能只會透過 UWP 控制項的功能. 這表示您可以使用例如[Fluent Design 系統](../design/fluent-design-system/index.md)和在您的現有 WPF、 Windows Forms 和 C/c + + Win32 應用程式中的[Windows Ink](../design/input/pen-and-stylus-interactions.md)的 UWP 功能。 這個案例中開發人員有時稱為*XAML 群島*。

我們將會提供數種方式來使用您的傳統型應用程式，取決於技術或您正在使用的架構中的 XAML 群島。

## <a name="wrapped-controls"></a>包裝的控制項

我們將會提供包裝 UWP 控制項的選取[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中的 WPF 及 Windows Forms 應用程式。 您可以直接在 WPF 或 Windows Forms 專案的設計介面中新增這些控制項，然後使用設計工具中的 [就像任何其他 WPF 或 Windows Forms 控制項一樣。 我們會將這些控制項做為*包裝控制項*因為它們換行的介面和特定的 UWP 控制項的功能。

請嘗試這個出目前使用[WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/webview)控制 Windows 社群工具組中。 這個控制項使用 Microsoft Edge 轉譯引擎在 WPF 或 Windows Forms 應用程式中顯示網頁內容。  

我們也會 WPF 規劃其他包裝的 UWP 控制項及 Windows Forms 應用程式未來版本的 Windows 社群工具組，包括：

* **WebViewCompatible**。 此控制項為**WebView**與 Windows 10 相容的版本與先前的 Windows 版本。 這個控制項使用 Microsoft Edge 轉譯引擎，以顯示在 Windows 10 上的網頁內容和 Internet Explorer 的轉譯引擎，以顯示較舊版本上的網頁內容。
* **InkCanvas**與**InkToolbar**。 這些控制項提供 surface 和相關的工具列中 Windows Forms 或 WPF 傳統型應用程式與 Windows Ink 為基礎的使用者互動。
* **MediaPlayerElement**。 此控制項中嵌入串流，並轉譯媒體內容，例如 Windows Forms 或 WPF 傳統型應用程式中的視訊的檢視。

多個 UWP 包裝控制項對於 WPF 及 Windows Forms 應用程式已計劃未來版本的 Windows 社群工具組。

> [!NOTE]
> C/c + + Win32 傳統型應用程式都不包裝的控制項。 這些類型的應用程式必須使用[UWP XAML 裝載 API](#uwp-xaml-hosting-api)。

## <a name="host-controls"></a>主控制項

可用的包裝控制項所未包含的情況下，WPF 和 Windows Forms 應用程式也可以使用[WindowsXamlHost](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/docs/controls/WindowsXAMLHost.md)控制[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中。 此控制項可以裝載衍生自[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括 Windows SDK，以及自訂使用者控制項所提供的任何 UWP 控制項的任何 UWP 控制項。

> [!NOTE]
> 主控制項沒有可用的 C/c + + Win32 傳統型應用程式。 這些類型的應用程式必須使用[UWP XAML 裝載 API](#uwp-xaml-hosting-api)。

## <a name="uwp-xaml-hosting-api"></a>UWP XAML 裝載 API

如果您有 C/c + + /winrt 應用程式，您可以使用*UWP XAML 裝載 API*來裝載衍生自[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) ，相關聯的視窗控制代碼 (HWND) 的應用程式中的任何 UI 項目中的任何 UWP 控制項。 如需有關如何使用此 API 的詳細資訊，請參閱[使用 XAML 裝載在傳統型應用程式中的 API](using-the-xaml-hosting-api.md)。

> [!NOTE]
> C/c + + Win32 傳統型應用程式必須使用 UWP XAML 裝載 API 來裝載 UWP 控制項。 包裝的控制項和主機沒有可用的這些類型的應用程式。 對於 WPF 及 Windows Forms 應用程式，我們建議您先使用 Windows 社群工具組，而不是 UWP XAML 中包裝的控制項和主控制項裝載 API。 這些控制項使用 UWP XAML 內部裝載 API，並提供更簡單的開發體驗。 不過，您可以使用 UWP XAML 裝載直接在 WPF 和 Windows Forms 應用程式中的 API，如果您選擇。

## <a name="architecture-overview"></a>架構概觀

以下可快速查看這些控制項在架構上的組織方式。 此圖表中使用的名稱會隨時變更。  

![裝載控制項架構](images/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。 您將新增到您的設計工具的控制項，是隨附於 Windows 社群工具組中的 Nuget 套件。

這些新的控制項都有限制，所以在您使用前，請花一些時間檢閱什麼尚未支援，以及哪些只有利用因應措施才能使用。

## <a name="limitations"></a>限制

### <a name="whats-supported"></a>支援的功能

大部分，支援全部，除非以下明確提出。

### <a name="whats-supported-only-with-workarounds"></a>只有利用因應措施才能使用

:heavy_check_mark︰在多個視窗內裝載多個收件匣控制項。 您將必須將每個視窗置於其本身的執行緒。

:heavy_check_mark：使用 ``x:Bind`` 和裝載的控制項。 您將必須在 .NET Standard 程式庫中宣告資料模型。

:heavy_check_mark：以 C# 為基礎的第三方控制項。 如果您有第三方控制項的原始程式碼，您可以對它進行編譯。

### <a name="whats-not-yet-supported"></a>不支援的功能

:no_entry_sign：無縫跨應用程式與裝載的控制項運作的協助工具。

:no_entry_sign：當地語系化您新增到不包含 Windows 應用程式套件的應用程式中的控制項內容。

:no_entry_sign：在不包含 Windows 應用程式套件的應用程式內以 XAML 製作的資產參考。

:no_entry_sign：正確對應 DPI 和縮放尺寸變更的控制項。

:no_entry_sign：新增  **WebView** 控制項到自訂使用者控制項 (開啟執行緒、關閉執行緒或退出處理器)。

:no_entry_sign：[顯色醒目提示](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent 效果。

:no_entry_sign：輸入控制項用的內嵌筆跡 @Places 和 @People。

:no_entry_sign：指派快速鍵。

:no_entry_sign： C++ 型協力廠商控制項。

:no_entry_sign︰裝載自訂使用者控制項。

我們會持續改善桌面 Fluent 的體驗，所以這份清單中的項目也可能會變更。  
