---
description: 本教學課程會示範如何新增 UWP XAML 使用者介面，建立 MSIX 套件，以及您的 WPF 應用程式中納入其他現代的元件。
title: 將 Contoso Expenses 應用程式移轉到 .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows form、 wpf、 xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6e303e7059edd72fcdeb5455f450e6ece9d58e02
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141852"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>第 1 部分：將 Contoso Expenses 應用程式移轉到 .NET Core 3

這是示範如何將範例 WPF 傳統型應用程式現代化名為 Contoso 費用的教學課程的第一個部分。 如需教學課程、 必要條件和指示，下載範例應用程式的概觀，請參閱[教學課程：將 WPF 應用程式現代化](modernize-wpf-tutorial.md)。
  
在這個部分的教學課程中，您將移轉整個 Contoso 費用報銷應用程式從.NET Framework 4.7.2 [.NET Core 3](modernize-wpf-tutorial.md#net-core-3)。 在開始本教學課程的這個部分之前，請確定您執行下列：

* [開啟和建置 ContosoExpenses 範例](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)在 Visual Studio 2019。
* 如果您使用 Visual Studio 2019 的發行版本，可讓.NET Core SDK 的預覽版本。 在 Visual Studio 中，移至**工具 > 選項**、 搜尋 方塊中，輸入 "Preview"，然後選取**使用的.NET Core SDK 預覽**。 如果您使用[預覽版本的 Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)，您不需要選取此選項，因為預設會啟用.NET Core 預覽版。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>將 ContosoExpenses 專案移轉至.NET Core 3

在本節中，您會將 ContosoExpenses 專案中的 Contoso 費用報銷應用程式移轉至.NET Core 3。 您將建立新的專案檔，其中包含現有 ContosoExpenses 專案，但目標.NET Core 3，而不是.NET Framework 4.7.2 相同的檔案來執行這項操作。 這可讓您維護單一方案中使用的應用程式的.NET Framework 和.NET Core 版本。

1. 請確認 ContosoExpenses 目前專案目標.NET Framework 4.7.2。 在 方案總管 中，以滑鼠右鍵按一下**ContosoExpenses**專案，選擇**屬性**，並確認**目標 framework**屬性**應用程式** 索引標籤設為.NET Framework 4.7.2。

    ![.NET framework 4.7.2 版的專案](images/wpf-modernize-tutorial/NETFramework472.png)

3. 在 Windows 檔案總管中，瀏覽至**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses**資料夾，並建立名為新的文字檔**ContosoExpenses.Core.csproj**。

4. 以滑鼠右鍵按一下檔案，選擇**以開啟**，然後開啟您的選擇，例如記事本、 Visual Studio Code 或 Visual Studio 文字編輯器中。

5. 將下列文字複製到檔案，並將它儲存。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 關閉檔案，並回到**ContosoExpenses** Visual Studio 中的方案。

7. 以滑鼠右鍵按一下**ContosoExpenses**方案，然後選擇**加入現有專案]-> [** 。 選取  **ContosoExpenses.Core.csproj**您剛才在中建立檔案`C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses`資料夾將它加入至方案。

**ContosoExpenses.Core.csproj**包含下列元素：

* **專案**項目指定的 SDK 版本**Microsoft.NET.Sdk.WindowsDesktop**。 這是指 for Windows Desktop.NET 應用程式，並包含 WPF 和 Windows Forms 應用程式的元件。
* **PropertyGroup**元素包含子元素，表示專案輸出會是可執行檔 (不是 DLL)、 目標.NET Core 3，以及使用 WPF。 Windows Forms 應用程式中，您會使用**UseWinForms**項目，而不是**UseWPF**項目。

> [!NOTE]
> 當使用所導入的.NET Core 3.0 為.csproj 格式，.csproj 中，相同的資料夾中的所有檔案會被都視為專案的一部分。 因此，您不需要指定包含在專案中的每個檔案。 您必須指定想要排除的只有在您要定義自訂的建置動作，或您的檔案。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>將 ContosoExpenses.Data 專案移轉至.NET Standard

**ContosoExpenses**解決方案包括**ContosoExpenses.Data**類別庫，其中包含模型和服務的介面，並以.NET 4.7.2 為目標。 .NET core 3.0 應用程式可以使用.NET Framework 程式庫，只要將不會使用無法在.NET Core 應用程式開發介面。 不過，最佳的現代化路徑，就是將您的程式庫移至.NET Standard。 如此可確保您的.NET Core 3.0 應用程式完全支援您的程式庫。 此外，您可以重複使用程式庫也與其他平台，例如 （透過 ASP.NET Core) 的 web 和行動電話 （透過 Xamarin)。

