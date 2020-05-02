---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件，以及將其他新式元件併入您的 UWP 應用程式。
title: 將 Contoso Expenses 應用程式移轉到 .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "71317099"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>第 1 部分：將 Contoso Expenses 應用程式移轉到 .NET Core 3

這是教學課程的第一個部分，示範如何讓名為 Contoso Expenses 的範例 WPF 傳統型應用程式現代化。 如需教學課程概觀、必要條件和下載範例應用程式的指示，請參閱[教學課程：讓 WPF 應用程式現代化](modernize-wpf-tutorial.md)。
  
在本教學課程的這個部分，您會將整個 Contoso Expenses 應用程式從 .NET Framework 4.7.2 遷移至 [.NET Core 3](modernize-wpf-tutorial.md#net-core-3)。 在開始本教學課程的這個部分之前，請確定您在 Visual Studio 2019 中[開啟並建立 ContosoExpenses 範例](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)。

> [!NOTE]
> 如需將 WPF 應用程式從 .NET Framework 遷移至 .NET Core 3 的詳細資訊，請參閱[此部落格系列](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>將 ContosoExpenses 專案遷移到 .NET Core 3

在本節中，您會將 Contoso Expenses 應用程式中的 ContosoExpenses 專案遷移至 .NET Core 3。 若要這麼做，您可建立新的專案檔，其中包含與現有 ContosoExpenses 專案相同的檔案，但以 .NET Core 3 為目標 (而不是 .NET Framework 4.7.2)。 這可讓您透過應用程式的 .NET Framework 與 .NET Core 版本，維護單一方案。

1. 確認 ContosoExpenses 專案目前以 .NET Framework 4.7.2 為目標。 在 [方案總管] 中，以滑鼠右鍵按一下 **ContosoExpenses** 專案，選擇 [屬性]  並確認 [應用程式]  索引標籤上的 [目標架構]  屬性已設定為 .NET Framework 4.7.2。

    ![專案的 .NET Framework 4.7.2 版](images/wpf-modernize-tutorial/NETFramework472.png)

3. 在 [Windows 檔案總管] 中，瀏覽至 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** 資料夾，然後建立名為 **ContosoExpenses.Core.csproj** 的新文字檔。

4. 在檔案上按一下滑鼠右鍵，選擇 [開啟方式]  ，然後在您選擇的文字編輯器 (例如 [記事本]、Visual Studio Code 或 Visual Studio) 中加以開啟。

5. 將下列文字複製到檔案並加以儲存。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 關閉檔案，然後回到 Visual Studio 中的 **ContosoExpenses** 方案。

7. 以滑鼠右鍵按一下 **ContosoExpenses** 方案，然後選擇 [新增] -> [現有專案]  。 選取您剛在 `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` 資料夾中建立的 **ContosoExpenses.Core.csproj** 檔案，將其新增至方案。

**ContosoExpenses.Core.csproj** 包含下列元素：

* **Project** 元素會指定 SDK 版本的 **Microsoft.NET.Sdk.WindowsDesktop**。 這是指 Windows 桌面版 .NET 應用程式，其中包含 WPF 和 Windows Forms 應用程式的元件。
* **PropertyGroup** 元素包含子元素，其表示專案輸出是可執行檔 (而非 DLL)，以 .NET Core 3 為目標，並使用 WPF。 針對 Windows Forms 應用程式，您會使用 **UseWinForms** 元素，而不是 **UseWPF** 元素。

> [!NOTE]
> 使用 .NET Core 3.0 引進的 .csproj 格式時，所有與 .csproj 位於相同資料夾中的檔案都會被視為專案的一部分。 因此，您不必指定專案中包含的每個檔案。 您只必須指定要定義自訂建置動作或要排除的檔案。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>將 ContosoExpenses.Data 專案遷移到 .NET Standard

**ContosoExpenses** 方案包含 **ContosoExpenses.Data**類別庫，其中含有服務的模型和介面並以 .NET 4.7.2 為目標。 .NET Core 3.0 應用程式可以使用 .NET Framework 程式庫，前提是不使用 .NET Core 中未提供的 API。 不過，最好的現代化路徑是將您的程式庫移至 .NET Standard。 這可確保您的程式庫受到 .NET Core 3.0 應用程式的完整支援。 此外，您也可與其他平台重複使用程式庫，例如 Web (透過 ASP.NET Core) 和行動裝置 (透過 Xamarin)。

若要將 **ContosoExpenses.Data** 專案遷移到 .NET Standard：

1. 在 Visual Studio 中，以滑鼠右鍵按一下 **ContosoExpenses.Data** 專案，然後選擇 [卸載專案]  。 再次以滑鼠右鍵按一下專案，然後選擇 [編輯 ContosoExpenses.Data.cspro]  。

2. 刪除專案檔案的整個內容。

3. 複製並貼上下列 XML，然後儲存檔案。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 以滑鼠右鍵按一下 [ContosoExpenses.Data]  專案，然後選擇 [重新載入專案]  。

## <a name="configure-nuget-packages-and-dependencies"></a>設定 NuGet 套件和相依性

當您在前面幾節中遷移 **ContosoExpenses.Core** 和 **ContosoExpenses.Data** 專案時，您已從專案中移除 NuGet 套件參考。 在本節中，您會將這些參考加回去。

若要設定 **ContosoExpenses.Data** 專案的 NuGet 套件：

1. 在 **ContosoExpenses.Data** 專案中，展開 [相依性]  節點。 請注意，缺少 **NuGet** 區段。

    ![NuGet 套件](images/wpf-modernize-tutorial/NuGetPackages.png)

    如果您在 [方案總管]  中開啟 **Packages.config**，您會在其使用完整的 .NET Framework 時，找到 NuGet 套件所用專案的「舊」參考。

    ![相依性和套件](images/wpf-modernize-tutorial/Packages.png)

    以下是 **Packages.config** 檔案的內容。 您會注意到，所有 NuGet 套件都是以完整的 .NET Framework 4.7.2 為目標：

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. 在 **ContosoExpenses.Data** 專案中，刪除 **Packages.config** 檔案。

4. 在 **ContosoExpenses.Data** 專案中，以滑鼠右鍵按一下 [相依性]  節點，然後選擇 [管理 NuGet 套件]  。

  ![管理 NuGet 套件](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. 在 [NuGet 套件管理員]  視窗中，按一下 [瀏覽]  。 搜尋 `Bogus` 套件，並安裝最新的穩定版本。

    ![假造的 NuGet 套件](images/wpf-modernize-tutorial/Bogus.png)

6. 搜尋 `LiteDB` 套件，並安裝最新的穩定版本。

    ![LiteDB NuGet 套件](images/wpf-modernize-tutorial/LiteDB.png)

    您可能會想知道這些 NuGet 套件的儲存位置，因為專案不再具有 packages.config 檔案。 參考的 NuGet 套件會直接儲存在 .csproj 檔案中。 在文字編輯器中檢視 **ContosoExpenses.Data.csproj** 專案檔案的內容，即可檢查這點。 您會在檔案結尾處找到下列幾行：

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 您可能也會注意到，您為此 .NET Core 3 專案安裝的套件與 .NET Framework 4.7.2 專案所用的套件相同。 NuGet 套件支援多目標。 程式庫作者可以在同一個套件中包含不同版本的程式庫，並針對不同的架構和平台進行編譯。 這些套件都支援完整的 .NET Framework，以及與 .NET Core 3 專案相容的 .NET Standard 2.0。 如需有關 .NET Framework、.NET Core 和 .NET Standard 差異的詳細資訊，請參閱 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)。

若要設定 **ContosoExpenses.Core** 專案的 NuGet 套件：

1. 在 **ContosoExpenses.Core** 專案中，開啟 **packages.config** 檔案。 請注意，其目前包含下列以 .NET Framework 4.7.2 為目標的參考。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    在下列步驟中，您將使用 .NET Standard 版本的 `MvvmLightLibs` 和 `Unity` 套件。 其他兩個則是在您安裝這兩個程式庫時，NuGet 所自動下載的相依性。

2. 在 **ContosoExpenses.Core** 專案中，刪除 **Packages.config** 檔案。

3. 以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [管理 NuGet 專案]  。

4. 在 [NuGet 套件管理員]  視窗中，按一下 [瀏覽]  。 搜尋 `Unity` 套件，並安裝最新的穩定版本。

    ![Unity 套件](images/wpf-modernize-tutorial/UnityPackage.png)

5. 搜尋 `MvvmLightLibsStd10` 套件，並安裝最新的穩定版本。 這是 `MvvmLightLibs` 套件的 .NET Standard 版本。 針對此套件，作者選擇將程式庫的 .NET Standard 版本封裝在與 .NET Framework 版本不同的套件中。

    ![MvvmLightsLibs 套件](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. 在 **ContosoExpenses.Core** 專案中，以滑鼠右鍵按一下 [相依性]  節點，然後選擇 [新增參考]  。

7. 在 [專案] > [方案]  類別中，選取 [ContosoExpenses.Data]  ，然後按一下 [確定]  。

    ![新增參考](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>停用自動產生的組件屬性

在遷移程序的這個時間點，如果您嘗試建立 **ContosoExpenses.Core** 專案，則會看到一些錯誤。

![.NET Core 3 組建新錯誤](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

發生此問題的原因是 .NET Core 3.0 引進的新 .csproj 格式將組件資訊儲存在專案檔案中，而不是 **AssemblyInfo.cs** 檔案中。 若要修正這些錯誤，請停用此行為，並且讓專案繼續使用 **AssemblyInfo.cs** 檔案。

1. 在 Visual Studio 中，以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [卸載專案]  。 再次以滑鼠右鍵按一下專案，然後選擇 [編輯 ContosoExpenses.Core.csproj]  。

1. 在 [PropertyGroup]  區段中新增下列專案並儲存檔案。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    新增此元素之後，[PropertyGroup]  區段現在應該如下所示：

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [重新載入專案]  。

4. 以滑鼠右鍵按一下 [ContosoExpenses.Data]  專案，然後選擇 [卸載專案]  。 再次以滑鼠右鍵按一下專案，然後選擇 [編輯 ContosoExpenses.Data.cspro]  。

5. 在 [PropertyGroup]  區段中新增相同輸入內容並儲存檔案。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    新增此元素之後，[PropertyGroup]  區段現在應該如下所示：

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 以滑鼠右鍵按一下 [ContosoExpenses.Data]  專案，然後選擇 [重新載入專案]  。

## <a name="add-the-windows-compatibility-pack"></a>新增 Windows 相容性套件

如果您現在嘗試編譯 **ContosoExpenses.Core** 和 **ContosoExpenses.Data** 專案，您會看到先前的錯誤現已修正，但 **ContosoExpenses.Data** 程式庫中仍有一些類似的錯誤。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

這些錯誤是將 **ContosoExpenses.Data** 專案從 .NET Framework 程式庫 (Windows 專屬) 轉換成 .NET Standard 程式庫 (其可在多個平台上執行，包括 Linux、Android、iOS 等) 的結果。 **ContosoExpenses.Data** 專案包含名為 **RegistryService** 的類別，其會與登錄互動 (僅限 Windows 的概念)。

若要解決這些錯誤，請安裝 [Windows 相容性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet 套件。 此套件可支援要在 .NET Standard 程式庫中使用的許多 Windows 專屬 API。 使用此套件之後，此程式庫將不再跨平台，但仍會以 .NET Standard 為目標。 

1. 以滑鼠右鍵按一下 [ContosoExpenses.Data]  專案。
2. 選擇 [管理 NuGet 套件]  。
3. 在 [NuGet 套件管理員]  視窗中，按一下 [瀏覽]  。 搜尋 `Microsoft.Windows.Compatibility` 套件，並安裝最新的穩定版本。

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 現在，以滑鼠右鍵按一下 [ContosoExpenses.Data]  專案，然後選擇 [建置]  ，再次嘗試編譯專案。

這次建置程序將會完成，而不會發生任何錯誤。

## <a name="test-and-debug-the-migration"></a>測試和偵錯移轉

專案現已成功建立，您已準備好執行和測試應用程式，以查看是否有任何執行階段錯誤。

1. 以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [設定為啟動專案]  。

2. 按 F5 以在偵錯工具中啟動 [ContosoExpenses.Core]  專案。 您會看到類似下面的例外狀況。

    ![Visual Studio 中顯示的例外狀況](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    引發此例外狀況的原因是，當您在移轉開始的時候從 .csproj 檔案中刪除內容時，您移除了映像檔案的**建置動作**相關資訊。 下列步驟可修正此問題。

3. 停止偵錯工具。

4. 以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [卸載專案]  。 再次以滑鼠右鍵按一下專案，然後選擇 [編輯 ContosoExpenses.Core.csproj]  。

5. 在關閉 **Project** 元素之前，新增下列輸入內容：

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [重新載入專案]  。

7. 若要將 Contoso.ico 指派給應用程式，以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [屬性]  。 在開啟的頁面中，按一下 [圖示]  下方的下拉式清單，然後選取 `Images\contoso.ico`。

    ![專案屬性中的 Contoso 圖示](images/wpf-modernize-tutorial/ContosoIco.png)

8. 按一下 **[儲存]** 。

9. 按 F5 以在偵錯工具中啟動 [ContosoExpenses.Core]  專案。 確認應用程式現已執行。

## <a name="next-steps"></a>接下來的步驟

目前在此教學課程中，您已成功將 Contoso Expenses 應用程式遷移至 .NET Core 3。 您現在已準備好開始進行[第 2 部分：使用 XAML Islands 新增 UWP InkCanvas 控制項](modernize-wpf-tutorial-2.md)。

> [!NOTE]
> 如果您有高解析度的螢幕，您可能會注意到應用程式看起來非常小。 您會在本教學課程的下一個步驟中解決此問題。
