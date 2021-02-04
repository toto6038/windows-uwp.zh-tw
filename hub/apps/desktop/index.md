---
description: 了解如何開始建置 Windows PC 適用的傳統型應用程式，包括如何為新應用程式選擇正確的應用程式平台，以及如何為 Windows 10 讓現有應用程式現代化。
title: 建置 Windows PC 適用的傳統型應用程式
ms.topic: article
ms.date: 02/03/2021
keywords: windows win32, 傳統型應用程式
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2518d838ca41e5ca2faa6dbd5697d64ea9257922
ms.sourcegitcommit: 382ae62f9d9bf980399a3f654e40ef4f85eae328
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/04/2021
ms.locfileid: "99534367"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>建置 Windows PC 適用的傳統型應用程式

本文提供您開始建立適用於 Windows 的傳統型應用程式，或更新現有的傳統型應用程式以採用 Windows 10 最新體驗所需的資訊。

## <a name="platforms-for-desktop-apps"></a>適用於傳統型應用程式的平台

建立適用於 Windows PC 的傳統型應用程式有四個主要平台。 每個平台都提供一個應用程式模型，該模型定義應用程式的生命週期、完整 UI 架構、一組 UI 控制項 (可讓您建立像是 Word、Excel 和 Photoshop 的桌面應用程式)，以及用來使用 Windows 功能的一組全方位受控或原生 API 的存取權限。 

