---
description: C#/WinRT 是一項工具組，可為 C# 程式碼提供 WinRT 投影支援。
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, uwp, 標準, c#, winrt, cswinrt, 投影
ms.localizationpriority: medium
ms.openlocfilehash: c3cac3049dbd5d22c23716a2da38a41fb6000a71
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984494"
---
# <a name="cwinrt"></a>C#/WinRT

> [!IMPORTANT]
> C#/WinRT 處於公開預覽狀態，最終發行之前可能會經過大幅修改。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

C#/WinRT 是 NuGet 封裝的工具組，其針對 C# 語言提供 Windows 執行階段 (WinRT) 投影支援。 「投影」是指 interop 組件之類的轉譯層，可讓您以自然且熟悉的方式，針對目標語言進行 WinRT API 程式設計。 例如，C#/WinRT 投影會隱藏 C# 和 WinRT 介面之間的 interop 詳細資料，並將許多 WinRT 類型對應提供給適當的 .NET 對等項目，例如字串、URI、通用值類型和泛型集合。

C#/WinRT 目前提供使用 WinRT 類型的支援，而目前的預覽版可讓您[建立](#create-an-interop-assembly)和[參考](#reference-an-interop-assembly) WinRT interop 元件。 C# /WinRT 的未來版本將會新增以 C# 撰寫 WinRT 類型的支援。

## <a name="motivation-for-cwinrt"></a>C#/WinRT 的動機

[.Net Core](/dotnet/core/) 是 .NET 平台的重點，而 .NET 5 則是下一個主要版本。 此架構是開放原始碼的跨平台執行階段，可用來建置裝置、雲端和 IoT 應用程式。

舊版 .NET Framework 和 .NET Core 已內建 Windows 特有技術 WinRT 的知識。 為了支援 .NET 5 的可攜性和效率目標，我們已從 .NET 編譯器和執行階段提取 WinRT 投影支援，並將其移至 C#/WinRT 工具組。 C#/WinRT 的目標是提供等同於舊版 C# 編譯器和 .NET 執行階段內建 WinRT 支援的架構。 如需詳細資訊，請參閱 [Windows 執行階段類型的 .NET 對應](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)。

C#/WinRT 也支援 WinUI 3.0。 此版 WinUI 會從作業系統中提取原生的 Microsoft UI 控制項和功能。 這可讓應用程式開發人員使用 Windows 10 1803 版和更新版本上的最新控制項和視覺效果。

最後，C# /WinRT 是一般的工具組，目的是為了支援無法在 C# 編譯器或 .NET 執行階段中使用內建 WinRT 支援的其他案例。 C#/WinRT 支援相容於 .NET Standard 2.0 的 .NET 執行階段版本，例如 Mono 5.4。

如需有關 C#/WinRT 的詳細資訊，請參閱 [C#/WinRT GitHub 存放庫](https://aka.ms/cswinrt/repo)。

## <a name="create-an-interop-assembly"></a>建立 interop 組件

WinRT API 定義於 Windows 中繼資料 (*.winmd) 檔案中。 C#/WinRT NuGet 套件 ([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) 包含 C# /WinRT 編譯器 (**cswinrt**)，可讓您用來處理 Windows 中繼資料檔案並產生 .NET 5.0 C# 程式碼。 您可以將這些來源檔案編譯成 interop 組件，類似於 [C++/WinRT](../cpp-and-winrt-apis/index.md) 產生 C++ 語言投影標頭的方式。 接著，除了 C#/WinRT 執行階段組件，您還可以散發 C#/WinRT interop 組件讓應用程式參考。

如需示範如何建立 Interop 組件的逐步解說，請參閱 [逐步解說：從 C++/WinRT 元件產生 .NET 5.0 投影並更新 NuGet](net-projection-from-cppwinrt-component.md)。

### <a name="invoke-cswinrtexe"></a>叫用 cswinrt.exe

若要顯示命令列選項，請執行 `cswinrt -?`。 若要從專案叫用 cswinrt.exe，建議您使用 Directory.Build.Targets 檔案。 下列專案片段會示範 **cswinrt** 的簡單叫用，以在 Contoso 命名空間中產生各類型的投影來源。 這些來源接著會包含在專案組建中。

