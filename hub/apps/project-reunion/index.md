---
description: 了解 Project Reunion、其為開發人員提供的好處、現已準備好供開發人員使用的功能，以及如何提供意見反應。
title: Project Reunion
ms.topic: article
ms.date: 03/09/2021
keywords: windows win32, 桌面開發, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2354e7f42ab9c487275c66c9f709f8791ba5005e
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629231"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05-preview-march-2021"></a>使用 Project 留尼旺島 0.5 Preview 建立桌面 Windows 應用程式 () 年3月2021

Project 留尼旺島是一組全新的開發人員元件和工具，代表 Windows 應用程式開發平臺的下一次演進。 Project 留尼旺島提供一組整合的 Api 和工具，可讓您在一組廣泛的目標 Windows 10 作業系統版本上，以一致的方式使用這些應用程式。 

Project 留尼旺島不會取代現有的 Windows 應用程式平臺和架構（例如 .NET (），包括 Windows Forms 和 WPF) 和 c + +/Win32。 相反地，其會透過一組可讓開發人員在這些平台上仰賴的通用 API 和工具，來與這些現有平台互補。

> [!NOTE]
> Project 留尼旺島 0.5 Preview 是開發人員預覽版本。 建議您在開發環境中試用此版本。 不過，請注意，Project 留尼旺島在現在和1.0 版本之間會有許多方面的改變。 在生產環境中使用的應用程式不支援 Project 留尼旺島0.5 預覽版。 在未來的版本中，**Project Reunion** 這個程式碼名稱可能會變更。

## <a name="benefits-of-project-reunion-for-windows-app-developers"></a>適用于 Windows 應用程式開發人員之專案留尼旺島的優點

Project Reunion 提供了一組廣泛的 Windows API，其實作會與 OS 分離，並且會透過 NuGet 套件發行給開發人員。 Project Reunion 不是為了取代 Windows SDK。 Windows SDK 會繼續正常運作，而且有許多 Windows 核心元件會繼續透過 API (透過 OS 和 Windows SDK 版本傳遞) 來發展。 歡迎開發人員依照自己的步調採用 Project Reunion。

#### <a name="unified-api-surface-across-desktop-app-platforms"></a>跨桌面應用程式平臺的整合 API 介面

想要建立桌面 Windows 應用程式的開發人員必須在數個應用程式平臺和架構之間進行選擇。 雖然每個平台都提供許多功能和 API 供使用其他平台加以建置的應用程式使用，但某些功能和 API 只能供特定平台使用。 Project 留尼旺島可統一存取所有桌面 Windows 10 應用程式的 Windows Api。 無論您選擇哪一種應用程式模型，您都可以存取可於 Project Reunion 中取得的同一組 Windows API。

隨著時間的推移，我們打算進一步投資 Project Reunion，以去除不同應用程式模型之間的差異。 Project Reunion 會同時包含 WinRT API 和原生 C API。

#### <a name="consistent-support-across-windows-10-versions"></a>跨 Windows 10 版本提供一致的支援

當 Windows API 隨著新 OS 版本的推出而不斷演化時，開發人員必須使用[版本調適型程式碼](/windows/uwp/debug-test-perf/version-adaptive-code)之類的技術來考量各個版本的所有差異，以便能觸及其應用程式對象。 這會增加程式碼和開發體驗的複雜性。

Project 留尼旺島 Api 適用于 Windows 10 1809 版和更新版本的 Windows 10。 這表示，只要您的客戶是在 Windows 10、1809版或更新版本上，您就可以在發行時立即使用新的專案留尼旺島 Api 和功能，而不需要撰寫版本彈性程式碼。

#### <a name="faster-release-cadence"></a>更快速的發行步調

新的 Windows API 和功能一般會繫結至發行步調為每年發行一到兩次的 OS 版本。 Project 留尼旺島會以更快的步調提供更新，讓您可以在建立 Windows 開發平臺時，儘早且更快速地存取這些創新。

## <a name="components-available-with-project-reunion"></a>專案留尼旺島提供的元件

Project 留尼旺島 0.5 Preview 包含下列元件。