如需這些平台的深入比較以及每個平台的其他資源，請參閱[選擇您的應用程式平台](choose-your-platform.md)。

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
<td><a href="/windows/uwp/">通用 Windows 平台 (UWP)</a></td>
<td><p>適用於 Windows 10 應用程式和遊戲的先進平台。 您可以建立專門使用 UWP 控制項和 API 的 UWP 應用程式，也可以在使用其他平台之一所建立的傳統型應用程式中使用 UWP 控制項和 API。</p></td>
<td><a href="/windows/uwp/get-started/">開始使用</a><br/><a href="/uwp/">API 參考</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">範例</a></td>
</tr>
<tr class="even">
<td><a href="/windows/win32/">C++/Win32</a></td>
<td><p>這是適用於原生 Windows 應用程式的首選平台，該應用程式需要直接存取 Windows 和硬體。</p></td>
<td><a href="/windows/win32/desktop-programming/">開始使用</a><br/><a href="/windows/win32/apiindex/windows-api-list/">API 參考</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">範例</a></td>
</tr>
<tr class="odd">
<td><a href="/dotnet/framework/wpf/">WPF</a></td>
<td><p>已建立以 .NET 為基礎的平台，適用於使用 XAML UI 模型之圖形豐富的受控 Windows 應用程式。 這些應用程式可以 <a href="/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 或完整的 .NET Framework 為目標。</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">開始使用</a><br/><a href="/dotnet/api/index">API 參照 (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">範例</a></td>
</tr>
<tr class="even">
<td><a href="/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>一個以 .NET 為基礎的平台，旨在為具有輕量 UI 模型的受管企業營運應用程式所設計。 這些應用程式可以 <a href="/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 或完整的 .NET Framework 為目標。</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">開始使用</a><br/><a href="/dotnet/api/index">API 參照 (.NET)</a></td>
</tr>
</tbody>
</table>

### <a name="future-roadmap"></a>未來藍圖

未來，我們將使用 Windows UI 程式庫 (WinUI) 和專案留尼旺島來發展 Windows 應用程式開發平臺。

* **WinUI** 是適用於 Windows 10 應用程式的原生使用者體驗 (UX) 架構。 WinUI 是以工具組的形式開始，為以舊版 Windows 10 為目標的 UWP 應用程式提供新的和更新版本的 WinRT 控制項。 從 WinUI 3 (仍處於預覽) ，WinUI 在範圍內不斷成長，成為頂級的原生使用者介面 (UI) 架構，以在 UWP、.NET 和 Win32 應用程式平臺之間 Windows 10 應用程式。

    如需詳細資訊，請參閱 [WINDOWS UI 程式庫 (WinUI) ](../winui/index.md)。

* **Project Reunion** (目前為預覽版) 是一組廣泛的新開發人員元件和工具的程式碼名稱，其代表的是 Windows 應用程式開發平台的新一代進化。 Project Reunion 提供了一組整合的 API 和工具，可供一組廣泛的目標 Windows 10 OS 版本上的任何應用程式以一致的方式進行使用。 Project Reunion 會透過一組可讓開發人員在這些平台上仰賴的通用 API 和工具，來與現有的 Windows 應用程式平台和架構 (例如 UWP 和原生 Win32 以及 .NET) 互補。 

    如需詳細資訊，請參閱 [Project Reunion](../project-reunion/index.md)。

## <a name="update-existing-desktop-apps-for-windows-10"></a>更新現有適用於 Windows 10 的傳統型應用程式

如果您有現有的 WPF、Windows Forms 或原生 Win32 傳統型應用程式，Windows 10 和通用 Windows 平台 (UWP) 會提供許多功能，您可以用來在應用程式中傳遞新式體驗。 這些功能大部分都可作為模組化元件，您可以依照自己的步調在應用程式中採用，不需要為了不同的平台而重新撰寫您的應用程式。

以下只是一些可用來增強現有傳統型應用程式的功能：

* 使用 [MSIX](/windows/msix/) 來封裝和部署您的傳統型應用程式。 MSIX 是新式 Windows 應用程式套件格式，為所有 Windows 應用程式提供通用封裝體驗。 MSIX 綜合 MSI、.appx、App-V 和 ClickOnce 安裝的優勢，建置成為安全、受到保護且可靠的格式。
* 使用[套件延伸模組](./modernize/desktop-to-uwp-extensions.md)將您的傳統型應用程式與 Windows 10 體驗整合。 例如，將「開始」磚指向您的應用程式、讓應用程式成為共用目標，或從應用程式傳送快顯通知。
* 使用 [XAML 群島](./modernize/xaml-islands.md)在您的傳統型應用程式中裝載 UWP XAML 控制項。 許多最新的 Windows 10 UI 功能僅適用於 UWP XAML 控制項。

如需詳細資訊，請參閱這些主題。

<br/>

| 文章 | 說明 |
|---------|-------------|
| [傳統型應用程式現代化](./modernize/index.md) | 說明可在任何傳統型應用程式中使用的最新 Windows 10 和 UWP 開發功能，包括 WPF、Windows Forms 和 C++ Win32 應用程式。 |
| [教學課程：將 WPF 應用程式現代化](./modernize/modernize-wpf-tutorial.md) | 依照逐步指示，將 UWP 筆跡和行事曆控制項新增至應用程式，並將其封裝在 MSIX 套件中，以將現有 WPF 企業營運範例應用程式現代化。  |

## <a name="create-new-desktop-apps"></a>建立新的傳統型應用程式

如果您要建立新的 Windows 傳統型應用程式，以下是可協助您著手進行的一些資源。

<br/>

| 文章 | 說明 |
|---------|-------------|
| [選擇您的應用程式平台](choose-your-platform.md) | 提供主要傳統型應用程式平台的深入比較，並可協助您選擇適合您需求的平台。 本文也會針對每個平台的文件提供實用的連結。 |
| [適用於 Windows 應用程式的 Visual Studio 專案範本](visual-studio-templates.md) | 描述專案和項目範本，Visual Studio 提供這些項目協助您使用 C\# 或 C++ 來建置適用於 Windows 10 裝置的應用程式。 |
| [傳統型應用程式現代化](./modernize/index.md) | 說明可在任何傳統型應用程式中使用的最新 Windows 10 和 UWP 開發功能，包括 WPF、Windows Forms 和 C++ Win32 應用程式。 |
| [功能和技術](../features-and-technologies.md) | 提供可透過每個主要傳統型應用程式平台存取的 Windows 功能概觀，並提供相關文件的連結。 |

## <a name="related-documentation-and-technologies"></a>相關文件和技術

| 資源 | 描述 |
|---------|-------------|
| [.NET Core 3.1](/dotnet/core/whats-new/dotnet-core-3-1) | 深入瞭解 .NET Core 3.1 的最新功能，包括 WPF 和 Windows Forms 應用程式的增強功能。 |
| [.NET 5](/dotnet/core/dotnet-five) | 本文詳細說明 .NET 5 中包含的內容，這是 3.1 版之後的下一版 .NET Core。 |
| [適用於 WPF 和 .NET Core 的桌面指南](/dotnet/desktop-wpf/overview/index) | 開發以 .NET Core 為目標的 WPF 應用程式，而不是完整的 .NET Framework。  |
| [Azure](/azure/) | 使用 Azure 雲端服務擴充應用程式的範圍。 |
| [Visual Studio](/visualstudio/) | 了解如何使用 Visual Studio 來開發應用程式和服務。 |
| [MSIX](/windows/msix/) | 以現代化和通用的封裝格式封裝和部署任何 Windows 應用程式。 |
| [Windows AI](/windows/ai/) | 使用 Windows AI 針對您應用程式中的複雜問題建立智慧型解決方案。 |
| [Windows 容器](/virtualization/windowscontainers/) | 在快速、完全隔離的 Windows 環境中，將應用程式與相依性封裝在一起。 |
| [漸進式 Web 應用程式](/microsoft-edge/progressive-web-apps) | 將您的 Web 應用程式轉換成可在 Windows 10 上以 UWP 應用程式散發和執行的漸進式 Web 應用程式。 |
| [Xamarin](/xamarin/) | 使用 .NET 程式碼和平台特定的使用者介面，建立適用於 Windows、Android、iOS 和 macOS 的跨平台應用程式。 |
| [Windows 8.x 和舊版的文件封存](/previous-versions/windows/) | 存取為 Windows 8.x 和更早版本建立應用程式的封存文件。 |