若要移轉**ContosoExpenses.Data**專案以.NET Standard:

1. 在 Visual Studio 中，以滑鼠右鍵按一下**ContosoExpenses.Data**專案，然後選擇**卸載專案**。 再次以滑鼠右鍵按一下專案，然後選擇 **編輯 ContosoExpenses.Data.csproj**。

2. 刪除專案檔的整個內容。

3. 複製並貼上下列 XML 程式碼，然後儲存檔案。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 以滑鼠右鍵按一下**ContosoExpenses.Data**專案，然後選擇**重新載入專案**。

## <a name="configure-nuget-packages-and-dependencies"></a>設定 NuGet 套件和相依性

當您移轉**ContosoExpenses.Core**並**ContosoExpenses.Data**專案在先前章節中，您必須移除 NuGet 套件參考的專案中。 在本節中，您將新增回這些參考。

若要設定的 NuGet 套件**ContosoExpenses.Data**專案：

1.  在  **ContosoExpenses.Data**專案中，展開**相依性**節點。 請注意， **NuGet**區段遺漏。

    ![NuGet 套件](images/wpf-modernize-tutorial/NuGetPackages.png)

    如果您開啟**Packages.config**中**方案總管 中**您會發現 'old' 的參考，NuGet 套件時使用完整的.NET Framework，請使用專案。

    ![相依性和封裝](images/wpf-modernize-tutorial/Packages.png)

    以下是內容**Packages.config**檔案。 您會注意到所有的 NuGet 套件的目標完整的.NET Framework 4.7.2:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. 在  **ContosoExpenses.Data**專案中，刪除**Packages.config**檔案。

