---
Description: 當您想要建立新的傳統型應用程式時，首先必須決定要使用 Win32 和 COM API 還是 .NET。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 選擇您的應用程式平台
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 57f3101b1134b4fdceb6a38f392e54677d2b884d
ms.sourcegitcommit: f3dd633a3149d2e206981fa52ad424d408e5508c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799574"
---
# <a name="choose-your-app-platform"></a>選擇您的應用程式平台

當您想要為 Windows 電腦建立新的傳統型應用程式時，首先必須決定要使用哪個應用程式平台。 Windows 提供四個主要應用程式平台，各有不同的優勢：

* [通用 Windows 平台 (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

所有這些應用程式平台都可讓您建立在傳統 Windows 桌面上執行的傳統型應用程式 (例如 Word、Excel 和 Photoshop)，並充分利用該環境的特定功能。 不過，其中有些平台會共用一些特性，而且更適合特定類型的應用程式：

* **UWP、WPF 和 Windows Forms**。 這些平台提供受管理的執行階段環境 (適用於 UWP 的 Windows 執行階段，以及適用於 Windows Forms 和 WPF 的 .NET)，其有許多優點，特別是在開發人員生產力、複雜且可自訂的 UI，以及應用程式安全性等方面。 由於這些架構支援視覺化設計工具和 UI 標記來快速建立 UI，因此特別適用於企業營運應用程式。

* **Win32 API**。 Win32 API (也稱為 Windows API) 是適用於原生 C/C++ Windows 應用程式的原創平台，而這些應用程式需要直接存取 Windows 和硬體。 它提供了一流的開發體驗，而不需取決於受管理的執行階段環境 (例如 .NET 和 WinRT)。 這讓 Win32 API 成為應用程式的首選平台，因為這些應用程式需要最高階效能及直接存取系統硬體。

本文會更詳細地說明這些平台，並協助您判斷最適合您應用程式的平台。 

> [!NOTE]
> 無論您選擇哪一個應用程式平台，都可以使用通用 Windows 平台 (UWP) 的眾多功能，以在 Windows 10 上的應用程式中提供現代化體驗。 例如，即使您的傳統型應用程式是使用 WPF、Windows Forms 或 Win32 API 建置的，您仍然可以使用 UWP 首次引進的眾多功能，例如 MSIX 套件部署和 UWP XAML 控制項。 如需詳細資訊，請參閱[讓您的傳統型應用程式現代化](modernize/index.md)。

## <a name="uwp"></a>UWP

UWP 是適用於 Windows 10 應用程式和遊戲的先進平台。 它是高度可自訂的平臺，其會使用 XAML 標記來分隔 UI (簡報) 與程式碼 (商務邏輯)。 UWP 適用於需要複雜 UI、樣式自訂和圖形密集案例的傳統型應用程式。 UWP 也提供 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/) 的內建支援，以取得預設的 UX 體驗，並可讓您存取 [Windows 執行階段 (WinRT) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)。 藉由採用 Fluent，UWP 會自動支援一般輸入法，例如筆跡、觸控、遊戲台、鍵盤和滑鼠。

您不僅可以使用 UWP 來建立適用於 Windows 電腦的傳統型應用程式，而且 UWP 也是 Xbox、HoloLens 和 Surface Hub 應用程式唯一支援的平台。 UWP 是我們最先進的應用程式平台。

如需 UWP 的詳細資訊，請參閱 [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)。

## <a name="wpf"></a>WPF

WPF 是針對受管理 Windows 應用程式所建立的平台，可存取完整的 .NET Framework，而且也會使用 XAML 標記來區隔 UI 與程式碼。 此平台是針對需要複雜 UI、樣式自訂和圖形密集案例的傳統型應用程式而設計的。 WPF 開發技能類似於 UWP 開發技能，因此從 WPF 遷移至 UWP 應用程式比從 Windows Forms 進行遷移更容易。

如需 WPF 的詳細資訊，請參閱[開始使用 (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)。

## <a name="windows-forms"></a>Windows Forms

Windows Forms 是適用於受管理 Windows 應用程式的原創平台，具有輕量型 UI 模型並可存取完整的 .NET Framework。 它擅長於讓開發人員快速開始建立應用程式，甚至適用於初次使用平台的開發人員。 這是表單型快速應用程式開發平台，其中包含一組大型內建的視覺效果和非視覺效果拖放控制項集合。 Windows Forms 不會使用 XAML，因此稍後決定將應用程式延伸至 UWP 時，需要完整重新撰寫您的 UI。

如需 Windows Form 的詳細資訊，請參閱[開始使用 Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)。

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
| **有限支援的案例** |  <ul><li>多重視窗支援<sup>1</sup></li><li>支援平台進行輸入驗證<sup>1</sup></li><li>不支援 Windows 7</li><li>某些 UWP API 需要特定的 Windows 10 最低版本</li><li>完整平台支援和命令介面整合 (例如，UWP 目前不支援系統匣整合或完整存取所有裝置)</li><li>直接存取磁碟上的所有檔案</li><li>ADO.NET</li><li>使用非 .NET Standard 或非 Windows 應用程式認證套件相容 API 的現有程式碼基底類別程式庫</li><li>區域網路回送支援 (亦即，如果您的應用程式需要與 localhost 通訊，而不在目標裝置上建立回送豁免)</li><li>密集檔案 I/O</li></ul>     |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li></ul>  |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li><li>可自訂的 UI</li><li>豐富的圖形和使用者體驗 (例如觸控和動畫)</li><li>豐富的視圖和資料模型抽象</li></ul>    |   |

<sup>1</sup> 我們已公開宣佈的功能，可在未來的 Windows 10 版本中解決此案例。

<sup>2</sup> 雖然平台缺少此案例的第一級 API 支援，但開發人員可以透過因應措施來支援此案例。

## <a name="win32"></a>Win32

搭配使用 Win32 API 與 C++ 可讓您實現最高階效能和效率，方法為嚴加控制目標平台與可能位於受管理執行階段環境 (例如 WinRT 和 .NET) 的未受管理程式碼。 不過，對於應用程式執行進行這種層級的控制，需要更加小心謹慎以免出錯，導致開發生產力雖然提高，但執行階段效能卻降低。

以下是 Win32 API 和 C++提供的幾個重點，可讓您建置高效能的應用程式。

-   硬體層級的最佳化，包括嚴格控制資源配置、物件存留期、資料配置、對齊、位元組封裝等。
-   透過內建函式存取效能導向的指令集 (例如 SSE 和 AVX)。
-   使用範本進行有效率的型別安全泛型程式設計。
-   有效率且安全的容器和演算法。
-   DirectX，特別是 Direct3D 和 DirectCompute (請注意，UWP 也提供 DirectX interop)。
-   C++ AMP。

如需詳細資訊，請參閱[使用 Win32 API 的傳統型 Windows 應用程式入門](/windows/desktop/desktop-programming)和[傳統型應用程式技術](/windows/desktop/desktop-app-technologies)。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>適用於傳統型應用程式的 Win32 和 C++

以 C++ 撰寫傳統型應用程式時，您可以選擇適用於 UI 的 Win32 或 MFC，或許多同時支援非 Windows 平台的協力廠商應用程式架構。

-   **Win32：** 這是以控制碼為基礎的 Windows 平台 C 語言 API，包括但不限於 UI 功能，例如視窗化、繪製和 UI 控制項。 因為它是以控制碼為基礎的低階 C 語言 API，所以不常選來建立新式的 UI 密集應用程式。 不過，它會提供與 Windows 平台互動所需的基本 API，並且若應用程式具有簡單的 UI 需求，或只想要 Windows UI 儘可能不要參與 (例如，遊戲)，則它是適合的選項。
-   **MFC (Microsoft Foundation Class 程式庫)：** 這是自 1992 以來提供給 Windows 開發人員的早期應用程式架構和 UI 程式庫。 它是一種精簡的 C++ 包裝函式，架構在以控制碼為基礎的 C 語言 Win32 API 之上，並為許多預先定義的 Windows、通用控制項和其他 windows 物件提供物件導向介面。 雖然 .NET 生態系統中的許多新式 UI 架構都比 MFC 更為便利，但對於為 Windows 桌面建立應用程式的 C++ 許多開發人員而言，仍會選擇它做為原生 UI 架構。
-   **協力廠商應用程式架構：** 由於 C++ 可以在各種不同的平台上執行，且未繫結至 Windows 或 .NET 執行階段，因此協力廠商已為 C++ 開發新的應用程式和 UI 架構，以輕鬆開發具有豐富使用者介面的跨平台應用程式。 其中一些架構會提供自己的外觀及操作，而 wxWidgets 或 Qt 之類的其他架構則會使用或模擬平台的原生控制集。 使用這些程式庫，可以在 Windows 或其他平台 (例如 OSX 或 Linux) 上執行的應用程式版本之間共用幾乎所有應用程式的原始程式碼。

## <a name="other-app-platforms"></a>其他應用程式平台

### <a name="progressive-web-apps-pwas"></a>漸進式 Web 應用程式 (PWA)

PWA 可讓開發人員封裝他們的網站程式碼，以便能夠在 Windows 10 電腦上安裝並且像應用程式一般執行。 如需詳細資訊，請參閱[漸進式 Web 應用程式](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)。

### <a name="xamarin"></a>Xamarin

使用 Xamarin 可建置跨平台應用程式，不只適用於 Windows 10，也可以在 iOS 和 Android 上執行。 如需詳細資訊，請參閱 [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)。
