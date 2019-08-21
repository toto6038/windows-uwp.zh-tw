---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件, 以及將其他現代化元件併入您的 WPF 應用程式中。
title: 將 Contoso Expenses 應用程式移轉到 .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 97488a913605916c067861b5941d7aa127b00917
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643416"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>第 1 部分：將 Contoso Expenses 應用程式移轉到 .NET Core 3

這是教學課程的第一個部分, 示範如何將名為 Contoso 費用的範例 WPF 桌面應用程式現代化。 如需教學課程、必要條件和下載範例應用程式指示的總覽, 請參閱[教學課程:將 WPF 應用程式](modernize-wpf-tutorial.md)現代化。
  
在本教學課程的這個部分中, 您會將整個 Contoso 費用應用程式從 .NET Framework 4.7.2 遷移至[.Net Core 3](modernize-wpf-tutorial.md#net-core-3)。 在您開始本教學課程的這個部分之前, 請確定您已執行下列動作:

* 在 Visual Studio 2019 中[開啟並建立 ContosoExpenses 範例](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)。
* 如果您使用的是 Visual Studio 2019 的發行版本, 請啟用 .NET Core SDK 的預覽版本。 在 Visual Studio 中, 移至 [工具] [ **> 選項**], 在 [搜尋] 方塊中輸入 "Preview", 然後選取 [**使用 .NET Core SDK 的預覽**]。 如果您使用的是[Visual Studio 2019 的預覽版本](https://visualstudio.microsoft.com/vs/preview/), 則不需要選取此選項, 因為預設會啟用 .net Core 預覽。

> [!NOTE]
> 如需將 WPF 應用程式從 .NET Framework 遷移至 .NET Core 3 的詳細資訊, 請參閱[此 blog 系列](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>將 ContosoExpenses 專案遷移至 .NET Core 3

在本節中, 您會將 Contoso 費用應用程式中的 ContosoExpenses 專案遷移至 .NET Core 3。 若要這麼做, 您可以建立新的專案檔, 其中包含與現有 ContosoExpenses 專案相同的檔案, 但以 .NET Core 3 為目標, 而不是 .NET Framework 4.7.2。 這可讓您維護應用程式 .NET Framework 和 .NET Core 版本的單一解決方案。

1. 確認 ContosoExpenses 專案目前以 .NET Framework 4.7.2 為目標。 在方案總管中, 以滑鼠右鍵按一下**ContosoExpenses**專案, 選擇 [**屬性**], 然後確認 [**應用程式**] 索引標籤上的 [**目標 framework** ] 屬性設定為 [.NET Framework 4.7.2]。

    ![專案的 .NET Framework 版本4.7。2](images/wpf-modernize-tutorial/NETFramework472.png)

3. 在 Windows Explorer 中, 流覽至 [ **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** ] 資料夾, 然後建立名為**ContosoExpenses**的新文字檔。

4. 以滑鼠右鍵按一下檔案, 選擇 [**開啟方式**], 然後在您選擇的文字編輯器中開啟它, 例如 [記事本]、[Visual Studio Code] 或 [Visual Studio]。

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

6. 關閉檔案, 然後回到 Visual Studio 中的 [ **ContosoExpenses** ] 方案。

7. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 方案, 然後選擇 [**加入 > 現有專案**]。 選取您剛才在`C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses`資料夾中建立的**ContosoExpenses .csproj**檔案, 將它新增至方案。

**ContosoExpenses**包含下列元素:

* **Project**元素會指定**WindowsDesktop**的 SDK 版本。 這是指適用于 Windows 桌面的 .NET 應用程式, 其中包含 WPF 和 Windows Forms 應用程式的元件。
* **PropertyGroup**元素包含的子項目, 會指出專案輸出是可執行檔 (而非 DLL), 以 .net Core 3 為目標, 並使用 WPF。 針對 Windows Forms 應用程式, 您會使用**UseWinForms**元素, 而不是**UseWPF**專案。

> [!NOTE]
> 使用 .NET Core 3.0 引進的 .csproj 格式時, 與 .csproj 位於相同資料夾中的所有檔案都會被視為專案的一部分。 因此, 您不需要指定專案中包含的每個檔案。 您必須只指定要定義自訂群組建動作或要排除的檔案。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>將 ContosoExpenses 資料項目目遷移至 .NET Standard

**ContosoExpenses**解決方案包含 ContosoExpenses 的**資料**類別庫, 其中包含服務的模型和介面, 以及 .net 4.7.2 的目標。 .NET Core 3.0 應用程式可以使用 .NET Framework 程式庫, 前提是它們不會使用 .NET Core 中未提供的 Api。 不過, 最好的現代化路徑是將您的程式庫移至 .NET Standard。 這可確保您的程式庫受到 .NET Core 3.0 應用程式的完整支援。 此外, 您也可以重複使用程式庫與其他平臺, 例如 web (透過 ASP.NET Core) 和行動裝置 (透過 Xamarin)。

若要將**ContosoExpenses 資料**專案遷移至 .NET Standard:

1. 在 Visual Studio 中, 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 **[卸載專案**]。 再次以滑鼠右鍵按一下專案, 然後選擇 [**編輯 ContosoExpenses**]。

2. 刪除專案檔的整個內容。

3. 複製並貼上下列 XML 並儲存檔案。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**重載專案**]。

## <a name="configure-nuget-packages-and-dependencies"></a>設定 NuGet 套件和相依性

當您在上一節中遷移**ContosoExpenses**和**ContosoExpenses**專案時, 您已從專案中移除 NuGet 套件參考。 在本節中, 您會將這些參考新增回去。

若要設定**ContosoExpenses**專案的 NuGet 套件:

1. 在 [ **ContosoExpenses** ] 專案中, 展開 [相依性 **]** 節點。 請注意, **NuGet**區段遺失。

    ![NuGet 套件](images/wpf-modernize-tutorial/NuGetPackages.png)

    如果您在**方案總管**中開啟**封裝**, 您會在使用完整 .NET Framework 時, 發現 NuGet 套件的「舊」參考已使用此專案。

    ![相依性和封裝](images/wpf-modernize-tutorial/Packages.png)

    以下是**封裝 .config**檔案的內容。 您會注意到, 所有 NuGet 套件的目標都是完整的 .NET Framework 4.7.2:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. 在**ContosoExpenses**專案中, 刪除**封裝 .config**檔案。

4. 在 [ **ContosoExpenses** ] 專案中, 以滑鼠右鍵按一下 [相依性] 節點, 然後選擇 [**管理 NuGet 封裝** **]** 。

  ![管理 NuGet 套件 。](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. 在 [ **NuGet 套件管理員**] 視窗中, 按一下 **[流覽]** 。 `Bogus`搜尋套件並安裝最新的穩定版本。

    ![假的 NuGet 套件](images/wpf-modernize-tutorial/Bogus.png)

6. `LiteDB`搜尋套件並安裝最新的穩定版本。

    ![LiteDB NuGet 套件](images/wpf-modernize-tutorial/LiteDB.png)

    您可能會想知道這些 NuGet 套件的儲存位置, 因為專案不再具有封裝 .config 檔案。 參考的 NuGet 套件會直接儲存在 .csproj 檔案中。 您可以在文字編輯器中, 透過查看**ContosoExpenses**的內容來檢查此專案。 您會在檔案結尾處找到下列幾行:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 您可能也會注意到, 您要為此 .NET Core 3 專案安裝與 .NET Framework 4.7.2 專案所使用的套件相同的封裝。 NuGet 套件支援多目標。 程式庫作者可以在同一個套件中包含不同版本的程式庫, 並針對不同的架構和平臺進行編譯。 這些套件支援完整的 .NET Framework, 以及與 .NET Core 3 專案相容的 .NET Standard 2.0。 如需有關 .NET Framework、.NET Core 和 .NET Standard 差異的詳細資訊, 請參閱[.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)。

若要設定**ContosoExpenses**專案的 NuGet 套件:

1. 在**ContosoExpenses**專案中, 開啟**封裝 .config**檔案。 請注意, 它目前包含下列以 .NET Framework 4.7.2 為目標的參考。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    在下列步驟中, 您將 .NET Standard `MvvmLightLibs`和`Unity`套件的版本。 其他兩個則是當您安裝這兩個程式庫時, NuGet 會自動下載的相依性。

2. 在**ContosoExpenses**專案中, 刪除**封裝 .config**檔案。

3. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**管理 NuGet 封裝**]。

4. 在 [ **NuGet 套件管理員**] 視窗中, 按一下 **[流覽]** 。 `Unity`搜尋套件並安裝最新的穩定版本。

    ![Unity 封裝](images/wpf-modernize-tutorial/UnityPackage.png)

5. `MvvmLightLibsStd10`搜尋套件並安裝最新的穩定版本。 這是`MvvmLightLibs`套件的 .NET Standard 版本。 針對此套件, 作者選擇將程式庫的 .NET Standard 版本封裝在不同于 .NET Framework 版本的封裝中。

    ![MvvmLightsLibs 套件](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. 在**ContosoExpenses**專案中, 以滑鼠右鍵按一下 [相依性] 節點, 然後選擇 [**加入參考** **]** 。

7. 在 [**專案 > 方案**] 分類中, 選取 [ **ContosoExpenses** ], 然後按一下 **[確定]** 。

    ![新增參考](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>停用自動產生的元件屬性

此時, 在遷移程式中, 如果您嘗試建立**ContosoExpenses**專案, 您會看到一些錯誤。

![.NET Core 3 建立新的錯誤](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

發生此問題的原因是, .NET Core 3.0 引進的新 .csproj 格式會將元件資訊儲存在專案檔中, 而不是**AssemblyInfo.cs**檔案中。 若要修正這些錯誤, 請停用此行為, 並讓專案繼續使用**AssemblyInfo.cs**檔案。

1. 在 Visual Studio 中, 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 **[卸載專案**]。 再次以滑鼠右鍵按一下專案, 然後選擇 [**編輯 ContosoExpenses**]。

1. 在**PropertyGroup**區段中新增下列元素, 並儲存檔案。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    新增此元素之後, **PropertyGroup**區段現在應該如下所示:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**重載專案**]。

4. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 **[卸載專案**]。 再次以滑鼠右鍵按一下專案, 然後選擇 [**編輯 ContosoExpenses**]。

5. 在**PropertyGroup**區段中新增相同的專案, 並儲存檔案。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    新增此元素之後, **PropertyGroup**區段現在應該如下所示:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**重載專案**]。

## <a name="add-the-windows-compatibility-pack"></a>新增 Windows 相容性套件

如果您現在嘗試編譯**ContosoExpenses**和**ContosoExpenses**專案, 您會看到先前的錯誤現在已修正, 但**ContosoExpenses**中仍有一些類似的錯誤。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

這些錯誤是從 .NET Framework 程式庫 (適用于 Windows) 將**ContosoExpenses**專案轉換為 .NET Standard 程式庫所產生的結果, 該程式庫可在多種平臺上執行, 包括 Linux、Android、iOS 等。 **ContosoExpenses**專案包含名為**RegistryService**的類別, 它會與登錄 (僅限 Windows 的概念) 互動。

若要解決這些錯誤, 請安裝[Windows 相容性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)NuGet 套件。 此套件可支援在 .NET Standard 程式庫中使用的許多 Windows 特定 Api。 使用此套件之後, 程式庫將不再是跨平臺, 但仍會以 .NET Standard 為目標。 

1. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案。
2. 選擇 [**管理 NuGet 封裝**]。
3. 在 [ **NuGet 套件管理員**] 視窗中, 按一下 **[流覽]** 。 `Microsoft.Windows.Compatibility`搜尋套件並安裝最新的穩定版本。

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 現在請以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**組建**], 以再次嘗試編譯專案。

這次組建程式將會完成, 而不會發生錯誤。

## <a name="test-and-debug-the-migration"></a>測試和調試進行遷移

現在已成功建立專案, 您已準備好執行和測試應用程式, 以查看是否有任何執行階段錯誤。

1. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**設定為啟始專案**]。

2. 按 F5 啟動偵錯工具中的**ContosoExpenses**專案。 您會看到類似下面的例外狀況。

    ![Visual Studio 中顯示的例外狀況](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    引發此例外狀況的原因是, 當您在遷移開始時從 .csproj 檔案中刪除內容時, 已移除影像檔案的**組建動作**相關資訊。 下列步驟會修正此問題。

3. 停止偵錯工具。

4. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 **[卸載專案**]。 再次以滑鼠右鍵按一下專案, 然後選擇 [**編輯 ContosoExpenses**]。

5. 在關閉**專案**元素之前, 新增下列專案:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**重載專案**]。

7. 若要將 Contoso .ico 指派給應用程式, 請以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**屬性**]。 在開啟的頁面中, 按一下 [**圖示**] 底下的下拉式`Images\contoso.ico`選, 然後選取 []。

    ![專案屬性中的 Contoso 圖示](images/wpf-modernize-tutorial/ContosoIco.png)

8. 按一下 [儲存]。

9. 按 F5 啟動偵錯工具中的**ContosoExpenses**專案。 確認應用程式現在會執行。

## <a name="next-steps"></a>後續步驟

在本教學課程中, 您已成功將 Contoso 費用應用程式遷移至 .NET Core 3。 您現在已準備好[開始進行第2部分:使用 XAML 群島](modernize-wpf-tutorial-2.md)新增 UWP InkCanvas 控制項。

> [!NOTE]
> 如果您有高解析度的螢幕, 您可能會注意到應用程式看起來很小。 您會在本教學課程的下一個步驟中解決此問題。
