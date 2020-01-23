---
description: 本文示範如何使用 XAML 島，在 WPF 應用程式中裝載標準 UWP 控制項。
title: 使用 XAML 群島在 WPF 應用程式中裝載標準 UWP 控制項
ms.date: 01/10/2010
ms.topic: article
keywords: windows 10，uwp，windows forms，wpf，xaml 群島，包裝的控制項，標準控制項，InkCanvas，InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 47998d9c79dbbb41060b4fbd52fe3ee805aecc52
ms.sourcegitcommit: 85fd390b1e602707bd9342cb4b84b97ae0d8b831
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2020
ms.locfileid: "76520383"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>使用 XAML 群島在 WPF 應用程式中裝載標準 UWP 控制項

本文示範使用[XAML Islands](xaml-islands.md)，在 WPF 應用程式中裝載標準 UWP 控制項（也就是 Windows SDK 或 WinUI 程式庫所提供的第一方 UWP 控制項）的兩種方式：

* 它會示範如何使用 Windows 社區工具組中的[包裝控制項](xaml-islands.md#wrapped-controls)來裝載 UWP [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)和[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)控制項。 這些控制項會包裝一小部分有用 UWP 控制項的介面和功能。 您可以將它們直接加入至 WPF 或 Windows Forms 專案的設計介面，然後像設計工具中的任何其他 WPF 或 Windows Forms 控制項一樣使用它們。

* 它也會示範如何使用 Windows 社區工具組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項來裝載 UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)控制項。 因為只有一小組 UWP 控制項是以包裝控制項的形式提供，所以您可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)來裝載任何其他標準 UWP 控制項。

若要在 WPF 應用程式中裝載 UWP 控制項，您需要下列元件。 本文提供建立這些元件的指示。

* WPF 應用程式的專案和原始程式碼。
* UWP 應用程式專案，可從 Windows 社區工具組定義 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 類別的實例。
  > [!NOTE]
  > 為了確保您的應用程式在所有 XAML 島案例中都能正常運作，您的 WPF （或 Windows Forms）專案必須具有 `XamlApplication` 物件的存取權。 這個物件會作為根中繼資料提供者，以便在應用程式目前目錄的元件中載入 UWP XAML 類型的中繼資料。 建議的做法是將**空白應用程式（通用 Windows）** 專案加入至與 WPF （或 Windows Forms）專案相同的方案中，並修訂此專案中的預設 `App` 類別，以衍生自 `XamlApplication`。
  >
  > 雖然簡單的 XAML 島案例（例如裝載第一方 UWP 控制項）不需要此步驟，但您的 WPF 應用程式需要此 `XamlApplication` 物件，才能支援完整範圍的 XAML 島案例，包括裝載自訂 UWP 控制項。 建議您一律新增 UWP 專案，並在您使用 XAML Islands 的任何方案中定義 `XamlApplication` 物件。 您的方案只能包含一個定義 `XamlApplication` 物件的專案。 應用程式中的所有自訂 UWP 控制項都會共用相同的 `XamlApplication` 物件。

雖然本文示範如何在 WPF 應用程式中裝載 UWP 控制項，但此程式類似于 Windows Forms 應用程式。

## <a name="create-a-wpf-project"></a>建立 WPF 專案

開始使用之前，請遵循這些指示來建立 WPF 專案，並將其設定為裝載 XAML 島。 如果您有現有的 WPF 專案，您可以針對您的專案調整這些步驟和程式碼範例。

1. 在 Visual Studio 2019 中，建立新的**Wpf 應用程式（[.NET Framework）** ] 或 [ **wpf 應用程式（.net Core）** ] 專案。 如果您想要建立**WPF 應用程式（.Net core）** 專案，您必須先安裝最新版的[.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)。

