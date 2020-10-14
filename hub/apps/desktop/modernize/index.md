---
Description: 新增新式 XAML 使用者介面、建立 MSIX 套件，以及將其他新式元件併入您的傳統型應用程式。
title: 讓您適用於 Windows 的傳統型應用程式現代化
ms.topic: article
ms.date: 10/02/2020
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: b7c640e89f36dfcea6e0080cbbe3887e0231ddfc
ms.sourcegitcommit: 27552ed7d3d889f50d8e01776a24b8d486a8d97c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2020
ms.locfileid: "91958721"
---
# <a name="modernize-your-desktop-apps"></a>讓您的傳統型應用程式現代化

Windows 10 和通用 Windows 平台 (UWP) 提供許多功能，您可以用來在您的傳統型應用程式中傳遞新式體驗。 這些功能大部分都可作為模組化元件，您可以依照您自己的步調在傳統型應用程式中採用，不需要為了不同的平台而重新撰寫您的應用程式。 您可以藉由選擇要採用哪些部分的 Windows 10 和 UWP，來增強您現有的傳統型應用程式。

本文說明您現在可以在您的傳統型應用程式中使用的 Windows 10 和 UWP 功能。 有關示範如何將現有的應用程式現代化，以使用本文中所述許多功能的教學課程，請參閱[將 WPF 應用程式現代化](modernize-wpf-tutorial.md)教學課程。

> [!NOTE]
> 您是否需要將傳統型應用程式遷移到 Windows 10 方面的協助？ [桌面應用程式保證](/FastTrack/win-10-desktop-app-assure)服務為將應用程式移植到 Windows 10 的開發人員提供直接、無成本的支援。 這個方案適用於所有 ISV 和符合資格的企業。 如需方案本身資格的詳細資訊，請造訪 [/fasttrack/win-10-app-assure-assistance-offered](/fasttrack/win-10-app-assure-assistance-offered)。 若要立即開始使用，請[提交您的要求](https://fasttrack.microsoft.com/dl/daa)。

## <a name="windows-ui-library"></a>Windows UI 程式庫

Windows UI 程式庫一組 NuGet 套件，可提供 Windows 10 應用程式的控制項和其他使用者介面元素。 WinUI 一開始是以工具組的形式提供，旨在針對舊版 Windows 10 提供適用於 UWP 應用程式的全新或更新版 WinRT XAML 控制項。 WinUI 已擴大範圍，現在是適用於 UWP、.NET 和原生 Win32 上 Windows 10 應用程式的新式原生使用者介面 (UI) 平台。

您可以透過下列方式在桌面應用程式中使用 WinUI：

* 您可以將現有的 WPF、Windows Forms 和 C++/Win32 應用程式更新為使用 [XAML Islands](xaml-islands.md)，以在應用程式中裝載 WinUI 2.x 控制項。
* 從 [WinUi 3.0 預覽版 1](../../winui/winui3/index.md) 開始，您可以建立 [.NET 和 C++ /Win32 應用程式來使用完全以 WinUI 為基礎的 UI](../../winui/winui3/get-started-winui3-for-desktop.md)。

請參閱 [Windows UI (WinUI) 程式庫](../../winui/index.md)。

## <a name="msix-packages"></a>MSIX 套件

MSIX 是新式 Windows 應用程式套件格式，為所有 Windows 應用程式提供通用封裝體驗，包括 UWP、WPF、Windows Forms 及 Win32 應用程式。 MSIX 綜合 MSI、.appx、App-V 和 ClickOnce 安裝技術的優勢，以提供新式且可靠的封裝體驗。

將您的傳統型 Windows 應用程式封裝在 MSIX 套件中，可讓您存取強固的安裝和更新體驗、具有彈性功能系統的受控安全性模型、Microsoft Store 的支援、企業管理，以及許多自訂散發模型。

如需詳細資訊，請參閱 MSIX 文件中的[封裝傳統型應用程式](/windows/msix/desktop/desktop-to-uwp-root)。

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 是 .NET Core 的最新主要版本。 這個版本的重點是支援 Windows 傳統型應用程式，包括 Windows Forms 和 WPF 應用程式。 您可以在 .NET Core 3 上執行新的和現有的 Windows 傳統型應用程式，並享有 .NET Core 帶來的所有優點。 裝載於 [XAML Islands](xaml-islands.md) 中的 WinRT XAML 控制項，也可以在目標為 .NET Core 3 的 Windows Forms 和 WPF 應用程式中使用。

如需詳細資訊，請參閱 [.NET Core 3.0 的新功能](/dotnet/core/whats-new/dotnet-core-3-0)。

## <a name="windows-runtime-apis"></a>Windows 執行階段 API

您可以在您的 WPF、Windows Forms 或 C++ Win32 傳統型應用程式中直接呼叫許多 Windows 執行階段 API，整合為 Windows 10 使用者帶來好處的新式體驗。 例如，您可以呼叫 Windows 執行階段 API 以將快顯通知新增至您的傳統型應用程式。

如需詳細資訊，請參閱[在傳統型應用程式中使用 Windows 執行階段 API](desktop-to-uwp-enhance.md)。

## <a name="host-winrt-xaml-controls-xaml-islands"></a>主機 WinRT XAML 控制項 (XAML Islands)

從 Windows 10 1903 版開始，您可以直接將 [UWP XAML 控制項](/windows/uwp/design/controls-and-patterns/controls-by-function)新增至 WPF、Windows Forms 或 C++ Win32 應用程式的任何 UI 元素 (與視窗控制代碼 (HWND) 相關聯)。 這表示您可以將最新的 UWP 功能 (例如 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions))，與支援 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index)的控制項，完全整合至傳統型應用程式中的視窗和其他顯示表面。 此開發人員案例有時候稱為 XAML islands。

