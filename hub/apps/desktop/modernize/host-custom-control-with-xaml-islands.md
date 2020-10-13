---
description: 本文示範如何使用 XAML Islands 在 WPF 應用程式中裝載自訂 WinRT XAML 控制項。
title: 在 WPF 應用程式中使用 XAML Islands 裝載自訂 WinRT XAML 控制項
ms.date: 10/02/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, 自訂控制項, 使用者控制項, 主控制項
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: e516d887f0bfc668551c0a43b135e98765f3300f
ms.sourcegitcommit: b8d0e2c6186ab28fe07eddeec372fb2814bd4a55
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2020
ms.locfileid: "91671537"
---
# <a name="host-a-custom-winrt-xaml-control-in-a-wpf-app-using-xaml-islands"></a>在 WPF 應用程式中使用 XAML Islands 裝載自訂 WinRT XAML 控制項

本文示範如何使用 Windows 社群工具組中的 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項，在以 .NET Core 3.1 為目標的 WPF 應用程式中裝載自訂 WinRT XAML 控制項。 自訂控制項包含來自 Windows SDK 的幾個第一方控制項，並將其中一個 WinRT XAML 控制項中的屬性繫結至 WPF 應用程式中的字串。 本文也會示範如何從 [WinUI 程式庫](/uwp/toolkits/winui/)裝載控制項。

雖然本文示範如何在 WPF 應用程式中執行這項作業，但此程序類似於 Windows Forms 應用程式。 如需在 WPF 和 Windows Forms 應用程式中裝載 WinRT XAML 控制項的概觀，請參閱[這篇文章](xaml-islands.md#wpf-and-windows-forms-applications)。

## <a name="required-components"></a>必要元件

若要在 WPF (或 Windows Forms) 應用程式中裝載自訂 WinRT XAML 控制項，您的方案需要有下列元件。 本文提供建立每個元件的指示。

* **應用程式的專案和原始程式碼**。 只有在以 .NET Core 3.x 為目標的應用程式中，才支援使用 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項來裝載自訂控制項。 以 .NET Framework 為目標的應用程式不支援此案例。

* **自訂 WinRT XAML 控制項**。 您需要想裝載之自訂控制項的原始程式碼，才能使用您的應用程式進行編譯。 自訂控制項通常會定義於 UWP 類別庫專案，而您會在與 WPF 或 Windows Forms 專案相同的方案中參考該專案。

* **UWP 應用程式專案，可定義從 XamlApplication 衍生的根應用程式類別**。 您的 WPF 或 Windows Forms 專案必須能存取 Windows 社群工具組所提供 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) \(英文)\ 類別的執行個體，讓其能夠探索和載入自訂 UWP XAML 控制項。 建議的做法是在個別的 UWP 應用程式專案中定義此物件，而該專案屬於 WPF 或 Windows Forms 應用程式的方案。 

    > [!NOTE]
    > 您的方案只能包含一個可定義 `XamlApplication` 物件的專案。 您應用程式中的所有自訂 WinRT XAML 控制項都會共用相同的 `XamlApplication` 物件。 定義 `XamlApplication` 物件的專案必須包含所有其他 WinRT 程式庫的參考，以及用來在 XAML Island 上裝載控制項的專案。

## <a name="create-a-wpf-project"></a>建立 WPF 專案

開始使用之前，請遵循下列指示來建立 WPF 專案，並將其設定為裝載 XAML Islands。 如果有現有的 WPF 專案，則可針對您的專案調整這些步驟和程式碼範例。

