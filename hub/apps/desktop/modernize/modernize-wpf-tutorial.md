---
description: 本教學課程會示範如何新增 UWP XAML 使用者介面，建立 MSIX 套件，以及您的 WPF 應用程式中納入其他現代的元件。
title: 教學課程：將 WPF 應用程式現代化
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows form、 wpf、 xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420082"
---
# <a name="tutorial-modernize-a-wpf-app"></a>教學課程：將 WPF 應用程式現代化 

有很多種[現代化](index.md)現有的傳統型應用程式，將最新的 Windows 功能整合到現有的原始程式碼，而不是重寫從可用的應用程式。 在本教學課程中，我們將探討數種方式可將現有的 WPF 特定業務應用程式現代化使用這些功能：

* .NET Core 3
* 使用 XAML 群島的 UWP XAML 控制項
* 自適性卡片和 Windows 10 通知
* MSIX 部署

本教學課程需要下列的開發技能：

* 在開發使用 WPF 的 Windows 傳統型應用程式體驗。
* 基本知識C#和 XAML。
* UWP 的基本知識。

## <a name="overview"></a>總覽

本教學課程會提供名為 Contoso 費用簡單 WPF 特定業務應用程式的程式碼。 在本教學課程的虛構案例中，Contoso 費用會是內部應用程式的 Contoso Corporation 的管理員用來追蹤其報表所提交的費用。 管理員現在具備觸控功能的裝置，他們想要使用沒有滑鼠或鍵盤 Contoso 費用報銷應用程式。 不幸的是，目前版本的應用程式不是觸控易記。

Contoso 想要以新的 Windows 功能，讓員工能夠更有效率地建立費用報銷報告此應用程式現代化。 許多功能可以藉由建置新的 UWP 應用程式輕鬆地實作。 不過，現有的應用程式很複雜，而且是由不同小組開發的許多年的結果。 因此，使用新技術從頭重寫不是選項。 小組在尋找的新功能加入現有的程式碼基底的最佳方法。

在本教學課程開頭，Contoso 費用會以.NET Framework 4.7.2 為目標，並使用下列的第 3 個合作對象程式庫：

* MVVM 模式的基本實作 MVVM Light。
* Unity 的相依性插入容器。
* LiteDb，內嵌的 NoSQL 解決方案來儲存資料。
* 假，工具，以產生假的資料。

在教學課程中，您將會使用新的 Windows 功能增強 Contoso 費用：

* 將現有的 WPF 應用程式移轉到.NET Core 3.0。 這會在未來開啟新的和重要的案例。
* 主機使用 XAML 群島**InkCanvas**並**MapControl**包裝 Windows 社群工具組所提供的控制項。
* 使用 XAML 群島，來裝載任何標準的 UWP XAML 控制項 (在此情況下， **CalendardView**)。
* 整合應用程式中的自適性卡片和 Windows 10 的通知。
* Azure DevOps，讓您可以自動提供新版本的應用程式給測試人員和使用者便可為管線的 MSIX 與 CI/CD 設定應用程式的套件。

## <a name="prerequisites"></a>必要條件

若要執行本教學課程中，您的開發電腦必須已安裝這些必要條件：

