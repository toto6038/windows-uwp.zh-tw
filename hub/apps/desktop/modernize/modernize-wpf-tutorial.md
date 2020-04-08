---
description: 此教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件，以及將其他新式元件併入您的 UWP 應用程式。
title: 教學課程：將 WPF 應用程式現代化
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521276"
---
# <a name="tutorial-modernize-a-wpf-app"></a>教學課程：將 WPF 應用程式現代化 

有許多方法可藉由將最新的 Windows 功能整合到現有的原始程式碼，而不是從頭重新撰寫應用程式，來將現有的傳統型應用程式[現代化](index.md)。 在此教學課程中，我們將探討使用下列功能，來將現有 WPF 企業營運應用程式現代化的數種方式：

* .NET Core 3
* 搭配 XAML Islands 的 UWP XAML 控制項
* 調適型卡片和 Windows 10 通知
* MSIX 部署

此教學課程需要下列開發技能：

* 使用 WPF 開發 Windows 傳統型應用程式的體驗。
* C# 和 XAML 的基本知識。
* UWP 的基本知識。

## <a name="overview"></a>概觀

此教學課程提供名為 Contoso Expenses 的簡單 WPF 企業營運應用程式的程式碼。 在此教學課程的虛構案例中，Contoso Expenses 是 Contoso Corporation 的經理用來追蹤其部屬所提交費用的內部應用程式。 經理目前配備具有觸控功能的裝置，而其想要在沒有滑鼠或鍵盤的情況下使用 Contoso Expenses 應用程式。 可惜的是，目前的應用程式版本並不容易觸控。

Contoso 想要使用新的 Windows 功能來將此應用程式現代化，讓員工能夠更有效率地建立費用報告。 藉由建置新的 UWP 應用程式，輕鬆地實作許多功能。 不過，現有的應用程式很複雜，而且是由不同小組開發多年的結果。 因此，無法選擇使用新技術，從頭開始重寫該應用程式。 該小組正在尋找將新功能新增到現有程式碼基底的最佳方法。

在此教學課程一開始，Contoso Expenses 的目標是 .NET Framework 4.7.2，並使用下列協力廠商程式庫：

* MVVM Light，這是適用於 MVVM 模式的基本實作。
* Unity，此為相依性插入容器。
* LiteDb，此為用來儲存資料的內嵌 NoSQL 解決方案。
* Bogus，此為產生假資料的工具。

在此教學課程中，您將使用新的 Windows 功能來增強 Contoso Expenses：

* 將現有的 WPF 應用程式移轉至 .NET Core 3.0。 這將會在未來開啟新的重要案例。
* 使用 XAML Islands 來裝載由 Windows 社群工具組所提供的 **InkCanvas** 和 **MapControl** 包裝的控制項。
* 使用 XAML Islands 來裝載任何標準 UWP XAML 控制項 (在此案例中為 **CalendardView**)。
* 將調適型卡片和 Windows 10 通知整合到應用程式。
* 使用 MSIX 來封裝應用程式，並在 Azure DevOps 上設定 CI/CD 管線，讓您能夠在新版應用程式可供使用時，立即自動將其傳遞給測試人員和使用者。

## <a name="prerequisites"></a>必要條件

若要執行此教學課程，您的開發電腦必須已安裝下列先決條件：

