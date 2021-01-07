---
description: '本逐步解說將示範如何使用 c #/WinRT 來產生 c + +/WinRT 元件的 .NET 5 投影。'
title: 從 c + +/WinRT 元件產生 .NET 5 投影並散發 NuGet 的逐步解說
ms.date: 11/12/2020
ms.topic: article
keywords: 'windows 10、c #、winrt、cswinrt、投影'
ms.localizationpriority: medium
ms.openlocfilehash: 57bc5c49d47dacee910cd3d80964f797633ef587
ms.sourcegitcommit: 6da85cc75c02a5a7417966abddc8824ac87fb619
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964731"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>逐步解說：從 C++/WinRT 元件產生 .NET 5.0 投影並散發 NuGet

本逐步解說將示範如何使用 [c #/WinRT](index.md) 來產生 c + +/WinRT 元件的 .net 5 投射、建立相關聯的 nuget 套件，以及從 .Net 5 c # 主控台應用程式參考 NuGet 套件。

您可以 [從 GitHub 下載](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)本逐步解說的完整範例。

## <a name="prerequisites"></a>先決條件

本逐步解說和對應的範例需要下列工具和元件：

- 已安裝通用 Windows 平臺開發工作負載的[Visual Studio 16.8](https://visualstudio.microsoft.com/downloads/) (或更新版本) 。 在 **安裝詳細資料**  >  **通用 Windows 平臺開發** 中，檢查 **c + + (v14x) 通用 Windows 平臺工具**] 選項。
- [.Net 5.0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)。
- C + + 的 c + + [/WINRT VSIX 擴充](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)功能/WinRT 專案範本。

## <a name="create-a-simple-cwinrt-runtime-component"></a>建立簡單的 c + +/WinRT 執行時間元件

若要遵循這個逐步解說，您必須先有要建立 .NET 5 投影的 c + +/WinRT 元件。 本逐步解說會 [使用 GitHub 的](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample/SimpleMathComponent)相關範例中的 **SimpleMathComponent** 專案。 這是使用 [c + +/WINRT VSIX 擴充](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)功能所建立 **(c + +/WinRT) 專案的 Windows 執行階段元件**。 將專案複製到您的開發電腦之後，請在 Visual Studio 2019 Preview 中開啟解決方案。

此專案中的程式碼提供以下標頭檔中所示之基本數學運算的功能。 

```cpp
// SimpleMath.h
...
namespace winrt::SimpleMathComponent::implementation
{
    struct SimpleMath: SimpleMathT<SimpleMath>
    {
        SimpleMath() = default;
        double add(double firstNumber, double secondNumber);
        double subtract(double firstNumber, double secondNumber);
        double multiply(double firstNumber, double secondNumber);
        double divide(double firstNumber, double secondNumber);
    };
}
```

如需有關建立 c + +/WinRT 元件和產生 winmd 檔案的詳細步驟，請參閱 [使用 c + +/WinRT Windows 執行階段元件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)。

> [!NOTE]
> 如果您要在元件中執行 [IInspectable：： GetRuntimeClassName](/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) ，它 **必須** 傳回有效的 WinRT 類別名稱。 由於 c #/WinRT 會使用類別名稱字串來進行 interop，因此不正確的運行時類別名稱將引發 **InvalidCastException**。

## <a name="add-a-projection-project-to-the-component-solution"></a>將投射專案加入至元件方案

如果您已從存放庫複製範例，請先刪除 **SimpleMathProjection** 專案，以依照逐步解說逐步進行。

1. 在您的方案中加入新的 **類別庫 ( .Net Core)** 專案。

    1. 在 **方案總管** 中，以滑鼠右鍵按一下方案節點，然後按一下 [**加入**  ->  **新專案**]。
    2. 在 [ **加入新專案] 對話方塊** 中，搜尋 **類別庫 ( .net Core)** 專案範本。 選取範本，然後按 **[下一步]**。
    3. 將新專案命名為 **SimpleMathProjection** ，然後按一下 [ **建立**]。

2. 從專案中刪除空白的 **Class1.cs** 檔案。

3. 安裝 [c #/WinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT)。

    1. 在 **方案總管** 中，以滑鼠右鍵按一下 **SimpleMathProjection** 專案，然後選取 [ **管理 NuGet 套件**]。 
    2. 搜尋 **CsWinRT** NuGet 套件，並安裝最新版本。

4. 將專案參考加入至 **SimpleMathComponent** 專案。 在 **方案總管** 中，以滑鼠右鍵按一下 **SimpleMathProjection** 專案底下的 [相依性] 節點，選取 [**加入專案參考** **]** ，然後選取 **SimpleMathComponent** 專案。

在這些步驟之後，您的 **方案總管** 看起來應該像這樣。

![顯示投射專案相依性的方案總管](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>編輯專案檔案以執行 c #/WinRT

您必須先編輯投射專案的專案檔，才能叫用 **cswinrt.exe** 並產生投影元件。

1. 在 **方案總管** 中，按兩下 [ **SimpleMathProjection** ] 節點，以在編輯器中開啟專案檔。

2. 更新 `TargetFramework` 元素以參考 Windows SDK。 這會加入 interop 和投射支援所需的元件 depedencies。 我們的範例會以本逐步解說的最新 Windows 10 版本為目標，而 **net 5.0-Windows 10.0.19041.0** (也稱為 SDK 2004 版) 。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. 加入 `PropertyGroup` 設定數個 **cswinrt** 屬性的新元素。

    ```xml
    <PropertyGroup>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    以下是此範例中設定的一些詳細資料：

    - `CsWinRTIncludes`屬性會指定要投影的命名空間。
    - `CsWinRTGeneratedFilesDir`屬性會設定輸出目錄，其中會產生投射中的檔案，我們會在下一節中將其設定為從來源中建立。

4. 本逐步解說的最新 c #/WinRT 版本可能需要指定 Windows 中繼資料。 您可以使用下列其中一種方式來提供：

    - NuGet 套件參考，例如： Microsoft。 [合約]( https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts/)：

      ```xml
      <ItemGroup>
        <PackageReference Include="Microsoft.Windows.SDK.Contracts" Version="10.0.19041.1" />
      </ItemGroup>
      ```

    - 另一個選項是在 `CsWinRTWindowsMetadata` 步驟3中將下列屬性加入至 `PropertyGroup` ：

      ```xml
      <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
      ```

5. 儲存並關閉 **SimpleMathProjection .csproj** 檔案。

## <a name="build-projects-out-of-source"></a>從來源建立專案

在 [相關的範例](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)中，會使用 **.props** 檔案來設定組建。 建立 **SimpleMathComponent** 和 **SimpleMathProjection** 專案所產生的檔案會出現在方案層級的 *_build* 資料夾中。 若要將您的專案設定為從來源中建立，請將下面的 **.props** 檔案複製到包含方案檔的目錄。

```xml
<Project>
  <PropertyGroup>
    <BuildOutDir>$([MSBuild]::NormalizeDirectory('$(SolutionDir)_build', '$(Platform)', '$(Configuration)'))</BuildOutDir>
    <OutDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'bin'))</OutDir>
    <IntDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'obj'))</IntDir>
  </PropertyGroup>
