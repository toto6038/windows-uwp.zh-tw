---
description: C++/ WinRT&mdash;Windows 執行階段 API 的標準 C++ 語言投影的簡介。
title: C++/WinRT 簡介
ms.date: 04/18/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 簡介
ms.localizationpriority: medium
ms.openlocfilehash: 250e3626c5abee43cf3b8ca3320c78ec4f8f9751
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80662411"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 簡介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/X41j_gzSwOY]

&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 使用 C++/WinRT，您可以撰寫及取用使用任何符合標準 C++17 編譯器的 Windows 執行階段 API。 Windows SDK 包含 C++/WinRT；其在版本 10.0.17134.0 (Windows 10，版本 1803 ) 中引進。

C++/WinRT 是 Microsoft 針對 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 語言投影和 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) 建議的替代方案。 [C++/WinRT 相關主題](index.md#topics-about-cwinrt)的完整清單包含有關與 C++/CX 和 WRL 交互操作，以及從從移植的資訊。

> [!IMPORTANT]
> 最重要的一些 C++/WinRT 項目會在 [C++/WinRT 的 SDK 支援](#sdk-support-for-cwinrt)和 [C++/WinRT、XAML、VSIX 擴充功能和 NuGet 套件的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)等章節中說明。

## <a name="language-projections"></a>語言投影
Windows 執行階段根據元件物件模型 (COM) API，且設計它透過「語言投影」  來存取。 投影隱藏 COM 的詳細資訊，並針對特定語言提供更自然的程式設計體驗。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 參考內容中的 C++/WinRT 語言投影
當您在瀏覽[Windows UWP Api](https://docs.microsoft.com/uwp/api/)，按一下右上方下拉式方塊中的 [**語言**]，然後選取**C++/WinRT**，當它們在 C++/WinRT 語言投影中顯示時，檢視 API 語法區塊。

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>C++/WinRT、XAML、VSIX 擴充功能和 NuGet 套件的 Visual Studio 支援
如需 Visual Studio 支援，您需要 Visual Studio 2019 或 Visual Studio 2017 (至少 15.6 版；我們建議至少 15.7 版)。 從 Visual Studio 安裝程式中，安裝**通用 Windows 平台開發**工作負載。 在 [安裝詳細資料]   > [通用 Windows 平台開發]  中，勾選 [C++ (v14x) 通用 Windows 平台工具]  選項 (如果尚未勾選)。 而在 Windows [設定]   > [更新與安全性] **\&**  > [針對開發人員]  中，選擇 [開發人員模式]  選項，而非 [側載應用程式]  選項。

雖然我們建議您使用最新版的 Visual Studio 和 Windows SDK 進行開發，但如果您使用 10.0.17763.0 (Windows 10 版本 1809) 之前的 Windows SDK 之前隨附的 C++/WinRT 版本，若要使用上述的 Windows 命名空間標頭，您的 10.0.17134.0 (Windows 10 版本 1803) 專案中需要最小的 Windows SDK 目標版本。

您會想要從 [Visual Studio Marketplace](https://marketplace.visualstudio.com/)下載並安裝最新版的 [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。

- VSIX 擴充功能為您在 Visual Studio 中提供 C++/WinRT 專案和項目範本，以便您開始進行 C++/WinRT 開發。
- 此外，它也會提供您 C++/WinRT 投影類型的 Visual Studio 原生偵錯視覺效果 (natvis)；提供與 C# 偵錯相似的體驗。 Natvis 會自動偵錯組建。 您可以透過定義符號 WINRT_NATVIS 選擇加入到發行組建。

下列各節會說明 C++/WinRT 的 Visual Studio 專案範本。 當您建立已安裝最新版 VSIX 擴充功能的新 C++/WinRT 專案時候，新的 C++/WinRT 專案就會自動安裝 [Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 **Microsoft.Windows.CppWinRT** NuGet 套件提供 C++/WinRT 建置支援 (MSBuild 屬性和目標)，讓您的專案能在開發電腦與建置代理程式之間可攜 (在其上只會安裝 NuGet套件，而不會安裝 VSIX 擴充功能)。

或者，您可藉由手動安裝 **Microsoft.Windows.CppWinRT** NuGet 套件來轉換現有的專案。 安裝 (或更新為) 最新版 VSIX 擴充功能之後，請在 Visual Studio 中開啟現有的專案，按一下 [專案]  \>[管理 NuGet 套件...]  \>[瀏覽]  、在搜尋方塊中輸入或貼上 **Microsoft.Windows.CppWinRT**、在搜尋結果中選取項目，然後按一下 [安裝]  以安裝適用於該專案的套件。 新增該套件後，您會收到適用於專案的 C++/WinRT MSBuild 支援，包含叫用 `cppwinrt.exe` 工具。

> [!IMPORTANT]
> 如果您有使用 1.0.190128.4 之前的 VSIX 擴充功能版本建立 (或升級使用) 的專案，則查看[舊版的 VSIX 擴充功能](#earlier-versions-of-the-vsix-extension)。 該區段包含您的專案組態相關重要資訊，您必須知道要將其升級為使用最新版的 VSIX 擴充功能。

- 因為 C++/ WinRT 使用來自 C++17 標準的功能，因此 NuGet 會在 Visual Studio 中設定專案屬性 [C/C++]   > [語言]   > [C++ 語言標準]   > [ISO C++17 標準 (/std:c++17)]  。
- 它也會新增 [/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file) 編譯器選項。
- 其新增 [/await](/cpp/build/reference/await-enable-coroutine-support) 編譯器選項來啟用 `co_await`。
- 並指示 XAML 編譯器發出 C++/WinRT Codegen。
- 您可能也想要設定 [一致性模式:  是 (/permissive--)]，其進一步將您的程式碼限制為符合標準。
- 另一個要注意的專案屬性是 [C/C++]   > [一般]   > [警告為錯誤]  。 將此設定為 [Yes(/WX)]  或 [No(/WX-)]  來品味。 有時候，`cppwinrt.exe` 工具產生的來源檔案，會產生警告，直到您將實作新增至其中。

如上所述設定您的系統，您將能夠在 Visual Studio 中建立和建置或開啟 C++/WinRT 專案，並加以部署。

從 2.0 版起，**Microsoft.Windows.CppWinRT** NuGet 套件包含 `cppwinrt.exe` 工具。 您可以指向位於 Windows 執行階段中繼資料 (`.winmd`) 檔案的 `cppwinrt.exe` 工具，以產生標頭檔案式標準 C++ 程式庫，其會「投影」  中繼資料中所述的 API，從 C++/WinRT 程式碼耗用。 Windows 執行階段中繼資料 (`.winmd`) 檔案，提供描述 Windows 執行階段 API 介面的正式方式。 透過在您可以產生程式庫的中繼資料指出 `cppwinrt.exe`，以在第二方或第三方 Windows 執行階段元件中搭配使用任何執行階段類別實作，或在您自己的應用程式中實作。 如需詳細資訊，請參閱[使用 C++/WinRT 取用 API](consume-apis.md)。

透過使用 C++/WinRT，您也可以使用標準 C++ 實作自己的執行階段類別，而不用求助於 COM 樣式程式設計。 對於執行階段類別，您只要在 IDL 檔案中描述您的類型，而 `midl.exe` 與 `cppwinrt.exe` 會為您產生實作重複使用的原始程式碼檔案。 或者您可以只要實作衍生自 C++/WinRT 基底類別的介面。 如需詳細資訊，請參閱[使用 C++/WinRT 撰寫 API](author-apis.md)。

如需適用於 `cppwinrt.exe` 工具的自訂選項清單 (透過專案屬性設定)，請參閱 Microsoft.Windows.CppWinRT NuGet 套件[讀我檔案](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。

您可以依照專案中安裝的 **Microsoft.Windows.CppWinRT** NuGet 套件顯示狀態，找出使用 C++/WinRT MSBuild 支援的專案。

以下是 VSIX 擴充功能所提供的 Visual Studio 專案範本。

### <a name="blank-app-cwinrt"></a>空白的應用程式 (C++/WinRT)
適用於擁有 XAML 使用者介面的通用 Windows 平台 (UWP) 的應用程式範本。

Visual Studio 提供 XAML 編譯器支援，並從位於每個 XAML 標記檔案後面的介面定義語言 (IDL) (`.idl`) 產生實作與標頭虛設常式。 在 IDL 檔案中，定義任何您想要在應用程式 XAML 網頁中參考的本機執行階段類別，然後見一次專案，在 `Generated Files` 中產生實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考的虛設類型定義，實作您的本機執行階段類別。 請參閱[將執行階段類別分解成 Midl 檔案 (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)。

Visual Studio 2019 中針對 C++/WinRT 的 XAML 設計介面支援即使用 C# 進行同位檢查。 在 Visual Studio 2019 中，您可以使用 [屬性]  視窗的 [事件]  索引標籤，在 C++/WinRT 專案內新增事件處理常式。 您也可以將事件處理常式手動新增到您的程式碼，&mdash;請參閱[在 C++/WinRT 中使用委派來處理事件](handle-events.md)，以取得詳細資訊。

### <a name="core-app-cwinrt"></a>核心應用程式 (C++/WinRT)
適用於不使用 XAML 的通用 Windows 平台 (UWP) 應用程式的專案範本。

但是，它使用適用於 Windows.ApplicationModel.Core 命名空間的 C++/WinRT Windows 命名空間標頭。 在建置與執行後，請按一下空白空間新增彩色方格；然後再按一下彩色方格拖曳它。

### <a name="windows-console-application-cwinrt"></a>Windows 主控台應用程式 (C++/WinRT)
Windows 傳統型 C++/WinRT 用戶端應用程式的專案範本 (具有主控台使用者介面)。

### <a name="windows-desktop-application-cwinrt"></a>Windows 傳統型應用程式 (C++/WinRT)
Windows 傳統型 C++/WinRT 用戶端應用程式的專案範本，其在 Win32 **MessageBox** 內顯示 Windows 執行階段 [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri)。

### <a name="windows-runtime-component-cwinrt"></a>Windows 執行階段元件 (C++/WinRT)
元件的專案範本；通常適用於通用 Windows 平台 (UWP) 的使用量。

此範本示範 `midl.exe` > `cppwinrt.exe` 工具鏈，其中從 IDL 產生 Windows 執行階段中繼資料 (`.winmd`)，然後從 Windows Runtime 中繼資料產生實作及標頭虛設常式。

在 IDL 檔案中，在您的元件、其預設的介面，以及任何其實作的其他介面中，定義執行階段類別。 建置一次專案產生 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考的虛設類型定義，實作您元件中的執行階段類別。 請參閱[將執行階段類別分解成 Midl 檔案 (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)。

將已建置的 Windows 執行階段元件二進位與其 `.winmd` 和使用它們的 UWP 應用程式搭配一起。

## <a name="earlier-versions-of-the-vsix-extension"></a>舊版 VSIX 擴充功能
我們建議您安裝 (或更新為) 最新版的 [VSIX 擴充功能](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。 預設情況下，它會設定為自行更新。 如果您這麼做，且具有使用 1.0.190128.4 之前的 VSIX 擴充功能版本建立的專案，則這一節包含將這些專案升級為使用新版本的重要資訊。 如果您未更新，您仍會發現這一節中的資訊很有用。

就支援的 Windows SDK 和 Visual Studio 版本，以及 Visual Studio 組態而言，上面 [C++/WinRT、XAML、VSIX 擴充功能和 NuGet 套件的 Visual Studio 支援](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)一節中的資訊適用於舊版的 VSIX 擴充功能。 以下資訊針對使用舊版建立 (或升級使用) 的專案，說明其行為和組態的重要差異。

### <a name="created-earlier-than-101810022"></a>在 1.0.181002.2 之前建立
如果您的專案使用 1.0.181002.2 之前的 VSIX 擴充功能版本建立，然後C++/WinRT 建置支援內建在該版本的 VSIX 擴充功能中。 您的專案已在 `.vcxproj` 檔案中設定 `<CppWinRTEnabled>true</CppWinRTEnabled>` 屬性。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

您可以藉由手動安裝 **Microsoft.Windows.CppWinRT** NuGet 套件來升級您的專案。 安裝 (或升級為) 最新版 VSIX 擴充功能之後，請在 Visual Studio 中開啟您的專案，按一下 [專案]  \>[管理 NuGet 套件...]  \>[瀏覽]  、在搜尋方塊中輸入或貼上 **Microsoft.Windows.CppWinRT**、在搜尋結果中選取項目，然後按一下 [安裝]  以安裝適用於您專案的套件。

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>使用 1.0.181002.2 與 1.0.190128.3 之間版本建立 (或升級)
如果您的專案使用 1.0.181002.2 與 1.0.190128.3 (含) 之間的 VSIX 擴充功能版本建立，則專案範本會在專案中自動建立 **Microsoft.Windows.CppWinRT** NuGet 套件。 您可能也已將舊專案升級為使用此範圍中的 VSIX 擴充功能版本。 如果您這樣做，則&mdash;因為建置支援也仍然存在於此範圍內的 VSIX 擴充功能版本中&mdash;您升級的專案不一定已安裝 **Microsoft.Windows.CppWinRT** NuGet 套件。

若要升級您的專案，請遵循上一節中的指示，並確保您的專案已經安裝 **Microsoft.Windows.CppWinRT** NuGet 套件。

### <a name="invalid-upgrade-configurations"></a>無效的升級組態
使用最新版的 VSIX 擴充功能時，如果也還沒安裝 **Microsoft.Windows.CppWinRT**NuGet 套件，專案就不適合具有 `<CppWinRTEnabled>true</CppWinRTEnabled>` 屬性。 具有此組態的專案會產生建置錯誤訊息「C++/WinRT VSIX 不再提供專案的建置支援。  請將專案參考加入 Microsoft.Windows.CppWinRT Nuget 套件」。

如上所述， C++/WinRT 專案現在必須已安裝的 NuGet 套件。

由於 `<CppWinRTEnabled>` 元素現在已過時，您可以選擇性地編輯 `.vcxproj`，以及刪除此元素。 它不是絕對必要，但是個選項。

此外，如果 `.vcxproj` 包含 `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`，則可以將它移除，以便在不需安裝 C++/WinRT VSIX 擴充功能的情況下建置。

## <a name="sdk-support-for-cwinrt"></a>適用於 C++/WinRT 的 SDK 支援
雖然現在僅針對相容性理由存在，但自版本 10.0.17134.0 (Windows 10，版本 1803) 開始，Windows SDK 包含標頭檔案式的標準 C++ 程式庫，以取用 Windows 命名空間中第一方 Windows API (Windows 執行階段 API)。 這些標頭位於 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 資料夾內。 從 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809) 起，系統會為您在專案 *$(GeneratedFilesDir)* 資料夾內產生這些標頭。

針對相容性，Windows SDK 也會再次隨附 `cppwinrt.exe` 工具。 不過，我們建議您改為安裝並使用最新版的 `cppwinrt.exe`，這隨附於 **Microsoft.Windows.CppWinRT** NuGet 套件。 該套件以及 `cppwinrt.exe` 已在上述各節中說明。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自訂類型
在您的 C++/WinRT 程式設計中，您可以使用標準 C++ 語言功能和[標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)&mdash;包括一些 C++ 標準程式庫資料類型。 但也您會注意到投影中的某些自訂資料類型，且您可以選擇使用它們。 例如，我們會使用[開始使用 C++/WinRT](get-started.md) 中快速程式碼範例的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) 是您在某些時候可能會使用的另一種類型。 但是您比較不可能直接使用 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view)等類型。 或者，您可以選擇不想使用它，如果且當 C++ 標準程式庫中出現對等項目類型時，才不會變更任何程式碼。

> [!WARNING]
> 如果您仔細研究 C++/WinRT Windows 命名空間標頭，您也會看到這類類型。 一個範例是 **winrt::param::hstring**，但有也會收集範例。 這些會存在只是要最佳化輸入參數的繫結，並且它們會產生大幅的效能提升，讓大部分呼叫模式「只適用於」相關的標準 C++ 類型和容器。 這些類型僅由投影在新增大多數值時使用。 它們會高度最佳化，它們不用於一般用途，切勿冒險自行使用它們。 您也不應該使用 `winrt::impl` 命名空間的任何項目，因為這些是實作類型，因此會隨時變更。 您應該繼續使用標準類型，或來自 [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)的類型。
>
> 另請參閱[將參數傳入 ABI 界限](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi)。

## <a name="important-apis"></a>重要 API
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)
* [開始使用 C++/WinRT](get-started.md)
* [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT 中的字串處理](strings.md)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)
