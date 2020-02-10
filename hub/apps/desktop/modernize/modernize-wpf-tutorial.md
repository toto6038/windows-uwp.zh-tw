---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件，以及將其他現代化元件併入您的 WPF 應用程式中。
title: 教學課程：將 WPF 應用程式現代化
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 397c301564c0d4799c6b41db209da9659725103d
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089304"
---
# <a name="tutorial-modernize-a-wpf-app"></a>教學課程：將 WPF 應用程式現代化 

將最新的 Windows 功能整合到現有的原始程式碼，而不是從頭重新撰寫應用程式，有許多方法可以將現有的桌面應用程式[現代化](index.md)。 在本教學課程中，我們將探討使用下列功能，將現有 WPF 企業營運應用程式現代化的幾種方式：

* .NET Core 3
* 具有 XAML 群島的 UWP XAML 控制項
* 調適型卡片和 Windows 10 通知
* MSIX 部署

本教學課程需要下列開發技能：

* 使用 WPF 開發 Windows 桌面應用程式的經驗。
* 和 XAML 的C#基本知識。
* UWP 的基本知識。

## <a name="overview"></a>概觀

本教學課程提供名為 Contoso 費用的簡單 WPF 企業營運應用程式的程式碼。 在本教學課程的虛構案例中，Contoso 費用是 Contoso Corporation 經理用來追蹤其報告所提交之費用的內部應用程式。 管理員現在具備具備觸控功能的裝置，而且想要在沒有滑鼠或鍵盤的情況下使用 Contoso 費用應用程式。 可惜的是，目前的應用程式版本並不容易觸控。

Contoso 想要使用新的 Windows 功能將此應用程式現代化，讓員工能夠更有效率地建立費用報表。 藉由建立新的 UWP 應用程式，可以輕鬆地執行許多功能。 不過，現有的應用程式很複雜，而且是由不同小組開發多年來的結果。 因此，不提供新的技術來從頭開始重寫。 小組正在尋找將新功能新增至現有程式碼基底的最佳方法。

在本教學課程一開始，Contoso 費用的目標是 .NET Framework 4.7.2，並使用下列協力廠商程式庫：

* MVVM Light，這是 MVVM 模式的基本實作為。
* Unity，相依性插入容器。
* LiteDb，這是用來儲存資料的內嵌 NoSQL 解決方案。
* 假的，這是產生假資料的工具。

在本教學課程中，您將使用新的 Windows 功能來增強 Contoso 費用：

* 將現有的 WPF 應用程式遷移至 .NET Core 3.0。 未來將會開啟新的重要案例。
* 使用 XAML 島來裝載 Windows 社區工具組所提供的**InkCanvas**和**MapControl**包裝控制項。
* 使用 XAML 島來裝載任何標準 UWP XAML 控制項（在此案例中為**CalendardView**）。
* 將調適型卡片和 Windows 10 通知整合到應用程式中。
* 使用 MSIX 封裝應用程式，並在 Azure DevOps 上設定 CI/CD 管線，讓您可以在應用程式和使用者可供使用時，自動將其傳遞給測試人員和使用者。

## <a name="prerequisites"></a>必要條件

若要執行本教學課程，您的開發電腦必須安裝下列必要條件：