| 元件 | 描述 |
|---------|-------------|
| [Windows UI 程式庫 3](../winui/winui3/index.md) | Windows UI 程式庫 (WinUI) 3 是新一代的 Windows 使用者經驗 (UX) 平臺，適用于桌面 ( .NET 和 c + +/Win32) 和 UWP 應用程式。 此版本包含 Visual Studio 專案範本，可協助您開始 [使用以 WinUI 為基礎的使用者介面來建立應用程式](..\winui\winui3\index.md#create-winui-projects)，以及包含 WinUI 程式庫的 NuGet 套件。  |
| [使用 MRT.LOG Core 管理資源](mrtcore/mrtcore-overview.md) | MRT.LOG 核心提供 Api 來載入和管理您的應用程式所使用的資源。 MRT.LOG Core 是新式 [Windows 資源管理系統](/windows/uwp/app-resources/resource-management-system)的精簡版。 |
| [使用 DWriteCore 轉譯文字](dwritecore.md) | DWriteCore 可讓您存取文字轉譯的所有目前的 DirectWrite 功能，包括與裝置無關的文字配置系統、硬體加速文字、多重格式的文字，以及寬語言支援。  |

您可以在[這裡](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)深入了解未來要將其他元件帶入到 Project Reunion 的計畫。

## <a name="set-up-your-development-environment"></a>設定開發環境

1. 確定您的開發電腦已安裝 Windows 10 版本 1809 (組建 17763) 或更新的 OS 版本。

2. 如果您尚未安裝 [Visual Studio 2019、16.10 版 Preview](https://visualstudio.microsoft.com/vs/preview/) (或更新版本，請) 。

    安裝 Visual Studio 時，您必須包含下列元件：
    - 在 [ **工作負載** ] 索引標籤上，確認已選取 [ **通用 Windows 平臺開發** ]。
    - 在 [個別元件] 索引標籤上，請確定已在 [SDK、程式庫和架構] 區段中選取 [Windows 10 SDK (10.0.19041.0)]。

    若要建立 .NET 應用程式，您也必須包含下列元件：
    - **.Net 桌面開發** 工作負載。

    若要建立 c + + 應用程式，您也必須包含下列元件：
    - **使用 c + + 工作負載進行桌面開發** 。
    - **C + + (適用于 v142)** 通用 windows **平臺開發** 工作負載的通用 windows 平臺工具選用元件。

3. 如果您先前已從舊版 WinUI 3 preview 版本安裝了 [WinUI 3 preview 延伸](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates) 模組，請將擴充功能卸載。 如需如何卸載延伸模組的詳細資訊，請參閱 [管理 Visual Studio 的擴充](/visualstudio/ide/finding-and-using-visual-studio-extensions)功能。

4. 請確定您的系統已針對 **nuget.org** 啟用 NuGet 套件來源。如需詳細資訊，請參閱 [常見的 NuGet 設定](/nuget/consume-packages/configuring-nuget-behavior)。

5. 在 Visual Studio 2019 中，按一下 [**擴充** 功能  >  **管理延伸** 模組]、搜尋 **project 留尼旺島**，然後安裝「**專案留尼旺島**」延伸模組。 或者，您也可以直接從 Visual Studio Marketplace 下載並安裝 [Project 留尼旺島延伸](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) 模組。 如需如何將 VSIX 套件新增至 Visual Studio 的指示，請參閱 [管理 Visual studio 的擴充](/visualstudio/ide/finding-and-using-visual-studio-extensions)功能。

6. 若要使用 [即時視覺化樹狀]、[熱重載] 和 [即時屬性瀏覽器] 這類 WinUI 3 工具，您必須使用 Visual Studio Preview 功能啟用 WinUI 3 工具，如 [這裡的指示](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述。

## <a name="get-started-developing-with-project-reunion"></a>使用 Project 留尼旺島開始開發

您可以在使用 Project 留尼旺島延伸模組隨附的 WinUI 3 專案範本所建立的新應用程式中使用 Project 留尼旺島 0.5 Preview，也可以藉由安裝 NuGet 套件，在現有的專案中使用 Project 留尼旺島元件。

> [!NOTE]
> Project 留尼旺島 0.5 Preview 僅支援 MSIX 封裝的應用程式。

#### <a name="create-a-new-project-that-uses-project-reunion"></a>建立使用 Project 留尼旺島的新專案

Visual Studio 2019 的 Project 留尼旺島 0.5 Preview 延伸模組包含專案範本，可產生具有 WinUI 3 型 UI 層的專案，並可讓您存取所有其他的 Project 留尼旺島 Api。 您可以使用這些範本來建立 MSIX 封裝的桌面應用程式 (c #/.NET 5 或 c + +/WinRT) ，或使用 Project 留尼旺島的 UWP 應用程式。 如需可用專案範本的詳細資訊，請參閱 [建立 WinUI 專案](..\winui\winui3\index.md#create-winui-projects)。

若要建立使用 Project 留尼旺島 0.5 Preview 的新專案：

1. 遵循下列文章中的指示：

    - [開始使用適用於桌面應用程式的 WinUI 3](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [開始使用適用於 UWP 應用程式的 WinUI 3](..\winui\winui3\get-started-winui3-for-uwp.md)

2. 在您建立專案之後，除了通常可供桌面和 UWP 應用程式使用的所有其他 Windows 和 .NET Api 之外，還可以存取下列 Project 留尼旺島 Api 和元件。

    - [Windows UI 程式庫 3](../winui/winui3/index.md)
    - [管理資源的 MRT.LOG 核心](mrtcore/mrtcore-overview.md)
    - [使用 DWriteCore 轉譯文字](dwritecore.md)

若要確認您的新專案使用 project 留尼旺島，請  >  在 [**方案瀏覽器**] 中展開專案底下的 [相依性 **套件**] 節點。 您應該會看到此節點底下列出數個 **ProjectReunion** 套件，如下圖所示。

![方案 Explorer 窗格中專案留尼旺島套件的螢幕擷取畫面](images/reunion-packages.png)

#### <a name="use-project-reunion-in-an-existing-project"></a>在現有的專案中使用專案留尼旺島

如果您有現有的桌面專案 (您要在其中使用專案留尼旺島的 c #/.NET 5 或 c + +/WinRT) ，您可以在專案中安裝 Project 留尼旺島 0.5 Preview NuGet 套件。 

> [!NOTE]
> 在此案例中，您只能在專案中使用屬於 Project 留尼旺島一部分的非 WinUI 3 元件。 若要使用 WinUI 3，您必須依照上一節所述，使用其中一個 WinUI 3 專案範本來建立新專案。

1. 開啟現有的桌面專案 (c #/.NET 5 或 c + +/WinRT) 或 Visual Studio 2019 中的 UWP 專案。

2. 請確定已啟用[套件參考](/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，按一下 [工具] -> [NuGet 套件管理員]-> [套件管理員設定]。
    2. 確定已針對 [預設套件管理格式] 選取 [PackageReference]。

3. 以滑鼠右鍵按一下 [方案總管]  中的專案，然後選擇 [管理 NuGet 套件]  。

4. 在 [NuGet 套件管理員] 視窗中，選取 [瀏覽] 索引標籤，然後搜尋 `Microsoft.ProjectReunion`。

5. 找到 **ProjectReunion** 套件之後，在 [ **NuGet 套件管理員** ] 視窗的右窗格中，按一下 [ **安裝**]。

6. 安裝套件之後，您可以在專案中使用下列專案留尼旺島 Api 和元件：

    - [管理資源的 MRT.LOG 核心](mrtcore/mrtcore-overview.md)
    - [使用 DWriteCore 轉譯文字](dwritecore.md)

#### <a name="deploying-apps-that-use-project-reunion"></a>部署使用 Project 留尼旺島的應用程式

目前，使用專案留尼旺島的應用程式必須使用 [MSIX](/windows/msix)封裝。 依預設，當您使用 Visual Studio 的 Project 留尼旺島延伸模組所提供的其中一個 WinUI 專案範本來建立專案時，您的專案會包含 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) ，此專案已設定為將應用程式建立到 MSIX 封裝中。 如需設定此專案以為應用程式建立 MSIX 套件的詳細資訊，請參閱 [在 Visual Studio 中封裝桌面或 UWP 應用程式](/windows/msix/package/packaging-uwp-apps)。

為您的應用程式建立 MSIX 套件之後，您有數個選項可將其部署到其他電腦。 如需詳細資訊，請參閱 [管理您的 MSIX 部署](/windows/msix/desktop/managing-your-msix-deployment-overview)。

> [!NOTE]
> 使用 Project 留尼旺島 0.5 Preview 的應用程式無法發行至存放區。

## <a name="samples"></a>範例

您目前可以使用下列 Project 留尼旺島範例來進行探索。

- [DWriteCore 資源庫範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery)：此範例應用程式會示範 [DWriteCore](dwritecore.md) API。
- [MRT 核心範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)：此範例應用程式會示範 [MRT 核心](mrtcore/mrtcore-overview.md) API。
- [Hello World 範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp)：此範例會示範與 Project Reunion NuGet 套件的基本整合。

## <a name="limitations-and-known-issues"></a>限制與已知問題

- 在生產環境中使用的應用程式不支援此版本。 預期錯誤、限制和其他問題。
- 此版本只能用於 MSIX 封裝的桌面應用程式， (c #/.NET 5 或 c + +/Win32) 。 無法在未封裝的桌面應用程式中使用。
- [WinUI 3 的工具限制](..\winui\winui3\index.md#developer-tools)也適用于使用 project 留尼旺島 0.5 Preview 的任何專案。

## <a name="developer-roadmap"></a>開發人員藍圖

如需最新的 Project Reunion 計畫，請參閱我們的 [Github 頁面](https://github.com/microsoft/ProjectReunion)。

## <a name="give-feedback-and-contribute"></a>提供意見反應和參與其中

我們會將 Project Reunion 建置為開放原始碼專案。 我們的 [Github 頁面](https://github.com/microsoft/ProjectReunion)上有更多詳細資訊可讓您了解我們打算如何讓 Project Reunion 成真，誠摯地邀請您參與開發流程。 請查看我們的[參與者指南](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)，以提出問題、開始討論或提出功能提案。 我們想要確保 Project Reunion 能為開發人員帶來最大的好處。
