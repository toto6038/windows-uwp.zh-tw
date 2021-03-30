---
description: 使用 WinUI 3 UI 來建立 .NET 和 c + +/Win32 桌面應用程式。
title: 建立適用于桌面的基本 WinUI 3 應用程式
ms.date: 03/19/2021
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: e7453f620b7c82c46a44e293a36a9eb51bfb36ae
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730882"
---
# <a name="build-a-basic-winui-3---project-reunion-05-desktop-app"></a>建立基本 WinUI 3-Project 留尼旺島0.5 傳統型應用程式

在本文中，我們將逐步解說如何建立基本的 managed c #/.NET 5 和原生 c + +/Win32 WinUI 3-Project 留尼旺島0.5 傳統型應用程式，其使用者介面完全由 WinUI 3 控制項和功能組成。

## <a name="prerequisites"></a>必要條件

1. 設定您的開發環境。 請參閱 [開始使用 Project 留尼旺島](../../project-reunion/get-started-with-project-reunion.md)。
1. 建立第一個適用于 c # 和 .NET 5 專案的 WinUI 3 傳統型應用程式，以測試您的設定。 請參閱 [開始使用適用于桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)。

:::image type="content" source="images/build-basic/template-app.png" alt-text="正在執行的初始範本應用程式。":::<br/>
*正在執行的初始範本應用程式。*

## <a name="initial-template-app"></a>初始範本應用程式

我們將從初始範本應用程式開始建立，但在開始之前，我們先來看看範本應用程式的結構和內容。

### <a name="the-application-solution"></a>應用程式解決方案

根據預設，此方案包含兩個專案：應用程式本身，另一個用於建立 [MSIX](/windows/msix) 應用程式套件。

> [!NOTE]
> 目前需要 MSIX 應用程式套件，才能將使用 Project 留尼旺島0.5 的應用程式部署到其他電腦。 不過，未來的 Project 留尼旺島版本將支援部署未封裝的應用程式。

:::image type="content" source="images/build-basic/template-app-solution-explorer.png" alt-text="方案總管顯示初始範本應用程式的檔案結構。":::<br/>
*方案總管顯示初始範本應用程式的檔案結構。*

### <a name="the-application-project"></a>應用程式專案

現在，按兩下應用程式專案檔 (或按一下滑鼠右鍵，然後選取 [編輯專案檔] ) ，這會在 XML 文字編輯器中開啟檔案。

以下是來自初始範本應用程式的專案檔，其中包含兩個注意專案：

- `TargetFramework`是 .net [5](/dotnet/core/dotnet-five)，這是 .net 的主要實作為進展。
- `PackageReference`各種專案留尼旺島功能的 NuGet 元素，包括 WinUI。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    <TargetPlatformMinVersion>10.0.17763.0</TargetPlatformMinVersion>
    <RootNamespace>WinUIApp2</RootNamespace>
    <Platforms>x86;x64;arm64</Platforms>
    <RuntimeIdentifiers>win10-x86;win10-x64;win10-arm64</RuntimeIdentifiers>
    <NoWin32Manifest>true</NoWin32Manifest>
    <ApplicationIcon />
    <StartupObject />
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.ProjectReunion" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.Foundation" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="0.5.0-prerelease" />
    <Manifest Include="$(ApplicationManifest)" />
  </ItemGroup>
</Project>
```

### <a name="the-mainwindowxaml-file"></a>MainWindow .xaml 檔案

使用 WinUI 3，您現在可以在 XAML 標記中建立 [視窗](/windows/winui/api/microsoft.ui.xaml.window) 類別的實例。

XAML 視窗類別已擴充為支援桌面視窗，將它轉換成 UWP 和傳統型應用程式模型所使用的每個低層級視窗執行的抽象概念。 具體而言，UWP 會使用 CoreWindow，而 Win32 會使用視窗控制碼 (或 Hwnd) 和對應的 Win32 Api。

下列程式碼範例是來自初始範本應用程式的 MainWindow .xaml 檔案，其使用 Window 類別做為應用程式的根項目。

```xaml
<Window
    x:Class="WinUIApp2.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:WinUIApp2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
    </StackPanel>