* Windows 10 版本1903（組建18362）或更新版本。
* [Visual Studio 2019](https://www.visualstudio.com)。
* [.Net Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) （安裝最新版本）。

請確定您已使用 Visual Studio 2019 安裝下列工作負載和選用功能：

* .NET 桌面開發
* 通用 Windows 平台開發
* Windows 10 SDK （10.0.18362.0 或更新版本）

## <a name="get-the-contoso-expenses-sample-app"></a>取得 Contoso 費用範例應用程式

開始本教學課程之前，請先下載 Contoso 費用應用程式的原始程式碼，並確定您可以在 Visual Studio 中建立程式碼。

1. 從[AppConsult WinAppsModernization 研討會存放庫](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)的 [**發行**] 索引標籤下載應用程式原始程式碼。 直接連結為[https://aka.ms/wamwc](https://aka.ms/wamwc)。
2. 開啟 zip 檔案，並將所有內容解壓縮到**C：\\** 磁片磁碟機的根目錄。 它會建立名為**C:\WinAppsModernizationWorkshop**的資料夾。
3. 開啟 Visual Studio 2019，然後按兩下**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln**檔案以開啟方案。
4. 藉由按下 [**開始**] 按鈕或 CTRL + F5，確認您可以建立、執行和偵錯工具 CONTOSO 費用 WPF 專案。

## <a name="get-started"></a>開始使用

在您擁有 Contoso 費用範例應用程式的原始程式碼之後，您可以確認是否可以在 Visual Studio 中建立，您已準備好開始進行教學課程：

* [第1部分：將 Contoso 費用應用程式遷移至 .NET Core 3](modernize-wpf-tutorial-1.md)
* [第2部分：使用 XAML 群島新增 UWP InkCanvas 控制項](modernize-wpf-tutorial-2.md)
* [第3部分：使用 XAML 群島新增 UWP CalendarView 控制項](modernize-wpf-tutorial-3.md)
* [第4部分：新增 Windows 10 使用者活動和通知](modernize-wpf-tutorial-4.md)
* [第5部分：使用 MSIX 封裝和部署](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要概念

下列各節提供本教學課程中討論的一些重要概念的背景。 如果您已經熟悉這些概念，可以略過本節。

### <a name="universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP)

在 Windows 8 中，Microsoft 引進了一個稱為 Windows 執行階段（WinRT）的新架構。 與 .NET Framework 不同的是，WinRT 是直接向應用程式公開的原生 Api 層。 WinRT 也引進了語言預測，也就是在執行時間之上新增的圖層，可讓開發人員使用C#和 JavaScript 等語言，以及與C++來與它互動。 預測可讓開發人員在 WinRT 之上建立應用程式，以運用C#在使用 .NET Framework 建立應用程式中所取得的相同和 XAML 知識。 

在 Windows 10 中，Microsoft 引進了以 WinRT 為基礎的[通用 Windows 平臺（UWP）](/windows/uwp/get-started/universal-application-platform-guide)。 UWP 最重要的功能是，它會在每個裝置平臺上提供一組通用的 Api：無論應用程式是在桌面、Xbox One 還是 HoloLens 上執行，您都可以使用相同的 Api。

接下來，大部分新的 Windows 10 功能都是透過 WinRT Api 公開，包括時程表、Project 羅馬和 Windows Hello 等功能。

### <a name="msix-packaging"></a>MSIX 封裝

[MSIX](/windows/msix/)是適用于 Windows 應用程式的新式封裝模型。 MSIX 支援 UWP 應用程式以及使用 Win32、WPF、Windows Forms、JAVA、Electron 等技術建立的桌面應用程式。 當您在 MSIX 套件中封裝桌面應用程式時，您可以將應用程式發佈至 Microsoft Store。 您的桌面應用程式也會在安裝時取得套件身分識別，讓您的桌面應用程式能夠使用更廣泛的 WinRT Api 集合。

如需詳細資訊，請參閱下列文章：

* [封裝桌面應用程式](/windows/uwp/porting/desktop-to-uwp-root)
* [在您已封裝的桌面應用程式幕後](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML 群島

從 Windows 10 版本1903開始，您可以使用稱為*XAML Islands*的功能，將 uwp 控制項裝載在非 UWP 桌面應用程式中。 這項功能可讓您使用僅透過 UWP 控制項提供的最新 Windows 10 UI 功能，來增強現有桌面應用程式的外觀、風格和功能。 這表示您可以在現有的 WPF、Windows Forms 和C++ Win32 應用程式中使用 UWP 功能（例如 Windows Ink 和控制項），以支援流暢的設計系統。

如需詳細資訊，請參閱[桌面應用程式中的 UWP 控制項（XAML 島）](/windows/uwp/xaml-platform/xaml-host-controls)。 本教學課程會引導您完成使用兩種不同類型 XAML 島控制項的程式：

* Windows 社區工具組中的[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) 。 這些 WPF 控制項會包裝對應 UWP 控制項的介面和功能，而且可以像 Visual Studio 設計工具中的任何其他 WPF 控制項一樣使用。

* UWP 行事[曆視圖](/windows/uwp/design/controls-and-patterns/calendar-view)控制項。 這是您將使用 Windows 社區工具組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項裝載的標準 UWP 控制項。

### <a name="net-core-3"></a>.NET Core 3

[.Net Core](https://docs.microsoft.com/dotnet/core/)是一個開放原始碼架構，可實現跨平臺、輕量且易於擴充的完整 .NET Framework 版本。 相較于完整的 .NET Framework，.NET Core 啟動時間更快，而且許多 Api 都已經過優化。

透過其前幾個版本，.NET Core 的重點在於支援 web 或後端應用程式。 有了 .NET Core，您就可以輕鬆建立可調整的 web 應用程式或 Api，以裝載于 Windows、Linux 或 Docker 容器之類的微服務架構中。

.NET Core 3 是 .NET Core 的最新版本。 這個版本的重點是支援 Windows 傳統型應用程式，包括 Windows Forms 和 WPF 應用程式。 您可以在 .NET Core 3 上執行新的和現有的 Windows 傳統型應用程式，並享有 .NET Core 帶來的所有優點。 裝載於 [XAML Islands](xaml-islands.md) 中的 UWP 控制項，也可以在目標為 .NET Core 3 的 Windows Forms 和 WPF 應用程式中使用。

> [!NOTE]
> WPF 和 Windows Forms 不會成為跨平臺，而且您無法在 Linux 和 MacOS 上執行 WPF 或 Windows Forms。 WPF 和 Windows Forms 的 UI 元件仍然具有 Windows 轉譯系統的相依性。

如需詳細資訊，請參閱 [.NET Core 3.0 的新功能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)。