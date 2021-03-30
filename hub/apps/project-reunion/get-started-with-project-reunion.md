---
description: 本文說明如何在您的開發電腦上安裝 Visual Studio 2019 的 Project 留尼旺島延伸模組，以及在新的或現有的專案中使用 Project 留尼旺島。
title: 開始使用 Project 留尼旺島
ms.topic: article
ms.date: 03/19/2021
keywords: windows win32, 桌面開發, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 08d25334014d90f4aaec7119bc9ee84444547115
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730782"
---
# <a name="get-started-with-project-reunion"></a>開始使用 Project 留尼旺島

本文說明如何在您的開發電腦上安裝 Visual Studio 2019 的 Project 留尼旺島延伸模組，以及在新的或現有的專案中使用 Project 留尼旺島。 在您安裝和使用 Project 留尼旺島之前，請先參閱 [限制和已知問題](index.md#limitations-and-known-issues)。

## <a name="set-up-your-development-environment"></a>設定開發環境

1. 確定您的開發電腦已安裝 Windows 10 版本 1809 (組建 17763) 或更新的 OS 版本。

2. 安裝 [Visual Studio 2019、16.10 版 Preview](https://visualstudio.microsoft.com/vs/preview/) (或更新版本的) （如果您尚未這樣做）。

    > [!NOTE]
    > Visual Studio 2019，16.9 版也支援 Project 留尼旺島，但不支援所有 WinUI 3 工具功能。 如需 WinUI 3 工具支援的詳細資訊，請參閱 [WINDOWS UI 程式庫 3-Project 留尼旺島 0.5](../winui/winui3/index.md)。

    安裝 Visual Studio 時，您必須包含下列元件：
    - 在 [安裝] 對話方塊的 [ **工作負載** ] 索引標籤上，確認已選取 **通用 Windows 平臺開發** 。
    - 在 [安裝] 對話方塊的 [**個別元件**] 索引標籤上，確定已在 [sdk] **、** [程式庫] 和 [架構] 區段中選取 **Windows 10 SDK (10.0.19041.0)** 。

    若要建立 .NET 應用程式，您也必須包含下列元件：
    - 在 [安裝] 對話方塊的 [ **工作負載** ] 索引標籤上，確認已選取 [ **.net 桌面開發** ]。

    若要建立 c + + 應用程式，您也必須包含下列元件：
    - 在安裝對話方塊的 [ **工作負載** ] 索引標籤上，確定已選取 [ **使用 c + + 進行桌面開發** ]。
    - 在 [安裝] 對話方塊右側的 [**安裝詳細資料**] 窗格中，確定已選取 [**通用 Windows 平臺開發**] 區段中的 [ **c + + (適用于 v142) 通用 Windows 平臺工具** 選用元件]。

3. 如果您先前已安裝 [Visual Studio 的 WinUI 3 Preview 延伸](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates)模組，請將擴充功能卸載。 如需有關如何卸載延伸模組的詳細資訊，請參閱 [管理 Visual Studio 的擴充](/visualstudio/ide/finding-and-using-visual-studio-extensions)功能。

4. 請確定您的系統已針對 **nuget.org** 啟用 NuGet 套件來源。如需詳細資訊，請參閱 [常見的 NuGet 設定](/nuget/consume-packages/configuring-nuget-behavior)。

5. 下載並安裝適用于 Visual Studio 的 Project 留尼旺島0.5 延伸模組。 延伸模組有兩個版本：一個適用于 desktop (c #/.NET 5 或 c + +/WinRT) 應用程式，另一個適用于 UWP 應用程式。

    若要在 desktop 中使用專案留尼旺島 (c #/.NET 5 或 c + +/WinRT) 應用程式：
    - 在 Visual Studio 2019 中，按一下 [**擴充** 功能  >  **管理延伸** 模組]、搜尋 **project 留尼旺島**，然後安裝 **專案留尼旺島** 延伸模組。
    - 或者，您也可以直接從 Visual Studio Marketplace 下載並安裝 [Project 留尼旺島0.5 擴充](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) 功能。

    若要在 UWP 應用程式中使用專案留尼旺島，您必須安裝不支援生產環境的延伸模組預覽版本：
    - 卸載任何現有版本的 Project 留尼旺島 VSIX。
    - 在 Visual Studio 2019 中，按一下 [**擴充** 功能  >  **管理延伸** 模組]，然後按一下左下角的 [**變更延伸** 模組的設定]。 在安裝較舊版本之前，請為所有使用者關閉已安裝套件的自動更新。
    - 下載並安裝 [Project 留尼旺島 0.5 Preview 延伸](https://download.microsoft.com/download/9/9/8/9981a84b-8fd8-4645-9dce-c62761601f17/ProjectReunion.Extension.vsix)模組。

    如需有關如何將 VSIX 封裝新增至 Visual Studio 的指示，請參閱 [管理 Visual Studio 的擴充](/visualstudio/ide/finding-and-using-visual-studio-extensions)功能。

    ![正在安裝之專案留尼旺島延伸模組的螢幕擷取畫面](images/reunion-extension-install.png)

6. 若要在 Visual Studio 2019 16.10 Preview 中使用 WinUI 3 工具（例如即時視覺化樹狀結構、熱重新載入和即時屬性瀏覽器），您必須使用 Visual Studio 預覽功能來啟用 WinUI 3 工具。 如需相關指示，請參閱 [VS 16.9 Preview 4 中的如何啟用 WinUI 3 的 UI 工具](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)。

## <a name="create-a-new-project-that-uses-project-reunion"></a>建立使用 Project 留尼旺島的新專案

適用于 Visual Studio 2019 (的 Project 留尼旺島0.5 擴充功能（包括適用于桌面應用程式的延伸模組和 UWP 應用) 程式的預覽延伸模組）提供專案範本，可產生具有 WinUI 3 型 UI 層的專案，並可讓您存取所有其他的 Project 留尼旺島 Api。 如需可用專案範本的詳細資訊，請參閱 [Visual Studio 中的 WinUI 3 專案範本](..\winui\winui3\winui-project-templates-in-visual-studio.md)。

> [!NOTE]
> 在生產環境中支援使用 desktop (c #/.NET 5 和 c + +/WinRT) 專案範本。 UWP 專案範本僅以開發人員預覽版的形式提供，無法用來建立生產環境的應用程式。

若要建立使用 Project 留尼旺島0.5 的新專案：

1. 遵循下列文章中的指示：

    - [開始使用適用於桌面應用程式的 WinUI 3](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [開始使用適用于 UWP 應用程式的 WinUI 3 (Preview) ](..\winui\winui3\get-started-winui3-for-uwp.md)
    - [建立基本的 WinUI 3 傳統型應用程式](..\winui\winui3\desktop-build-basic-winui3-app.md)

2. 在您建立專案之後，除了通常可供桌面和 UWP 應用程式使用的所有其他 Windows 和 .NET Api 之外，還可以存取下列 Project 留尼旺島 Api 和元件。

    - [Windows UI 程式庫 3](../winui/winui3/index.md)
    - [管理資源的 MRT.LOG 核心](mrtcore/mrtcore-overview.md)
    - [使用 DWriteCore 轉譯文字](dwritecore.md)

若要確認您的新專案使用 project 留尼旺島，請在方案總管中展開專案底下的 [相依性套件 **]**  >  節點。  您應該會看到此節點底下列出數個 **ProjectReunion** 套件，如下圖所示。

![方案總管窗格中專案留尼旺島套件的螢幕擷取畫面](images/reunion-packages.png)

## <a name="use-project-reunion-in-an-existing-project"></a>在現有的專案中使用專案留尼旺島

如果您有想要使用 Project 留尼旺島的現有專案，可以在專案中安裝 Project 留尼旺島 0.5 NuGet 套件。 此案例有 [一些限制](#limitations-for-using-project-reunion-in-existing-projects)。

1. 開啟現有的桌面專案 (Visual Studio 2019 中的 c #/.NET 5 或 c + +/WinRT) 或 UWP 專案。

2. 請確定已啟用[套件參考](/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，按一下 [工具] -> [NuGet 套件管理員]-> [套件管理員設定]。
    2. 確定已針對 [預設套件管理格式] 選取 [PackageReference]。

3. 以滑鼠右鍵按一下 [方案總管]  中的專案，然後選擇 [管理 NuGet 套件]  。

4. 在 [NuGet 套件管理員] 視窗中，選取 [瀏覽] 索引標籤，然後搜尋 `Microsoft.ProjectReunion`。

5. 找到 **ProjectReunion** 套件之後，在 [ **NuGet 封裝管理員** ] 視窗的右窗格中，按一下 [ **安裝**]。

    ![已安裝 Project 留尼旺島 NuGet 套件的螢幕擷取畫面](images/reunion-nuget-install.png)

6. 安裝套件之後，您可以在專案中使用下列專案留尼旺島 Api 和元件：

    - [管理資源的 MRT.LOG 核心](mrtcore/mrtcore-overview.md)
    - [使用 DWriteCore 轉譯文字](dwritecore.md)

### <a name="limitations-for-using-project-reunion-in-existing-projects"></a>在現有的專案中使用專案留尼旺島的限制

如果您想要在現有的專案中使用 Project 留尼旺島0.5，請注意下列限制：

- 如果您將 Project 留尼旺島 0.5 NuGet 套件安裝到現有的專案中，您只能在專案中使用屬於 Project 留尼旺島的非 WinUI 3 元件。 若要使用 WinUI 3，您必須依照上一節所述，使用其中一個 WinUI 3 專案範本來建立新專案。
-  (名為 **ProjectReunion**) 的 Project 留尼旺島 0.5 NuGet 套件包含其他的子封裝 (包括 **ProjectReunion** 、mrt.log Core 和 WinUI 在內的元件的執行，包括 **ProjectReunion) 。** 在目前的版本中，您不能個別安裝這些子封裝來參考專案中的特定元件。 您必須安裝 **ProjectReunion** 封裝，其中包含所有的元件。  
- WPF 專案目前不支援安裝 Project 留尼旺島 0.5 NuGet 套件。
- Project 留尼旺島 0.5 NuGet 套件支援在生產環境中使用 desktop (c #/.NET 5 和 c + +/WinRT) 專案。 它是 UWP 專案的開發人員預覽版本，不支援在生產環境中搭配 UWP 專案使用。

## <a name="samples"></a>範例

您目前可以使用下列 Project 留尼旺島範例來進行探索。

- [DWriteCore 資源庫範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery)：此範例應用程式會示範 [DWriteCore](dwritecore.md) API。
- [MRT 核心範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)：此範例應用程式會示範 [MRT 核心](mrtcore/mrtcore-overview.md) API。
- [Hello World 範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp)：此範例會示範與 Project Reunion NuGet 套件的基本整合。
- [Xaml 控制項資源庫](https://aka.ms/winui3/xcg)：這是一個範例應用程式，可展示所有 WinUI 的3個控制項的作用。 

## <a name="related-topics"></a>相關主題

- [使用 Project 留尼旺島建立桌面 Windows 應用程式](index.md)
- [部署使用 Project 留尼旺島的應用程式](deploy-apps-that-use-project-reunion.md)