* Windows 10、windows 版 1903年 （組建 18362） 或更新版本。
* [Visual Studio 2019](https://www.visualstudio.com)。
* [.NET core 3 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) （安裝最新可用的預覽版本）。

請確定您使用 Visual Studio 2019 上安裝下列工作負載和選用功能：

* .NET 桌面開發
* 通用 Windows 平台開發
* Windows 10 SDK (10.0.18362.0 或更新版本)

## <a name="get-the-contoso-expenses-sample-app"></a>取得 Contoso 費用範例應用程式

在開始本教學課程之前，請下載 Contoso 費用報銷應用程式的原始程式碼，並確定您可以建置 Visual Studio 中的程式碼。

1. 下載應用程式來源程式碼，從**釋放**索引標籤[AppConsult WinAppsModernization 研討會存放庫](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)。 直接連結[ https://aka.ms/wamwc ](https://aka.ms/wamwc)。
2. 開啟 zip 檔案，並擷取到的根目錄的所有內容您**c:\\** 磁碟機。 它會建立名為的資料夾**C:\WinAppsModernizationWorkshop**。
3. 開啟 Visual Studio 2019，然後按兩下**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln**來開啟的方案檔案。
4. 請確認您可以建置、 執行和偵錯 Contoso 費用 WPF 專案，藉由按下**啟動**按鈕或按 CTRL + F5。

## <a name="get-started"></a>立即開始

您有 Contoso 費用範例應用程式的原始程式碼，而且您可以確認您可以在 Visual Studio 中建置它之後，您即可開始本教學課程：

* [第 1 部分：將 Contoso 的移轉到.NET Core 3 費用報銷應用程式](modernize-wpf-tutorial-1.md)
* [第 2 部分：新增 UWP InkCanvas 控制項使用 XAML 群島](modernize-wpf-tutorial-2.md)
* [第 3 部分：新增 UWP CalendarView 控制項使用 XAML 群島](modernize-wpf-tutorial-3.md)
* [第 4 部分：新增 Windows 10 使用者活動及通知](modernize-wpf-tutorial-4.md)
* [第 5 部分：封裝和部署使用 MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要概念

下列各節提供一些在本教學課程所討論的重要概念背景。 如果您已經熟悉這些概念，您可以略過本節。

### <a name="universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP)

在 Windows 8 中，Microsoft 引進了新的架構，稱為 Windows 執行階段 (WinRT)。 不同.NET Framework 中，於 WinRT 會是原生應用程式開發介面會直接公開給應用程式層。 WinRT 也引進了語言投影，也就是可讓開發人員互動使用的語言，例如它在執行階段上增加層C#和 JavaScript，除了C++。 投影可讓開發人員建置運用相同 WinRT 應用程式C#和他們在使用.NET Framework 建置應用程式所獲得的 XAML 知識。 

在 Windows 10 中，Microsoft 引進了[通用 Windows 平台 (UWP)](/windows/uwp/get-started/universal-application-platform-guide)，這建置在 WinRT 之上。 UWP 的最重要功能是它會提供一組常用的 Api 在每個裝置平台： 不論應用程式正在執行的桌面、 Xbox One 上或 HoloLens，您是否能夠使用相同的 Api。

從現在開始，最新的 Windows 10 功能會公開透過 WinRT Api，包括時間軸、 Project Rome 和 Windows Hello 等功能。

### <a name="msix-packaging"></a>MSIX 封裝

[MSIX](http://aka.ms/msix) （前身為 AppX） 是 Windows 應用程式的最新的封裝模型。 MSIX 支援 UWP 應用程式，以及使用 Win32、 WPF、 Windows Form、 Java、 Electron，等技術建置的傳統型應用程式。 當您封裝 MSIX 封裝中的傳統型應用程式時，您可以發佈您的應用程式至 Microsoft Store。 傳統型應用程式在安裝時，可讓您的桌面應用程式使用廣泛的 WinRT Api，也會取得封裝識別碼。

如需詳細資訊，請參閱下列文章：

* [桌面應用程式封裝](/windows/uwp/porting/desktop-to-uwp-root)
* [在幕後的桌面應用程式封裝](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML 群島

從 Windows 10 版本 1903，您可以裝載在非 UWP 傳統型應用程式使用稱為功能中的 UWP 控制項*XAML 群島*。 這項功能可讓您加強的外觀、 風格和您現有的傳統型應用程式最新的 Windows 10 UI 功能才可透過 UWP 控制項的功能。 這表示您可以使用 UWP 功能，例如 Windows Ink 和支援 Fluent Design System，在您現有的 WPF、 Windows Form 控制項和C++Win32 應用程式。

如需詳細資訊，請參閱 <<c0> [ 桌面應用程式 (XAML Ostrovy) 中的 UWP 控制項](/windows/uwp/xaml-platform/xaml-host-controls)。 本教學課程會引導您完成使用兩種不同的 XAML 島控制項的程序：

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)並[MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) Windows 社群工具組中。 這些 WPF 控制項包裝介面和對應的 UWP 控制項的功能，並可以使用像是 Visual Studio 設計工具中的任何其他 WPF 控制項。

* UWP[行事曆檢視](/windows/uwp/design/controls-and-patterns/calendar-view)控制項。 這是標準的 UWP 控制項，您會使用裝載[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Windows 社群工具組中的控制項。

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/)是一種開放原始碼架構，實作完整的.NET Framework 的跨平台、 輕量型且易於擴充版本。 相較於完整.NET Framework，.NET Core 啟動時間會更快，並已進行最佳化的許多 Api。

透過其第一次的數個版本，.NET Core 的重點是支援 web 或後端應用程式。 使用.NET Core，您可以輕鬆地建置可擴充的 web 應用程式或可在 Windows、 Linux 或微服務架構，例如在 Docker 容器中裝載的 Api。

.NET Core 3 是 .NET Core 的下一個主要版本。 這個即將推出版本的亮點是支援 Windows 傳統型應用程式，包括 Windows Forms 和 WPF 應用程式。 您可以在.NET Core 3 上執行新的和現有 Windows 傳統型應用程式，並享受.NET Core 所提供的所有權益。 裝載於 [XAML Islands](xaml-islands.md) 中的 UWP 控制項，也可以在目標為 .NET Core 3 的 Windows Forms 和 WPF 應用程式中使用。

> [!NOTE]
> WPF 和 Windows Form 不會成為跨平台，您無法在 Linux 和 MacOS 上執行的 WPF 或 Windows Form。 WPF 和 Windows Forms 的 UI 元件會 Windows 呈現系統上，仍有相依性。

如需詳細資訊，請參閱下列文章：

* [.NET Core 3.0 Preview 1 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/) (英文)
* [.NET Core 3.0 Preview 2 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/) (英文)
* [.NET Core 3.0 Preview 2 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/) (英文)
* [.NET Core 3.0 Preview 4 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/) (英文)
* [.NET Core 3.0 (Preview 2) 的新功能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)。