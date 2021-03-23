---
description: C#/WinRT 是一項工具組，可為 C# 程式碼提供 WinRT 投影支援。
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, uwp, 標準, c#, winrt, cswinrt, 投影
ms.localizationpriority: medium
ms.openlocfilehash: 55fbc91bb67b0853eafebdf05ffcf116637233bf
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804772"
---
# <a name="cwinrt"></a>C#/WinRT

C#/WinRT 是 NuGet 封裝的工具組，其針對 C# 語言提供 Windows 執行階段 (WinRT) 投影支援。 *投射* 是指 interop 元件，它可讓您以自然且熟悉的方式來進行目的語言的程式設計 WinRT api。 C #/WinRT 投射會隱藏 c # 和 WinRT 介面之間 interop 的詳細資料，並將許多 [WinRT 類型的對應提供給適當的 .net 對](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)等專案，例如字串、uri、通用數值型別和泛型集合。

C #/WinRT 目前支援在 .NET 5 + 中使用 [目標 Framework](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option) 標記來取用 WinRT api。 使用特定的 Windows 版本指定目標 Framework 標記，會將參考加入至 c # 所產生的 Windows SDK 投射和執行時間元件/Winrt

最新的 [c #/WinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/) 可讓您針對 .net 5 + 取用者 [建立](#create-an-interop-assembly) 和 [參考](#reference-an-interop-assembly) 您自己的 WinRT interop 元件。 最新的 c #/WinRT 版本也包含在 c # 中撰寫 WinRT 類型的 [預覽](create-windows-runtime-component-cswinrt.md) 。

如需有關 C#/WinRT 的詳細資訊，請參閱 [C#/WinRT GitHub 存放庫](https://aka.ms/cswinrt/repo)。

## <a name="motivation-for-cwinrt"></a>C#/WinRT 的動機

[.Net Core](/dotnet/core/) 是 .NET 平台的重點，而 .NET 5 則是最新的主要版本。 此架構是開放原始碼的跨平台執行階段，可用來建置裝置、雲端和 IoT 應用程式。

舊版 .NET Framework 和 .NET Core 已內建 Windows 特有技術 WinRT 的知識。 為了支援 .NET 5 的可攜性和效率目標，我們已從 .NET 編譯器和執行階段提取 WinRT 投影支援，並將其移至 C#/WinRT 工具組。 C#/WinRT 的目標是提供等同於舊版 C# 編譯器和 .NET 執行階段內建 WinRT 支援的架構。 如需詳細資訊，請參閱 [Windows 執行階段類型的 .NET 對應](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)。

C#/WinRT 也支援 WinUI 3.0。 此版 WinUI 會從作業系統中提取原生的 Microsoft UI 控制項和功能。 這可讓應用程式開發人員使用 Windows 10 1803 版和更新版本上的最新控制項和視覺效果。

最後，C# /WinRT 是一般的工具組，目的是為了支援無法在 C# 編譯器或 .NET 執行階段中使用內建 WinRT 支援的其他案例。

## <a name="create-an-interop-assembly"></a>建立 interop 組件

WinRT Api 是在 Windows 中繼資料中定義 (\* winmd) 檔。 C #/WinRT NuGet 套件 ([CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) 包含 c #/WinRT 編譯器 **cswinrt.exe**，您可以用它來處理 Windows 中繼資料檔案，並產生 .net 5 c # 程式碼。 您可以將這些來源檔案編譯成 interop 組件，類似於 [C++/WinRT](../cpp-and-winrt-apis/index.md) 產生 C++ 語言投影標頭的方式。 然後，您可以將 .NET 5 應用程式的 c #/WinRT interop 元件散發給參考，以及 c #/WinRT 執行時間元件。

如需示範如何建立和散發 Interop 組件 (如 NuGet 套件) 的逐步解說，請參閱[逐步解說：從 C++/WinRT 元件產生 .NET 5.0 投影並更新 NuGet](net-projection-from-cppwinrt-component.md)。

### <a name="invoke-cswinrtexe"></a>叫用 cswinrt.exe

若要從專案叫用 cswinrt.exe，請安裝最新的 [C#/WinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)。 接著，您可以在 **C# 類別庫 (.NET Core)** 專案中設定 C#/WinRT 特定的專案屬性，以產生 Interop 組件。 下列專案片段會示範 **cswinrt** 的簡單叫用，以在 Contoso 命名空間中產生各類型的投影來源。 這些來源接著會包含在專案組建中。

```xml
<PropertyGroup>
  <CsWinRTIncludes>Contoso</CsWinRTIncludes>
</PropertyGroup>
```

在此專案中，您也需要參考 c #/WinRT NuGet 套件，以及您想要投射的專案專屬 winmd 檔案，不論是透過 NuGet 套件、專案參考或直接檔案參考。 根據預設，不會投射 **Windows** 和 **Microsoft** 命名空間，因為 c #/WinRT 已經透過支援目標 Framework 標記和 WinUI 3 來產生這些投射。 如需 c #/WinRT NuGet 專案屬性的完整清單，請參閱 [CsWinRT nuget 檔](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)。

### <a name="distribute-the-interop-assembly"></a>散發 interop 組件

Interop 組件通常會散發為 NuGet 套件，而且在 C#/WinRT NuGet 套件上還有用於必要 C#/WinRT 執行階段組件的相依性 **WinRT.Runtime.dll**。

為確保針對 .NET 5 應用程式部署正確的 c #/WinRT 執行時間版本，請 `targetFramework` 在 nuspec 檔案中加入具有 c #/WinRT NuGet 套件相依性的條件。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework="net5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="1.1.4" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> 適用于 .NET 5.0 的目標 Framework 標記正在移動，來源為。NETCoreApp 5.0 "到" net 5.0 "。

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

## <a name="common-errors-and-troubleshooting"></a>常見的錯誤以及疑難排解

- 錯誤：「未提供或未偵測到 Windows 中繼資料。」

  您可以使用 `<CsWinRTWindowsMetadata>` 專案屬性來指定 Windows 中繼資料，例如：
  ```xml
  <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
  ```
  
- 錯誤 CS0246：找不到名為 'Windows' 的類型或命名空間名稱 (是否遺漏 using 指示詞或組件參考？)

  若要解決此錯誤，請編輯 `<TargetFramework>` 屬性，以特定的 Windows 版本為目標，例如：
  ```xml
  <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
  ```
  如需指定 `<TargetFramework>` 屬性的詳細資訊，請參閱[呼叫 Windows 執行階段 API](/windows/apps/desktop/modernize/desktop-to-uwp-enhance) 上的文件。


### <a name="net-sdk-versioning-errors"></a>.NET SDK 版本設定錯誤

在以比其任何相依性還舊的 .NET SDK 版本所建置的專案中，您可能會遇到下列錯誤或警告。

| 錯誤或警告訊息 | 原因 |
|--------------------------|--------|
| 警告 MSB3277：無法解決在不同的 WinRT.Runtime 或 Microsoft.Windows.SDK.NET 版本之間發現的衝突。 | 如果參考的程式庫在其 API 介面上公開 Windows SDK 類型，就會發生此組建警告。 |
| [錯誤 CS1705](/dotnet/csharp/language-reference/compiler-messages/cs1705)：組件 'AssemblyName1' 使用的 'TypeName' 版本高於所參考的組件 'AssemblyName2' | 如果參考並取用程式庫中公開的 Windows SDK 類型，就會發生此組建編譯器錯誤。 |
| System.IO.FileLoadException | 在未公開 Windows SDK 類型的程式庫中呼叫 API 時，可能會發生此執行階段錯誤。 |

若要修正這些錯誤，請將您的 .NET SDK 更新為最新版本。 這麼做可確保應用程式所使用的執行階段和 Windows SDK 組件版本會與所有相依性相容。 .NET 5 SDK 的早期服務/功能更新可能會發生這些錯誤，因為執行階段修正可能需要更新組件版本。

## <a name="known-issues"></a>已知問題

已知問題和重大變更會在 [C#/WinRT GitHub 存放庫](https://aka.ms/cswinrt/repo)中註明。

如果您在 C# /WinRT NuGet 套件、cswinrt.exe 編譯器或產生的投影來源上遇到任何功能問題，請透過 [C#/WinRT 問題頁面](https://github.com/microsoft/CsWinRT/issues)，將問題提交給我們。

## <a name="additional-resources"></a>其他資源

* [C#/WinRT GitHub 存放庫](https://aka.ms/cswinrt/repo)
