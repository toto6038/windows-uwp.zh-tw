---
description: 本文示範如何使用 XAML 島，在 WPF 應用程式中裝載自訂 UWP 控制項。
title: 使用 XAML 群島在 WPF 應用程式中裝載自訂 UWP 控制項
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10，uwp，windows forms，wpf，xaml 島，自訂控制項，使用者控制項，主控制項
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 3c14cfaefcf10aa051e3054d5df2e6da9fd77602
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317059"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>使用 XAML 群島在 WPF 應用程式中裝載自訂 UWP 控制項

本文示範如何使用 Windows 社區工具組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項，在以 .net Core 3 為目標的 WPF 應用程式中裝載自訂 UWP 控制項。 自訂控制項包含來自 Windows SDK 的幾個第一方 UWP 控制項，並將其中一個 UWP 控制項中的屬性系結至 WPF 應用程式中的字串。 本文也會示範如何也從[WinUI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)裝載第一方 UWP 控制項。

雖然本文示範如何在 WPF 應用程式中執行這項作業，但此程式與 Windows Forms 應用程式的處理方式類似。 如需有關在 WPF 和 Windows Forms 應用程式中裝載 UWP 控制項的總覽，請參閱[這篇文章](xaml-islands.md#wpf-and-windows-forms-applications)。

## <a name="overview"></a>總覽

若要在 WPF 應用程式中裝載自訂 UWP 控制項，您需要下列元件。 本文提供建立這些元件的指示。

* **WPF 應用程式的專案和原始程式碼**。 只有在 WPF 和以 .NET Core 3 為目標的 Windows Forms 應用程式中，才支援使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項來裝載自訂 UWP 控制項。 以 .NET Framework 為目標的應用程式不支援此案例。

* **自訂 UWP 控制項**。 您需要裝載的自訂 UWP 控制項的原始程式碼，才能使用您的應用程式進行編譯。 自訂控制項通常會定義在 UWP 類別庫專案中，而您會在與 WPF （或 Windows Forms）專案相同的方案中參考它。

* **定義 XamlApplication 物件的 UWP 應用程式專案**。 您的 WPF （或 Windows Forms）專案必須能夠存取 Windows 社區工具組`Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication`所提供之類別的實例。 這個物件會作為根中繼資料提供者，以便在應用程式目前目錄的元件中載入自訂 UWP XAML 類型的中繼資料。 建議的做法是將**空白應用程式（通用 Windows）** 專案加入至與 WPF （或 Windows Forms）專案相同的方案中，並修訂此專案中的`App`預設類別。
  > [!NOTE]
  > 您的方案只能包含一個定義`XamlApplication`物件的專案。 應用程式中的所有自訂 UWP 控制項都會`XamlApplication`共用相同的物件。 定義`XamlApplication`物件的專案必須包含所有其他 UWP 程式庫的參考，以及在 XAML 島中主控 UWP 控制項的專案。

## <a name="create-a-wpf-project"></a>建立 WPF 專案

開始使用之前，請遵循這些指示來建立 WPF 專案，並將其設定為裝載 XAML 島。 如果您有現有的 WPF 專案，您可以針對您的專案調整這些步驟和程式碼範例。

> [!NOTE]
> 如果您有以 .NET Framework 為目標的現有專案，您必須將專案遷移至 .NET Core 3。 如需詳細資訊，請參閱[此 blog 系列](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

1. 如果您還沒有這麼做，請安裝最新版的[.Net Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)。

2. 在 Visual Studio 2019 中，建立新的**WPF 應用程式（.Net Core）** 專案。

3. 請確定已啟用[套件參考](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，按一下 **工具-> NuGet 套件管理員-> 套件管理員設定**。
    2. 請確定已選取 [**預設套件管理格式**] 的 [ **PackageReference** ]。

4. 以滑鼠右鍵按一下**方案總管**中的 WPF 專案，然後選擇 [**管理 NuGet 封裝**]。

5. 在 [ **NuGet 套件管理員**] 視窗中，確認已選取 [**包含發行**前版本]。

6. 選取 [**流覽**] 索引標籤，搜尋[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)套件（版本 v 6.0.0-preview7 或更新版本），並安裝封裝。 此套件提供您使用**WindowsXamlHost**控制項來裝載 UWP 控制項所需的所有專案，包括其他相關的 NuGet 套件。
    > [!NOTE]
    > Windows Forms 應用程式必須使用[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)套件（版本 v 6.0.0-preview7 或更新版本）。

7. 將您的解決方案設定為以特定平臺（例如 x86 或 x64）為目標。 以**任何 CPU**為目標的專案不支援自訂 UWP 控制項。

    1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選取 [**屬性** -> ] [設定**屬性** -> ] [**Configuration Manager**]。 
    2. 在 [使用中的**方案平臺**] 底下，選取 [**新增**]。 
    3. 在 [**新增方案平臺**] 對話方塊中，選取 [ **X64** ] 或 [ **X86** ] 並按 **[確定]** 。 
    4. 關閉開啟的對話方塊。

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>在 UWP 應用程式專案中建立 XamlApplication 物件

接下來，將 UWP 應用程式專案新增至與 WPF 專案相同的方案中。 您將修改此專案`App`中的預設類別，以衍生自`Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` Windows 社區工具組所提供的類別。 WPF 應用程式中的**WindowsXamlHost**物件需要此`XamlApplication`物件來裝載自訂 UWP 控制項。

1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選取 [**加入** -> **新專案**]。
2. 新增 **[空白應用程式 (通用 Windows)\]** 專案到您的方案。 請確定 [目標版本] 和 [最小版本] 都設定為**Windows 10 1903 版**或更新版本。
3. 在 UWP 應用程式專案中，安裝[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 套件（6.0.0-preview7 或更新版本）。
4. 開啟**app.xaml**檔案，並以下列 xaml 取代此檔案的內容。 將`MyUWPApp`取代為 UWP 應用程式專案的命名空間。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. 開啟**App.xaml.cs**檔案，並以下列程式碼取代此檔案的內容。 將`MyUWPApp`取代為 UWP 應用程式專案的命名空間。

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

## <a name="create-a-custom-uwp-control"></a>建立自訂 UWP 控制項

若要在 WPF 應用程式中裝載自訂 UWP 控制項，您必須具有控制項的原始程式碼，才能使用您的應用程式進行編譯。 通常自訂控制項會定義于 UWP 類別庫專案中，以方便移植。

在本節中，您會在新的類別庫專案中定義簡單的自訂 UWP 控制項。 您也可以在上一節中建立的 UWP 應用程式專案中，定義自訂 UWP 控制項。 不過，這些步驟會在個別的類別庫專案中執行此動作，以供說明之用，因為這通常是為了可攜性而實行自訂控制項的方式。

如果您已經有自訂控制項，您可以使用它，而不是此處所示的控制項。 不過，您仍然需要設定包含控制項的專案，如下列步驟所示。

1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選取 [**加入** -> **新專案**]。
2. 將 **[類別庫（通用 Windows）** ] 專案新增至您的方案。 請確定 [目標版本] 和 [最小版本] 都設定為**Windows 10 1903 版**或更新版本。
3. 以滑鼠右鍵按一下專案檔，然後選取 **[卸載專案**]。 再次以滑鼠右鍵按一下專案檔，然後選取 [**編輯**]。
4. 在結尾`</Project>`元素之前，新增下列 XML 以停用數個屬性，然後儲存專案檔。 您必須啟用這些屬性，才能將自訂 UWP 控制項裝載在 WPF （或 Windows Forms）應用程式中。

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. 以滑鼠右鍵按一下專案檔，然後選取 [**重載專案**]。
6. 刪除預設的**Class1.cs**檔案，並將新的**使用者控制項**專案加入至專案。
7. 在使用者控制項的 XAML 檔案中，加入下列`StackPanel`做為預設`Grid`的子系。 這個範例會加入``TextBlock``控制項，然後``Text``將該控制項的屬性系結至``XamlIslandMessage``欄位。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. 在使用者控制項的程式碼後置檔案中，將`XamlIslandMessage`欄位加入至使用者控制項類別，如下所示。

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
10. 在 WPF 專案中，以滑鼠右鍵按一下 [相依性 **]** 節點，並加入 UWP 類別庫專案的參考。
11. 在您稍早設定的 UWP 應用程式專案中，以滑鼠右鍵按一下 [**參考**] 節點，並加入 UWP 類別庫專案的參考。
12. 重建整個方案，並確認所有專案都已成功建立。

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>在您的 WPF 應用程式中裝載自訂 UWP 控制項

1. 在**方案總管**中，展開 WPF 專案，然後開啟 [mainwindow.xaml] 檔案或其他您要裝載自訂控制項的視窗。
2. 在 XAML 檔案中，將下列命名空間宣告新增至`<Window>`元素。

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 在相同的檔案中，將下列控制項新增至`<Grid>`專案。 `InitialTypeName`將屬性變更為 UWP 類別庫專案中使用者控制項的完整名稱。

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 開啟程式碼後置檔案，並將下列程式碼新增`Window`至類別。 這段程式碼`ChildChanged`會定義事件處理常式，將 UWP ``XamlIslandMessage``自訂控制項的欄位值指派`WPFMessage`給 WPF 應用程式中的欄位值。 在`UWPClassLibrary.MyUserControl` UWP 類別庫專案中，變更為使用者控制項的完整名稱。

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

6. 建立並執行您的應用程式，並確認 UWP 使用者控制項如預期般顯示。

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>從 WinUI 程式庫將控制項加入至自訂控制項

在過去，UWP 控制項已發行為 Windows 10 OS 的一部分，並可透過 Windows SDK 提供給開發人員使用。 [WinUI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)是一種替代的方法，其中 Windows SDK 的第一方 UWP 控制項的更新版本會散發在未系結至 Windows SDK 版本的 NuGet 套件中。 此程式庫也包含不屬於 Windows SDK 和預設 UWP 平臺的新控制項。 如需詳細資訊，請參閱我們的[WinUI 程式庫藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

本節示範如何將 UWP 控制項從 WinUI 程式庫新增至您的使用者控制項，讓您可以將此控制項裝載于 WPF 應用程式中。 

1. 在 UWP 應用程式專案中，安裝最新的發行前版本的[Microsoft. UI. Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 套件。
    > [!NOTE]
    > 請確定您已安裝最新的*發行*前版本。 目前，如果您選擇在[MSIX 套件](https://docs.microsoft.com/windows/msix)中封裝應用程式以進行部署，則只有此套件的發行前版本才可正常使用。

2. 在此專案的 app.xaml 檔案中，將下列子項目加入至`<xaml:Application>`元素。

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    新增此元素之後，此檔案的內容現在看起來應該像這樣。

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

3. 在 UWP 類別庫專案中，安裝最[新的搶](https://www.nuget.org/packages/Microsoft.UI.Xaml)鮮版（您在 UWP 應用程式專案中安裝的版本）。

4. 在相同的專案中，開啟使用者控制項的 XAML 檔案，並將下列命名空間宣告新增至`<UserControl>`專案。

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. 在相同的檔案中，加入`<winui:RatingControl />`專案做為的`<StackPanel>`子系。 這個元素會從 WinUI 程式庫加入[RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2)類別的實例。 加入這個元素之後， `<StackPanel>`現在應該看起來像這樣。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. 建立並執行您的應用程式，並確認新的評等控制項如預期般顯示。

## <a name="package-the-app"></a>封裝應用程式

您可以選擇性地封裝[MSIX 封裝](https://docs.microsoft.com/windows/msix)中的 WPF 應用程式以進行部署。 MSIX 是適用于 Windows 的現代化應用程式封裝技術，它是以 MSI、APPX、App-v 和 ClickOnce 安裝技術的組合為基礎。

下列指示說明如何使用 Visual Studio 2019 中的[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，在 MSIX 套件中封裝解決方案中的所有元件。 只有當您想要在 MSIX 套件中封裝 WPF 應用程式時，才需要執行這些步驟。 請注意，這些步驟目前包含裝載自訂 UWP 控制項之案例特有的一些因應措施。

1. 將新的[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增至您的方案。 當您建立專案時，請選取 [ **Windows 10，版本1903（10.0;）]。組建18362）** ，適用于**目標版本**和**最低版本**。

2. 在封裝專案中，以滑鼠右鍵按一下 [**應用程式**] 節點，然後選擇 [**加入參考**]。 在專案清單中，選取方案中的 WPF 專案，然後按一下 **[確定]** 。

3. 編輯封裝專案檔案。 封裝以 .NET Core 3 為目標並裝載 XAML 孤島的 WPF 應用程式時，目前需要進行這些變更。

    1. 在方案總管中, 以滑鼠右鍵按一下封裝專案節點, 然後選取 [**編輯專案檔案**]。
    2. 從檔案中找出 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 元素。 以下列 XML 取代此元素。 這些變更目前需要用於封裝以 .NET Core 3 為目標並裝載 UWP 控制項的 WPF 應用程式。

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

    3. 儲存專案檔案並將它關閉。

4. 編輯套件資訊清單，以參考正確的預設啟動顯示畫面影像。 目前需要此解決方法，才能封裝裝載自訂 UWP 控制項的 WPF 應用程式。

    1. 在封裝專案中，以滑鼠右鍵按一下**package.appxmanifest.xml**檔案，然後按一下 [**查看程式碼**]。
    2. 在檔案中找出下列元素。

        ```<uap:SplashScreen Image="Images\SplashScreen.png" />```

    3. 將此元素變更為：

        ```<uap:SplashScreen Image="Images\SplashScreen.scale-200.png" />```

    4. 儲存**package.appxmanifest.xml**檔案並將它關閉。

5. 編輯 WPF 專案檔案。 目前需要這些變更，才能封裝裝載自訂 UWP 控制項的 WPF 應用程式。

    1. 在方案總管中，以滑鼠右鍵按一下 WPF 專案節點，然後選取 **[卸載專案**]。
    2. 以滑鼠右鍵按一下 WPF 專案節點，然後選取 [**編輯**]。
    3. 找出檔案`</PropertyGroup>`中的最後一個結束記號，然後緊接在該標記之後加入下列 XML。

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. 儲存專案檔案並將它關閉。
    5. 以滑鼠右鍵按一下 WPF 專案節點，然後選擇 [**重載專案**]。

6. 建立並執行封裝專案。 確認 WPF 執行，而且 UWP 自訂控制項如預期般顯示。

## <a name="related-topics"></a>相關主題

* [桌面應用程式中的 UWP 控制項](xaml-islands.md)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)