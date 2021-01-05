---
description: 當您想要建立新的 Windows 傳統型應用程式時，首先必須決定要使用 Win32 和 COM API 還是 .NET。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 選擇您的 Windows 應用程式平台
ms.topic: article
ms.date: 11/04/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: windows win32, 傳統型應用程式
ms.openlocfilehash: 51d799a4779f6d3ecee2119277b6c41485e0d377
ms.sourcegitcommit: e1c182ea23da9b0bd9e89425f7f1a00baec81136
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97826269"
---
# <a name="choose-your-windows-app-platform"></a>選擇您的 Windows 應用程式平台

當您想要為 Windows 電腦建立新的傳統型應用程式時，首先必須決定要使用哪個應用程式平台。 Windows 提供四個主要應用程式平台，各有不同的優勢：

* [通用 Windows 平台 (UWP)](#uwp)：此平台為執行 Windows 10 的所有裝置提供一般型別系統、API 和應用程式模型。 UWP 應用程式可以是原生或受管理的。
* [WPF](#wpf) 和 [Windows Forms](#windows-forms)：這些 .NET 型平台會針對受控應用程式提供一般類型的系統、API 和應用程式模型。
* [Win32](#win32)：這是適用於原生 C/C++ Windows 應用程式的原創平台，需要直接存取 Windows 和硬體。 這讓 Win32 API 成為應用程式的首選平台，因為這些應用程式需要最高階效能及直接存取系統硬體。

所有這些應用程式平台都包含完整的使用者介面架構和一組 UI 控制項，可讓您建立在傳統 Windows 桌面上執行的桌面應用程式 (例如 Word、Excel 和 Photoshop)，並充分利用該環境的特定功能。 在 Windows 10 上，所有這些平台都支援使用 [Windows UI (WinUI) 程式庫](#windows-ui-library)來建立其使用者介面。

其中有些平台會共用一些特性，而且更適合特定類型的應用程式。 例如，UWP 和 .NET 都與 Visual Studio 進行深度整合。 這可提供許多優點，特別是在開發人員生產力、精細且可自訂的使用者介面，以及應用程式安全性等方面。 由於這些架構支援視覺化設計工具和 UI 標記來快速建立 UI，因此特別適用於企業營運應用程式。

> [!NOTE]
> 無論您選擇哪一個應用程式平台，都可以使用眾多 Windows 10 功能為您的應用程式提供現代化體驗。 例如，即使您的桌面應用程式是使用 WPF、Windows Forms 或 Win32 API 建置的，您仍然可以使用 MSIX 套件部署。 如需有關如何讓桌面應用程式現代化的詳細資訊，請參閱[讓您的桌面應用程式現代化](modernize/index.md)。

## <a name="uwp"></a>UWP

UWP 是適用於 Windows 10 應用程式和遊戲的先進平台。 它是高度可自訂的平臺，其會使用 XAML 標記來分隔 UI (簡報) 與程式碼 (商務邏輯)。 UWP 適用於需要複雜 UI、樣式自訂和圖形密集案例的傳統型應用程式。 UWP 也提供 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/) 的內建支援，以取得預設的 UX 體驗，並可讓您存取 [Windows 執行階段 (WinRT) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)。 藉由採用 Fluent，UWP 會自動支援一般輸入法，例如筆跡、觸控、遊戲台、鍵盤和滑鼠。

您不僅可以使用 UWP 來建立適用於 Windows 電腦的傳統型應用程式，而且 UWP 也是 Xbox、HoloLens 和 Surface Hub 應用程式唯一支援的平台。 UWP 是我們最先進的應用程式平台。

如需 UWP 的詳細資訊，請參閱下列文章：

* [開始使用](/windows/uwp/get-started/)
* [專案範本](visual-studio-templates.md#uwp-templates)
* [設計與 UI](/windows/uwp/design/)
* [技術和功能](/windows/uwp/develop/)
* [API 參考](/uwp/)
* [範例](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

WPF 是針對受管理 Windows 應用程式所建立的平台，可存取 .NET Core 或完整的 .NET Framework，而且也會使用 XAML 標記來區隔 UI 與程式碼。 此平台是針對需要複雜 UI、樣式自訂和圖形密集案例的傳統型應用程式而設計的。 WPF 開發技能類似於 UWP 開發技能，因此從 WPF 遷移至 UWP 應用程式比從 Windows Forms 進行遷移更容易。

如需 WPF 的詳細資訊，請參閱下列文章：

* [使用者入門 (WPF)](/dotnet/framework/wpf/getting-started/)
* [專案範本](visual-studio-templates.md#net-templates)
* [建立您的第一個應用程式 (.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [建立您的第一個應用程式 (.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [將 WPF 應用程式遷移至 .NET Core](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [API 參照 (.NET)](/dotnet/api/index)
* [範例](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows Forms

Windows Forms 是適用於受管理 Windows 應用程式的原創平台，具有輕量型 UI 模型並可存取 .NET Core 或完整的 .NET Framework。 它擅長於讓開發人員快速開始建立應用程式，甚至適用於初次使用平台的開發人員。 這是表單型快速應用程式開發平台，其中包含一組大型內建的視覺效果和非視覺效果拖放控制項集合。 Windows Forms 不會使用 XAML，因此稍後決定將應用程式延伸至 UWP 時，需要完整重新撰寫您的 UI。

如需 Windows Forms 的詳細資訊，請參閱下列文章：

* [Windows Form 使用者入門](/dotnet/framework/winforms/getting-started-with-windows-forms)
* [專案範本](visual-studio-templates.md#net-templates)
* [建立您的第一個 Windows Forms 應用程式](/dotnet/framework/winforms/creating-a-new-windows-form)
* [教學課程：建立圖片檢視器](/visualstudio/ide/tutorial-1-create-a-picture-viewer)
* [API 參照 (.NET)](/dotnet/api/index)
* [增強 Windows Forms 應用程式](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

搭配使用 Win32 API 與 C++ 可讓您實現最高階效能和效率，方法為嚴加控制目標平台與可能位於受管理執行階段環境 (例如 WinRT 和 .NET) 的未受管理程式碼。 不過，對於應用程式執行進行這種層級的控制，需要更加小心謹慎以免出錯，導致開發生產力雖然提高，但執行階段效能卻降低。

以下是 Win32 API 和 C++提供的幾個重點，可讓您建置高效能的應用程式。

* 硬體層級的最佳化，包括嚴格控制資源配置、物件存留期、資料配置、對齊、位元組封裝等。
* 透過內建函式存取效能導向的指令集 (例如 SSE 和 AVX)。
* 使用範本進行有效率的型別安全泛型程式設計。
* 有效率且安全的容器和演算法。
* DirectX，特別是 Direct3D 和 DirectCompute (請注意，UWP 也提供 DirectX interop)。

如需詳細資訊，請參閱下列文章：

* [開始使用](/windows/win32/desktop-programming/)
* [專案範本](visual-studio-templates.md#cwin32-templates)
* [建立您的第一個 Win32 和 C++ 應用程式](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [技術和功能](/windows/win32/desktop-app-technologies)
* [API 參考](/windows/win32/apiindex/windows-api-list/)
* [範例](https://github.com/Microsoft/Windows-classic-samples)

## <a name="windows-ui-library"></a>Windows UI 程式庫

在 Windows 10 上，每一個主要的桌面平台都支援使用 [Windows UI (WinUI) 程式庫](../winui/index.md)來建立其使用者介面。 WinUI 一開始是以工具組的形式提供，旨在針對舊版 Windows 10 提供適用於 UWP 應用程式的全新或更新版 UWP 控制項。 WinUI 已擴大範圍，現在是適用於 UWP、.NET 和 Win32 上 Windows 10 應用程式的新式原生使用者介面 (UI) 平台。

您可以透過下列方式在桌面應用程式中使用 WinUI：

* UWP 應用程式可以使用 WinUI 控制項來取代 Windows SDK 所提供的 UWP 控制項。
* 您可以將現有的 WPF、Windows Forms 和 C++/Win32 應用程式更新為使用 [XAML Islands](modernize/xaml-islands.md)，以在應用程式中裝載 WinUI 2.x 控制項。
* 從 [WinUi 3.0](../winui/winui3/index.md) 開始，您可以建立 [.NET 和 C++ /Win32 應用程式來使用完全以 WinUI 為基礎的 UI](../winui/winui3/get-started-winui3-for-desktop.md)。

## <a name="project-reunion-preview"></a>Project Reunion (預覽)

Project Reunion 是一組廣泛的新開發人員元件和工具的程式碼名稱，其代表的是 Windows 應用程式開發平台的新一代進化。 Project Reunion 提供了一組整合的 API 和工具，可供一組廣泛的目標 Windows 10 OS 版本上的任何應用程式以一致的方式進行使用。 Project Reunion 會透過一組可讓開發人員在這些平台上仰賴的通用 API 和工具，來與現有的 Windows 應用程式平台和架構 (例如 UWP 和原生 Win32 以及 .NET) 互補。

Project Reunion 目前提供早期開發人員預覽。 建議您在開發環境中試用此版本。 但請注意，Project Reunion 的現行版本與最終版本之間，會在許多方面有所變化。 在生產環境中使用的應用程式不支援 Project Reunion。

如需詳細資訊，請參閱 [Project Reunion](../project-reunion/index.md) 以及我們的 [GitHub 存放庫](https://github.com/microsoft/ProjectReunion/)。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>平台比較：UWP、WPF 和 Windows Forms

下表詳細比較 Windows Forms、WPF 和 UWP 的各種特性。

| 功能或案例  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **支援版本**      |  Windows 10   |  Windows 7 和更新版本 |  Windows 7 和更新版本  |
| **語言**      |   C\#、C++/WinRT、C++/CX、VB、JavaScript   |  C\#、C++/CLI (Managed Extensions for C++)、F\#VB |  C\#、C++/CLI (Managed Extensions for C++)、F\#VB   |
| **UI 執行階段** |    原生 (C++/WinRT 和 C++/CX) 和受管理 (.NET Native)  |  受管理 (.NET Framework 和 .NET Core 3) |   受管理 (.NET Framework 和 .NET Core 3)   |
| **開放原始碼** | [是 (僅限 Windows UI 程式庫)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [是 (僅限 .NET Core)](https://github.com/dotnet/wpf) | [是 (僅限 .NET Core)](https://github.com/dotnet/winforms)  |
| **支援 XAML** |   是   |  是  |   否   |
| **優勢**  |  <ul><li>適用於 UI 的 XAML 標記</li><li>豐富且可自訂的 UX</li><li>您現有的程式碼基底相容於 .NET Standard</li><li>高 DPI 支援</li><li>支援跨 Windows 裝置的多種輸入類型 (包括觸控、手寫筆、遊戲台、滑鼠和鍵盤)</li><li>支援 Xbox、HoloLens、IoT 或 Surface Hub</li><li>選擇性密集 (精簡) UI</li><li>支援原生 C++</li><li>最佳化的電池使用時間</li><li>新式協助工具支援 (例如螢幕助讀程式)</li><li>Rich Text 格式資料功能 (例如內建的拼字檢查)</li><li>筆跡支援</li><li>透過應用程式容器保護執行安全 (例如，未受信任的內容會沙箱化)</li></ul>  |  <ul><li>適用於 UI 的 XAML 標記</li><li>豐富且可自訂的 UX</li><li>來自 Microsoft 和合作夥伴的大型控制項集合</li><li>密集 UI</li><li>支援 Windows 7</li><li>支援平台進行輸入驗證</li></ul> | <ul><li>快速應用程式開發</li><li>用於建置 UI 的 WYSIWYG 編輯器</li><li>來自 Microsoft 和合作夥伴的大型控制項集合</li><li>密集 UI</li><li>支援 Windows 7</li><li>鍵盤和滑鼠輸入</li></ul>          |
| **有限支援的案例** |  <ul><li>多重視窗支援<sup>1</sup></li><li>支援平台進行輸入驗證<sup>1</sup></li><li>不支援 Windows 7</li><li>某些Windows 執行階段 API 需要特定的 Windows 10 最低版本</li><li>完整平台支援和命令介面整合 (例如，UWP 目前不支援系統匣整合或完整存取所有裝置)</li><li>直接存取磁碟上的所有檔案</li><li>ADO.NET</li><li>使用非 .NET Standard 或非 Windows 應用程式認證套件相容 API 的現有程式碼基底類別程式庫</li><li>區域網路回送支援 (亦即，如果您的應用程式需要與 localhost 通訊，而不在目標裝置上建立回送豁免)</li><li>密集檔案 I/O</li></ul>     |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li></ul>  |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li><li>可自訂的 UI</li><li>豐富的圖形和使用者體驗 (例如觸控和動畫)</li><li>豐富的視圖和資料模型抽象</li></ul>    |   |

<sup>1</sup> 我們已公開宣佈的功能，可在未來的 Windows 10 版本中解決此案例。

<sup>2</sup> 雖然平台缺少此案例的第一級 API 支援，但開發人員可以透過因應措施來支援此案例。

## <a name="other-app-platforms"></a>其他應用程式平台

### <a name="progressive-web-apps-pwas"></a>漸進式 Web 應用程式 (PWA)

PWA 可讓開發人員封裝他們的網站程式碼，以便能夠在 Windows 10 電腦上安裝並且像應用程式一般執行。 如需詳細資訊，請參閱[漸進式 Web 應用程式](/microsoft-edge/progressive-web-apps-chromium/get-started)。

### <a name="xamarin"></a>Xamarin

使用 Xamarin 可建置跨平台應用程式，不只適用於 Windows 10，也可以在 iOS 和 Android 上執行。 如需詳細資訊，請參閱 [Xamarin](/xamarin/xamarin-forms/get-started/index)。

### <a name="uno-platform"></a>Uno 平台

Uno 平台可讓 Windows UWP 型的程式碼 (C# 和 XAML) 在 iOS、Android、macOS、Linux 和 WebAssembly 上執行。 其會針對 [Windows 10 2004 (19041)](/windows/uwp/whats-new/windows-10-build-19041) 中的 UWP 提供完整的 API 定義，以及部分 UWP API 的實作 (例如 [Windows.UI.Xaml](/uwp/api/windows.ui.xaml.documents))，讓 UWP 應用程式可以在這些平台上執行。 如需詳細資訊，請參閱 [Uno 平台文件](https://platform.uno/docs/articles/intro.html)。
