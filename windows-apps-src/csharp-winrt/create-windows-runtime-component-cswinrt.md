---
description: '使用 c #/WinRT 撰寫 Windows 執行階段元件，並從原生應用程式使用'
title: '建立 c #/WinRT 元件並從 c + +/WinRT 使用它'
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f5157f97163a72ccce1ce9fc3f560fb4e16b1df
ms.sourcegitcommit: 61a874d00991f7ca06466a99a557ef0777bd0f7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989648"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>逐步解說：建立 c #/WinRT 元件並從 c + +/WinRT 使用它

> [!NOTE]
> 本文中所述的 c #/WinRT 撰寫支援目前為預覽版本，自 c #/WinRT 版本 1.1.2-發行前版本。210208.6。 在此版本中，它只能用於早期的意見反應和評估。

C #/WinRT 可讓 .NET 5 開發人員使用類別庫專案，在 c # 中撰寫自己的 Windows 執行階段元件。 撰寫的元件可以在原生桌面應用程式中做為封裝參考，或做為專案參考來取用，但有幾項修改。

本逐步解說示範如何使用 c #/WinRT 建立簡單的 Windows 執行階段元件、將元件發佈為 NuGet 套件，以及從 c + +/WinRT 主控台應用程式取用元件。 您可以 [在 Github 上](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)找到此逐步解說的範例程式碼。