> [!NOTE]
> 如果您有以 .NET Framework 為目標的現有專案，則必須將此專案遷移至 .NET Core 3.1。 如需詳細資訊，請參閱[這個部落格系列](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

1. 如果您尚未這麼做，請安裝最新版的 [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/current)。

2. 在 Visual Studio 2019 中，建立新的 [WPF 應用程式 (.NET Core)]  專案。

3. 請確定已啟用[套件參考](/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，按一下 [工具] -> [NuGet 套件管理員]-> [套件管理員設定]  。
    2. 確定已針對 [預設套件管理格式]  選取 [PackageReference]  。

4. 請以滑鼠右鍵按一下 [方案總管]  中的 WPF 專案，然後選擇 [管理 NuGet 套件]  。

5. 在 [NuGet 套件管理員]  視窗中，確定已選取 [包含發行前版本]  。

6. 選取 [瀏覽] 索引標籤，搜尋 [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) 套件，然後安裝最新的穩定版本。 此套件會提供使用 **WindowsXamlHost** 控制項來裝載 WinRT XAML 控制項所需的一切，包括其他相關的 NuGet 套件。
    > [!NOTE]
    > Windows Forms 應用程式必須使用 [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) 套件。

7. 將您的方案設定為以特定平台 (例如 x86 或 x64) 為目標。 以 **任何 CPU**為目標的專案不支援自訂 WinRT XAML 控制項。

    1. 在 [方案總管]  中，以滑鼠右鍵按一下方案節點，然後選取 [屬性]   -> [設定屬性]   -> [設定管理員]  。
    2. 在 [使用中的方案平台]  中，選取 [新增]  。 
    3. 在 [新增方案平台]  對話方塊中，選取 [x64]  或 [x86]  ，然後按 [確定]  。 
    4. 關閉已開啟的對話方塊。

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>在 UWP 應用程式專案中定義 XamlApplication 類別

接著，將 UWP 應用程式專案新增至您的方案，並修訂此專案中的預設 `App` 類別，以便從 Windows 社群工具組所提供的 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 類別衍生。 這個類別支援 [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider) 介面，該介面可讓您的應用程式在執行時探索和載入自訂 UWP XAML 控制項的中繼資料，而這些中繼資料位於應用程式目前目錄組件中。 這個類別也會初始化目前執行緒的 UWP XAML 架構。 

1. 在 [方案總管]  中，在方案節點上按一下滑鼠右鍵，然後選取 [新增]   -> [新增專案]  。
2. 將 [空白應用程式 (通用 Windows)]  專案新增到您的方案。 請確定目標版本和最低版本都設定為 [Windows 10 1903 版 (組件 18362)] 或更新版本。
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

6. 從 UWP 應用程式專案中刪除 **MainPage.xaml**檔案。
7. 清除 UWP 應用程式專案，然後加以建置。

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

## <a name="create-a-custom-winrt-xaml-control"></a>建立自訂 WinRT XAML 控制項

若要在 WPF 應用程式中裝載自訂 WinRT XAML 控制項，您必須擁有控制項的原始程式碼，才能使用您的應用程式進行編譯。 通常自訂控制項會定義於 UWP 類別庫專案，以便移植。

在本節中，您會在新的類別庫專案中定義簡單的自訂控制項。 您也可以在上一節中建立的 UWP 應用程式專案中，定義自訂控制項。 不過，這些步驟會在個別的類別庫專案中執行此動作，以供說明之用，因為這通常是自訂控制項為了便於移植的實作方式。

如果您已經有自訂控制項，即可加以使用，代替此處所示的控制項。 不過，您仍然需要設定包含此控制項的專案，如下列步驟所示。

1. 在 [方案總管]  中，在方案節點上按一下滑鼠右鍵，然後選取 [新增]   -> [新增專案]  。
2. 將 [類別庫 (通用 Windows)]  專案新增到您的方案。 請確定 [目標版本] 和 [最小版本] 都已設定為與 UWP 專案相同的目標和最小 OS 組建。
3. 以滑鼠右鍵按一下專案檔案，然後選取 [卸載專案]  。 再次以滑鼠右鍵按一下專案檔案，然後選取 [編輯]  。
4. 在結尾 `</Project>` 元素之前，新增下列 XML 以停用數個屬性，然後儲存專案檔案。 您必須啟用這些屬性，才能在 WPF (或 Windows Forms) 應用程式中裝載自訂控制項。

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. 以滑鼠右鍵按一下專案檔案，然後選取 [重新載入專案]  。
6. 刪除預設 **Class1.cs** 檔案，然後將新的 [使用者控制項]  項目新增至專案。
7. 在使用者控制項的 XAML 檔案中，新增下列 `StackPanel` 作為預設 `Grid` 的子系。 此範例會新增 ``TextBlock`` 控制項，然後將該控制項的 ``Text`` 屬性繫結至 ``XamlIslandMessage`` 欄位。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom WinRT XAML control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. 在使用者控制項的程式碼後置檔案中，將 `XamlIslandMessage` 欄位新增至使用者控制項類別，如下所示。

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. 建立 UWP 類別庫專案。
10. 在 WPF 專案中，以滑鼠右鍵按一下 [相依性]  節點，並新增 UWP 類別庫專案的參考。
11. 在您稍早設定的 UWP 應用程式專案中，以滑鼠右鍵按一下 [參考]  節點，並新增 UWP 類別庫專案的參考。
12. 重新建置整個方案，並確定所有專案都建立成功。

## <a name="host-the-custom-winrt-xaml-control-in-your-wpf-app"></a>在 WPF 應用程式中裝載自訂 WinRT XAML 控制項

1. 在 [方案總管]  中，展開 WPF 專案，然後開啟 MainWindow.xaml 檔案或您想在其中裝載自訂控制項的視窗。
2. 在 XAML 檔案中，將下列命名空間宣告新增至 `<Window>` 元素。

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 在相同的檔案中，將下列控制項新增至 `<Grid>` 元素。 將 `InitialTypeName` 屬性變更為 UWP 類別庫專案中使用者控制項的完整名稱。

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 開啟程式碼後置檔案，然後將下列程式碼新增到 `Window` 類別。 此程式碼會定義 `ChildChanged` 事件處理常式，以將 UWP 自訂控制項的 ``XamlIslandMessage`` 欄位值，指派給 WPF 應用程式中 `WPFMessage` 欄位的值。 將 `UWPClassLibrary.MyUserControl` 變更為 UWP 類別庫專案中使用者控制項的完整名稱。

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. 建置並執行您的應用程式，並確認 UWP 使用者控制項如預期般顯示。

## <a name="add-a-control-from-the-winui-2x-library-to-the-custom-control"></a>將控制項從 WinUI 2.x 程式庫新增至自訂控制項

習慣上，WinRT XAML 控制項已當作 Windows 10 OS 的一部分發行，並可透過 Windows SDK 提供給開發人員使用。 [WinUI 程式庫](/uwp/toolkits/winui/)是替代方法，其中 Windows SDK 中已更新的 WinRT XAML 控制項版本會在未繫結至 Windows SDK 版本的 NuGet 套件中散發。 此程式庫也包含不屬於 Windows SDK 和預設 UWP 平台的新控制項。 如需詳細資訊，請參閱我們的 [WinUI 程式庫藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

本節示範如何將 WinRT XAML 控制項從 WinUI 2.x 程式庫新增至您的使用者控制項。

> [!NOTE]
> 目前，XAML Islands 僅支援從 WinUI 2.x 程式庫裝載控制項。 從 WinUI 3 程式庫裝載控制項的支援會在之後的版本中推出。

1. 在 UWP 應用程式專案中，安裝 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 套件的最新版本或發行前版本。

    > [!NOTE]
    > 如果您的傳統型應用程式封裝在 [MSIX 套件](/windows/msix)中，您可使用 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NugGet 套件的發行前或發行版本。 如果您的傳統型應用程式未使用 MSIX 進行封裝，您必須安裝 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 套件的發行前版本。

2. 在此專案的 App.xaml 檔案中，將下列子元素新增至 `<xaml:XamlApplication>` 元素。

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    新增此元素之後，此檔案的內容現在應該如下所示。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. 在 UWP 類別庫專案中，安裝最新版的 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 套件 (與您在 UWP 應用程式專案中安裝的相同版本)。

4. 在相同的專案中，開啟使用者控制項的 XAML 檔案，然後將下列命名空間宣告新增至 `<UserControl>` 元素。

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. 在相同的檔案中，新增 `<winui:RatingControl />` 元素作為 `<StackPanel>` 的子系。 此元素會從 WinUI 程式庫新增 [RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol) 類別的執行個體。 新增此屬性之後，`<StackPanel>` 現在應該如下所示。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom WinRT XAML control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. 建置並執行您的應用程式，並確認新的評分控制項如預期般顯示。

## <a name="package-the-app"></a>封裝應用程式

您可選擇性地在 [MSIX 套件](/windows/msix)中封裝 WPF 應用程式以供部署。 MSIX 是 Windows 的新式應用程式封裝技術，以 MSI、.appx、App-V 和 ClickOnce 安裝技術的組合為基礎。

下列指示說明如何使用 Visual Studio 2019 中的 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，將方案中的所有元件封裝在 MSIX 套件中。 只有當您想要在 MSIX 套件中封裝 WPF 應用程式時，才需要執行這些步驟。 

> [!NOTE]
> 如果您選擇不要在 [MSIX 套件](/windows/msix)中封裝應用程式以供部署，則執行您應用程式的電腦必須安裝 [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)。

1. 將新的 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增到您的方案。 當您建立專案時，請選取和您為 UWP 專案所選取的值相同的 [目標版本] 和 [最小版本]。

2. 在封裝專案中，以滑鼠右鍵按一下 [應用程式] 接點，然後選擇 [新增參考]。 在專案清單中，選取您方案中的 WPF 專案，然後按一下 [確定]。

3. 建置和執行封裝專案。 確認 WPF 可執行，而且 UWP 自訂控制項如預期般顯示。

## <a name="related-topics"></a>相關主題

* [在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)