2. 請確定已啟用[套件參考](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，按一下 **工具-> NuGet 套件管理員-> 套件管理員設定**。
    2. 請確定已選取 [**預設套件管理格式**] 的 [ **PackageReference** ]。

3. 以滑鼠右鍵按一下**方案總管**中的 WPF 專案，然後選擇 [**管理 NuGet 封裝**]。

4. 在 [ **NuGet 套件管理員**] 視窗中，確認已選取 [**包含發行**前版本]。

5. 選取 [**流覽**] 索引標籤，[搜尋 [6.0.0](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) ] 或更新版本的 []，然後安裝封裝。 此套件提供使用適用于 WPF 的包裝 UWP 控制項所需的所有專案（包括[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) ，以及[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項）。
    > [!NOTE]
    > Windows Forms 的應用程式必須使用6.0.0 或更新版本的[工具](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)套件。

6. 將您的解決方案設定為以特定平臺（例如 x86 或 x64）為目標。 以**任何 CPU**為目標的專案不支援大部分的 XAML 島案例。

    1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選取 **屬性** ** -> ** 設定 屬性 -> **Configuration Manager**。 
    2. 在 [使用中的**方案平臺**] 底下，選取 [**新增**]。 
    3. 在 [**新增方案平臺**] 對話方塊中，選取 [ **X64** ] 或 [ **X86** ] 並按 **[確定]** 。 
    4. 關閉開啟的對話方塊。

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>在 UWP 應用程式專案中建立 XamlApplication 物件

接下來，將 UWP 應用程式專案新增至與 WPF 專案相同的方案中。 您將會修訂此專案中的預設 `App` 類別，以衍生自 Windows 社區工具組所提供的 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 類別。 雖然簡單的 XAML 島案例（例如裝載單一第一方 UWP 控制項）不需要此步驟，但您的 WPF 應用程式需要此 `XamlApplication` 物件，才能支援完整的 XAML 島案例。 我們建議您一律將此專案新增至您使用 XAML Islands 的任何解決方案。

1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選取 [**加入** -> **新增專案**]。
2. 新增 **[空白應用程式 (通用 Windows)\]** 專案到您的方案。 請確定 [目標版本] 和 [最小版本] 都設定為**Windows 10 1903 版**或更新版本。
3. 在 UWP 應用程式專案中，安裝[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 套件（6.0.0 版或更新版本）。
4. 開啟**app.xaml**檔案，並以下列 xaml 取代此檔案的內容。 將 `MyUWPApp` 取代為 UWP 應用程式專案的命名空間。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. 開啟**App.xaml.cs**檔案，並以下列程式碼取代此檔案的內容。 將 `MyUWPApp` 取代為 UWP 應用程式專案的命名空間。

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. 從 UWP 應用程式專案中刪除**MainPage。**
7. 建立 UWP 應用程式專案。
8. 在 WPF 專案中，以滑鼠右鍵按一下 [相依性 **]** 節點，並加入 UWP 應用程式專案的參考。

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>使用包裝的控制項裝載 InkCanvas 和 InkToolbar

現在您已將專案設定為使用 UWP XAML Islands，現在已準備好將[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)包裝的 UWP 控制項新增至應用程式。

1. 在 **方案總管**中，開啟 **MainWindow.xaml** 檔。

2. 在 XAML 檔案頂端附近的**Window**元素中，新增下列屬性。 這會參考[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)包裝 UWP 控制項的 XAML 命名空間。

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    加入這個屬性之後， **Window**元素現在看起來應該像這樣。

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

3. 在**mainwindow.xaml**中，以下列 xaml 取代現有的 `<Grid>` 元素。 此 XAML 會將[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)控制項（前面加上您稍早定義的**Controls**關鍵字做為命名空間）新增至 `<Grid>`。

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
    > 您也可以將這些和其他已包裝的控制項，從 [**工具箱**] 的 [ **Windows 控制項工具**組] 區段拖曳到設計工具中，將它們加入至視窗。

4. 儲存**mainwindow.xaml. xaml**檔案。

    如果您的裝置支援數位畫筆（例如介面），而且您是在實體機器上執行此實驗室，您現在可以使用畫筆來建立並執行應用程式，並在螢幕上繪製數位筆跡。 不過，如果您沒有具備畫筆功能的裝置，而且嘗試使用滑鼠來簽署，就不會發生任何事。 發生這種情況的原因是，預設只會針對數位畫筆啟用**InkCanvas**控制項。 然而，您可以變更此行為。

5. 開啟**MainWindow.xaml.cs**檔案。

6. 將下列命名空間宣告新增至檔案頂端：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. 找出 `MainWindow()` 的函式。 在 `InitializeComponent()` 方法後面新增下列程式程式碼，並儲存程式碼檔案。

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    您可以使用**InkPresenter**物件來自訂預設的筆跡體驗。 這段程式碼會使用**InputDeviceTypes**屬性來啟用滑鼠和手寫筆輸入。

8. 再按一次 F5，以在偵錯工具中重建並執行應用程式。 如果您是使用具有滑鼠的電腦，請確認您可以使用滑鼠在筆墨畫布空間中繪製東西。

## <a name="host-a-calendarview-by-using-the-host-control"></a>使用主控制項裝載 CalendarView

現在您已將[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)和[INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)包裝的 UWP 控制項新增至應用程式，現在已準備好使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項，將[CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)新增至應用程式。

> [!NOTE]
> [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項是由[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)封裝所提供的。 此套件包含在您稍早安裝的[Microsoft. 工具](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)組中。

1. 在 **方案總管**中，開啟 **MainWindow.xaml** 檔。

2. 在 XAML 檔案頂端附近的**Window**元素中，新增下列屬性。 這會參考[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項的 XAML 命名空間。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    加入這個屬性之後， **Window**元素現在看起來應該像這樣。

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

4. 在**mainwindow.xaml**中，以下列 xaml 取代現有的 `<Grid>` 元素。 此 XAML 會在方格中加入一個資料列，並將[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)物件加入到最後一個資料列。 為了裝載 UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)控制項，這個 XAML 會將 `InitialTypeName` 屬性設定為控制項的完整名稱。 此 XAML 也會定義 `ChildChanged` 事件的事件處理常式，這會在已呈現裝載的控制項時引發。

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

5. 儲存**mainwindow.xaml** ，然後開啟**MainWindow.xaml.cs**檔案。

7. 將下列命名空間宣告新增至檔案頂端：

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. 將下列 `ChildChanged` 事件處理常式方法新增至 `MainWindow` 類別，並儲存程式碼檔案。 當主控制項已轉譯時，這個事件處理常式會執行，並為行事曆控制項的 `SelectedDatesChanged` 事件建立簡單的事件處理常式。

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

11. 再按一次 F5，以在偵錯工具中重建並執行應用程式。 確認行事曆控制項現在顯示在視窗底部。

## <a name="package-the-app"></a>封裝應用程式

您可以選擇性地封裝[MSIX 封裝](https://docs.microsoft.com/windows/msix)中的 WPF 應用程式以進行部署。 MSIX 是適用于 Windows 的現代化應用程式封裝技術，它是以 MSI、APPX、App-v 和 ClickOnce 安裝技術的組合為基礎。

下列指示說明如何使用 Visual Studio 2019 中的[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，在 MSIX 套件中封裝解決方案中的所有元件。 只有當您想要在 MSIX 套件中封裝 WPF 應用程式時，才需要執行這些步驟。

1. 將新的[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增至您的方案。 當您建立專案時，請選取 [ **Windows 10，版本1903（10.0;）]。組建18362）** ，適用于**目標版本**和**最低版本**。

2. 在封裝專案中，以滑鼠右鍵按一下 [**應用程式**] 節點，然後選擇 [**加入參考**]。 在專案清單中，選取方案中的 WPF 專案，然後按一下 **[確定]** 。

3. 如果您的 WPF 專案是以 .NET Core 3 為目標，您必須編輯封裝專案檔。 這些變更目前需要用於封裝以 .NET Core 3 為目標並裝載 UWP 控制項的 WPF 應用程式。

    1. 在方案總管中，以滑鼠右鍵按一下封裝專案節點，然後選取 [**編輯專案檔案**]。
    2. 從檔案中找出 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 元素。 以下列 XML 取代此元素。

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject></SourceProject>
                <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. 儲存並關閉專案檔。

4. 將您的解決方案設定為以特定平臺（例如 x86 或 x64）為目標。 這是使用 Windows 應用程式封裝專案將 WPF 應用程式建立到 MSIX 套件的必要條件。

    1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選取 **屬性** ** -> ** 設定 屬性 -> **Configuration Manager**。
    2. 在 [使用中的**方案平臺**] 底下，選取 [ **x64**或**x86**]。
    3. 在 WPF 專案的資料列中，選取 [**平臺**] 資料行中的 [**新增**]。
    4. 在 [**新增方案平臺**] 對話方塊中，選取 [ **x64** ] 或 [ **x86** ] （您為使用中的**方案平臺**選取的相同平臺），然後按一下 **[確定]** 。
    5. 關閉開啟的對話方塊。

5. 建立並執行封裝專案。 確認 WPF 執行，而且 UWP 自訂控制項如預期般顯示。

## <a name="related-topics"></a>相關主題

* [桌面應用程式中的 UWP 控制項](xaml-islands.md)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
