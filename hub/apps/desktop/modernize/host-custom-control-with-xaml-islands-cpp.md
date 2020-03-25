---
description: 本文示範如何使用 XAML 裝載 API，在C++ Win32 應用程式中裝載自訂 UWP 控制項。
title: 使用 XAML 裝載 API 在C++ Win32 應用程式中裝載自訂 UWP 控制項
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10，uwp， C++，Win32，xaml islands，自訂控制項，使用者控制項，主控制項
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226312"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>在C++ Win32 應用程式中裝載自訂 UWP 控制項

本文示範如何使用[UWP xaml 裝載 API](using-the-xaml-hosting-api.md) ，在新C++的 Win32 應用程式中裝載自訂 UWP xaml 控制項。 如果您有現有C++的 Win32 應用程式專案，您可以針對您的專案調整這些步驟和程式碼範例。

若要裝載自訂 UWP XAML 控制項，您將在本逐步解說中建立下列專案和元件：

* **Windows 桌面應用程式專案**。 此專案會執行原C++生 Win32 桌面應用程式。 您會將程式碼加入此專案，以使用 UWP XAML 裝載 API 來裝載自訂 UWP XAML 控制項。

* **UWP 應用程式專案C++（/WinRT）** 。 此專案會實作為自訂 UWP XAML 控制項。 它也會實行根中繼資料提供者，以便在專案中載入自訂 UWP XAML 類型的中繼資料。

## <a name="requirements"></a>需求

