---
Description: 當您想要建立新的桌面應用程式時，您的第一個決策是使用 Win32 和 COM API 或.NET。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 選擇您的應用程式平台
ms.topic: article
ms.date: 03/18/2019
ms.openlocfilehash: 960dda5e4cb7e8edc1edf7ce2e81da8306555f1c
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984488"
---
# <a name="choose-your-app-platform"></a>選擇您的應用程式平台

當您想要建立新的桌面應用程式的 Windows 電腦時，第一個您決定是要使用哪一個應用程式平台。 Windows 提供四個主要的應用程式平台，各有不同的優點：

* [通用 Windows 平台 (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

所有這些應用程式平台可讓您建立傳統 Windows 桌面和 take 充分善用該環境的特定功能在執行像是 Word、 Excel 和 Photoshop 傳統型應用程式。 不過，部分這些平台共用某些特性，而且更適合特定類型的應用程式：

* **UWP、 WPF 和 Windows Form**。 這些平台會提供許多好處，尤其是在開發人員生產力、 複雜和可自訂的 UI 中，與應用程式的安全性領域的受管理的執行階段環境 （Windows 執行階段 UWP 和.NET 的 Windows Forms 和 WPF）。 這些架構支援快速建立 UI 的視覺化設計工具和 UI 的標記，因為它們是特別適用於特定業務應用程式。

* **Win32 API**。 （也稱為 Windows API） 的 Win32 API 是原始的平台，適用於原生 C /C++需要直接存取 Windows 及硬體的 Windows 應用程式。 它提供絕佳的開發體驗，而不根據如.NET 和 WinRT 的受管理的執行階段環境。 這可讓 Win32 API 需要最高層級的效能和系統硬體的直接存取的應用程式選擇的平台。

本文章說明這些平台，在更多詳細資料，並可協助您判斷最適合您的應用程式。

## <a name="uwp"></a>UWP

UWP 是 Windows 10 應用程式和遊戲的頂尖平台。 它是高度可自訂的平台 UX (presentation) 分開 （商務邏輯） 的程式碼中使用 XAML 標記。 UWP 是適用於桌面應用程式，需要複雜的 UI、 樣式自訂和使用大量圖形的案例。 UWP 也有內建支援[Fluent Design System](/windows/uwp/design/fluent-design-system/)預設 UX 體驗並提供存取權[Windows 執行階段 (WinRT) Api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)。 藉由採用 Fluent，UWP 會自動支援常見的輸入的法，如筆墨、 觸控、 遊戲台、 鍵盤和滑鼠。

您可以使用 UWP 方式，來建立適用於 Windows 電腦的桌面應用程式不僅 UWP 也是唯一支援的平台為 Xbox、 HoloLens、 和 Surface Hub 應用程式。 UWP 是我們的最新、 最新的應用程式平台。

如需 UWP 的詳細資訊，請參閱[開始使用 Windows 10 應用程式](/windows/uwp/get-started/)。

## <a name="wpf"></a>WPF

WPF 是 managed Windows 應用程式存取完整的.NET framework 中，建立平台，它也會使用 XAML 標記來分開的程式碼的 UX。 此平台被設計用於需要複雜的 UI、 樣式自訂及圖形密集的案例的桌面應用程式。 從 WPF 移轉至 UWP 應用程式遠比從 Windows Form 的移轉之後，很類似於 UWP 開發技能，WPF 開發技能。

如需有關 WPF 的詳細資訊，請參閱[入門 (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)。

## <a name="windows-forms"></a>Windows Forms

Windows Form 是 managed Windows 應用程式與輕量型 UI 模型及存取完整的.NET Framework 的原始平台。 它擅長讓開發人員能夠快速地開始建置應用程式，甚至是適用於平台的新手的開發人員。 這是與大型的內建集合的視覺和非視覺化的拖放控制項的表單型、 快速的應用程式開發平台。 Windows Form 不會使用 XAML，以便稍後決定要擴充至 UWP 應用程式需要完整重新寫入您的 UI。

如需有關 Windows Form 的詳細資訊，請參閱 <<c0> [ 開始使用 Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>平台比較：UWP、 WPF 和 Windows Form

下表比較詳細的 Windows Form、 WPF 和 UWP 的各種特性。

| 功能或案例  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **支援的版本**      |  Windows 10   |  Windows 7 及更新版本 |  Windows 7 及更新版本  |
| **語言**      |   C\#， C++/WinRT， C++/CX、 VB、 JavaScript   |  C\#， C++/CLI (Managed Extensions for C++)，F\#，VB |  C\#， C++/CLI (Managed Extensions for C++)，F\#，VB   |
| **UI 的執行階段** |    原生 (C++/WinRT 和C++/CX) 和 managed (.NET Native)  |  Managed (.NET Framework)<br/><br/>即將推出的.NET Core 3 的支援  |   Managed (.NET Framework)<br/><br/>即將推出的.NET Core 3 的支援    |
| **開放原始碼** | [是 （僅 Windows UI 庫）](https://github.com/Microsoft/microsoft-ui-xaml)  |  [是 (僅限.NET Core)](https://github.com/dotnet/wpf) | [是 (僅限.NET Core)](https://github.com/dotnet/winforms)  |
| **支援 XAML** |   是   |  是  |   否   |
| **優點**  |  <ul><li>UI 的 XAML 標記</li><li>豐富且可自訂的 UX</li><li>您現有的程式碼基底是.NET Standard 相容</li><li>高 DPI 支援</li><li>支援多個輸入類型跨 Windows 裝置 （包括觸控、 畫筆、 遊戲台、 滑鼠及鍵盤）</li><li>Xbox、 HoloLens、 IoT 或 Surface Hub 的支援</li><li>原生支援C++</li><li>最佳化的電池壽命</li><li>現代的協助工具支援 （例如螢幕助讀程式）</li><li>Rtf 文字資料功能 （例如內建的拼字檢查）</li><li>筆跡支援</li><li>保護透過應用程式容器的執行 （例如，未受信任的內容已沙箱化）</li></ul>  |  <ul><li>UI 的 XAML 標記</li><li>豐富且可自訂的 UX</li><li>大量的 Microsoft 和合作夥伴的控制項集合</li><li>密集的 UI</li><li>Windows 7 的支援</li><li>平台支援的輸入驗證</li></ul> | <ul><li>快速應用程式開發</li><li>建置 UI 的 WYSIWYG 編輯器</li><li>大量的 Microsoft 和合作夥伴的控制項集合</li><li>密集的 UI</li><li>Windows 7 的支援</li><li>鍵盤和滑鼠輸入</li></ul>          |
| **支援有限的案例** |  <ul><li>密集的 UI （建立密集 UI 需要自訂樣式）<sup>1</sup></li><li>支援多個視窗<sup>1</sup></li><li>輸入驗證的平台支援<sup>1</sup></li><li>不支援 Windows 7</li><li>某些 UWP Api 需要特定的 Windows 10 的最小版本</li><li>完整支援和殼層整合的平台 （例如，UWP 目前不支援系統匣整合或完整存取權的所有裝置）</li><li>直接存取磁碟上的所有檔案</li><li>ADO.NET</li><li>使用非-.NET Standard 的現有程式碼基底類別程式庫或非-Windows 應用程式認證套件相容的 Api</li><li>區域網路回送支援 （亦即，如果您的應用程式需要與本機主機通訊，而不需要在目標裝置上建立回送豁免）</li><li>大量檔案 I/O</li></ul>     |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li></ul>  |  <ul><li>高 DPI 支援<sup>2</sup></li><li>觸控輸入<sup>2</sup></li><li>可自訂的 UI</li><li>豐富的圖形和使用者體驗 （例如觸控和動畫）</li><li>檢視和資料模型的豐富的抽象概念</li></ul>    |   |

<sup>1</sup>我們已公開發表會解決此案例的未來版本的 Windows 10 的功能。

<sup>2</sup>雖然在平台缺少第一級的 API 支援此案例中，開發人員可支援此案例中的因應措施。

## <a name="win32"></a>Win32

使用 Win32 APIC++讓您能夠進一步控制與 unmanaged 程式碼的目標平台比在 WinRT 等.NET 受管理的執行階段環境達到最高的層級的效能和效率。 不過，運用這種層級的掌控您的應用程式執行，必須更小心，要取得正確的注意及往來開發產能，以執行階段效能。

以下是幾個重點的 Win32 API 和C++提供可讓您建置高效能的應用程式。

-   硬體層級最佳化，包括控制資源配置、 物件存留期、 資料配置、 對齊方式、 位元組封裝，以及更多。
-   透過內建函式，例如 SSE 和 AVX 設定存取效能導向的指示。
-   使用範本的有效率、 類型安全泛型程式設計。
-   有效且安全的容器和演算法。
-   特定 Direct3D 和 DirectCompute （請注意 UWP 也提供 DirectX interop） 中的 DirectX。
-   C++P。

如需詳細資訊，請參閱 <<c0> [ 傳統型 Windows 應用程式使用 Win32 API 的快速入門](/windows/desktop/desktop-programming)並[傳統型應用程式技術](/windows/desktop/desktop-app-technologies)。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 和C++的傳統桌面應用程式

在中撰寫的桌面應用程式時C++，您可以選擇 Win32 或 MFC 的 UI 或也支援非 Windows 平台的第三方應用程式架構的主機。

-   **Win32:** 這是 Windows 平台，包括但不是限於 UI 功能，例如視窗、 繪圖和 UI 控制項的控制代碼為主、 以 C 語言 API。 因為它是根據控制代碼的低層級，與 C 語言 API，它會是用於建立現代、 需要大量 UI 的應用程式不頻繁的選擇。 不過，它會提供與 Windows 平台互動所需的基本 Api，而且是適合應用程式具有簡單的 UI 需求，或只想要的 Windows UI，比方說，保持最大開的遊戲。
-   **MFC （Microsoft Foundation 類別程式庫）：** 這是德高望重的應用程式架構和具有 1992 提供 Windows 開發人員的 UI 程式庫。 它是精簡型C++控制代碼為主、 以 C 語言的 Win32 api 的包裝函式，並提供許多預先定義的 windows、 常見控制項和其他 Windows 物件的物件導向的介面。 雖然許多現代 UI 架構，在.NET 生態系統中的，大於 MFC 在為了方便起見，它仍然是原生 UI 架構提供了許多的選擇的C++建立適用於 Windows 桌面應用程式的開發人員。
-   **第三方應用程式架構：** 因為C++可以在各種平台上執行，並不會繫結至 Windows 或.NET 執行階段，協力廠商已經開發新的應用程式和 UI 架構，如C++為了簡化開發具有豐富的使用者介面的跨平台應用程式。 這些架構的一些提供自己的外觀與風格，而 wxWidgets 或 Qt 等其他人使用，或模擬平台的原生控制項集合。 使用這些程式庫，就可以共用幾乎所有的應用程式在 Windows 或其他平台，例如 OSX 或 Linux 執行的版本之間的應用程式的原始程式碼。

## <a name="other-app-platforms"></a>其他應用程式平台

### <a name="progressive-web-apps-pwas"></a>革新型 Web 應用程式 (Pwa)

Pwa 可讓他們的網站程式碼，使它可以安裝並在 Windows 10 電腦上執行等應用程式的開發人員套件。 如需詳細資訊，請參閱 <<c0> [ 漸進式 Web 應用程式](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)。

### <a name="xamarin"></a>Xamarin

使用 Xamarin 建置跨平台應用程式，也可以在 iOS 執行的 Windows 10 和 Android。 如需詳細資訊，請參閱 < [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)。
