---
description: 了解 Project Reunion、其為開發人員提供的好處、現已準備好供開發人員使用的功能，以及如何提供意見反應。
title: Project Reunion
ms.topic: article
ms.date: 01/07/2021
keywords: windows win32, 桌面開發, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e4b5507c36da520c7356b07857b8532162e05785
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2021
ms.locfileid: "100335091"
---
# <a name="build-windows-apps-with-project-reunion-prerelease"></a>使用 Project Reunion 建置 Windows 應用程式 (發行前版本)

Project Reunion 0.1 發行前版本是一組新的開發人員元件和工具的預覽，其代表的是 Windows 應用程式開發平台的新一代進化。 Project Reunion 提供了一組整合的 API 和工具，可供一組廣泛的目標 Windows 10 OS 版本上的任何應用程式以一致的方式進行使用。 Project Reunion 不會取代現有的 Windows 應用程式平台和架構，例如 UWP 和原生 Win32 與 .NET。 相反地，其會透過一組可讓開發人員在這些平台上仰賴的通用 API 和工具，來與這些現有平台互補。

> [!NOTE]
> Project Reunion 0.1 發行前版本是早期開發人員預覽版本。 建議您在開發環境中試用此版本。 但請注意，Project Reunion 的現行版本與最終版本之間，會在許多方面有所變化。 在生產環境中使用的應用程式不支援 Project Reunion 0.1 發行前版本。 在未來的版本中，**Project Reunion** 這個程式碼名稱可能會變更。

## <a name="goals-of-project-reunion"></a>Project Reunion 的目標

Project Reunion 提供了一組廣泛的 Windows API，其實作會與 OS 分離，並且會透過 NuGet 套件發行給開發人員。 Project Reunion 不是為了取代 Windows SDK。 Windows SDK 會繼續正常運作，而且有許多 Windows 核心元件會繼續透過 API (透過 OS 和 Windows SDK 版本傳遞) 來發展。 歡迎開發人員依照自己的步調採用 Project Reunion。

建立 Project Reunion 是為了支援下列目標。

#### <a name="unified-api-surface-across-different-types-of-windows-apps"></a>跨不同類型 Windows 應用程式的整合 API 介面

想要建立 Windows 應用程式的開發人員必須在數個應用程式平台和架構之間做選擇。 雖然每個平台都提供許多功能和 API 供使用其他平台加以建置的應用程式使用，但某些功能和 API 只能供特定平台使用。 Project Reunion 會針對所有 Windows 10 應用程式，統一其 Windows API 的存取方式。 無論您選擇哪一種應用程式模型，您都可以存取可於 Project Reunion 中取得的同一組 Windows API。

隨著時間的推移，我們打算進一步投資 Project Reunion，以去除不同應用程式模型之間的差異。 Project Reunion 會同時包含 WinRT API 和原生 C API。

#### <a name="consistent-support-across-windows-10-versions"></a>跨 Windows 10 版本提供一致的支援

當 Windows API 隨著新 OS 版本的推出而不斷演化時，開發人員必須使用[版本調適型程式碼](/windows/uwp/debug-test-perf/version-adaptive-code)之類的技術來考量各個版本的所有差異，以便能觸及其應用程式對象。 這會增加程式碼和開發體驗的複雜性。 Project Reunion 讓我們能夠統一不同 OS 版本和所有 Windows 10 裝置上的 Project Reunion API 存取方式，而讓我們能伺機向更多開發人員推出更新，而不只是向最新版本的 Windows 推出。 目前的計畫是讓 Project Reunion 支援 Windows 10 版本 1809 以及之後的所有 Windows 10 版本。

#### <a name="faster-release-cadence"></a>更快速的發行步調

新的 Windows API 和功能一般會繫結至發行步調為每年發行一到兩次的 OS 版本。 Project Reunion 可讓我們以更頻繁且靈活的方式來發行新的生產品質 API 和功能。

## <a name="get-started"></a>開始使用

Project Reunion 0.1 發行前版本包含適用於下列功能區域的新 API。

