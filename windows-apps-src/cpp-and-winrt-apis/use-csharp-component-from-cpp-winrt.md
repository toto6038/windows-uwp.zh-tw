---
description: '本主題將逐步引導您完成將簡單 c # 元件新增至 c + +/WinRT 應用程式的程式'
title: '撰寫 c # Windows 執行階段元件以從 c + +/WinRT 應用程式使用'
ms.date: 12/30/2020
ms.topic: article
keywords: windows 10、uwp、standard、c + +、cpp、winrt、C#
ms.localizationpriority: medium
ms.openlocfilehash: 0b2b4f67d67fc1a23e650d1fbe5d58964add1a70
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804752"
---
# <a name="authoring-a-c-windows-runtime-component-for-use-from-a-cwinrt-app"></a>撰寫 c # Windows 執行階段元件以從 c + +/WinRT 應用程式使用

本主題將逐步引導您完成將簡單 c # 元件新增至 c + +/WinRT 專案的程式。

Visual Studio 可讓您輕鬆地在 Windows 執行階段元件內撰寫和部署您自己的自訂 Windows 執行階段類型 (以 c # 或) 撰寫的 WRC Visual Basic 專案，然後從 c + + 應用程式專案參考該 WRC，以及從該應用程式取用這些自訂類型。

就內部而言，您的 Windows 執行階段類型可以使用 UWP 應用程式中允許的任何 .NET 功能。

> [!NOTE]
> For more info, see [Windows Runtime components with C# and Visual Basic](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) and [.NET for UWP apps overview](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true).

就外部而言，您的型別成員只能公開其參數和傳回值的 Windows 執行階段類型。 當您建立方案時，Visual Studio 會建立您的 .NET WRC 專案，然後執行建立 Windows 中繼資料 ( winmd) 檔的組建步驟。 這是您的 Windows 執行階段元件 (WRC) ，Visual Studio 包含在您的應用程式中。

> [!NOTE]
> .NET 會將一些常用的 .NET 型別（例如基本型別和集合型別），自動對應到它們的 Windows 執行階段對應專案。 這些 .NET 型別可用於 Windows 執行階段元件的公用介面，並且會顯示給元件的使用者，作為對應的 Windows 執行階段類型。 請參閱 [使用 c # 和 Visual Basic Windows 執行階段元件](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)。

## <a name="prerequisites"></a>必要條件：

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-blank-app"></a>建立空白應用程式

在 Visual Studio 中，使用 [空白應用程式 (C++/WinRT)] 專案範本建立新的專案。 請確定您正在使用 (C++/WinRT) 範本，而不是 (通用 Windows) 範本。

將新專案的名稱設定為 *CppToCSharpWinRT* ，讓您的資料夾結構符合逐步解說。

## <a name="add-a-c-windows-runtime-component-to-the-solution"></a>將 c # Windows 執行階段元件新增至方案

在 Visual Studio 中，建立元件專案：在方案總管中，開啟 *CppToCSharpWinRT* 方案的快捷方式功能表並選擇 [ **加入**]，然後選擇 [ **新增專案** ]，將新的 c # 專案加入至方案。 在 [**加入新專案**] 對話方塊的 [**已安裝的範本**] 區段中，選擇 [ **Visual c #**]，然後選擇 [ **Windows**]，然後選擇 [**通用**]。 選擇 [ **通用 Windows) 範本] (Windows 執行階段元件** ，然後針對專案名稱輸入 **SampleComponent** 。 

> [!NOTE]
> 在 [ **新的通用 Windows 平臺專案** ] 對話方塊中，選擇 [ **Windows 10 Creators Update (10.0;組建 15063)** 的最小版本。 如需詳細資訊，請參閱下面的「 [應用程式最小版本](#application-minimum-version) 」一節。

## <a name="add-the-c-getmystring-method"></a>新增 c # GetMyString 方法

在 *SampleComponent* 專案中，將類別的名稱從 *Class1* 變更為 *Example*。 然後將兩個簡單成員加入至類別、私用 `int` 欄位和名為 *GetMyString* 的實例方法：

> ```csharp
>     public sealed class Example
>     {
>         int MyNumber;
> 
>         public string GetMyString()
>         {
>             return $"This is call #: {++MyNumber}";
>         }
>     }
> ```

> [!NOTE]
> 根據預設，類別會標示為 **public sealed**。 您從元件公開的所有 Windows 執行階段類別都必須 **密封**。

> [!NOTE]
> 選擇性：若要啟用新加入成員的 IntelliSense，請在方案總管中，開啟 *SampleComponent* 專案的快捷方式功能表，然後選擇 [ **建立**]。

## <a name="reference-the-c-samplecomponent-from-the-cpptocsharpwinrt-project"></a>參考 CppToCSharpWinRT 專案中的 c # SampleComponent

在方案總管的 c + +/WinRT 專案中，開啟 [ **參考**] 的快捷方式功能表，然後選擇 [ **加入參考** ] 以開啟 [ **加入參考** ] 對話方塊。 選擇 **\[專案]\**，然後選擇 **\[方案\]**。 選取 *SampleComponent* 專案的核取方塊，然後選擇 **[確定]** 以加入參考。

> [!NOTE]
> 選擇性：若要啟用 c + +/WinRT 專案的 IntelliSense，請在方案總管中開啟 *CppToCSharpWinRT* 專案的快捷方式功能表，然後選擇 [ **建立**]。

## <a name="edit-mainpageh"></a>編輯 MainPage。h

`MainPage.h`在 *CppToCSharpWinRT* 專案中開啟，然後新增兩個專案。  首先 `#include "winrt/SampleComponent.h"` ，在語句的結尾加入 `#include` ，然後將 `winrt::SampleComponent::Example` 欄位加入至 `MainPage` 結構。

```cppwinrt
// MainPage.h
...
#include "winrt/SampleComponent.h"

namespace winrt::CppToCSharpWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        winrt::SampleComponent::Example myExample;
...
    };
}
```

> [!NOTE]
> 在 Visual Studio 中， `MainPage.h` 會列在底下 `MainPage.xaml` 。

## <a name="edit-mainpagecpp"></a>編輯 MainPage.cpp

在中 `MainPage.cpp` ，變更 `Mainpage::ClickHandler` 實作為呼叫 c # 方法 `GetMyString` 。

```cppwinrt
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    //myButton().Content(box_value(L"Clicked"));

    hstring myString = myExample.GetMyString();

    myButton().Content(box_value(myString));
}
```
## <a name="run-the-project"></a>執行專案

您現在可以建置及執行專案。 每次按一下按鈕時，按鈕中的數位都會遞增。

![C + +/WinRT Windows 呼叫 c # 元件的螢幕擷取畫面](images/csharp-cpp-sample.png)

> [!TIP]
> 在 Visual Studio 中，建立元件專案：在方案總管中，開啟 *CppToCSharpWinRT* 專案的快捷方式功能表並選擇 [**屬性**]，然後選擇 [設定 **屬性**] 下的 [**調試** 程式]。 如果您想要同時對 c # (managed) 和 c + + (原生) 程式碼進行偵錯工具，請將偵錯工具類型設定為 *managed 和 native* 。
> ![C + + 調試屬性](images/cpp-debugging-properties.png)

## <a name="application-minimum-version"></a>最低應用程式版本

[**應用程式最少**](../updates-and-versions/choose-a-uwp-version.md)的 c # 專案版本將會控制用來編譯應用程式的 .net 版本。 例如，選擇 **Windows 10 Fall Creators Update (10.0;組建 16299)** 或更高版本將會啟用 .NET Standard 2.0 和 WINDOWS ARM64 處理器支援。 

> [!TIP]
> 建議您在不需要 .NET Standard 2.0 或 ARM64 支援時，使用低於16299的 **應用程式最低** 版本，以避免額外的組建設定。

## <a name="configure-for-windows-10-fall-creators-update-100-build-16299"></a>設定 Windows 10 Fall Creators Update (10.0;組建 16299) 

依照下列步驟，在 c + +/WinRT 專案所參考的 c # 專案中啟用 .NET Standard 2.0 或 Windows ARM64 支援。 

在 Visual Studio 中，移至方案總管，然後開啟 *CppToCSharpWinRT* 專案的快捷方式功能表。  選擇 [ **屬性** ]，並將通用 Windows 應用程式的最小版本設為 **Windows 10 Fall Creators Update (10.0;組建 16299)** (或更高版本) 。 對 *SampleComponent* 專案進行相同的動作。

在 Visual Studio 中，開啟 *CppToCSharpWinRT* 專案的快捷方式功能表，然後選擇 **[卸載專案** ] 以 `CppToCSharpWinRT.vcxproj` 在文字編輯器中開啟。 

將下列 XML 複製並貼入中的第 `PropertyGroup` 一個 `CPPWinRTCSharpV2.vcxproj` 。 

```xml
   <!-- Start Custom .NET Native properties -->
   <DotNetNativeVersion>2.2.9-rel-29512-01</DotNetNativeVersion>
   <DotNetNativeSharedLibary>2.2.8-rel-29512-01</DotNetNativeSharedLibary>
   <UWPCoreRuntimeSdkVersion>2.2.11</UWPCoreRuntimeSdkVersion>
   <!--<NugetPath>$(USERPROFILE)\.nuget\packages</NugetPath>-->
   <NugetPath>$(ProgramFiles)\Microsoft SDKs\UWPNuGetPackages</NugetPath>
   <!-- End Custom .NET Native properties -->
```

`DotNetNativeVersion`、和的值 `DotNetNativeSharedLibary` `UWPCoreRuntimeSdkVersion` 可能會根據 Visual Studio 的版本而有所不同。  若要將它們設定為正確的值，請開啟， `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages` 並查看下表中每個值的子目錄。  例如，目錄中 `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` 會有一個名為的目錄 `2.2.9-rel-29512-01` 。

> | MSBuild 變數 | 目錄 | 範例
> |-| - | -
> | DotNetNativeVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` | `2.2.9-rel-29512-01`
> | DotNetNativeSharedLibary | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.SharedLibrary` | `2.2.8-rel-29512-01`
> | UWPCoreRuntimeSdkVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.UWPCoreRuntimeSdk` | `2.2.11`

接下來，緊接在第一個之後 `PropertyGroup` ，新增下列 (未改變的) 。

```xml
  <!-- Start Custom .NET Native targets -->
  <!-- Import all of the .NET Native / CoreCLR props at the beginning of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.props" />
  <!-- End Custom .NET Native targets -->
```

在專案檔案的結尾，在結束記號之前， `Project` 將下列 (未改變的) 。

```xml
  <!-- Import all of the .NET Native / CoreCLR targets at the end of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.targets" />
  <!-- End Custom .NET Native targets -->
```

重載 Visual Studio 中的專案檔。 若要這樣做，請在 [Visual Studio] 方案總管中，開啟 *CppToCSharpWinRT* 專案的快捷方式功能表，然後選擇 [ **重載專案**]。

## <a name="building-for-net-native"></a>建立 .NET Native

建議您使用針對 .NET native 建立的 c # 元件，來建立並測試您的應用程式。 在 Visual Studio 中，開啟 *CppToCSharpWinRT* 專案的快捷方式功能表，然後選擇 **[卸載專案** ] 以 `CppToCSharpWinRT.vcxproj` 在文字編輯器中開啟。 

接下來，在 `UseDotNetNativeToolchain` `true` c + + 專案檔的發行和 ARM64 設定中，將屬性設定為。

在 [Visual Studio] 方案總管中，開啟 *CppToCSharpWinRT* 專案的快捷方式功能表，然後選擇 [ **重載專案**]。 

```xml
  <PropertyGroup Condition="'$(Configuration)'=='Release'" Label="Configuration">
...
    <UseDotNetNativeToolchain>true</UseDotNetNativeToolchain>
  </PropertyGroup>
  <PropertyGroup Condition="$(Platform)'='ARM64'" Label="Configuration">
    <UseDotNetNativeToolchain Condition="'$(UseDotNetNativeToolchain)'==''">true</UseDotNetNativeToolchain>
  </PropertyGroup>
```

## <a name="related-topics"></a>相關主題
* [使用 C# 和 Visual Basic 建立 Windows 執行階段元件](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [使用 C++/WinRT 的 Windows 執行階段元件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)