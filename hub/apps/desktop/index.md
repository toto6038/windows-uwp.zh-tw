---
Description: 了解如何開始建置 Windows PC 適用的傳統型應用程式，包括如何為新應用程式選擇正確的應用程式平台，以及如何為 Windows 10 讓現有應用程式現代化。
title: 建置 Windows PC 適用的傳統型應用程式
ms.topic: article
ms.date: 11/04/2019
keywords: windows win32, 傳統型應用程式
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: d052ad0f670bccd9b32d2e3643520dd6129ed22a
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262739"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>建置 Windows PC 適用的傳統型應用程式

本文提供您開始建立適用於 Windows 的傳統型應用程式，或更新現有的傳統型應用程式以採用 Windows 10 最新體驗所需的資訊。

## <a name="platforms-for-desktop-apps"></a>適用於傳統型應用程式的平台

建立適用於 Windows PC 的傳統型應用程式有四個主要平台。 每個平台都提供一個應用程式模型，該模型定義應用程式的生命週期、一組完整的 UI 控制項，以及用來使用 Windows 功能的一組全方位受控或原生 API 的存取權限。

下表為這些平台的簡介。 如需這些平台的深入比較以及每個平台的其他資源，請參閱[選擇您的應用程式平台](choose-your-platform.md)。

<br/>

<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>平台</th>
<th>說明</th>
<th>文件與資源</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://docs.microsoft.com/windows/uwp/">通用 Windows 平台 (UWP)</a></td>
<td><p>適用於 Windows 10 應用程式和遊戲的先進平台。 您可以建立專門使用 UWP 控制項和 API 的 UWP 應用程式，也可以在使用其他平台之一所建立的傳統型應用程式中使用 UWP 控制項和 API。</p></td>
<td><a href="/windows/uwp/get-started/">開始使用</a><br/><a href="/uwp/">API 參考</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">範例</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/windows/win32/">Win32</a></td>
<td><p>這是適用於原生 C/C++ Windows 應用程式的首選平台，該應用程式需要直接存取 Windows 和硬體。</p></td>
<td><a href="/windows/win32/desktop-programming/">開始使用</a><br/><a href="/windows/win32/apiindex/windows-api-list/">API 參考</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">範例</a></td>
</tr>
<tr class="odd">
<td><a href="https://docs.microsoft.com/dotnet/framework/wpf/">WPF</a></td>
<td><p>已建立以 .NET 為基礎的平台，適用於使用 XAML UI 模型之圖形豐富的受控 Windows 應用程式。 這些應用程式可以 <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 或完整的 .NET Framework 為目標。</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">開始使用</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API 參照 (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">範例</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>一個以 .NET 為基礎的平台，旨在為具有輕量 UI 模型的受管企業營運應用程式所設計。 這些應用程式可以 <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 或完整的 .NET Framework 為目標。</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">開始使用</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API 參照 (.NET)</a></td>
</tr>
</tbody>
</table>