如需詳細資訊，請參閱[傳統型應用程式中的 WinRT XAML 控制項](xaml-islands.md)

## <a name="use-the-visual-layer-in-desktop-apps"></a>在傳統型應用程式中使用視覺層

您現在可以在非 UWP 傳統型應用程式中使用 Windows 執行階段 API，來增強您的 WPF、Windows Forms 和 C++ Win32 應用程式的外觀、風格及功能，並且利用只能透過 UWP 取用的最新 Windows 10 UI 功能。 當您需要建立自訂體驗，超越使用 XAML Islands 可以裝載的內建 WinRT XAML 控制項時，這個方式非常有用。

如需詳細資訊，請參閱[使用視覺層讓您的傳統型應用程式現代化](visual-layer-in-desktop-apps.md)。

## <a name="additional-features-available-to-apps-with-package-identity"></a>具有套件身分識別之應用程式可用的其他功能

部分新式 Windows 10 體驗只能在具有[套件身分識別](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)的傳統型應用程式中獲得。 這些功能包括特定 Windows 執行階段 API、套件擴充功能和 UWP 元件。 如需詳細資訊，請參閱[需要套件身分識別的功能](modernize-packaged-apps.md)。

將身分識別授與傳統型應用程式有數種方式：

* 將其封裝在 [MSIX 套件](/windows/msix/desktop/desktop-to-uwp-root)中。 MSIX 是新式應用程式套件格式，為所有 Windows 應用程式、UWP、WPF、Windows Forms 及 Win32 應用程式提供通用封裝體驗。 這可以提供強固的安裝和更新體驗、具有彈性功能系統的受控安全性模型、Microsoft Store 的支援、企業管理，以及許多自訂散發模型。 如需詳細資訊，請參閱 MSIX 文件中的[封裝傳統型應用程式](/windows/msix/desktop/desktop-to-uwp-root)。
* 如果您無法採用 MSIX 封裝來部署傳統型應用程式，請從 Windows 10 版本 2004 開始，藉由建立僅包含套件資訊清單的*疏鬆 MSIX 套件*來授與套件識別。 如需詳細資訊，請參閱[將身分識別授與非封裝的傳統型應用程式](grant-identity-to-nonpackaged-apps.md)。

<a id="desktop-uwp-controls"></a>

## <a name="winrt-xaml-controls-optimized-for-desktop-apps"></a>針對傳統型應用程式最佳化的 WinRT XAML 控制項

不論您是要建置完全以桌面裝置系列作為目標的 WinRT XAML 應用程式，或是要在 WPF、Windows Forms 或 C++ Win32 傳統型應用程式中使用 WinRT XAML 控制項，下列新的和更新的 UWP 控制項的設計目的，是透過 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index)提供桌面最佳化體驗。 這些控制項是在 Windows 10 1809 版 (2018 年 10 月更新，或 10.0.17763 版) 中導入。

| 控制 |  說明 |
|------ |--------------|
| [MenuBar](/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | 針對可能需要比 **CommandBar** 所允許更多組織或群組的應用程式，提供一種快速且簡單的方法來公開一組命令。 |
| [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | 將＞形箭號顯示為視覺指標，具有包含許多選項的附加飛出視窗。  |
| [SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | 提供一個按鈕，具有可以分別叫用的兩個組件。 一個組件的行為類似標準按鈕，並且會叫用立即動作。 另一個組件會叫用飛出視窗，其中包含使用者可以選擇的其他選項。|
| [ToggleSplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | 提供一個按鈕，具有可以分別叫用的兩個組件。 一個組件的行為類似可以開或關的切換按鈕。 另一個組件會叫用飛出視窗，其中包含使用者可以選擇的其他選項。 |
| [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  可讓您在 UI 畫布上的項目內容中顯示一般使用者工作。 |
| [ComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | 您現在可以讓下拉式方塊變成可編輯，讓使用者可以輸入未列在控制項中的值。  |
| [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) | 您現在可以設定樹狀檢視，以啟用資料繫結、項目範本及拖放。  |
| [DataGridView](/windows/communitytoolkit/controls/datagrid) |   可讓您靈活顯示資料列和資料行中的資料集合。 這個控制項可於 [Windows 社群工具組](/windows/uwpcommunitytoolkit/) (英文) 中取得。  |

## <a name="other-technologies-for-modern-desktop-apps"></a>新式傳統型應用程式的其他技術

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph 是 API 的集合，您可以用來為與數百萬使用者資料互動的組織和取用者建置應用程式。 Microsoft Graph 會公開 REST API 和用戶端程式庫，以存取下列位置上的資料：
* Azure Active Directory
* Microsoft 365 Office 應用程式：SharePoint、OneDrive、Outlook/Exchange、Microsoft Teams、OneNote、Planner 及 Excel
* Enterprise Mobility + Security 服務：Identity Manager、Intune、Advanced Threat Analytics 及進階威脅防護。
* Windows 10 服務：活動和裝置

如需詳細資訊，請參閱 [Microsoft Graph 文件](/graph/overview) (英文)。

### <a name="adaptive-cards"></a>調適型卡片

調適型卡片是開放、跨平台架構，您可以用來在裝置和平台之間以通用且一致的方式交換卡片型 UI 內容。

如需詳細資訊，請參閱[調適型卡片文件](/adaptive-cards/)。