```xml
  <Target Name="GenerateProjection" BeforeTargets="Build">
    <PropertyGroup>
      <CsWinRTParams>
# This sample demonstrates using a response file for cswinrt execution.
# Run "cswinrt -h" to see all command line options.
-verbose
# Include Windows SDK metadata to satisfy references to 
# Windows types from project-specific metadata.
-in 10.0.18362.0
# Don't project referenced Windows types, as these are 
# provided by the Windows interop assembly.
-exclude Windows 
# Reference project-specific winmd files, defined elsewhere,
# such as from a NuGet package.
-in @(ContosoWinMDs->'"%(FullPath)"', ' ')
# Include project-specific namespaces/types in the projection
-include Contoso 
# Write projection sources to the "Generated Files" folder,
# which should be excluded from checkin (e.g., .gitignored).
-out "$(ProjectDir)Generated Files"
      </CsWinRTParams>
    </PropertyGroup>
    <WriteLinesToFile
        File="$(CsWinRTResponseFile)" Lines="$(CsWinRTParams)"
        Overwrite="true" WriteOnlyWhenDifferent="true" />
    <Message Text="$(CsWinRTCommand)" Importance="$(CsWinRTVerbosity)" />
    <Exec Command="$(CsWinRTCommand)" />
  </Target>

  <Target Name="IncludeProjection" BeforeTargets="CoreCompile" AfterTargets="GenerateProjection">
    <ItemGroup>
      <Compile Include="$(ProjectDir)Generated Files/*.cs" Exclude="@(Compile)" />
    </ItemGroup>
  </Target>
```

### <a name="distribute-the-interop-assembly"></a>散發 interop 組件

Interop 組件通常會散發為 NuGet 套件，而且在 C#/WinRT NuGet 套件上還有用於必要 C#/WinRT 執行階段組件的相依性 **winrt.runtime.dll**。 C#/WinRT 執行階段組件有兩個版本，一個是以 .NET Standard 2.0 為目標，另一個則是以 .NET 5.0 為目標。 我們只會根據應用程式的目標架構來部署其中一個。 

* 如果您想讓舊版跨平台應用程式在 Windows 上提供煥然一新的功能，則適用以 .NET Standard 2.0 為目標。
* 針對需要在原生物件參考 (例如 XAML 應用程式) 上進行正確記憶體回收的新式 Windows 應用程式，則建議以 .NET 5.0 為目標。

Interop 組件可以在 nuspec 檔中包含 `targetFramework` 條件，以確保能為應用程式部署正確的 C#/WinRT 執行階段版本。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
      <group targetFramework=".NET5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> .NET 5.0 的目標架構別稱已從 ".NETCoreApp5.0" 變更為 ".NET5.0"。 C#/WinRT 發行前版本會使用其中一種。

## <a name="reference-an-interop-assembly"></a>參考 interop 組件

通常，C#/WinRT interop 組件會由應用程式專案來參考。 但是，也可以讓中繼 interop 組件來輪流參考。 例如，WinUI interop 組件會參考 Windows SDK interop 組件。

若要取用投影的 C#/WinRT 類型，請將適當 C#/WinRT interop NuGet 套件的參考新增至您的專案。 這會讓 interop 組件和 C#/WinRT 執行階段組件都新增至專案。

如果您散發第三方 WinRT 元件，但沒有官方 interop 組件，應用程式專案可能會遵循[建立 interop 組件](#create-an-interop-assembly)的程序，來產生自己的私用投影來源。 我們不建議使用這種方法，因為這可能會在一個程序內產生相同類型的投影衝突。 遵循[語意化版本控制系統](https://semver.org)結構描述的 NuGet 套件，就是為避免此情況而設計的。 建議您使用官方的第三方 interop 元件。

### <a name="winrt-type-activation"></a>WinRT 類型啟用

C#/WinRT 支援啟用作業系統所裝載的 WinRT 類型，以及 [Win2D](https://www.nuget.org/packages/Win2D.uwp/) 之類的第三方元件。 桌面應用程式中的第三方元件啟用支援，可透過 Windows 10 1903 版和更新版本中的[註冊免費 WinRT 啟用](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/)來加以啟用。 如果已建立目標 UWP 應用程式的元件，則可能也需要使用 [VCRT 轉寄站](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/)套件。

如果 Windows 無法啟用上述類型，則 C#/WinRT 也會提供啟用後援方法。 在此情況下，C# /WinRT 會嘗試根據完整類型名稱來尋找原生的實作 DLL，並逐漸移除元素。 例如，後援邏輯會嘗試依序啟用下列模組中的 Contoso.Controls.Widget 類型：

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT 會使用 [LoadLibrary 替代搜尋順序](/windows/win32/dlls/dynamic-link-library-search-order#alternate-search-order-for-desktop-applications)來尋找實作 DLL。 依賴此後援行為的應用程式應該會將實作 DLL 連同應用程式模組封裝在一起。

## <a name="known-issues"></a>已知問題

目前的 C#/WinRT 預覽版中有一些已知的 interop 相關效能問題。 這些問題會在 2020 年年底的最終版本之前解決。

如果您在 C# /WinRT NuGet 套件、cswinrt.exe 編譯器或產生的投影來源上遇到任何功能問題，請透過 [C#/WinRT 問題頁面](https://github.com/microsoft/CsWinRT/issues)，將問題提交給我們。

## <a name="additional-resources"></a>其他資源

* [C#/WinRT GitHub 存放庫](https://aka.ms/cswinrt/repo)