> [!NOTE]
> 所有這些應用程式平台都提供完整的使用者介面架構和一組 UI 控制項，可讓您建立在傳統 Windows 桌面上執行的桌面應用程式 (例如 Word、Excel 和 Photoshop)，並充分利用該環境的特定功能。 在 Windows 10 上，所有這些平台都支援使用 Windows UI (WinUI) 程式庫來建立其使用者介面。 若要深入了解適用於桌面應用程式的 WinUI，請參閱[本節](choose-your-platform.md#windows-ui-library)。

## <a name="update-existing-desktop-apps-for-windows-10"></a>更新現有適用於 Windows 10 的傳統型應用程式

如果您有現有的 WPF、Windows Forms 或原生 Win32 傳統型應用程式，Windows 10 和通用 Windows 平台 (UWP) 會提供許多功能，您可以用來在應用程式中傳遞新式體驗。 這些功能大部分都可作為模組化元件，您可以依照自己的步調在應用程式中採用，不需要為了不同的平台而重新撰寫您的應用程式。

以下只是一些可用來增強現有傳統型應用程式的功能：

* 使用 [MSIX](/windows/msix/) 來封裝和部署您的傳統型應用程式。 MSIX 是新式 Windows 應用程式套件格式，為所有 Windows 應用程式提供通用封裝體驗。 MSIX 綜合 MSI、.appx、App-V 和 ClickOnce 安裝的優勢，建置成為安全、受到保護且可靠的格式。
* 使用[套件延伸模組](/windows/apps/desktop/modernize/desktop-to-uwp-extensions)將您的傳統型應用程式與 Windows 10 體驗整合。 例如，將「開始」磚指向您的應用程式、讓應用程式成為共用目標，或從應用程式傳送快顯通知。
* 使用 [XAML 群島](/windows/apps/desktop/modernize/xaml-islands)在您的傳統型應用程式中裝載 UWP XAML 控制項。 許多最新的 Windows 10 UI 功能僅適用於 UWP XAML 控制項。

如需詳細資訊，請參閱這些主題。

<br/>

| 文章 | 說明 |
|---------|-------------|
| [傳統型應用程式現代化](/windows/apps/desktop/modernize) | 說明可在任何傳統型應用程式中使用的最新 Windows 10 和 UWP 開發功能，包括 WPF、Windows Forms 和 C++ Win32 應用程式。 |
| [教學課程：將 WPF 應用程式現代化](/windows/apps/desktop/modernize/modernize-wpf-tutorial) | 依照逐步指示，將 UWP 筆跡和行事曆控制項新增至應用程式，並將其封裝在 MSIX 套件中，以將現有 WPF 企業營運範例應用程式現代化。  |

## <a name="create-new-desktop-apps"></a>建立新的傳統型應用程式

如果您要建立新的 Windows 傳統型應用程式，以下是可協助您著手進行的一些資源。

<br/>

| 文章 | 說明 |
|---------|-------------|
| [選擇您的應用程式平台](choose-your-platform.md) | 提供主要傳統型應用程式平台的深入比較，並可協助您選擇適合您需求的平台。 本文也會針對每個平台的文件提供實用的連結。 |
| [傳統型應用程式現代化](/windows/apps/desktop/modernize) | 說明可在任何傳統型應用程式中使用的最新 Windows 10 和 UWP 開發功能，包括 WPF、Windows Forms 和 C++ Win32 應用程式。 |
| [功能和技術](/windows/apps/features-and-technologies) | 提供可透過每個主要傳統型應用程式平台存取的 Windows 功能概觀，並提供相關文件的連結。 |

## <a name="related-documentation-and-technologies"></a>相關文件和技術

| 資源 | 說明 |
|---------|-------------|
| [.NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0) | 深入瞭解 .NET Core 3.0 的最新功能，包括 WPF 和 Windows Forms 應用程式的增強功能。 |
| [適用於 WPF 和 .NET Core 3.0 的桌面指南](https://docs.microsoft.com/dotnet/desktop-wpf/overview/index) | 開發以 .NET Core 3.0 為目標的 WPF 應用程式，而不是完整的 .NET Framework。  |
| [Azure](https://docs.microsoft.com/azure/) | 使用 Azure 雲端服務擴充應用程式的範圍。 |
| [Visual Studio](https://docs.microsoft.com/visualstudio/) | 了解如何使用 Visual Studio 來開發應用程式和服務。 |
| [MSIX](https://docs.microsoft.com/windows/msix/) | 以現代化和通用的封裝格式封裝和部署任何 Windows 應用程式。 |
| [Windows AI](https://docs.microsoft.com/windows/ai/) | 使用 Windows AI 針對您應用程式中的複雜問題建立智慧型解決方案。 |
| [Windows 容器](https://docs.microsoft.com/virtualization/windowscontainers/) | 在快速、完全隔離的 Windows 環境中，將應用程式與相依性封裝在一起。 |
| [漸進式 Web 應用程式](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | 將您的 Web 應用程式轉換成可在 Windows 10 上以 UWP 應用程式散發和執行的漸進式 Web 應用程式。 |
| [Xamarin](https://docs.microsoft.com/xamarin/) | 使用 .NET 程式碼和平台特定的使用者介面，建立適用於 Windows、Android、iOS 和 macOS 的跨平台應用程式。 |
| [Windows 8.x 和舊版的文件封存](https://docs.microsoft.com/previous-versions/windows/) | 存取為 Windows 8.x 和更早版本建立應用程式的封存文件。 |