</Project>
```

雖然不需要執行此步驟來產生投影，但它可讓您輕鬆地從相同目錄中的兩個專案產生組建檔案，讓組建清除更容易。 請注意，如果您未建立來源，則會在各自的專案資料夾中，于不同的目錄中產生 **SimpleMathComponent** 和 interop 元件 **SimpleMathComponent.dll** 。 這些檔案都是在下面的 **SimpleMathProjection** 中參考，因此必須據此變更路徑。

## <a name="create-a-nuget-package-from-the-projection"></a>從投影建立 NuGet 套件

若要散發和使用 interop 元件，您可以在建立方案時自動建立 NuGet 套件，方法是新增一些額外的專案屬性。 針對 .NET 5.0 目標，套件應該包含 interop 元件、實元件，以及所需 c #/WinRT 執行時間元件的 c #/WinRT NuGet 套件相依性， **WinRT.Runtime.dll**。

1. 將 NuGet 規格 (. nuspec) 檔案新增至 **SimpleMathProjection** 專案。

    1. 在 **方案總管** 中，以滑鼠右鍵按一下 [ **SimpleMathProjection** ] 節點 **，選擇 [**  ->  **新增資料夾**]，然後將資料夾命名為 **nuget**。 
    2. 以滑鼠右鍵按一下 [ **nuget** ] 資料夾，選擇 [**加入**  ->  **新專案**]，選擇 XML 檔案，並將它命名為 **SimpleMathProjection. nuspec**。 

2. 將下列各項新增至 **SimpleMathProjection** ，以自動產生封裝。 這些屬性會指定 `NuspecFile` 要產生 NuGet 套件的和目錄。

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example NuGet spec for distributing the interop assembly from the C++/WinRT component. Note that for .NET 5.0 targets, under the `dependencies` node there is a dependency on CsWinRT, and under the `files` node **SimpleMathProjection.dll** is specified instead of **SimpleMathComponent.winmd** for the target `lib\net5.0\SimpleMathProjection.dll`. This behavior is new in .NET 5.0 and enabled by C#/WinRT. The implementation assembly, **SimpleMathComponent.dll**, must also be deployed for .NET 5.0 targets. 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
      <metadata>
        <id>SimpleMathComponent</id>
        <version>0.1.0-prerelease</version>
        <authors>Contoso Math Inc.</authors>
        <description>A simple component with basic math operations</description>
        <dependencies>
          <group targetFramework=".NETCoreApp3.0" />
          <group targetFramework="UAP10.0" />
          <group targetFramework=".NETFramework4.6" />
          <group targetFramework="net5.0">
            <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support netcore3, uap, net46+, net5, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>建立解決方案來產生投射和 NuGet 套件

現在您可以建立方案：以滑鼠右鍵按一下方案節點，然後選取 [ **建立方案**]。 這會先建立元件專案，然後再建立投射專案。 除了元件專案中的中繼資料檔案之外，還會在輸出目錄中產生 interop **.cs** 檔和元件。 您也可以在 **nuget** 資料夾中看到產生的 Nuget 套件 **simplemathcomponent 0.1.0-搶鮮版（nupkg）** 。

![顯示產生投射的方案總管](images/projection-generated-files.png)

## <a name="reference-the-nuget-package-in-a-c-net-50-console-application"></a>參考 c # .NET 5.0 主控台應用程式中的 NuGet 套件

若要取用投射的 **SimpleMathComponent**，您可以直接在應用程式中新增新建立之 NuGet 套件的參考。 下列步驟示範如何在不同的解決方案中建立簡單的主控台應用程式來執行這項操作。

1. 使用主控台應用程式建立新的解決方案， **( .Net Core)** 專案。

    1. 在 Visual Studio 中，選取 [檔案] ->  [新增] ->  [專案]。
    2. 在 [ **加入新專案] 對話方塊** 中，搜尋 **( .net Core) 專案範本的主控台應用程式** 。 選取範本，然後按 **[下一步]**。
    3. 將新專案命名為 **SampleConsoleApp** ，然後按一下 [ **建立**]。 在新的方案中建立此專案，可讓您分別還原 **SimpleMathComponent** NuGet 套件。

2. 在 **方案總管** 中，按兩下 [ **SampleConsoleApp** ] 節點開啟 [ **SampleConsoleApp** ] 專案檔，然後更新目標 framework 的 [標記] 和 [平臺設定]，如下列範例所示。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. 將 **SimpleMathComponent** NuGet 套件新增至 **SampleConsoleApp** 專案。 您也需要 VCRTForwarders 的 NuGet 套件，這在未封裝于 MSIX 套件中的應用程式中是必要的[。](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) 若要在建立專案時還原 **SimpleMathComponent** NuGet，您可以使用 `RestoreSources` 具有元件方案中 **NuGet** 資料夾路徑的屬性。

    ```xml
    <PropertyGroup>
      <RestoreSources>
        https://api.nuget.org/v3/index.json;
        ../../CppWinRTProjectionSample/SimpleMathProjection/nuget
      </RestoreSources>
    </PropertyGroup>

    <ItemGroup>
      <PackageReference Include="Microsoft.VCRTForwarders.140" Version="1.0.6" />
      <PackageReference Include="SimpleMathComponent" Version="0.1.0-prerelease" />
    </ItemGroup>
    ```

    請注意，在此逐步解說中， **SimpleMathComponent** 的 NuGet 還原路徑會假設兩個解決方案檔案都在相同的目錄中。 或者，您可以 [將本機 NuGet 套件摘要新增](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources) 至方案。

4. 編輯 **Program.cs** 檔案，以使用 **SimpleMathComponent** 所提供的功能。

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. 建立並執行主控台應用程式。 您應該會看到下列輸出。

    ![主控台 NET5 輸出](images/console-output.png)

## <a name="resources"></a>資源

- [本逐步解說的完整程式碼範例](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)