4. 在  **ContosoExpenses.Data**專案中，以滑鼠右鍵按一下**相依性**節點，然後選擇 **管理 NuGet 套件**。

  ![管理 NuGet 套件...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. 在 [ **NuGet 套件管理員**] 視窗中，按一下**瀏覽**。 搜尋`Bogus`套件並加以安裝。

    ![假的 NuGet 套件](images/wpf-modernize-tutorial/Bogus.png)

6. 搜尋`LiteDB`套件並加以安裝。

    ![LiteDB NuGet 套件](images/wpf-modernize-tutorial/LiteDB.png)

    您可能會好奇這些清單中的 NuGet 套件的儲存位置，因為專案不會再有 packages.config 檔案。 參考的 NuGet 套件直接儲存在.csproj 檔案。 您可以檢視的內容來檢查這**ContosoExpenses.Data.csproj**在文字編輯器中的專案檔。 您會找到檔案的結尾處加入下列幾行：

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 您可能也會注意到，您要安裝此.NET Core 3 專案相同的封裝為.NET Framework 4.7.2 專案所使用的項目。 NuGet 套件支援多目標。 程式庫作者可以包含不同版本的程式庫在相同的套件中，針對不同的架構及平台編譯。 這些套件支援完整的.NET Framework 與.NET Standard 2.0，這是與.NET Core 3 專案相容。 如需.NET Framework、.NET Core 和.NET Standard 的差異的詳細資訊，請參閱[.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)。

若要設定的 NuGet 套件**ContosoExpenses.Core**專案：

1. 在  **ContosoExpenses.Core**專案中，開啟**packages.config**檔案。 請注意，目前包含下列.NET Framework 4.7.2 為目標的參考。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    在下列步驟中，您將.NET Standard 版本`MvvmLightLibs`和`Unity`封裝。 其他兩個是當您安裝這些兩個程式庫時，自動下載 nuget 的相依性。

2. 在  **ContosoExpenses.Core**專案中，刪除**Packages.config**檔案。

3. 以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**管理 NuGet 套件**。

4. 在 [ **NuGet 套件管理員**] 視窗中，按一下**瀏覽**。 搜尋`Unity`套件並加以安裝。

    ![Unity 封裝](images/wpf-modernize-tutorial/UnityPackage.png)

5. 搜尋`MvvmLightLibsStd10`套件並加以安裝。 這是.NET Standard 版本`MvvmLightLibs`封裝。 此套件中，作者選擇封裝中使用不同的套件，.NET Framework 版本的程式庫的.NET Standard 版本。

    ![MvvmLightsLibs 封裝](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. 在  **ContosoExpenses.Core**專案中，以滑鼠右鍵按一下**相依性**節點，然後選擇 **加入參考**。

7. 在 **專案 > 解決方案**類別目錄中，選取**ContosoExpenses.Data**然後按一下**確定**。

    ![新增參考](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>停用自動產生的組件屬性

在這點在移轉過程中，如果您嘗試建置**ContosoExpenses.Core**專案，您會看到一些錯誤。

![.NET core 3 建置新的錯誤](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

因為新的.csproj 格式所導入的.NET Core 3.0 存放區的專案檔中的組件資訊，此問題的情況而非**AssemblyInfo.cs**檔案。 若要修正這些錯誤，請停用此行為，並讓專案繼續使用**AssemblyInfo.cs**檔案。

1. 在 Visual Studio 中，以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**卸載專案**。 再次以滑鼠右鍵按一下專案，然後選擇 **編輯 ContosoExpenses.Core.csproj**。

1. 新增下列項目中**PropertyGroup**區段，然後儲存檔案。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    加入這個項目之後**PropertyGroup**區段現在看起來應該像這樣：

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**重新載入專案**。

4. 以滑鼠右鍵按一下**ContosoExpenses.Data**專案，然後選擇**卸載專案**。 再次以滑鼠右鍵按一下專案，然後選擇 **編輯 ContosoExpenses.Data.csproj**。

5. 將相同的項目中新增**PropertyGroup**區段，然後儲存檔案。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    加入這個項目之後**PropertyGroup**區段現在看起來應該像這樣：

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 以滑鼠右鍵按一下**ContosoExpenses.Data**專案，然後選擇**重新載入專案**。

## <a name="add-the-windows-compatibility-pack"></a>新增 Windows 相容性套件

如果您現在嘗試編譯**ContosoExpenses.Core**並**ContosoExpenses.Data**專案，您會看到先前的錯誤現在已修正，但仍有某些錯誤中的**ContosoExpenses.Data**類似以下的程式庫。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

這些錯誤會將轉換的結果**ContosoExpenses.Data**專案.NET Standard 程式庫，可以在多個平台，包括 Linux、 Android、 iOS 執行，從.NET Framework 程式庫 （這是專為 Windows）和更多功能。 **ContosoExpenses.Data**專案中包含類別，稱為**RegistryService**，其互動登錄中，僅限 Windows 的概念。

若要解決這些錯誤，安裝[Windows 相容性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)NuGet 套件。 此封裝提供適用於使用.NET Standard 程式庫中的許多 Windows 特定 Api 的支援。 之後使用此封裝，但它將仍為目標.NET Standard 程式庫將不再會是跨平台。 

1. 以滑鼠右鍵按一下**ContosoExpenses.Data**專案。
2. 選擇**管理 NuGet 封裝**。
3. 在 [ **NuGet 套件管理員**] 視窗中，按一下**瀏覽**。 搜尋`Microsoft.Windows.Compatibility`套件並加以安裝。 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 現在再試一次編譯專案，以滑鼠右鍵按一下**ContosoExpenses.Data**專案，然後選擇**建置**。

這段建置流程會順利完成。

## <a name="test-and-debug-the-migration"></a>測試和偵錯移轉

既然已成功建置專案，您已準備好執行和測試應用程式，以查看是否有任何執行階段錯誤。

1. 以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**設定為啟始專案**。

2. 按 F5 以啟動**ContosoExpenses.Core**偵錯工具中的專案。 您會看到類似下面的例外狀況。

    ![在 Visual Studio 中顯示的例外狀況](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    因為當您從在移轉開始的.csproj 檔案中刪除內容，您移除了資訊有關引發這個例外狀況**建置動作**映像檔案。 下列步驟修正此問題。

3. 停止偵錯工具。

4. 以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**卸載專案**。 再次以滑鼠右鍵按一下專案，然後選擇 **編輯 ContosoExpenses.Core.csproj**。

5. 在關閉前**專案**項目，新增下列項目：

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**重新載入專案**。

7. 若要將 Contoso.ico 指派至應用程式，以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**屬性**。 在開啟的頁面中，按一下 [下拉式清單底下 **] 圖示**，然後選取`Images\contoso.ico`。

    ![在專案屬性中的 [Contoso] 圖示](images/wpf-modernize-tutorial/ContosoIco.png)

8. 按一下 [儲存]  。

9. 按 F5 以啟動**ContosoExpenses.Core**偵錯工具中的專案。 確認應用程式，現在會執行。

## <a name="next-steps"></a>後續步驟

此時在教學課程中，您已成功移轉 Contoso 費用報銷應用程式以.NET Core 3。 您現在準備好進行[第 2 部分：新增 UWP InkCanvas 控制項使用 XAML 群島](modernize-wpf-tutorial-2.md)。

> [!NOTE]
> 如果您有高解析度螢幕時，您可能會注意到，應用程式看起來很小。 您將會解決這個問題，在本教學課程的下一個步驟。
