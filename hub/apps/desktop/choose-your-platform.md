---
Description: 當您想要建立新的桌面應用程式時，您所做的第一個決定是要使用 Win32 和 COM API 或 .NET。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 選擇您的應用程式平台
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316995"
---
# <a name="choose-your-app-platform"></a>選擇您的應用程式平台

當您想要為 Windows 電腦建立新的桌面應用程式時，您所做的第一個決定是要使用哪個應用程式平臺。 Windows 提供四個主要的應用程式平臺，各有不同的優點：

* [通用 Windows 平台 (UWP)](#uwp)
* [WPF （.NET）](#wpf)
* [Windows Forms （.NET）](#windows-forms)
* [32](#win32)

所有這些應用程式平臺都可讓您建立在傳統 Windows 桌面上執行的桌面應用程式（例如 Word、Excel 和 Photoshop），並充分利用該環境的特定功能。 不過，其中有些平臺會共用一些特性，而且較適合特定類型的應用程式：

* **UWP、WPF 和 Windows Forms**。 這些平臺提供受控執行時間環境（適用于 UWP 的 Windows 執行階段，以及適用于 Windows Forms 和 WPF 的 .NET），有許多優點，特別是在開發人員生產力、複雜且可自訂的 UI，以及應用程式安全性等方面。 由於這些架構支援視覺化設計工具和 UI 標記來快速建立 UI，因此特別適用于企業營運應用程式。

* **WIN32 API**。 WIN32 API （也稱為 Windows API）是原生 C/C++ Windows 應用程式的原始平臺，需要直接存取 Windows 和硬體。 它提供了一流的開發體驗，而不需視 managed 執行時間環境（例如 .NET 和 WinRT）而定。 這讓 WIN32 API 成為需要最高層級效能及直接存取系統硬體之應用程式的理想平臺。

這篇文章會更詳細地說明這些平臺，並協助您判斷最適合您的應用程式。 

> [!NOTE]
> 無論您選擇哪一個應用程式平臺，您都可以使用通用 Windows 平臺（UWP）的許多功能，在 Windows 10 上的應用程式中提供現代化體驗。 例如，即使您的桌面應用程式是使用 WPF、Windows Forms 或 WIN32 API 建立的，您仍然可以使用 UWP 首次引進的許多功能，例如 MSIX 套件部署和 UWP XAML 控制項。 如需詳細資訊，請參閱[將您的桌面應用程式現代化](modernize/index.md)。

## <a name="uwp"></a>UWP

UWP 是 Windows 10 應用程式和遊戲的頂尖平臺。 它是高度可自訂的平臺，會使用 XAML 標記來分隔 UX （簡報）與程式碼（商務邏輯）。 UWP 適用于需要複雜 UI、樣式自訂和圖形密集案例的桌面應用程式。 UWP 也有針對預設 UX 體驗的[流暢設計系統](/windows/uwp/design/fluent-design-system/)內建支援，並可讓您存取[Windows 執行階段（WinRT） api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)。 藉由採用流暢的，UWP 會自動支援一般輸入法，例如筆墨、觸控、遊戲台、鍵盤和滑鼠。

您不僅可以使用 UWP 來建立適用于 Windows 電腦的桌面應用程式，而且 UWP 也是 Xbox、HoloLens 和 Surface Hub 應用程式唯一支援的平臺。 UWP 是我們最新的領先業界應用程式平臺。

如需 UWP 的詳細資訊，請參閱[開始使用 Windows 10 應用程式](/windows/uwp/get-started/)。

## <a name="wpf"></a>WPF

WPF 是針對受控 Windows 應用程式所建立的平臺，可存取完整的 .NET Framework，而且它也會使用 XAML 標記來分隔 UX 和程式碼。 此平臺是針對需要複雜 UI、樣式自訂和圖形密集案例的桌面應用程式所設計。 WPF 開發技能類似于 UWP 開發技能，因此從 WPF 遷移至 UWP 應用程式比從 Windows Forms 進行遷移更容易。

如需 WPF 的詳細資訊，請參閱使用者[入門（WPF）](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)。

## <a name="windows-forms"></a>Windows Forms

Windows Forms 是受控 Windows 應用程式的原始平臺，具備輕量 UI 模型和完整 .NET Framework 的存取權。 它的擅長是讓開發人員快速開始建立應用程式，甚至是對平臺新手的開發人員。 這是以表單為基礎且快速的應用程式開發平臺，其中包含一組大型內建的視覺效果和非視覺效果拖放控制項。 Windows Forms 不會使用 XAML，所以稍後決定將應用程式延伸至 UWP，需要完整重新撰寫您的 UI。

如需 Windows Forms 的詳細資訊，請參閱[開始使用 Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>平臺比較：UWP、WPF 和 Windows Forms

下表詳細比較 Windows Forms、WPF 和 UWP 的各種特性。

| 功能或案例  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **支援的版本**      |  Windows 10   |  Windows 7 和更新版本 |  Windows 7 和更新版本  |
| **語言**      |   C\#、 C++/WinRT、 C++/cx、VB、JavaScript   |  C\#、 C++/cli （適用于的C++Managed 延伸模組\#）、F、VB |  C\#、 C++/cli （適用于的C++Managed 延伸模組\#）、F、VB   |
| **UI 執行時間** |    原生C++（/WinRT C++和/cx）和 managed （.NET Native）  |  受控（.NET Framework 和 .NET Core 3） |   受控（.NET Framework 和 .NET Core 3）   |
| **開放原始碼** | [是（僅限 Windows UI 程式庫）](https://github.com/Microsoft/microsoft-ui-xaml)  |  [是（僅限 .NET Core）](https://github.com/dotnet/wpf) | [是（僅限 .NET Core）](https://github.com/dotnet/winforms)  |
| **支援 XAML** |   是   |  是  |   否   |
| **優缺點**  |  <ul><li>UI 的 XAML 標記</li><li>豐富且可自訂的 UX</li><li>您現有的程式碼基底 .NET Standard 相容</li><li>高 DPI 支援</li><li>支援跨 Windows 裝置的多種輸入類型（包括觸控、畫筆、遊戲台、滑鼠和鍵盤）</li><li>支援 Xbox、HoloLens、IoT 或 Surface Hub</li><li>Native 的支援C++</li><li>優化電池壽命</li><li>新式協助工具支援（例如螢幕閱讀者）</li><li>豐富文字資料功能（例如內建的拼寫檢查）</li><li>筆跡支援</li><li>透過應用程式容器保護執行安全（例如，未受信任的內容已沙箱化）</li></ul>  |  <ul><li>UI 的 XAML 標記</li><li>豐富且可自訂的 UX</li><li>由 Microsoft 和合作夥伴組成的大型控制項集合</li><li>密集 UI</li><li>Windows 7 支援</li><li>輸入驗證的平臺支援</li></ul> | <ul><li>快速應用程式開發</li><li>用於建立 UI 的 WYSIWYG 編輯器</li><li>由 Microsoft 和合作夥伴組成的大型控制項集合</li><li>密集 UI</li><li>Windows 7 支援</li><li>鍵盤和滑鼠輸入</li></ul>          |
| **支援有限的案例** |  <ul><li>密集 UI （建立密集 UI 需要自訂樣式）<sup>1</sup></li><li>多個視窗支援<sup>1</sup></li><li>輸入驗證的平臺支援<sup>1</sup></li><li>不支援 Windows 7</li><li>某些 UWP Api 需要特定的 Windows 10 最低版本</li><li>完整平臺支援和 shell 整合（例如，UWP 目前不支援系統匣整合或所有裝置的完整存取）</li><li>直接存取磁片上的所有檔案</li><li>ADO.NET</li><li>使用 non-.NET 標準或非 Windows 應用程式認證套件相容 Api 的現有程式碼基底類別程式庫</li><li>區域網路回送支援（也就是，如果您的應用程式需要在目標裝置上建立回送豁免，就必須與 localhost 通訊）</li><li>大量檔案 i/o</li></ul>     |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li></ul>  |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li><li>可自訂的 UI</li><li>豐富的圖形和使用者體驗（例如觸控和動畫）</li><li>豐富的視圖和資料模型抽象概念</li></ul>    |   |

<sup>1</sup>我們已公開宣佈的功能，將在未來的 Windows 10 版本中解決此案例。

<sup>2</sup>雖然平臺缺少此案例的第一級 API 支援，但開發人員可以透過因應措施來支援此案例。

## <a name="win32"></a>Win32

搭配使用 WIN32 API， C++可讓您以不受管理的執行時間環境（例如 WinRT 和 .net）為目標平臺進行更多的控制，來達到最高層級的效能和效率。 不過，對應用程式執行進行這種控制層級的處理，需要更多的護理和注意，並提高執行時間效能的開發產能。

以下是 WIN32 API 和C++提供的幾個重點，可讓您建立高效能的應用程式。

-   硬體層級的優化，包括對資源配置、物件存留期、資料配置、對齊、位元組封裝等緊密控制。
-   透過內建函式存取以效能導向的指令集（如 SSE 和 AVX）。
-   有效率的型別安全一般程式設計，使用範本。
-   有效率且安全的容器和演算法。
-   DirectX，特別是 Direct3D 和 DirectCompute （請注意，UWP 也提供 DirectX interop）。
-   C++服務與支援.

如需詳細資訊，請參閱開始使用使用 WIN32 API 和[桌面應用程式技術](/windows/desktop/desktop-app-technologies)的[桌面 Windows 應用程式](/windows/desktop/desktop-programming)。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 和C++傳統桌面應用程式

在中C++撰寫桌面應用程式時，您可以選擇 UI 的 WIN32 或 MFC，或同時支援非 Windows 平臺的協力廠商應用程式架構主機。

-   **32**這是 Windows 平臺的以控制碼為基礎的 C 語言 API，包括但不限於 UI 功能，例如視窗化、繪製和 UI 控制項。 因為它是以控制碼為基礎的低層級 C 語言 API，所以不常選擇建立現代化的 UI 密集型應用程式。 不過，它提供與 Windows 平臺互動所需的基本 Api，而且適合具有簡單 UI 需求的應用程式，或只想要讓 Windows UI 盡可能保持不變的情況（例如遊戲）的最佳選擇。
-   **MFC （MFC 程式庫）：** 這是自1992以來提供給 Windows 開發人員的早期應用程式架構和 UI 程式庫。 它是以句C++柄為基礎之 C 語言 WIN32 API 的精簡型包裝函式，並為許多預先定義的 windows、通用控制項和其他 windows 物件提供物件導向介面。 雖然 .NET 生態系統中的許多新式 UI 架構都比 MFC 更方便，但對於為 Windows 桌面建立應用程式的C++許多開發人員而言，它仍是選擇的原生 ui 架構。
-   **協力廠商應用程式架構：** 由於C++可以在各種不同的平臺上執行，且未系結至 Windows 或 .net 執行時間，因此協力廠商已開發新的C++應用程式和 UI 架構，以輕鬆開發具有豐富使用者介面的跨平臺應用程式。 其中一些架構會提供自己的外觀 & 風格，而 wxWidgets 或 Qt 之類的其他架構則會使用或模擬平臺的原生控制集。 使用這些程式庫，可以在 Windows 或其他平臺（例如 OSX 或 Linux）上執行的應用程式版本之間共用幾乎所有應用程式的原始程式碼。

## <a name="other-app-platforms"></a>其他應用程式平臺

### <a name="progressive-web-apps-pwas"></a>漸進式 Web Apps （Pwa）

Pwa 可讓開發人員封裝他們的網站程式碼，以便在 Windows 10 電腦上安裝並執行應用程式。 如需詳細資訊，請參閱[漸進式 Web Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)。

### <a name="xamarin"></a>Xamarin

使用 Xamarin 來建立 Windows 10 的跨平臺應用程式，也可以在 iOS 和 Android 上執行。 如需詳細資訊，請參閱[Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)。