</Window>
```

## <a name="basic-managed-cnet-5-app"></a>基本受控 c #/.NET 5 應用程式

在第一個範例中，讓我們從 User32.dll 使用 [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow) API，以程式設計方式將視窗最大化。

1. 首先，若要從 User32.dll 呼叫 Win32 Api，請從 [Visual Studio] 功能表中，將 [ **PInvoke** ] nuget 套件新增至您的專案 (，然後選取 [ **工具]-> [nuget 封裝管理員-> 管理解決方案的 nuget 套件** ]) 。如需詳細資訊，請參閱 [從 Managed 程式碼呼叫原生](/cpp/dotnet/calling-native-functions-from-managed-code)函式。
1. 新增封裝之後，請開啟 MainWindow，然後以下列程式碼取代該程式碼後端檔案，並 `MyButton_Click` 將事件處理常式取代為：

    ```csharp
            private void MyButton_Click(object sender, RoutedEventArgs e)
            {
                myButton.Content = "Clicked";
    
                IntPtr hwnd = (App.Current as App).MainWindowWindowHandle;
                PInvoke.User32.ShowWindow(hwnd, PInvoke.User32.WindowShowStyle.SW_MAXIMIZE);
            }
    ```

    [PInvoke](/dotnet/standard/native-interop/pinvoke) [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow)方法會使用 () 的視窗控制碼 `hwnd` 做為第一個參數，並使用第二個參數來指定應將視窗最大化。 

1. 若要取得視窗控制碼，請使用 [GetActiveWindow](/windows/win32/api/winuser/nf-winuser-getactivewindow) 方法。 這個方法會傳回目前使用中視窗的視窗控制碼 (在啟用目標視窗) 之後，呼叫這個方法。

    `MainWindow`物件 (請參閱 MainWindow，在應用程式中 `OnLaunched` 找到的事件處理常式中建立、具現化和啟動) 。 `OnLaunched`以下列程式碼取代事件：

    ```csharp
        protected override void OnLaunched(Microsoft.UI.Xaml.LaunchActivatedEventArgs args)
        {
            m_window = new MainWindow();
            m_window.Activate();
            m_windowhandle = PInvoke.User32.GetActiveWindow();
        }

        IntPtr m_windowhandle;
        public IntPtr MainWindowWindowHandle { get { return m_windowhandle; } }
    ```

1. 編譯和執行應用程式。
1. 最後，按 [Click me] 按鈕，應用程式視窗應該會最大化。

## <a name="full-trust-desktop-app"></a>「完全信任」的桌面應用程式

WinUI 3 提供在 [AppContainer](/windows/win32/secauthz/appcontainer-for-legacy-applications-)安全性沙箱以外以「完全信任」許可權執行應用程式的能力。

Managed c #/.NET 5 和原生 c + +/Win32 WinUI 3-Project 留尼旺島0.5 桌面應用程式可以呼叫 .NET 5 Api 而不受限制。 在下列範例中 (衍生自先前範例中所用的初始範本應用程式) 我們會示範如何查詢目前的進程，並取得所有載入的模組清單 (無法使用 UWP 應用程式) 。

1. 在 MainWindow .xaml 檔案中，加入 [ContentDialog](/windows/winui/api/microsoft.ui.xaml.controls.contentdialog) 元素。

    ```xaml
    <StackPanel 
        Orientation="Horizontal" 
        HorizontalAlignment="Center" 
        VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
        <ContentDialog x:Name="contentDialog" CloseButtonText="Close">
            <StackPanel>
                <TextBlock Name="cdTextBlock"/>
            </StackPanel>
        </ContentDialog>
    </StackPanel>
    ```

1. 在 MainWindow 程式碼後端檔案中，從 [系統](/dotnet/api/system.diagnostics) 呼叫 .net api，以取得 [目前進程](/dotnet/api/system.diagnostics.process.getcurrentprocess)中載入的模組。 在此範例中，我們只會逐一查看[Process. 模組](/dotnet/api/system.diagnostics.process.modules)物件中的每個[ProcessModule](/dotnet/api/system.diagnostics.processmodule) 。

    ```csharp
    private async void MyButton_Click(object sender, RoutedEventArgs e)
    {
        myButton.Content = "Clicked";

        var description = new System.Text.StringBuilder();
        var process = System.Diagnostics.Process.GetCurrentProcess();
        foreach (System.Diagnostics.ProcessModule module in process.Modules)
        {
            description.AppendLine(module.FileName);
        }

        cdTextBlock.Text = description.ToString();
        await contentDialog.ShowAsync();
    }
    ```
1. 編譯和執行應用程式。
1. 最後，按 [Click me （按我）] 按鈕，對話方塊應該會出現進程清單。

    :::image type="content" source="images/build-basic/template-app-full-trust.png" alt-text="完全信任的應用程式範例。":::<br/>*完全信任的應用程式範例。*

## <a name="summary"></a>總結

在本主題中，我們討論了如何存取基礎視窗執行（在此案例中為 Win32 和 Hwnd），並在您的應用程式中使用 Win32 Api 以及 Windows 10 的 WinRT Api。 這可讓您在建立新的 WinUI 3 傳統型應用程式時，使用大部分現有的桌面應用程式程式碼。

我們也討論了如何搭配使用 .NET 5 Api 與 WinUI 3 作為 UI 架構。

## <a name="see-also"></a>另請參閱

- [Windows UI 程式庫 3-Project 留尼旺島 0.5 (三月 2021) ](index.md)
- [開始使用 Project 留尼旺島](../../project-reunion/get-started-with-project-reunion.md)
- [開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)
