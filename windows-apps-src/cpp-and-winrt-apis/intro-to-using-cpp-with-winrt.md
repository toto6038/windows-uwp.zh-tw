---
author: stevewhims
description: C++/ WinRT&mdash;Windows 執行階段 API 的標準 C++ 語言投影的簡介。
title: C++/WinRT 的簡介
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, introduction, 標準, 投影, 撰寫, 事件, 簡介
ms.localizationpriority: medium
ms.openlocfilehash: 03abe68fd19573d7b2deba9937c515a8641e8fca
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3409584"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 的簡介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 使用 C++/WinRT，您可以撰寫及取用使用任何符合標準 C++17 編譯器的 Windows 執行階段 API。 Windows SDK 包含 C++/WinRT；其在版本 10.0.17134.0 (Windows 10，版本 1803 ) 中引進。

C + + /winrt 是 Microsoft 的建議替代方案為[C + + /CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)語言投影，與[Windows 執行階段 c + + 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)。 完整的清單[主題有關 C + + /winrt](index.md#topics-about-cwinrt)包含的資訊是有關互通，和從移植，C + + /CX 與 WRL。

> [!IMPORTANT]
> 最重要的兩個 C++/WinRT 項目會在 [C++/WinRT 的 SDK 支援](#sdk-support-for-cwinrt) 和 [C++/WinRT 的支援，以及 VSIX](#visual-studio-support-for-cwinrt-and-the-vsix) 等章節中說明。

## <a name="language-projections"></a>語言投影
Windows 執行階段根據元件物件模型 (COM) API，且設計它透過*語言投影*來存取。 投影隱藏 COM 的詳細資訊，並針對特定語言提供更自然的程式設計體驗。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 參考資料內容中的 C++/WinRT 語言投影
當您在瀏覽[Windows UWP Api](https://docs.microsoft.com/uwp/api/)，按一下右上方下拉式方塊中的 [**語言**]，然後選取**C++/WinRT**，當它們在 C++/WinRT 語言投影中顯示時，檢視 API 語法區塊。

## <a name="sdk-support-for-cwinrt"></a>適用於 C++/WinRT 的 SDK 支援
自版本 10.0.17134.0 (Windows 10，版本 1803) 開始，Windows SDK 包含標頭檔案式的標準 C++ 程式庫，以取用 Windows 命名空間中第一方 Windows API (Windows 執行階段 API)。 C++/WinRT 也隨附 `cppwinrt.exe` 工具，您可以用其在 Windows 執行階段中繼資料 (`.winmd`) 檔案指出，以產生標頭檔案式標準 C++ 程式庫，其會*投影*中繼資料中所述的 API，從 C++/WinRT 程式碼耗用。 Windows 執行階段中繼資料 (`.winmd`) 檔案，提供描述 Windows 執行階段 API 介面的正式方式。 透過在您可以產生程式庫的中繼資料指出 `cppwinrt.exe`，以在第二方或第三方 Windows 執行階段元件中搭配使用任何執行階段類別實作，或在您自己的應用程式中實作。 如需詳細資訊，請參閱 [使用 C++/WinRT 取用 API](consume-apis.md)。

透過使用 C++/WinRT，您也可以使用標準 C++ 實作自己的執行階段類別，而不用求助於 COM 樣式程式設計。 對於執行階段類別，您只要在 IDL 檔案中描述您的類型，而 `midl.exe` 與 `cppwinrt.exe` 會為您產生實作重複使用的原始程式碼檔案。 或者您可以只要實作衍生自 C++/WinRT 基底類別的介面。 如需詳細資訊，請參閱 [使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>C++/WinRT 的 Visual Studio 支援，以及 VSIX
適用於 Visual Studio 中的 C++/WinRT 專案範本，以及 C++/WinRT MSBuild 屬性及目標，從 [Visual Studio Marketplace](https://marketplace.visualstudio.com/) 下載並安裝 [C + + / WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)。

您將需要 Visual Studio 2017 (至少為版本 15.6；我們建議至少 15.7) 和 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803)。 如果您在尚未安裝它，您將需要安裝 Visual Studio 安裝程式中的從**c + + 通用 Windows 平台工具**選項。 和在 Windows**設定**中 > **更新 \ & 安全性** > **適用於開發人員**，選擇 [**開發人員模式**] 選項，而不是 [**側載應用程式**] 選項。

您將會接著能夠建立和建置時，或開啟，C + + /winrt 投影在 Visual Studio 中，並將它部署。 或者，您可以轉換現有的專案，新增`<CppWinRTEnabled>true</CppWinRTEnabled>`屬性設定為其`.vcxproj`檔案。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

一旦您新增該屬性，會收到適用於專案的 C++/WinRT MSBuild 支援，包含叫用 `cppwinrt.exe` 工具。

因為 C++/ WinRT 從 C++17 標準使用功能，它必須投影屬性**C/C++** > **語言** > **ISO C++17 標準 (/std: c++17)**。 您可能還需設定**一致性模式：是 (/已獲授權-)**，其進一步限制您的程式碼才能標準相容。

另一個要注意的專案屬性是**C/C++** > **一般** > **警告為錯誤**。 將此設定為**Yes(/WX)** 或**No(/WX-)** 來品味。 有時候，`cppwinrt.exe` 工具產生的來源檔案，會產生警告，直到您將實作新增至其中。

VSIX 也會提供您 C++/WinRT 投影類型的 Visual Studio 原生偵錯視覺效果 (natvis)；提供與 C# 偵錯相似的體驗。 Natvis 會自動偵錯組建。 您可以透過定義符號 WINRT_NATVIS 選擇加入到發行組建。

以下是 VSIX 所提供的 Visual Studio 專案範本。

### <a name="windows-console-application-cwinrt"></a>Windows 主控台應用程式 (C++/WinRT)
適用於 Windows 電腦的 C++/WinRT 用戶端應用程式專案範本，具有主控台使用者介面。

### <a name="blank-app-cwinrt"></a>空白的應用程式 (C++/WinRT)
適用於擁有 XAML 使用者介面的通用 Windows 平台 (UWP) 的應用程式範本。

Visual Studio 提供 XAML 編譯器支援，並從位於每個 XAML 標記檔案後面的介面定義語言 (IDL) (`.idl`) 產生實作與標頭虛設常式。 IDL 檔案中，定義任何您想要在應用程式 XAML 網頁中參考的本機執行階段類別，然後見一次專案，在 `Generated Files` 中產生實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您的本機執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

### <a name="core-app-cwinrt"></a>核心應用程式 (C++/WinRT)
適用於不使用 XAML 的通用 Windows 平台 (UWP) 的應用程式範本。

但是，它使用適用於 Windows.ApplicationModel.Core 命名空間的 C++/WinRT Windows 命名空間標頭。 在建置與執行後，請按一下空白空間新增彩色方格；然後再按一下彩色方格拖曳它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 執行階段元件 (C++/WinRT)
元件的專案範本；通常適用於通用 Windows 平台 (UWP) 的使用量。

此範本示範 `midl.exe` > `cppwinrt.exe` 工具鏈，其中從 IDL 產生 Windows 執行階段中繼資料 (`.winmd`)，然後從 Windows Runtime 中繼資料產生實作及標頭虛設常式。

IDL 檔案中，在您的元件、其預設的介面，以及任何其實作的其他介面中，定義執行階段類別。 建置一次專案產生 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您元件中的執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

將已建置的 Windows 執行階段元件二進位與其 `.winmd` 和使用它們的 UWP 應用程式搭配一起。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自訂類型
在您的 C++/WinRT 程式設計中，您可以使用標準 C++ 語言功能和[標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)&mdash;包括一些 C++ 標準程式庫資料類型&mdash。 但也您會注意到投影中的某些自訂資料類型，且您可以選擇使用它們。 例如，我們會使用[開始使用 C++/WinRT](get-started.md) 中快速程式碼範例的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) 是您在某些時候可能會使用的另一種類型。 但是您比較不會直接使用例如[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view)的類型。 或者，您可以選擇不想使用它，如果且當 C++ 標準程式庫中出現對等項目類型時，才不會變更任何程式碼。

> [!WARNING]
> 如果您仔細研究 C++/WinRT Windows 命名空間標頭，您也會看到這類類型。 一個範例是 **winrt::param::hstring**，但有也會收集範例。 這些會存在只是要最佳化輸入參數的繫結，並且它們會產生大幅的效能提升，讓大部分呼叫模式「只適用於」相關的標準 C++ 類型和容器。 這些類型僅由投影在新增大多數值時使用。 它們會高度最佳化，它們不用於一般用途，切勿冒險自行使用它們。 您也不應該使用 `winrt::impl` 命名空間的任何項目，因為這些是實作類型，因此會隨時變更。 您應該繼續使用標準類型，或來自 [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)的類型。

## <a name="important-apis"></a>重要 API
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [Winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)
* [開始使用 C++/WinRT](get-started.md)
* [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT 中的字串處理](strings.md)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)