* Visual Studio 2019 16.4.3 或更新版本。
* Windows 10 版本 1903 SDK （版本10.0.18362）或更新版本。
* 與 Visual Studio 一起安裝的[/WinRT Visual Studio 延伸模組（VSIX）。 C++ ](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 如需詳細資訊，請參閱[ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)。

## <a name="create-a-desktop-application-project"></a>建立桌面應用程式專案

1. 在 Visual Studio 中，建立名為**MyDesktopWin32App**的新**Windows 桌面應用程式**專案。 []、[ **C++** **Windows**] 和 [**桌面**] 專案篩選器下提供此專案範本。

2. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後按一下 [重定**目標方案**]，選取**10.0.18362.0**或更新版本的 SDK，然後按一下 **[確定]** 。

3. 安裝[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 套件，以在您的專案中啟用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)的支援：

    1. 以滑鼠右鍵按一下**方案總管**中的**MyDesktopWin32App**專案，然後選擇 [**管理 NuGet 封裝**]。
    2. 選取 [**流覽**] 索引標籤，搜尋[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)套件，然後安裝此套件的最新版本。

4. 在 [**管理 NuGet 套件**] 視窗中，安裝下列額外的 NuGet 套件：

    * 6\.0.0 （含）以後版本的[SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 。 此套件提供數個組建和執行時間資產，讓 XAML 孤島可在您的應用程式中運作。
    * [XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) （version v 6.0.0 或更新版本）。
    * [VCRTForwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140)。

5. 建立解決方案，並確認它已成功建立。

## <a name="create-a-uwp-app-project"></a>建立 UWP 應用程式專案

接下來，將**UWP （C++/WinRT）** 應用程式專案新增至您的方案，並對此專案進行一些設定變更。 稍後在本逐步解說中，您將在此專案中加入程式碼，以執行自訂 UWP XAML 控制項，並定義 Windows 社區工具組所提供的[XamlHost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)類別實例。 您的應用程式會使用此類別做為根中繼資料提供者，以載入自訂 UWP XAML 類型的中繼資料。

1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選取 [**加入** -> **新增專案**]。

2. 將**空白應用程式（C++/WinRT）** 專案新增至您的方案。 將專案命名為**MyUWPApp** ，並確定 [目標版本] 和 [最小版本] 都設定為**Windows 10 1903 版**或更新版本。

3. 在**MyUWPApp**專案中，安裝[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 套件：

    1. 以滑鼠右鍵按一下**MyUWPApp**專案，然後選擇 [**管理 NuGet 封裝**]。
    2. 選取 [**流覽**] 索引標籤，搜尋[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)套件，然後安裝此套件的 v 6.0.0 或更新版本。

4. 以滑鼠右鍵按一下 [ **MyUWPApp** ] 節點，然後選取 [**屬性**]。 在 [**通用屬性** ->  **C++/WinRT** ] 頁面上，設定下列屬性，然後**按一下 [** 套用]。

    * 將 [**詳細**資訊] 設定為 [**一般**]。
    * 將 [**優化**] 設定為 [**否**]。

    當您完成時，[屬性] 頁面看起來應該像這樣。

    ![C++/WinRT 專案屬性](images/xaml-islands/xaml-island-cpp-1.png)

5. 在 [屬性]**視窗的 [設定內容 -> ** **一般**] 頁面上，將 [設定**類型**] 設為 [**動態連結程式庫（.Dll）** ]，然後按一下 **[確定**] 關閉 [屬性] 視窗。

    ![一般專案屬性](images/xaml-islands/xaml-island-cpp-2.png)

6. 將預留位置可執行檔新增至**MyUWPApp**專案。 Visual Studio 需要此預留位置可執行檔，才能產生必要的專案檔，並正確地建立專案。

    1. 在**方案總管**中，以滑鼠右鍵按一下**MyUWPApp**專案節點，**然後選取 [** **新增 -> 新專案**]。
    2. 在 [**加入新專案**] 對話方塊中，選取左側頁面中的 [**公用程式**]，然後選取 [**文字檔（.txt）** ]。 輸入名稱**預留位置**，然後按一下 [**新增**]。
      ![新增文字檔](images/xaml-islands/xaml-island-cpp-3.png)
    3. 在 **方案總管**中，選取 **預留位置 .exe**檔案。 在 [**屬性**] 視窗中，確認 [**內容**] 屬性設定為 [ **True**]。
    4. 在**方案總管**中，以滑鼠右鍵按一下**MyUWPApp**專案中的**Package.appxmanifest.xml**檔案，選取 [**開啟方式**]，然後選取 [ **XML （文字）編輯器**]，再按一下 **[確定]** 。
    5. 尋找 **&lt;應用程式&gt;** 元素，並將**可執行檔**屬性變更為 `placeholder.exe`的值。 當您完成時， **&lt;應用程式&gt;** 元素看起來應該會像這樣。

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. 儲存並關閉**package.appxmanifest.xml**檔案。

7. 在**方案總管**中，以滑鼠右鍵按一下 [ **MyUWPApp** ] 節點，然後選取 **[卸載專案**]。
8. 以滑鼠右鍵按一下 [ **MyUWPApp** ] 節點，然後選取 [**編輯 MyUWPApp .vcxproj**]。
9. 尋找 `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` 的元素，並以下列 XML 取代它。 此 XML 會在專案前面加入數個新屬性。

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. 儲存並關閉專案檔。
11. 在**方案總管**中，以滑鼠右鍵按一下 [ **MyUWPApp** ] 節點，然後選取 [**重載專案**]。

## <a name="configure-the-solution"></a>設定解決方案

在本節中，您將更新包含這兩個專案的方案，以設定專案相依性和專案正確建立所需的組建屬性。

1. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後加入名為 **.PROPS**的新 XML 檔案。
2. 將下列 XML 新增至 **.props**檔案。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. 從 [ **view** ] 功能表按一下 [**屬性管理員**] （視您的設定而定，這可能會在 [ **View** -> **其他視窗**] 底下）。
4. 在 [**屬性管理員**] 視窗中，以滑鼠右鍵按一下**MyDesktopWin32App** ，然後選取 **[加入現有屬性工作表**]。 流覽至您剛才新增的 **.props**檔案，然後按一下 [**開啟**]。
5. 重複上一個步驟，將 **.props**檔案加入至 [**屬性管理員**] 視窗中的**MyUWPApp**專案。
6. 關閉 [**屬性管理員**] 視窗。
7. 確認已正確儲存屬性工作表變更。 在**方案總管**中，以滑鼠右鍵按一下**MyDesktopWin32App**專案，然後選擇 [**屬性**]。 按一下 [設定**屬性**] -> **Genneral**，並確認 [**輸出目錄**] 和 [**中繼目錄**] 屬性具有您新增至 **.props**檔案的值。 您也可以針對**MyUWPApp**專案確認相同的。
    ![專案屬性](images/xaml-islands/xaml-island-cpp-4.png)

8. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後選擇 [專案相依性 **]** 。 在 [**專案**] 下拉式清單中，確認已選取 [ **MyDesktopWin32App** ]，然後在 [**相依于**] 清單中選取 [ **MyUWPApp** ]。
    ![專案相依性](images/xaml-islands/xaml-island-cpp-5.png)

9. 按一下 [確定]。

## <a name="add-code-to-the-uwp-app-project"></a>將程式碼新增至 UWP 應用程式專案

您現在已準備好將程式碼新增至**MyUWPApp**專案，以執行下列工作：

* 執行自訂 UWP XAML 控制項。 稍後在本逐步解說中，您將會在**MyDesktopWin32App**專案中加入裝載此控制項的程式碼。
* 在 Windows 社區工具組中定義一個衍生自[XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)類別的類型。 這個類別會作為根中繼資料提供者，以便載入自訂 UWP XAML 類型的中繼資料。

### <a name="define-a-custom-uwp-xaml-control"></a>定義自訂 UWP XAML 控制項

1. 在**方案總管**中，以滑鼠右鍵按一下 [ **MyUWPApp** ]，**然後選取 [** 新增 -> **新專案**]。 選取左窗格中的 [  **C++視覺效果**] 節點，選取 [**空白C++使用者控制項（/WinRT）** ]，將它命名為**MyUserControl**，然後按一下 [**新增**]。
2. 在 XAML 編輯器中，以下列 XAML 取代**MyUserControl**的內容，然後儲存檔案。

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>定義 XamlApplication 類別

接下來，修改**MyUWPApp**專案中的預設**應用程式**類別，以衍生自 Windows 社區工具組所提供的[XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)類別。 稍後在本逐步解說中，您將更新桌面專案，以建立此類別的實例做為根中繼資料提供者，以載入自訂 UWP XAML 類型的中繼資料。

  > [!NOTE]
  > 使用 XAML 群島的每個解決方案都只能包含一個定義 `XamlApplication` 物件的專案。 應用程式中的所有自訂 UWP XAML 控制項都會共用相同的 `XamlApplication` 物件。 

1. 在**方案總管**中，以滑鼠右鍵按一下**MyUWPApp**專案中的**MainPage。** 按一下 [**移除**]，然後按一下 [**刪除**]，從專案中刪除此檔案 permamently。
2. 在**MyUWPApp**專案中，展開 [ **app.xaml**檔案]。
3. 使用下列程式碼取代**app.xaml**、應用程式 **.cpp**、 **app.config**和**應用程式 .idl**檔案的內容。

    * **App.xaml**：

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **應用程式 .idl**：

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App-v**：

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **應用程式 .cpp**：

        ```cpp
        #include "pch.h"
        #include "App.h"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

4. 將新的標頭檔新增至名為**app.config**的**MyUWPApp**專案。
5. 將下列程式碼新增至**app.config**檔案、儲存檔案，然後將它關閉。

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. 建立解決方案，並確認它已成功建立。

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>將桌面專案設定為使用自訂控制項類型

在**MyDesktopWin32App**應用程式可以在 xaml 島中裝載自訂 UWP XAML 控制項之前，必須先將它設定為使用**MyUWPApp**專案中的自訂控制項類型。 有兩種方式可執行此動作，而且您可以在完成此逐步解說時選擇任一選項。

### <a name="option-1-package-the-app-using-msix"></a>選項1：使用 MSIX 封裝應用程式

您可以在[MSIX 套件](https://docs.microsoft.com/windows/msix)中封裝應用程式以進行部署。 MSIX 是適用于 Windows 的現代化應用程式封裝技術，它是以 MSI、.appx、App-v 和 ClickOnce 安裝技術的組合為基礎。

1. 將新的[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增至您的方案。 當您建立專案時，請將它命名為**MyDesktopWin32Project** ，然後選取 **[Windows 10，版本1903（10.0;）]。組建18362）** ，適用于**目標版本**和**最低版本**。

2. 在封裝專案中，以滑鼠右鍵按一下 [**應用程式**] 節點，然後選擇 [**加入參考**]。 在專案清單中，選取**MyDesktopWin32App**專案旁的核取方塊，然後按一下 **[確定]** 。
    ![參考專案](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> 如果您選擇不要在[MSIX 套件](https://docs.microsoft.com/windows/msix)中封裝應用程式以進行部署，執行應用程式的電腦必須安裝[Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) 。

### <a name="option-2-create-an-application-manifest"></a>選項2：建立應用程式資訊清單

您可以將[應用程式資訊清單](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)新增至您的應用程式。

1. 以滑鼠右鍵按一下 [ **MyDesktopWin32App** ] 專案，**然後選取 [** **新增 -> 新專案**]。 
2. 在 [**加入新專案**] 對話方塊中，按一下左窗格中的 [ **Web** ]，然後選取 **[XML 檔案（.xml）** ]。 
3. 將新檔案命名為**app.config** ，**然後按一下 [新增]** 。
4. 將新檔案的內容取代為下列 XML。 這個 XML 會在**MyUWPApp**專案中註冊自訂控制項類型。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>設定其他桌面專案屬性

接下來，更新**MyDesktopWin32App**專案以定義其他 include 目錄的宏，並設定其他屬性。

1. 在**方案總管**中，以滑鼠右鍵按一下**MyDesktopWin32App**專案，然後選取 **[卸載專案**]。

2. 以滑鼠右鍵按一下 **[MyDesktopWin32App （卸載）** ]，然後選取 [**編輯 MyDesktopWin32App .vcxproj**]。

3. 在檔案結尾的結尾 `</Project>` 標記前面加入下列 XML。 然後，儲存並關閉檔案。

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. 在**方案總管**中，以滑鼠右鍵按一下 **[MyDesktopWin32App （卸載）** ]，然後選取 [**重載專案**]。

5. 以滑鼠右鍵按一下 [ **MyDesktopWin32App**]，選取 [**屬性**]，然後按一下左窗格中的 [ **C/C++**  ] 節點。 確認已從您在上一個步驟中所做的專案檔變更中定義了**其他 Include 目錄**宏。
    ![C/C++專案設定](images/xaml-islands/xaml-island-cpp-7.png)

6. 在 **屬性頁** 對話方塊中，展開 **資訊清單工具** -> **輸入和輸出**。 將 [ **DPI 感知**] 屬性設為 [**每個監視器高 DPI 感知**]。 如果您未設定此屬性，可能會在某些高 DPI 案例中遇到資訊清單設定錯誤。
    ![C/C++專案設定](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>在桌面專案中裝載自訂 UWP XAML 控制項

最後，您已準備好將程式碼加入至**MyDesktopWin32App**專案，以裝載您稍早在**MyUWPApp**專案中定義的自訂 UWP XAML 控制項。

1. 在**MyDesktopWin32App**專案中，開啟**架構 .h**檔案，並將下列程式程式碼標記為批註。 當您完成時，請儲存檔案。

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. 開啟**MyDesktopWin32App**檔案，並以下列程式碼取代此檔案的內容，以參考必要C++的/WinRT 標頭檔。 當您完成時，請儲存檔案。

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. 開啟**MyDesktopWin32App .cpp**檔案，然後將下列程式碼新增至 `Global Variables:` 區段。

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. 在相同的檔案中，將下列程式碼新增至 `Forward declarations of functions included in this code module:` 區段。

    ```cpp
    void AdjustLayout(HWND);
    ```

5. 在相同的檔案中，將下列程式碼緊接在 `wWinMain` 函式的 `TODO: Place code here.` 批註後面。

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. 在相同的檔案中，以下列程式碼取代預設的 `InitInstance` 函式。

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. 在相同的檔案中，將下列新函式新增至檔案結尾。

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. 在相同的檔案中，找出 `WndProc` 函式。 使用下列程式碼取代 switch 語句中的預設 `WM_DESTROY` 處理常式。

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. 儲存檔案。
10. 建立解決方案，並確認它已成功建立。

## <a name="test-the-app"></a>測試應用程式

執行方案，並確認**MyDesktopWin32App**會在下列視窗中開啟。

![MyDesktopWin32App 應用程式](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>後續步驟

許多裝載 XAML 孤島的桌面應用程式都必須處理其他案例，才能提供順暢的使用者體驗。 例如，桌面應用程式可能需要處理 XAML 島中的鍵盤輸入、XAML 島和其他 UI 專案之間的焦點導覽，以及版面配置變更。

如需有關處理這些案例和相關程式碼範例指標的詳細資訊，請參閱[Win32 應用程式C++中 XAML Islands 的 Advanced 案例](advanced-scenarios-xaml-islands-cpp.md)。

## <a name="related-topics"></a>相關主題

* [在桌面應用程式中裝載 UWP XAML 控制項（XAML 島）](xaml-islands.md)
* [在C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)
* [在C++ Win32 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [Win32 應用程式中C++ XAML 孤島的 Advanced 案例](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 島程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)
