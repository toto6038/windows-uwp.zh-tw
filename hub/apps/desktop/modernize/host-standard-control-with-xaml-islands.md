---
description: 本文示範如何使用 XAML Islands 在 WPF 應用程式中裝載標準 WinRT XAML 控制項。
title: 使用 XAML Islands 在 WPF 應用程式中裝載標準 WinRT XAML 控制項
ms.date: 10/02/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, 已包裝的控制項, 標準控制項, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 3358e679b44affdb5207207a30fe6b5282ac6759
ms.sourcegitcommit: f7c7a2ae6367e114a8b9d438963082440cd24043
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315081"
---
# <a name="host-a-standard-winrt-xaml-control-in-a-wpf-app-using-xaml-islands"></a>使用 XAML Islands 在 WPF 應用程式中裝載標準 WinRT XAML 控制項

本文示範兩種使用 [XAML 孤島](xaml-islands.md) 來裝載標準 WinRT xaml 控制項的方式， (也就是以 .net Core 3.1 為目標的 WPF 應用程式中，由 Windows SDK) 所提供的第一方 WinRT xaml 控制項：

* 其會示範如何使用 Windows 社群工具組中[包裝的控制項](xaml-islands.md#wrapped-controls)來裝載 UWP [InkCanvas](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 和 [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar) 控制項。 這些控制項會包裝一小部分有用 WinRT XAML 控制項的介面和功能。 您可以直接將控制項新增至 WPF 或 Windows Forms 專案的設計介面，然後像設計工具中的任何其他 WPF 或 Windows Forms 控制項一樣來使用它們。

* 其也會示範如何使用 Windows 社群工具組中的 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項來裝載 UWP [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 控制項。 只有一小組 WinRT XAML 控制項會以已包裝的控制項形式提供，因此您可使用 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 來裝載任何其他標準 WinRT XAML 控制項。

雖然本文示範如何在 WPF 應用程式中裝載 WinRT XAML 控制項，但此程序類似於 Windows Forms 應用程式。

> [!NOTE]
> 目前只有以 .NET Core 3.x 為目標的應用程式才支援使用 XAML 孤島來裝載 WPF 中的 WinRT XAML 控制項和 Windows Forms 應用程式。 以 .NET 5 為目標的應用程式，或任何 .NET Framework 版本的應用程式中，尚不支援 XAML 孤島。

## <a name="required-components"></a>必要元件

若要在 WPF (或 Windows Forms) 應用程式中裝載 WinRT XAML 控制項，您的方案需要有下列元件。 本文提供建立每個元件的指示。

* **應用程式的專案和原始程式碼**。 目前只有以 .NET Core 3.x 為目標的應用程式才支援使用 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項來裝載 WinRT XAML 控制項。

* **UWP 應用程式專案，可定義從 XamlApplication 衍生的根應用程式類別**。 您的 WPF 或 Windows Forms 專案必須能存取 Windows 社群工具組所提供 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) \(英文)\ 類別的執行個體，讓其能夠探索和載入自訂 UWP XAML 控制項。 建議的做法是在個別的 UWP 應用程式專案中定義此物件，而該專案屬於 WPF 或 Windows Forms 應用程式的方案。 

    > [!NOTE]
    > 雖然裝載第一方 WinRT XAML 控制項不需要 `XamlApplication` 物件，但您的應用程式需要此物件來支援完整範圍的 XAML Island 案例，包括裝載自訂 WinRT XAML 控制項。 因此，我們建議您一律在任何使用 XAML Islands 的方案中定義 `XamlApplication` 物件。

    > [!NOTE]
    > 您的方案只能包含一個可定義 `XamlApplication` 物件的專案。 定義 `XamlApplication` 物件的專案必須包含所有其他程式庫的參考，以及用來在 XAML Island 上裝載 WinRT XAML 控制項的專案。

## <a name="create-a-wpf-project"></a>建立 WPF 專案

開始使用之前，請遵循下列指示來建立 WPF 專案，並將其設定為裝載 XAML Islands。 如果有現有的 WPF 專案，則可針對您的專案調整這些步驟和程式碼範例。

1. 在 Visual Studio 2019 中，建立新的 [WPF 應用程式 (.NET Core)]  專案。 如果您尚未這麼做，則必須先安裝最新版的 [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/current)。

2. 請確定已啟用[套件參考](/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，按一下 [工具] -> [NuGet 套件管理員]-> [套件管理員設定]。
    2. 確定已針對 [預設套件管理格式] 選取 [PackageReference]。

3. 請以滑鼠右鍵按一下 [方案總管] 中的 WPF 專案，然後選擇 [管理 NuGet 套件]。

4. 選取 [瀏覽] 索引標籤，搜尋 [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 套件，然後安裝最新的穩定版本。 此套件會提供使用 WPF 適用的已包裝 WinRT XAML 控制項所需的一切 (包括 [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)、[InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 和 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項)。
    > [!NOTE]
    > Windows Forms 應用程式必須使用 [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) 套件。

5. 將您的方案設定為以特定平台 (例如 x86 或 x64) 為目標。 在以 **任何 CPU** 為目標的專案中，不支援大部分的 XAML Islands 案例。

    1. 在 [方案總管] 中，以滑鼠右鍵按一下方案節點，然後選取 [屬性] -> [設定屬性] -> [設定管理員]。 
    2. 在 [使用中的方案平台]  中，選取 [新增]  。 
    3. 在 [新增方案平台]  對話方塊中，選取 [x64]  或 [x86]  ，然後按 [確定]  。 
    4. 關閉已開啟的對話方塊。

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>在 UWP 應用程式專案中定義 XamlApplication 類別

接著，將 UWP 應用程式專案新增至您的方案，並修訂此專案中的預設 `App` 類別，以便從 Windows 社群工具組所提供的 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 類別衍生。 這個類別支援 [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider) 介面，該介面可讓您的應用程式在執行時探索和載入自訂 UWP XAML 控制項的中繼資料，而這些中繼資料位於應用程式目前目錄組件中。 這個類別也會初始化目前執行緒的 UWP XAML 架構。

> [!NOTE]
> 雖然裝載第一方 WinRT XAML 控制項不需要執行此步驟，但您的應用程式需要 `XamlApplication` 物件來支援完整範圍的 XAML Island 案例，包括裝載自訂 WinRT XAML 控制項。 因此，我們建議您一律在任何使用 XAML Islands 的方案中定義 `XamlApplication` 物件。

1. 在 [方案總管] 中，在方案節點上按一下滑鼠右鍵，然後選取 [新增] -> [新增專案]。
2. 將 [空白應用程式 (通用 Windows)]  專案新增到您的方案。 請確定目標版本和最低版本都設定為 [Windows 10 1903 版 (組件 18362)] 或更新版本。 也請確定這個新的 UWP 專案不在 WPF 專案的子資料夾中。 否則，WPF 應用程式稍後會嘗試建立 UWP XAML 標記，就像是 WPF XAML 一樣。
3. 在 UWP 應用程式專案中，安裝 [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 套件 (最新的穩定版本)。
4. 開啟 **pp.xaml** 檔案，並以下列 XAML 取代此檔案的內容。 以您 UWP 應用程式專案的命名空間取代 `MyUWPApp`。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. 開啟 **App.xaml.cs** 檔案，並以下列程式碼取代此檔案的內容。 以您 UWP 應用程式專案的命名空間取代 `MyUWPApp`。

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. 從 UWP 應用程式專案中刪除 **MainPage.xaml** 檔案。
7. 建置 UWP 應用程式專案。

## <a name="add-a-reference-to-the-uwp-project-in-your-wpf-project"></a>在 WPF 專案中新增 UWP 專案的參考

1. 在 WPF 專案檔中指定相容的 Framework 版本。 

    1. 在 [方案總管] 中，按兩下 [WPF 專案] 節點在編輯器中開啟專案檔。
    2. 在第一個 **PropertyGroup** 元素中，新增下列子元素。 視需要變更值的 `19041` 部分，以符合 UWP 專案的目標和最小 OS 組建。

        ```xml
        <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        ```

        當您完成時，**PropertyGroup** 元素看起來應該會像這樣。

        ```xml
        <PropertyGroup>
            <OutputType>WinExe</OutputType>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UseWPF>true</UseWPF>
            <Platforms>AnyCPU;x64</Platforms>
            <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        </PropertyGroup>
        ```

2. 在 [方案總管] 中，以滑鼠右鍵按一下 WPF 專案底下的 [相依性] 節點，並新增 UWP 應用程式專案的參考。

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>在 WPF 應用程式的進入點中具現化 XamlApplication 物件

接著，將程式碼新增至 WPF 應用程式的進入點，以建立您剛在 UWP 專案中所定義 `App` 類別的執行個體 (這是現在衍生自 `XamlApplication` 的類別)。

1. 在 WPF 專案中，以滑鼠右鍵按一下專案節點，選取 [新增] -> [新增項目]，然後選取 [類別]。 將類別命名為 **Program**，然後按一下 [新增]。

2. 以下列程式碼取代所產生的 `Program` 類別，然後儲存檔案。 以 UWP 應用程式專案的命名空間取代 `MyUWPApp`，並以 WPF 應用程式專案的命名空間取代 `MyWPFApp`。

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. 在專案節點上按一下滑鼠右鍵，然後選擇 [屬性]。

4. 在屬性的 [應用程式] 索引標籤上，按一下 [啟始物件] 下拉式清單，然後選擇您在上一個步驟中所新增 `Program` 類別的完整名稱。 
    > [!NOTE]
    > 根據預設，WPF 專案會在所產生的程式碼檔案中定義不打算修改的 `Main` 進入點函式。 此步驟會將您專案的進入點變更為新 `Program` 類別的 `Main` 方法，這可讓您在應用程式的啟動程序中，儘早新增可執行的程式碼。 

5. 將您的變更儲存至專案屬性。

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>使用包裝的控制項來裝載 InkCanvas 和 InkToolbar

既然您已將專案設定為使用 UWP XAML Islands，現在就準備好將 [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和 [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 包裝的 WinRT XAML 控制項新增至應用程式。

1. 在 WPF 專案中，開啟 **MainWindow.xaml** 檔案。

2. 在靠近 XAML 檔案頂端的 **Window** 元素中，新增下列屬性。 這會參考 [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 和 [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 包裝 WinRT XAML 控制項的 XAML 命名空間。

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    新增此屬性之後，Window 元素現在應該如下所示。

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. 在 **Mainwindow.xaml** 檔案中，以下列 XAML 取代現有的 `<Grid>` 元素。 此 XAML 會將 [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 和 [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 控制項 (前面加上您先前定義為命名空間的 **Controls** 關鍵字) 新增至 `<Grid>`。

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > 您也可以將這些和其他已包裝的控制項，從 [工具箱] 的 [Windows 社群工具組] 區段拖曳至設計工具，藉此新增到視窗。

4. 儲存 **MainWindow.xaml** 檔案。

    如果您的裝置支援數位筆 (例如介面)，而且您是在實體機器上執行此實驗室，現在即可建置和執行應用程式，並在螢幕上使用手寫筆繪製數位筆跡。 不過，如果您沒有具備手寫筆功能的裝置，而嘗試使用滑鼠來簽署，就不會發生任何作用。 發生這種情況的原因是，預設只會針對數位筆啟用 **InkCanvas** 控制項。 然而，您可以變更此行為。

5. 開啟 **MainWindow.xaml.cs** 檔案。

6. 在檔案頂端新增下列命名空間宣告：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. 找出 `MainWindow()` 建構函式。 在 `InitializeComponent()` 方法之後新增下列程式碼行，並儲存程式碼檔案。

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    您可以使用 **InkPresenter** 物件來自訂預設筆跡體驗。 此程式碼會使用 **InputDeviceTypes** 屬性來啟用滑鼠和手寫筆輸入。

8. 再次按 F5，以在偵錯工具中重建並執行應用程式。 如果您使用有滑鼠的電腦，請確認您可使用滑鼠在筆跡畫布空間中繪製東西。

## <a name="host-a-calendarview-by-using-the-host-control"></a>使用主控制項裝載 CalendarView

既然您已將 [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 和 [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 包裝 WinRT XAML 控制項新增至應用程式，現在就已準備好使用 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項，將 [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 新增至應用程式。

> [!NOTE]
> [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項是由 [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)套件所提供。 此套件隨附於您稍早安裝的 [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 套件。

1. 在 [方案總管] 中，開啟 [MainWindow.xaml] 檔案。

2. 在靠近 XAML 檔案頂端的 **Window** 元素中，新增下列屬性。 這會參考 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項的 XAML 命名空間。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    新增此屬性之後，Window 元素現在應該如下所示。

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. 在 **Mainwindow.xaml** 檔案中，以下列 XAML 取代現有的 `<Grid>` 元素。 此 XAML 會在方格中新增一列，並將 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 物件新增至最後一列。 為了裝載 UWP [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 控制項，此 XAML 會將 `InitialTypeName` 屬性設定為控制項的完整名稱。 此 XAML 也會定義 `ChildChanged` 事件的事件處理常式，已轉譯裝載的控制項時會引發此事件。

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. 儲存 **MainWindow.xaml** 檔案，然後開啟 **MainWindow.xaml.cs** 檔案。

7. 在檔案頂端新增下列命名空間宣告：

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. 將下列 `ChildChanged` 事件處理常式方法新增至 `MainWindow` 類別，並儲存程式碼檔案。 若已轉譯主控制項，此事件處理常式會執行，並且為行事曆控制項的 `SelectedDatesChanged` 事件建立簡單事件處理常式。

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. 再次按 F5，以在偵錯工具中重建並執行應用程式。 確認行事曆控制項現在顯示在視窗底部。

## <a name="package-the-app"></a>封裝應用程式

您可選擇性地在 [MSIX 套件](/windows/msix)中封裝 WPF 應用程式以供部署。 MSIX 是 Windows 的新式應用程式封裝技術，以 MSI、.appx、App-V 和 ClickOnce 安裝技術的組合為基礎。

下列指示說明如何使用 Visual Studio 2019 中的 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，將方案中的所有元件封裝在 MSIX 套件中。 只有當您想要在 MSIX 套件中封裝 WPF 應用程式時，才需要執行這些步驟。

> [!NOTE]
> 如果您選擇不要在 [MSIX 套件](/windows/msix)中封裝應用程式以供部署，則執行您應用程式的電腦必須安裝 [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)。

1. 將新的 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增到您的方案。 當您建立專案時，請選取和您為 UWP 專案所選取的值相同的 [目標版本] 和 [最小版本]。

2. 在封裝專案中，以滑鼠右鍵按一下 [應用程式] 接點，然後選擇 [新增參考]。 在專案清單中，選取您方案中的 WPF 專案，然後按一下 [確定]。

    > [!NOTE]
    > 如果您想要在 Microsoft Store 中發佈您的應用程式，您必須在封裝專案中新增 UWP 專案的參考。

3. 將您的方案設定為以特定平台 (例如 x86 或 x64) 為目標。 這是使用 Windows 應用程式封裝專案將 WPF 應用程式建置到 MSIX 套件中的必要條件。

    1. 在 [方案總管] 中，以滑鼠右鍵按一下方案節點，然後選取 [屬性] -> [設定屬性] -> [設定管理員]。
    2. 在 [使用中的方案平台] 中，選取 [x64] 或 [x86]。
    3. 在 WPF 專案的資料列中，選取 [平台] 資料行中的 [新增]。
    4. 在 [新的方案平台] 對話方塊中，選取 [x64] 或 [x86] (您針對 [使用中的方案平台] 選取的相同平台)，然後按一下 [確定]。
    5. 關閉已開啟的對話方塊。

5. 建置和執行封裝專案。 確認 WPF 執行，而且 UWP 控制項如預期般顯示。

## <a name="related-topics"></a>相關主題

* [在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
