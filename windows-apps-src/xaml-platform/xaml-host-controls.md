---
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 傳統型應用程式中的 UWP 控制項
ms.date: 01/11/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf25fea6ca6e8809c12324ae57a42cc712ded2a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619703"
---
# <a name="uwp-controls-in-desktop-applications"></a>傳統型應用程式中的 UWP 控制項

> [!NOTE]
> XAML 島是開發人員預覽目前可用的。 雖然我們鼓勵您試用看看在自己的原型程式碼現在，我們不建議，您使用它們在實際程式碼這一次。 這些 Api 和控制項將會繼續成熟並穩定未來 Windows 版本。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。
>
> 如果您有意見反應的相關 XAML 群島，建立新的問題，在[WindowsCommunityToolkit 存放庫](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues)和那里留下您的意見。 如果您想要私下提交您的意見反應，您可以傳送到XamlIslandsFeedback@microsoft.com。 您的深入解析和案例會對我們非常重要。

Windows 10 現在可讓您在非 UWP 桌面應用程式中使用 UWP 控制項，以便您可以增強的外觀、 風格和您現有的傳統型應用程式最新的 Windows 10 UI 功能才可透過 UWP 控制項的功能。 這表示您可以使用 UWP 功能這類[Windows Ink](../design/input/pen-and-stylus-interactions.md) ，並控制該支援[Fluent Design System](../design/fluent-design-system/index.md)現有 WPF、 Windows Form 和 c + + Win32 應用程式中。 此案例中開發人員有時稱為*XAML 群島*。

我們提供數種方式使用 XAML 群島，在您 WPF、 Windows Form 和 c + + Win32 應用程式的技術或您使用的架構而定。

## <a name="wrapped-controls"></a>已包裝的控制項

WPF 和 Windows Forms 應用程式可以使用選取的已包裝的 UWP 控制項中[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 我們將這些控制項做為*包裝控制項*因為它們所包裝的介面和特定的 UWP 控制項的功能。 您可以將這些控制項直接加入 WPF 或 Windows Form 專案的設計介面，然後依照像任何其他 WPF 或 Windows Form 控制項設計工具中使用它們。

> [!NOTE]
> 已包裝的控制項不是適用於 c + + Win32 桌面應用程式。 這類應用程式必須使用[UWP XAML 裝載 API](#uwp-xaml-hosting-api)。

下列已包裝的 UWP 控制項是目前適用於 WPF 和 Windows Form 應用程式。 Windows 社群工具組的未來版本的更多的 UWP 包裝控制項已計劃。

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [Web 檢視](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 您可以使用 Microsoft Edge 轉譯引擎來顯示 web 內容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供的版本**WebView**與更多的 OS 版本相容。 此控制項會使用 Microsoft Edge 轉譯引擎，以顯示在 Windows 10 在版本 1803年和更新版本上的 web 內容和 Internet Explorer 轉譯引擎，以顯示 web 內容在較早版本的 Windows 10，Windows 8.x 和 Windows 7。 |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 版本 1809年 （組建 17763） | 提供在 Windows Form 或 WPF 桌面應用程式中的 Windows Ink 為基礎的使用者互動的介面和相關的工具列。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 版本 1809年 （組建 17763） | 將內嵌資料流，並呈現媒體內容，例如視訊在 Windows Form 或 WPF 桌面應用程式中的檢視。 |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 版本 1809年 （組建 17763） | 可讓您在 Windows Form 或 WPF 桌面應用程式中顯示的符號或逼真的對應。 |

## <a name="host-controls"></a>主控制項

如需可用的已包裝控制項所未包含的案例，WPF 和 Windows Form 應用程式也可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制中[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 這個控制項可以裝載任何 UWP 控制項衍生自[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括任何由 Windows SDK 所提供的 UWP 控制項，以及自訂使用者控制項。 此控制項支援 Windows 10 Insider Preview SDK 建置 17709 和更新版本。

> [!NOTE]
> 無法使用 c + + Win32 桌面應用程式的主控制項。 這類應用程式必須使用[UWP XAML 裝載 API](#uwp-xaml-hosting-api)。

## <a name="uwp-xaml-hosting-api"></a>裝載 API 的 UWP XAML

如果您有 c + + Win32 應用程式時，您可以使用*UWP XAML 裝載 API*來裝載任何 UWP 控制項衍生自[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)中的任何 UI 項目中您有相關聯的視窗控制代碼 (HWND) 的應用程式。 此 API 是在 Windows 10 Insider Preview SDK 建置 17709 引進。 如需使用此 API 的詳細資訊，請參閱[使用桌面應用程式中裝載 API XAML](using-the-xaml-hosting-api.md)。

> [!NOTE]
> C + + Win32 桌面應用程式必須使用裝載 API 來裝載 UWP 控制項 UWP XAML。 已包裝的控制項和主控制項不適用於這些類型的應用程式。 WPF 和 Windows Forms 應用程式，我們建議您先使用 Windows 社群工具組，而不是 UWP XAML 中的已包裝的控制項和主控制項裝載 API。 這些控制項使用 UWP XAML，內部裝載 API，並提供更簡單的開發經驗。 不過，您可以使用裝載 API 直接在 WPF 和 Windows Form 應用程式中，如果您選擇 UWP XAML。

## <a name="architecture-overview"></a>架構概觀

以下可快速查看這些控制項在架構上的組織方式。 此圖表中使用的名稱會隨時變更。  

![裝載控制項架構](images/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。 您將新增到您的設計工具的控制項，是隨附於 Windows 社群工具組中的 Nuget 套件。

這些新的控制項都有限制，所以在您使用前，請花一些時間檢閱什麼尚未支援，以及哪些只有利用因應措施才能使用。

## <a name="limitations"></a>限制

### <a name="whats-supported"></a>支援的功能

大部分，支援全部，除非以下明確提出。

### <a name="whats-supported-only-with-workarounds"></a>只有利用因應措施才能使用

:heavy_check_mark:裝載多個視窗內的多個收件匣 控制項。 您將必須將每個視窗置於其本身的執行緒。

:heavy_check_mark:使用``x:Bind``裝載的控制項。 您將必須在 .NET Standard 程式庫中宣告資料模型。

:heavy_check_mark:C#-根據協力廠商控制項。 如果您有第三方控制項的原始程式碼，您可以對它進行編譯。

### <a name="whats-not-yet-supported"></a>不支援的功能

: no_entry_sign:跨應用程式順暢地運作，裝載控制項的協助工具。

: no_entry_sign:在您加入不含 Windows 應用程式套件的應用程式的控制項中的當地語系化的內容。

: no_entry_sign:在 XAML 中建立不包含 Windows 應用程式套件的應用程式內的資產參考。

: no_entry_sign:適當地回應變更 DPI 和小數位數的控制項。

: no_entry_sign:新增**WebView** （執行緒上，關閉執行緒，或從程序） 的自訂使用者控制項的控制項。

: no_entry_sign:[顯示反白顯示](https://docs.microsoft.com/windows/uwp/design/style/reveal)Fluent 的效果。

: no_entry_sign:內嵌的手寫筆跡功能， @Places，和@People輸入的控制項。

: no_entry_sign:指派的快速鍵。

: no_entry_sign:C + + 為基礎的協力廠商控制項。

: no_entry_sign:裝載的自訂使用者控制項。

我們會持續改善桌面 Fluent 的體驗，所以這份清單中的項目也可能會變更。  
