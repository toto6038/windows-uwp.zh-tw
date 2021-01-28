---
description: '使用 c #/WinRT 撰寫 Windows 執行階段元件，並從原生應用程式使用'
title: '建立 c #/WinRT 元件並從 c + +/WinRT 使用它'
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d489e293ca40aa38c27e4c3e19bba6f8a6705e3b
ms.sourcegitcommit: 6f15cc14e0c4c13999c862664fa7a70de8730b74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987126"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>逐步解說：建立 c #/WinRT 元件並從 c + +/WinRT 使用它

> [!NOTE]
> 此文章中所述的 c #/WinRT 撰寫支援目前為預覽版本，因為 c #/WinRT 1.1.1 版。 在此版本中，它只能用於早期的意見反應和評估。

C #/WinRT 可讓 .NET 5 開發人員使用類別庫專案，在 c # 中撰寫自己的 Windows 執行階段元件。 撰寫的元件可以在具有封裝參考的原生桌面應用程式中使用，或使用 **winmd** 檔案參考來取用。

本逐步解說會示範如何使用 c #/WinRT 來建立您自己的 Windows 執行階段類型、將它們封裝為 Windows 執行階段元件，以及從 c + +/WinRT 主控台應用程式取用元件。

撰寫您的執行時間元件時，請遵循 [本文中所](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 述的指導方針和類型限制，而您的元件中的 Windows 執行階段類型可以使用 UWP 應用程式中允許的任何 .net 功能。 如需詳細資訊，請參閱適用 [于 UWP 應用程式的 .net](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)。 就外部而言，您的型別成員只能公開其參數和傳回值的 Windows 執行階段類型。

> [!NOTE]
> 有一些 [對應至 .net 類型](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)的 Windows 執行階段類型。 這些 .NET 型別可用於 Windows 執行階段元件的公用介面，並且會顯示給元件的使用者，作為對應的 Windows 執行階段類型。

## <a name="prerequisites"></a>必要條件

本逐步解說需要下列工具和元件：

- Visual Studio 2019
- .NET 5.0 SDK
- C + + [/WINRT VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) For c + +/WinRT 專案範本

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>使用 c #/WinRT 建立簡單的 Windows 執行階段元件

首先，在 Visual Studio 2019 中建立新專案。 選取 **類別庫 ( .Net Core)** 專案範本，並將它命名為 **AuthoringDemo**。 您將需要對專案進行下列新增和修改：

1. 安裝最新版的 [c #/WinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)。

    a. 在方案總管中，以滑鼠右鍵按一下專案節點，然後選取 [ **管理 NuGet 套件**]。

    b. 搜尋 **CsWinRT** NuGet 套件，並安裝最新版本。 本逐步解說使用 c #/WinRT 1.1.1 版。

2. 更新 `TargetFramework` **AuthoringDemo .csproj** 檔案中的，並將下列元素加入至 `PropertyGroup` ：

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
        <AssemblyVersion>1.0.0.0</AssemblyVersion>
    </PropertyGroup>
    ```

    若為 `TargetFramework` 元素，您可以使用下列其中一個 [目標 framework 名字](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)。
    - **net 5.0-windows 10.0.17763。0**
    - **net 5.0-windows 10.0.18362。0**
    - **net 5.0-windows 10.0.19041。0**

    您也需要為 `AssemblyVersion` Windows 執行階段元件指定。

3. 加入 `PropertyGroup` 設定數個 **CsWinRT** 屬性的新元素。

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
        <CsWinRTEnableLogging>true</CsWinRTEnableLogging>
        <GeneratedFilesDir Condition="'$(GeneratedFilesDir)'==''">$([MSBuild]::NormalizeDirectory('$(MSBuildProjectDirectory)', '$(IntermediateOutputPath)', 'Generated Files'))</GeneratedFilesDir>
    </PropertyGroup>
      ```

      以下是此範例中屬性的一些詳細資料。 如需 CsWinRT 專案屬性的完整清單，請參閱 [CsWinRT NuGet 檔。](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - `CsWinRTComponent`屬性指定您的專案是 Windows 執行階段元件，以便為元件產生 WinMD 檔案。
    - `CsWinRTWindowsMetadata`屬性提供 Windows 中繼資料的來源。 這是版本1.1.1 的必要項。
    - `CsWinRTEnableLogging`當建立執行時間元件時，屬性會產生具有詳細輸出的 **log.txt** 檔案。
    - 在 `GeneratedFilesDir` 右邊的輸出目錄中產生 **winmd** 檔案時，需要屬性。 這是版本1.1.1 的必要項。

4. 您可以使用程式庫 **( .cs)** 類別檔案來撰寫您的執行時間類別。 以滑鼠右鍵按一下 **Class1.cs** 檔案，然後將它重新命名為 **Example.cs**。 將下列程式碼加入至這個檔案，這個檔案會將公用屬性和方法加入至執行時間類別。 請記得將您想要在執行時間元件中公開的任何類別標示 **為公開。**

    ```csharp
    namespace AuthoringDemo
    {
        public sealed class Example
        {
            public int SampleProperty { get; set; }

            public static string SayHello()
            {
                return "Hello from your C# WinRT component";
            }
        }
    }
    ```

5. 您現在可以建立專案，以產生元件的中繼資料檔案。 以滑鼠右鍵按一下 **方案總管** 中的專案，然後按一下 [ **建立**]。 您將會在 [**產生** 的檔案] 資料夾下的 **方案總管** 中，看到產生的 **AuthoringDemo winmd** 檔案，也會出現在組建輸出檔案夾中。

## <a name="generate-a-nuget-package-for-the-component"></a>產生元件的 NuGet 套件

若要將執行時間元件散發為 NuGet 封裝，您必須對 **AuthoringDemo** 專案進行下列修改。 如果您選擇不產生元件的 NuGet 套件，原生應用程式也可以使用產生的 **winmd** 檔案的直接參考來取用元件，如下一節所示。

1. 新增目標檔案，讓原生應用程式可以參考所產生的 NuGet 套件，並使用您的元件。 在 [ **方案總管** 中，以滑鼠右鍵按一下 **AuthoringDemo** 專案，然後選取 [ **加入-> 新專案**]。 搜尋 **XML** 檔案範本，並將檔案命名為 **AuthoringDemo**。

    > [!NOTE]
    > 目標檔案 **必須** 使用您的元件名稱來命名，格式為 *YourComponentName。*

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <Import Project="$(MSBuildThisDirectory)AuthoringDemo.CsWinRT.targets" />
    </Project> 
    ```

   匯入的 **AuthoringDemo CsWinRT .targets** 檔案將會新增至 NuGet 套件，此套件會使用 c #/WinRT 裝載元件來設定套件，以啟用原生應用程式的耗用量。  

2. 將下列元素加入至 **AuthoringDemo .csproj** 專案檔。

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

    <ItemGroup>
        <Content Include="AuthoringDemo.targets" PackagePath="build;buildTransitive"/>
    </ItemGroup>
    ```

    這些屬性會為您的元件產生 NuGet 套件，並將 CsWinRT 目標包含在套件中，以供原生應用程式取用。

3. 重新建立 **AuthoringDemo** 專案。 您現在應該會在組建輸出中看到 NuGet 套件 "AuthoringDemo nupkg" 已成功建立。

## <a name="consume-the-component-in-cwinrt"></a>使用 c + +/WinRT 中的元件

C #/WinRT 撰寫 Windows 執行階段元件可以從原生應用程式使用，但有一些修改。 下列步驟示範如何在原生主控台應用程式中呼叫上述撰寫的元件。

1. 將新的 **c + +/WinRT 主控台應用程式** 專案加入至方案。 請注意，如果您選擇此專案，此專案也可以是不同解決方案的一部分。

    a. 在 **方案總管** 中，以滑鼠右鍵按一下方案節點，然後按一下 [**加入**  ->  **新專案**]。

    b. 在 [ **加入新專案] 對話方塊** 中，搜尋 [ **c + +/WinRT 主控台應用程式** ] 專案範本。 選取範本，然後按 **[下一步]**。

    c. 將新專案命名為 **CppConsoleApp** ，然後按一下 [ **建立**]。

2. 新增 AuthoringDemo 元件的參考。 您可以將封裝參考新增至上一節所產生的 NuGet 套件，或 **AuthoringDemo** 的直接參考。

    - **選項 1 (套件參考)**：以滑鼠右鍵按一下 **CppConsoleApp** 專案，然後選取 [ **管理 NuGet 套件**]。 您可能需要設定套件來源，以新增 AuthoringDemo NuGet 套件的參考。 若要這樣做，請按一下 NuGet 封裝管理員中的設定齒輪，然後將套件來源新增至適當的路徑。

        ![NuGet 設定](images/nuget-sources-settings.png)

        設定套件來源之後，請搜尋 **AuthoringDemo** 套件，然後按一下 [ **安裝**]。

        ![安裝 NuGet 套件](images/install-authoring-nuget.png)

    - **選項 2 (直接參考)**：以滑鼠右鍵按一下 **CppConsoleApp** 專案，然後按一下 [ **加入-> 參考**]。 選取 [**流覽**] 索引標籤，然後從 **AuthoringDemo** 專案組建輸出中尋找並選取 **AuthoringDemo。**

3. 若要協助裝載元件，您必須在檔案和資訊清單檔案中新增 runtimeconfig.js。 如需受管理元件裝載的詳細資訊，請參閱 [這些裝載](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)檔。

    a. 若要將 runtimeconfig.js加入檔案，請以滑鼠右鍵按一下專案，然後選擇 [ **加入-> 新專案**]。 搜尋 **文字檔** 範本，並將它命名為 **WinRT.Host.runtimeconfig.js**。 貼上下列內容：

    ```json
    {
        "runtimeOptions": {
            "tfm": "net5.0",
            "rollForward": "LatestMinor",
            "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "5.0.0"
            }
        }
    }
    ```

    請注意，在此 `tfm` 專案中，您可以使用 DOTNET_ROOT 環境變數參考自訂的獨立 .net 5 安裝。

    b. 若要加入資訊清單檔，請以滑鼠右鍵按一下專案，然後選擇 [ **加入-> 新專案**]。 搜尋 **文字檔** 範本，並將它命名為 **CppConsoleApp.exe 資訊清單**。 貼上下列內容：

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
        <assemblyIdentity version="1.0.0.0" name="CppConsoleApp"/>
        <file name="WinRT.Host.dll">
            <activatableClass
                name="AuthoringDemo.Example"
                threadingModel="both"
                xmlns="urn:schemas-microsoft-com:winrt.v1" />
        </file>
    </assembly>
    ```

    非封裝的應用程式需要資訊清單檔案。 在此檔案中，您可以使用如上所示的「可以啟動的類別註冊專案」來指定執行時間類別。

4. 編輯原生專案檔，在部署專案時包含 >.runtimeconfig.json 和資訊清單檔案。 以滑鼠右鍵按一下專案，然後按一下 **[卸載專案**]。 卸載之後，再次以滑鼠右鍵按一下專案，然後選取 [ **編輯專案** 檔]。 找出 **WinRT.Host.runtimeconfig.js** 的專案，並 **CppConsoleApp.exe 資訊清單**，然後加入 `DeploymentContent` 屬性，如下所示。

    ```xml
    <ItemGroup>
        <None Include="WinRT.Host.runtimeconfig.json">
            <DeploymentContent>true</DeploymentContent>
        </None>

        <Manifest Include="CppConsoleApp.exe.manifest">
            <DeploymentContent>true</DeploymentContent>
        </Manifest>
    </ItemGroup> 
    ```

5. 在專案的標頭檔底下開啟 **pch** ，然後加入下列程式程式碼來包含您的元件。

    ```cpp
    #include <winrt/AuthoringDemo.h>
    ```

6. 在專案的原始程式檔下開啟 **主要 .cpp** ，並將其取代為下列內容。

    ```cpp
    #include "pch.h"
    #include "iostream"

    using namespace winrt;
    using namespace Windows::Foundation;

    int main()
    {
        init_apartment();

        AuthoringDemo::Example ex;
        ex.SampleProperty(42);
        std::wcout << ex.SampleProperty() << std::endl;
        std::wcout << ex.SayHello().c_str() << std::endl;
    }
    ```

7. 建立並執行 **CppConsoleApp** 專案。 您現在應該會看到下列輸出。

    ![C + +/WinRT 主控台輸出](images/consume-component-output.png)

## <a name="related-topics"></a>相關主題

- [撰寫元件](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [受控元件裝載](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