| 功能 | 說明 |
|---------|-------------|
| [MRT 核心 (新式資源管理系統)](mrtcore/mrtcore-overview.md) | MRT 核心是新式 [Windows 資源管理系統](/windows/uwp/app-resources/resource-management-system)的簡化版本，會隨著 Project Reunion 一起散發。 |
| [DWriteCore](dwritecore.md) | DWriteCore 是 DirectWrite 的其中一種形式，可在低至 Windows 8 的 Windows 版本上執行，並讓您自由地跨平台使用。 |

您可以在[這裡](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)深入了解未來要將其他元件帶入到 Project Reunion 的計畫。

> [!NOTE]
> 某些其他現有元件 (包括 Windows UI 程式庫 (WinUI)、MSIX 和 WebView2) 已經與 OS 分離，並且會遵循 Project Reunion 指導方針 (例如，在 Windows 10 版本 1809 以及之後的 OS 版本上都有支援)。 不過，這些其他元件目前還不是 Project Reunion NuGet 套件的一部分。  

### <a name="set-up-your-development-environment"></a>設定開發環境

如果您想要試用 Project Reunion 0.1 發行前版本，則必須從其中一個提供的 C++ 範例來開始。 這些範例已預先設定為使用 Project Reunion NuGet 套件。 此預覽不支援在您自己的專案中安裝 Project Reunion NuGet 套件。 請遵循下列步驟來設定您的開發環境，以使用其中一個範例。

1. 確定您的開發電腦已安裝 Windows 10 版本 1809 (組建 17763) 或更新的 OS 版本。

2. 安裝 [Visual Studio 2019 (版本 16.9 預覽 2 (或更新版本))](https://visualstudio.microsoft.com/vs/preview/) 請確定已在 Visual Studio 安裝程式中選取下列項目：
    - 在 [工作負載] 索引標籤上，確定已選取下列工作負載。
        - **.NET 桌面開發**
        - **使用 C++ 開發桌面**
        - **通用 Windows 平台開發** (也請確定在 **安裝詳細資料** 窗格中，已為此工作負載選取 **C++ (v142) 通用 Windows 平台工具** 選用元件)
    - 在 [個別元件] 索引標籤上，請確定已在 [SDK、程式庫和架構] 區段中選取 [Windows 10 SDK (10.0.19041.0)]。

3. 從 Visual Studio Marketplace 安裝最新版的 [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。

4. 請確定您的系統已針對 **nuget.org** 啟用 NuGet 套件來源。如需詳細資訊，請參閱 [常見的 NuGet 設定](/nuget/consume-packages/configuring-nuget-behavior)。

5. 下載並安裝 [WinUI 3 Preview 4 VSIX 套件](https://aka.ms/winui3/preview3-download)。 只有在已設定為使用 WinUI 3 的 Hello World 和 MRT 核心範例中，才需要執行此步驟。 如需如何將 VSIX 套件新增至 Visual Studio 的指示，請參閱[尋找和使用 Visual Studio 擴充功能](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)。

6. 複製並探索下列範例：
    - [DWriteCore 資源庫範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery)：此範例應用程式會示範 [DWriteCore](dwritecore.md) API。
    - [MRT 核心範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)：此範例應用程式會示範 [MRT 核心](mrtcore/mrtcore-overview.md) API。
    - [Hello World 範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp)：此範例會示範與 Project Reunion NuGet 套件的基本整合。

## <a name="known-issues"></a>已知問題

Project Reunion 0.1 發行前版本具有下列限制：

 - 此版本僅支援用於 MSIX 封裝的 C++/Win32 應用程式。
 - 此版本不支援 C#。
 - 此版本不支援 .NET 5。

## <a name="developer-roadmap"></a>開發人員藍圖

如需最新的 Project Reunion 計畫，請參閱我們的 [Github 頁面](https://github.com/microsoft/ProjectReunion)。

## <a name="give-feedback-and-contribute"></a>提供意見反應和參與其中

我們會將 Project Reunion 建置為開放原始碼專案。 我們的 [Github 頁面](https://github.com/microsoft/ProjectReunion)上有更多詳細資訊可讓您了解我們打算如何讓 Project Reunion 成真，誠摯地邀請您參與開發流程。 請查看我們的[參與者指南](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)，以提出問題、開始討論或提出功能提案。 我們想要確保 Project Reunion 能為開發人員帶來最大的好處。