撰寫您的執行時間元件時，請遵循本文中所述的指導方針和類型限制 [。](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 就內部而言，您的元件中的 Windows 執行階段類型可以使用 UWP 應用程式中允許的任何 .NET 功能。 如需詳細資訊，請參閱適用 [于 UWP 應用程式的 .net](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)。 就外部而言，您的型別成員只能公開其參數和傳回值的 Windows 執行階段類型。

> [!NOTE]
> 有一些 [對應至 .net 類型](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)的 Windows 執行階段類型。 這些 .NET 型別可用於 Windows 執行階段元件的公用介面，並且會顯示給元件的使用者，作為對應的 Windows 執行階段類型。

## <a name="prerequisites"></a>必要條件

本逐步解說需要下列工具和元件：

- Visual Studio 2019
- .NET 5.0 SDK
- C + + [/WINRT VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) For c + +/WinRT 專案範本

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>使用 c #/WinRT 建立簡單的 Windows 執行階段元件

首先，在 Visual Studio 2019 中建立新專案。 選取 **類別庫 ( .Net Core)** 專案範本，並將它命名為 **AuthoringDemo**。 您將需要對專案進行下列新增和修改：

1. 更新 `TargetFramework` **AuthoringDemo .csproj** 檔案中的，並將下列元素加入至 `PropertyGroup` ：

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

    若為 `TargetFramework` 元素，您可以使用下列其中一個 [目標 framework 名字](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)。
    - **net 5.0-windows 10.0.17763。0**
    - **net 5.0-windows 10.0.18362。0**
    - **net 5.0-windows 10.0.19041。0**

2. 安裝最新版的 [c #/WinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/1.1.2-prerelease.210208.6)。

    a. 在方案總管中，以滑鼠右鍵按一下專案節點，然後選取 [ **管理 NuGet 套件**]。

    b. 搜尋 **CsWinRT** NuGet 套件，並安裝最新版本。 本逐步解說使用 c #/WinRT 版本 1.1.2-搶鮮版（210208.6）。

3. 加入 `PropertyGroup` 設定數個 c #/WinRT 屬性的新元素。

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
    </PropertyGroup>
      ```

      以下是此範例中屬性的一些詳細資料。 如需 c #/WinRT 專案屬性的完整清單，請參閱 [c #/WinRT NuGet 檔。](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - `CsWinRTComponent`屬性指定您的專案是 Windows 執行階段元件，以便為元件產生 WinMD 檔案。
    - `CsWinRTWindowsMetadata`屬性提供 Windows 中繼資料的來源。 這是版本1.1.1 的必要項。

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

接下來，產生元件的 NuGet 套件。 當您產生封裝時，c #/WinRT 會設定套件中的元件和裝載元件，以供原生應用程式取用。

有幾種方式可產生 NuGet 套件：

* 如果您想要在每次建立專案時產生 NuGet 套件，請將下列屬性新增至 **AuthoringDemo** 專案檔，然後重建專案。

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>
    ```

* 或者，您也可以在 **方案總管** 中，以滑鼠右鍵按一下 **AuthoringDemo** 專案，然後選取 [**套件**]，來產生 NuGet 套件。

當您建立套件時，[ **組建** ] 視窗應該會顯示 `AuthoringDemo.1.0.0.nupkg` 已成功建立 NuGet 套件。

## <a name="consume-the-component-in-cwinrt"></a>使用 c + +/WinRT 中的元件

C #/WinRT 撰寫 Windows 執行階段元件可以從原生應用程式使用，但有一些修改。 下列步驟示範如何在原生主控台應用程式中呼叫上述撰寫的元件。 

1. 將新的 **c + +/WinRT 主控台應用程式** 專案加入至方案。 請注意，如果您選擇此專案，此專案也可以是不同解決方案的一部分。

    a. 在 **方案總管** 中，以滑鼠右鍵按一下方案節點，然後按一下 [**加入**  ->  **新專案**]。

    b. 在 [ **加入新專案] 對話方塊** 中，搜尋 [ **c + +/WinRT 主控台應用程式** ] 專案範本。 選取範本，然後按 **[下一步]**。

    c. 將新專案命名為 **CppConsoleApp** ，然後按一下 [ **建立**]。

2. 將參考新增至 AuthoringDemo 元件，以作為 NuGet 封裝或專案參考。

    - **選項 1 (套件參考)**：  

        a. 以滑鼠右鍵按一下 **CppConsoleApp** 專案，然後選取 [ **管理 NuGet 套件**]。 您可能需要設定套件來源，以新增 AuthoringDemo NuGet 套件的參考。 若要這樣做，請按一下 NuGet 封裝管理員中的 **設定** 圖示，並將套件來源新增至適當的路徑。

        ![NuGet 設定](images/nuget-sources-settings.png)

        b. 設定套件來源之後，請搜尋 **AuthoringDemo** 套件，然後按一下 [ **安裝**]。

        ![安裝 NuGet 套件](images/install-authoring-nuget.png)

    - **選項 2 (專案參考)**：
        
        a. 以滑鼠右鍵按一下 [ **CppConsoleApp** ] 專案，然後選取 [**加入**  ->  **參考**]。 在 [ **專案** ] 節點底下，加入 **AuthoringDemo** 專案的參考。 在此預覽版本中，您也需要從 [**流覽]** 節點將檔案參考加入至 **AuthoringDemo。** 產生的 winmd 檔案可以在 **AuthoringDemo** 專案的輸出目錄中找到。

        b. 針對此預覽，您也必須將下列屬性群組新增至 **CppConsoleApp. .vcxproj**。 若要編輯原生應用程式專案檔案，請先以滑鼠右鍵按一下 **CppConsoleApp** 專案節點，然後選取 **[卸載專案**]。

        ```xml
        <PropertyGroup>
            <TargetFrameworkVersion>net5.0</TargetFrameworkVersion>
            <TargetFramework>native</TargetFramework>
            <TargetRuntime>Native</TargetRuntime>
        </PropertyGroup>
        ```

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

4. 在部署專案時，修改專案以在輸出中包含 runtimeconfig.js和資訊清單檔案。 針對 **WinRT.Host.runtimeconfig.json** 和 **CppConsoleApp.exe 資訊清單** 檔案，請在 **方案總管** 中按一下該檔案，然後將 [ **內容** ] 屬性設定為 [ **True**]。 以下是這類內容的範例。

    ![部署內容](images/deploy-content.png)

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

- [範例程式碼](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)
- [撰寫元件](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [受控元件裝載](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
