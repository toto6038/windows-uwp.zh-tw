---
description: 了解 Project Reunion、其為開發人員提供的好處、現已準備好供開發人員使用的功能，以及如何提供意見反應。
title: 使用 Project 留尼旺島0.5 建立桌面 Windows 應用程式
ms.topic: article
ms.date: 03/19/2021
keywords: windows win32, 桌面開發, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: a68219986b66e96786cd4b0d0f3a553d8ea04e9b
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730492"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05"></a>使用 Project 留尼旺島0.5 建立桌面 Windows 應用程式

Project 留尼旺島是一組全新的開發人員元件和工具，代表 Windows 應用程式開發平臺的下一次演進。 Project 留尼旺島提供一組整合的 Api 和工具，可讓您以一致的方式在一組廣泛的目標 Windows 10 作業系統版本上，以一致的方式使用這些 Api 和工具。

Project 留尼旺島不會取代現有的桌面 Windows 應用程式平臺和架構（例如 .NET (），包括 Windows Forms 和 WPF) 和 c + +/Win32。 相反地，其會透過一組可讓開發人員在這些平台上仰賴的通用 API 和工具，來與這些現有平台互補。 如需詳細資訊，請參閱 [專案留尼旺島的優點](#benefits-of-project-reunion-for-windows-developers)。

> [!NOTE]
> 在生產環境中， (c #/.NET 5 或 c + +/Win32) 的 MSIX 封裝桌面應用程式中，可使用 Project 留尼旺島0.5。 使用 Project 留尼旺島0.5 的已封裝桌面應用程式可以發行至 Microsoft Store。 針對 UWP 應用程式，Project 留尼旺島0.5 僅供預覽。 在生產環境中使用的 UWP 應用程式不支援此版本。
>
>在未來的版本中，**Project Reunion** 這個程式碼名稱可能會變更。

## <a name="overview"></a>概觀

Project 留尼旺島提供 Visual Studio 2019 的延伸模組，其中包含設定為在新專案中使用專案留尼旺島元件的專案範本。 您也可以透過可在現有專案中安裝的 NuGet 套件，來使用 Project 留尼旺島程式庫。 如需詳細資訊，請參閱 [開始使用 Project 留尼旺島](get-started-with-project-reunion.md)。

在您建立使用 Project 留尼旺島的應用程式之後，您可以將它部署到其他電腦。 如需詳細資訊，請參閱 [部署使用 Project 留尼旺島的應用程式](deploy-apps-that-use-project-reunion.md)。

Project 留尼旺島0.5 包含下列您可以在應用程式中使用的 Api 和元件集合。 您可以在[這裡](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)深入了解未來要將其他元件帶入到 Project Reunion 的計畫。

| 元件 | 描述 |
|---------|-------------|
| [Windows UI 程式庫 3](../winui/winui3/index.md) | Windows UI 程式庫 (WinUI) 3 是 windows 應用程式的下一代 Windows 使用者經驗 (UX) 平臺。 此版本包含 Visual Studio 的專案範本，可協助您開始 [使用以 WinUI 為基礎的使用者介面來建立應用程式](..\winui\winui3\winui-project-templates-in-visual-studio.md)，以及包含 WinUI 程式庫的 NuGet 套件。  |
| [使用 MRT.LOG Core 管理資源](mrtcore/mrtcore-overview.md) | MRT.LOG 核心提供 Api 來載入和管理您的應用程式所使用的資源。 MRT.LOG Core 是新式 [Windows 資源管理系統](/windows/uwp/app-resources/resource-management-system)的精簡版。 |
| [使用 DWriteCore 轉譯文字](dwritecore.md) | DWriteCore 可讓您存取文字轉譯的所有目前的 DirectWrite 功能，包括與裝置無關的文字配置系統、硬體加速文字、多重格式的文字，以及寬語言支援。  |

## <a name="benefits-of-project-reunion-for-windows-developers"></a>Windows 開發人員的專案留尼旺島優點

Project Reunion 提供了一組廣泛的 Windows API，其實作會與 OS 分離，並且會透過 NuGet 套件發行給開發人員。 Project Reunion 不是為了取代 Windows SDK。 Windows SDK 會繼續正常運作，而且有許多 Windows 核心元件會繼續透過 API (透過 OS 和 Windows SDK 版本傳遞) 來發展。 歡迎開發人員依照自己的步調採用 Project Reunion。

### <a name="unified-api-surface-across-desktop-app-platforms"></a>跨桌面應用程式平臺的 Unified API 介面

想要建立桌面 Windows 應用程式的開發人員必須在數個應用程式平臺和架構之間進行選擇。 雖然每個平台都提供許多功能和 API 供使用其他平台加以建置的應用程式使用，但某些功能和 API 只能供特定平台使用。 Project 留尼旺島可統一存取所有桌面 Windows 10 應用程式的 Windows Api。 無論您選擇哪一種應用程式模型，您都可以存取可於 Project Reunion 中取得的同一組 Windows API。

隨著時間的推移，我們打算進一步投資 Project Reunion，以去除不同應用程式模型之間的差異。 Project Reunion 會同時包含 WinRT API 和原生 C API。

### <a name="consistent-support-across-windows-10-versions"></a>跨 Windows 10 版本提供一致的支援

當 Windows API 隨著新 OS 版本的推出而不斷演化時，開發人員必須使用[版本調適型程式碼](/windows/uwp/debug-test-perf/version-adaptive-code)之類的技術來考量各個版本的所有差異，以便能觸及其應用程式對象。 這會增加程式碼和開發體驗的複雜性。

Project 留尼旺島 Api 適用于 Windows 10 版本1809和所有較新版本的 Windows 10。 這表示，只要您的客戶 Windows 10 版本1809或任何較新版本，您就可以在發行時立即使用新的專案留尼旺島 Api 和功能，而不需要撰寫版本的自我調整程式碼。

### <a name="faster-release-cadence"></a>更快速的發行步調

新的 Windows API 和功能一般會繫結至發行步調為每年發行一到兩次的 OS 版本。 Project 留尼旺島會以更快的步調提供更新，讓您可以在建立 Windows 開發平臺時，儘早且更快速地存取這些創新。

## <a name="limitations-and-known-issues"></a>限制與已知問題

- 傳統型 **應用程式 (c #/.NET 5 或 c + +/Win32)**： Project 留尼旺島0.5 無法用於未封裝的桌面應用程式 (c #/.NET 5 或 c + +/Win32) 。 此版本僅支援在 MSIX 封裝的桌面應用程式中使用。
- **Uwp 應用程式**：在生產環境中使用的 uwp 應用程式不支援 Project 留尼旺島0.5。 若要在 UWP 應用程式中使用專案留尼旺島，您必須使用不支援生產環境的 Project 留尼旺島0.5 延伸模組預覽版本。 如需安裝預覽延伸模組的詳細資訊，請參閱 [設定您的開發環境](get-started-with-project-reunion.md#set-up-your-development-environment)。
- [WinUI 3 開發人員工具限制](..\winui\winui3\index.md#developer-tools)也適用于使用 project 留尼旺島0.5 的任何專案。
- 在現有的專案中安裝 Project 留尼旺島 0.5 NuGet 套件有 [一些限制](get-started-with-project-reunion.md#limitations-for-using-project-reunion-in-existing-projects) 。

#### <a name="asta-to-sta-threading-model"></a>ASTA 至 STA 執行緒模型

如果您要將程式碼從現有的 UWP 應用程式遷移到使用專案留尼旺島的新 c #/.NET 5 或 c + +/Win32 WinUI 3 專案，請注意新的專案會使用 [單一執行緒的單元 (STA) ](/windows/win32/com/single-threaded-apartments) 執行緒模型，而不是使用 [應用程式 STA (ASTA ](https://devblogs.microsoft.com/oldnewthing/20210224-00/?p=104901) UWP 應用程式所使用的) 執行緒模型。 如果您的程式碼假設 ASTA 執行緒模型的不可重新進入行為，則您的程式碼可能不會如預期般運作。

## <a name="developer-roadmap"></a>開發人員藍圖

如需最新的 Project 留尼旺島方案，請參閱我們的 [藍圖](https://github.com/microsoft/ProjectReunion/blob/main/docs/roadmap.md)。

## <a name="give-feedback-and-contribute"></a>提供意見反應和參與其中

我們會將 Project Reunion 建置為開放原始碼專案。 我們的 [Github 頁面](https://github.com/microsoft/ProjectReunion) 上有更多有關我們如何建立 Project 留尼旺島的資訊，以及您可以如何成為開發流程的一部分。 請查看我們的[參與者指南](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)，以提出問題、開始討論或提出功能提案。 我們想要確保 Project Reunion 能為開發人員帶來最大的好處。

## <a name="related-topics"></a>相關主題

- [使用 Project 留尼旺島建立桌面 Windows 應用程式](index.md)
- [開始使用 Project 留尼旺島](get-started-with-project-reunion.md)
- [部署使用 Project 留尼旺島的應用程式](deploy-apps-that-use-project-reunion.md)
- [Windows UI 程式庫 3](../winui/winui3/index.md)
- [使用 MRT.LOG Core 管理資源](mrtcore/mrtcore-overview.md)
- [使用 DWriteCore 轉譯文字](dwritecore.md)
