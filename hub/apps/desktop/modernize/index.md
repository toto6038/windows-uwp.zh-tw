---
Description: 新增新式 XAML 使用者介面、建立 MSIX 套件，以及將其他新式元件併入您的傳統型應用程式。
title: 讓您適用於 Windows 的傳統型應用程式現代化
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 6153a0a094d03081388c15ec31696ef277ef7081
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215208"
---
# <a name="modernize-your-desktop-apps"></a>讓您的傳統型應用程式現代化

Windows 10 和通用 Windows 平台 (UWP) 提供許多功能，您可以用來在您的傳統型應用程式中傳遞新式體驗。 這些功能大部分都可作為模組化元件，您可以依照您自己的步調在傳統型應用程式中採用，不需要為了不同的平台而重新撰寫您的應用程式。 您可以藉由選擇要採用哪些部分的 Windows 10 和 UWP，來增強您現有的傳統型應用程式。

本文說明您現在可以在您的傳統型應用程式中使用的 Windows 10 和 UWP 功能。

> [!NOTE]
> 您是否需要將傳統型應用程式遷移到 Windows 10 方面的協助？ [桌面應用程式保證](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure)服務為將應用程式移植到 Windows 10 的開發人員提供直接、無成本的支援。 這個方案適用於所有 ISV 和符合資格的企業。 如需方案本身資格的詳細資訊，請造訪 [https://aka.ms/DesktopAppAssure](https://aka.ms/DesktopAppAssure)。 若要立即開始使用，請[提交您的要求](https://aka.ms/DesktopAppAssureRequest)。

## <a name="msix-packages"></a>MSIX 套件

MSIX 是新式 Windows 應用程式套件格式，為所有 Windows 應用程式提供通用封裝體驗，包括 UWP、WPF、Windows Forms 及 Win32 應用程式。 MSIX 綜合 MSI、.appx、App-V 和 ClickOnce 安裝的優勢，建置成為安全、受到保護且可靠的格式。

將您的傳統型 Windows 應用程式封裝在 MSIX 套件中，可讓您存取強固的安裝和更新體驗、具有彈性功能系統的受控安全性模型、Microsoft Store 的支援、企業管理，以及許多自訂散發模型。

如需詳細資訊，請參閱 MSIX 文件中的[封裝傳統型應用程式](/windows/msix/desktop/desktop-to-uwp-root)。

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 是 .NET Core 的下一個主要版本。 這個即將推出版本的亮點是支援 Windows 傳統型應用程式，包括 Windows Forms 和 WPF 應用程式。 您將能夠在 .NET Core 3 上執行新的和現有的 Windows 傳統型應用程式，並且享用 .NET Core 帶來的所有優點。 裝載於 [XAML Islands](xaml-islands.md) 中的 UWP 控制項，也可以在目標為 .NET Core 3 的 Windows Forms 和 WPF 應用程式中使用。

如需詳細資訊，請參閱下列文章：

* [.NET Core 3.0 Preview 1 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/) (英文)
* [.NET Core 3.0 Preview 2 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/) (英文)
* [.NET Core 3.0 Preview 2 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/) (英文)
* [.NET Core 3.0 Preview 4 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/) (英文)
* [.NET Core 3.0 (Preview 2) 的新功能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)。

## <a name="uwp-apis"></a>UWP API

您可以在您的 WPF、Windows Forms 或 C++ Win32 傳統型應用程式中直接呼叫許多 UWP API，整合為 Windows 10 使用者帶來好處的新式體驗。 例如，您可以呼叫 UWP API 以將快顯通知新增至您的傳統型應用程式。

如需詳細資訊，請參閱[在傳統型應用程式中使用 UWP API](desktop-to-uwp-enhance.md)。

## <a name="host-uwp-controls-xaml-islands"></a>主機 UWP 控制項 (XAML Islands)

從 Windows 10 1903 版開始，您可以直接將 [UWP XAML 控制項](/windows/uwp/design/controls-and-patterns/controls-by-function)新增至 WPF、Windows Forms 或 C++ Win32 應用程式的任何 UI 元素 (與視窗控制代碼 (HWND) 相關聯)。 這表示您可以將最新的 UWP 功能 (例如 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions))，與支援 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index)的控制項，完全整合至傳統型應用程式中的視窗和其他顯示表面。 此開發人員案例有時候稱為 XAML islands。

如需詳細資訊，請參閱[傳統型應用程式中的 UWP 控制項](xaml-islands.md)