* Windows 10 1903 版 (組建 18362) 或更新版本。
* [Visual Studio 2019](https://www.visualstudio.com)。
* [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (安裝最新版本)。

確定您會使用 Visual Studio 2019 安裝下列工作負載和選用功能：

* .NET 桌面開發
* 通用 Windows 平台開發
* Windows 10 SDK (10.0.18362.0 或更新版本)

## <a name="get-the-contoso-expenses-sample-app"></a>取得 Contoso Expenses 範例應用程式

開始此教學課程之前，先下載 Contoso Expenses 應用程式的原始程式碼，並確定您可以在 Visual Studio 中建置程式碼。

1. 從 [AppConsult WinAppsModernization 研討會存放庫](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop) \(英文\) 的 [版本]  索引標籤中，下載應用程式原始程式碼。 直接連結是 [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases)。
2. 開啟 zip 檔案，並將所有內容解壓縮到您的 **C:\\** 磁碟機根目錄。 它將會建立名為 **C:\WinAppsModernizationWorkshop** 的資料夾。
3. 開啟 Visual Studio 2019，然後按兩下 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** 檔案以開啟解決方案。
4. 藉由按下 [開始]  按鈕或 CTRL + F5，來確認您可以建置、執行和偵錯 Contoso Expenses WPF 專案。

## <a name="get-started"></a>開始使用

在您擁有 Contoso Expenses 範例應用程式的原始程式碼，並確認可在 Visual Studio 中進行建置之後，您就準備好開始進行教學課程：

* [第 1 部分：將 Contoso Expenses 應用程式移轉到 .NET Core 3](modernize-wpf-tutorial-1.md)
* [第 2 部分：使用 XAML Islands 新增 UWP InkCanvas 控制項](modernize-wpf-tutorial-2.md)
* [第 3 部分：使用 XAML Islands 新增 UWP CalendarViews 控制項](modernize-wpf-tutorial-3.md)
* [第 4 部分：新增 Windows 10 使用者活動及通知](modernize-wpf-tutorial-4.md)
* [第 5 部分：使用 MSIX 封裝和部署](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要概念

下列各節提供此教學課程中所討論之一些重要概念的背景。 如果您已經熟悉這些概念，則可略過此節。

### <a name="universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP)

在 Windows 8 中，Microsoft 引進了一個稱為 Windows 執行階段 (WinRT) 的新架構。 與 .NET Framework 不同的是，WinRT 是直接向應用程式公開的原生 API 層。 WinRT 也引進了語言預測，這些層會新增於執行階段之上，讓開發人員除了 C++ 還能使用 C# 和 JavaScript 等語言來與之互動。 預測可讓開發人員在 WinRT 之上建置應用程式，以運用其在使用 .NET Framework 建置應用程式時所取得的相同 C# 和 XAML 知識。 

在 Windows 10 中，Microsoft 引進了[通用 Windows 平台 (UWP)](/windows/uwp/get-started/universal-application-platform-guide)，這會以 WinRT 為基礎來建置。 UWP 最重要的功能是，其會在每個裝置平台上提供一組通用 API：無論應用程式是在桌面、Xbox One 還是 HoloLens 上執行，您都可以使用相同的 API。

往後，大部分新的 Windows 10 功能都會透過 WinRT API 公開，包括時間軸、Project Rome 和 Windows Hello 等功能。

### <a name="msix-packaging"></a>MSIX 封裝

[MSIX](/windows/msix/) 是適用於 Windows 應用程式的新式封裝模型。 MSIX 支援 UWP 應用程式，以及使用 Win32、WPF、Windows Forms、Java、Electron 等技術建置的傳統型應用程式。 當您在 MSIX 套件中封裝傳統型應用程式時，您可以將應用程式發佈到 Microsoft Store。 您的傳統型應用程式也會在安裝時取得套件身分識別，讓您的傳統型應用程式能夠使用更廣泛的 WinRT API 組合。

如需詳細資訊，請參閱下列文章：

* [封裝傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-root)
* [已封裝桌面應用程式的運作情況](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML Islands

從 Windows 10 1903 版開始，您可以使用稱為「XAML Islands」  的功能，將 UWP 控制項裝載於非 UWP 傳統型應用程式中。 此功能可讓您使用僅能透過 UWP 控制項提供的最新 Windows 10 UI 功能，來增強現有傳統型應用程式的外觀、風格和功能。 這表示您可以在現有的 WPF、Windows Forms 和 C++ Win32 應用程式中，使用 Windows Ink 之類的 UWP 功能，以及支援現有 Fluent Design 系統的控制項。

如需詳細資訊，請參閱[傳統型應用程式中的 UWP 控制項 (XAML Islands)](/windows/uwp/xaml-platform/xaml-host-controls)。 此教學課程會引導您完成使用兩種不同類型 XAML Island 控制項的程序：

* Windows 社群工具組中的 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) \(英文\) 和 [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) \(英文\)。 這些 WPF 控制項會包裝對應 UWP 控制項的介面和功能，而且可以像 Visual Studio 設計工具中的任何其他 WPF 控制項一樣使用。

* UWP [行事曆檢視](/windows/uwp/design/controls-and-patterns/calendar-view)控制項。 這是您將使用 Windows 社群工具組中的 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) \(英文\) 控制項裝載的標準 UWP 控制項。

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](https://docs.microsoft.com/dotnet/core/) \(部分機器翻譯\) 是一個開放原始碼架構，可實作跨平台、輕量且易於擴充的完整 .NET Framework 版本。 相較於完整的 .NET Framework，.NET Core 啟動時間更快速，而且許多 API 都已經過最佳化。

透過其前幾個版本，.NET Core 的重點在於支援 Web 或後端應用程式。 有了 .NET Core，您就可以輕鬆地建置可調整的 Web 應用程式或 API，其可裝載於 Windows、Linux 或 Docker 容器之類的微服務架構中。

.NET Core 3 是 .NET Core 的最新版本。 這個版本的重點是支援 Windows 傳統型應用程式，包括 Windows Forms 和 WPF 應用程式。 您可以在 .NET Core 3 上執行新的和現有的 Windows 傳統型應用程式，並享有 .NET Core 帶來的所有優點。 裝載於 [XAML Islands](xaml-islands.md) 中的 UWP 控制項，也可以在目標為 .NET Core 3 的 Windows Forms 和 WPF 應用程式中使用。

> [!NOTE]
> WPF 和 Windows Forms 不會變成跨平台，而且您無法在 Linux 和 MacOS 上執行 WPF 或 Windows Forms。 WPF 和 Windows Forms 的 UI 元件仍然具有 Windows 轉譯系統的相依性。

如需詳細資訊，請參閱 [.NET Core 3.0 的新功能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)。