## <a name="use-the-visual-layer-in-desktop-apps"></a>在傳統型應用程式中使用視覺層

您現在可以在非 UWP 傳統型應用程式中使用 UWP API，來增強您的 WPF、Windows Forms 和 C++ Win32 應用程式的外觀、風格及功能，並且利用只能透過 UWP 取用的最新 Windows 10 UI 功能。 當您需要建立自訂體驗，超越使用 XAML Islands 可以裝載的內建 UWP 控制項時，這個方式非常有用。

如需詳細資訊，請參閱[使用視覺層讓您的傳統型應用程式現代化](visual-layer-in-desktop-apps.md)。

## <a name="additional-features-available-to-packaged-apps"></a>封裝應用程式可用的其他功能

部分新式 Windows 10 體驗只能在封裝於 [MSIX 套件](/windows/msix/desktop/desktop-to-uwp-root)中的傳統型應用程式中獲得。 如果您將您的傳統型應用程式封裝於 MSIX 套件中，可以使用 UWP API，它需要已封裝應用程式中的套件身分識別、套件擴充功能及 UWP 元件。

如需詳細資訊，請參閱[需要套件身分識別的功能](modernize-packaged-apps.md)。

<a id="desktop-uwp-controls"/>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>針對傳統型應用程式最佳化的 UWP 控制項

不論您是要建置完全以桌面裝置系列作為目標的 UWP 應用程式，或是要在 WPF、Windows Forms 或 C++ Win32 傳統型應用程式中使用 UWP 控制項，下列新的和更新的 UWP 控制項的設計目的，是透過 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index)提供桌面最佳化體驗。 這些控制項是在 Windows 10 1809 版 (2018 年 10 月更新，或 10.0.17763 版) 中導入。

| 控制項 |  描述 |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | 提供快速、簡單的方法，公開可能需要比 **CommandBar** 所允許還要多組織和群組的一組應用程式命令。 |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | 將＞形箭號顯示為視覺指標，具有包含許多選項的附加飛出視窗。  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | 提供一個按鈕，具有可以分別叫用的兩個組件。 一個組件的行為類似標準按鈕，並且會叫用立即動作。 另一個組件會叫用飛出視窗，其中包含使用者可以選擇的其他選項。|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | 提供一個按鈕，具有可以分別叫用的兩個組件。 一個組件的行為類似可以開或關的切換按鈕。 另一個組件會叫用飛出視窗，其中包含使用者可以選擇的其他選項。 |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  可讓您在 UI 畫布上的項目內容中顯示一般使用者工作。 |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | 您現在可以讓下拉式方塊變成可編輯，讓使用者可以輸入未列在控制項中的值。  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | 您現在可以設定樹狀檢視，以啟用資料繫結、項目範本及拖放。  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   可讓您靈活顯示資料列和資料行中的資料集合。 這個控制項可於 [Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) (英文) 中取得。  |

## <a name="windows-ui-library"></a>Windows UI 程式庫

Windows UI 程式庫是一組 NuGet 套件，可提供 UWP 應用程式的新控制項和其他使用者介面元素。 Windows UI 程式庫 API 可以在舊版 Windows 10 上運作，所以即使使用者未執行最新版本的 Windows 10，您的應用程式仍然可以運作。 如此可讓您在新控制項於 Windows UI 程式庫中發行時加以採用，不需要擔心是否包含版本檢查或條件式 XAML。

請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/) (英文)。

## <a name="other-technologies-for-modern-desktop-apps"></a>新式傳統型應用程式的其他技術

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph 是 API 的集合，您可以用來為與數百萬使用者資料互動的組織和取用者建置應用程式。 Microsoft Graph 會公開 REST API 和用戶端程式庫，以存取下列位置上的資料：
* Azure Active Directory
* Office 365 服務：SharePoint、OneDrive、Outlook/Exchange、Microsoft Teams、OneNote、Planner 及 Excel
* Enterprise Mobility + Security 服務：Identity Manager、Intune、Advanced Threat Analytics 及進階威脅防護。
* Windows 10 服務：活動和裝置

如需詳細資訊，請參閱 [Microsoft Graph 文件](https://developer.microsoft.com/graph/docs/concepts/overview) (英文)。

### <a name="adaptive-cards"></a>調適型卡片

調適型卡片是開放、跨平台架構，您可以用來在裝置和平台之間以通用且一致的方式交換卡片型 UI 內容。

如需詳細資訊，請參閱[調適型卡片文件](https://docs.microsoft.com/adaptive-cards/)。
