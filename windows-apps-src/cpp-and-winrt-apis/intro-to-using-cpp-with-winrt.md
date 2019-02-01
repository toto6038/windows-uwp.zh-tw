---
description: C++/ WinRT&mdash;Windows 執行階段 API 的標準 C++ 語言投影的簡介。
title: C++/WinRT 的簡介
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, introduction, 標準, 投影, 撰寫, 事件, 簡介
ms.localizationpriority: medium
ms.openlocfilehash: 5281049aa9ddec58a97283a2ca6ba5d229a49c4e
ms.sourcegitcommit: 038fe813c73804285d5e74d97864ac1a2fb531f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042602"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 的簡介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 使用 C++/WinRT，您可以撰寫及取用使用任何符合標準 C++17 編譯器的 Windows 執行階段 API。 Windows SDK 包含 C++/WinRT；其在版本 10.0.17134.0 (Windows 10，版本 1803 ) 中引進。

C + + /winrt 是 Microsoft 的建議替代方案，如[C + + /CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)語言投影，以及[Windows 執行階段 c + + 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)。 完整的清單[主題有關 C + + WinRT](index.md#topics-about-cwinrt)包含的資訊是有關同時互通，並從移植，C + + /CX 與 WRL。

> [!IMPORTANT]
> 兩個最重要的項目 C + WinRT 要注意的各節會說明[SDK 支援 C + + WinRT](#sdk-support-for-cwinrt)和[Visual Studio 支援 C + + WinRT、 XAML、 VSIX 延伸模組，以及 NuGet 套件](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="language-projections"></a>語言投影
Windows 執行階段根據元件物件模型 (COM) API，且設計它透過*語言投影*來存取。 投影隱藏 COM 的詳細資訊，並針對特定語言提供更自然的程式設計體驗。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 參考資料內容中的 C++/WinRT 語言投影
當您在瀏覽[Windows UWP Api](https://docs.microsoft.com/uwp/api/)，按一下右上方下拉式方塊中的 [**語言**]，然後選取**C++/WinRT**，當它們在 C++/WinRT 語言投影中顯示時，檢視 API 語法區塊。

## <a name="sdk-support-for-cwinrt"></a>適用於 C++/WinRT 的 SDK 支援
自版本 10.0.17134.0 (Windows 10，版本 1803) 開始，Windows SDK 包含標頭檔案式的標準 C++ 程式庫，以取用 Windows 命名空間中第一方 Windows API (Windows 執行階段 API)。 C++/WinRT 也隨附 `cppwinrt.exe` 工具，您可以用其在 Windows 執行階段中繼資料 (`.winmd`) 檔案指出，以產生標頭檔案式標準 C++ 程式庫，其會*投影*中繼資料中所述的 API，從 C++/WinRT 程式碼耗用。 Windows 執行階段中繼資料 (`.winmd`) 檔案，提供描述 Windows 執行階段 API 介面的正式方式。 透過在您可以產生程式庫的中繼資料指出 `cppwinrt.exe`，以在第二方或第三方 Windows 執行階段元件中搭配使用任何執行階段類別實作，或在您自己的應用程式中實作。 如需詳細資訊，請參閱 [使用 C++/WinRT 取用 API](consume-apis.md)。

透過使用 C++/WinRT，您也可以使用標準 C++ 實作自己的執行階段類別，而不用求助於 COM 樣式程式設計。 對於執行階段類別，您只要在 IDL 檔案中描述您的類型，而 `midl.exe` 與 `cppwinrt.exe` 會為您產生實作重複使用的原始程式碼檔案。 或者您可以只要實作衍生自 C++/WinRT 基底類別的介面。 如需詳細資訊，請參閱 [使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio 支援 C + + /winrt、 XAML、 VSIX 延伸模組，以及 NuGet 套件
Visual Studio 支援最小 Windows SDK 目標版本 10.0.17134.0 (Windows 10，版本 1803年)，除了您將需要 Visual Studio 2017 (至少版本 15.6; 我們建議至少 15.7)，或 Visual Studio 2019。 如果您在尚未安裝它，您將需要安裝 Visual Studio 安裝程式內從**c + + 通用 Windows 平台工具**選項。 和在 Windows**設定**中 > **更新 \& 安全性** > **適用於開發人員**，選擇 [**開發人員模式**] 選項，而不是 [**側載應用程式**] 選項。

您將需要下載並安裝最新版[C + + /winrt Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)從[Visual Studio Marketplace](https://marketplace.visualstudio.com/)。

- VSIX 擴充功能可讓您 C + + WinRT 專案範本和項目範本在 Visual Studio 中，以便您可以開始使用 C + + /winrt 開發。
- 此外，它會提供您 Visual Studio 原生偵錯視覺效果 (natvis) C + + /winrt 投影類型;提供與 C# 偵錯相似的體驗。 Natvis 會自動偵錯組建。 您可以透過定義符號 WINRT_NATVIS 選擇加入到發行組建。

Visual Studio 專案範本適用於 C + + /winrt 說明如下。 當您建立一個新的 C + + /winrt 專案與最新版本的 VSIX 延伸模組已安裝，新的 C + + /winrt 專案會自動安裝[Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 **Microsoft.Windows.CppWinRT** NuGet 套件提供 C + + WinRT 建置支援 （MSBuild 屬性及目標），讓您的專案可攜式開發電腦之間的組建代理程式 (在其上只 NuGet 套件，而非 VSIX 副檔名，已安裝）。

> [!IMPORTANT]
> 如果您有使用建立 （或升級至使用） 之專案 VSIX 延伸模組稍早的版本比 1.0.190128.4，然後查看[VSIX 擴充功能的較舊版本](#earlier-versions-of-the-vsix-extension)。 該節包含有關設定您的專案，您將需要知道他們使用 VSIX 擴充功能的最新版本升級的重要資訊。

因為 C + + WinRT 從 C + + 17 標準使用功能，它必須投影屬性**C/c + +** > **語言** > **標準 c + + 語言** > **ISO C + + 17 標準 (/ /std: c + + 17)**。 您可能還需設定**一致性模式：是 (/已獲授權-)**，其進一步限制您的程式碼才能標準相容。

另一個要注意的專案屬性是**C/C++** > **一般** > **警告為錯誤**。 將此設定為**Yes(/WX)** 或**No(/WX-)** 來品味。 有時候，`cppwinrt.exe` 工具產生的來源檔案，會產生警告，直到您將實作新增至其中。

使用您最多如上文所述設定的系統，您將能夠建立和建置，或開啟，C + + /winrt Visual Studio 中，在專案，並將它部署。

或者，您可以轉換現有的專案，藉由手動安裝**Microsoft.Windows.CppWinRT** NuGet 套件。 之後安裝 （或更新） VSIX 擴充功能的最新版本在 Visual Studio 中開啟現有的專案，按一下 [**專案**] \> **管理 NuGet 套件...** \> **瀏覽**，請輸入或貼上**Microsoft.Windows.CppWinRT** ，在搜尋方塊中，在搜尋結果中選取的項目，然後按一下 [安裝該專案的套件的**安裝**。 一旦您新增的套件，將會取得您 C + + /winrt MSBuild 支援專案中，包含叫用`cppwinrt.exe`工具。

您可以識別一個專案，使用 C + + /winrt MSBuild 支援**Microsoft.Windows.CppWinRT** NuGet 套件安裝在專案中的存在。

以下是 VSIX 擴充功能所提供的 Visual Studio 專案範本。

### <a name="windows-console-application-cwinrt"></a>Windows 主控台應用程式 (C++/WinRT)
適用於 Windows 電腦的 C++/WinRT 用戶端應用程式專案範本，具有主控台使用者介面。

### <a name="blank-app-cwinrt"></a>空白的應用程式 (C++/WinRT)
適用於擁有 XAML 使用者介面的通用 Windows 平台 (UWP) 的應用程式範本。

Visual Studio 提供 XAML 編譯器支援，並從位於每個 XAML 標記檔案後面的介面定義語言 (IDL) (`.idl`) 產生實作與標頭虛設常式。 IDL 檔案中，定義任何您想要在應用程式 XAML 網頁中參考的本機執行階段類別，然後見一次專案，在 `Generated Files` 中產生實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您的本機執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

Visual Studio XAML 設計表面支援 C + + /winrt 是接近搭配 C# 的同位。 一個例外是 [**事件**] 索引標籤的 [**屬性**] 視窗。 使用 C# 專案中，您可以使用該索引標籤，新增事件處理常式;使用 C + + /winrt 專案中，該功能不存在。 但請參閱[處理事件，藉由使用委派，在 C + + WinRT](handle-events.md)如需如何將事件處理常式新增到您的程式碼的資訊。

### <a name="core-app-cwinrt"></a>核心應用程式 (C++/WinRT)
適用於不使用 XAML 的通用 Windows 平台 (UWP) 的應用程式範本。

但是，它使用適用於 Windows.ApplicationModel.Core 命名空間的 C++/WinRT Windows 命名空間標頭。 在建置與執行後，請按一下空白空間新增彩色方格；然後再按一下彩色方格拖曳它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 執行階段元件 (C++/WinRT)
元件的專案範本；通常適用於通用 Windows 平台 (UWP) 的使用量。

此範本示範 `midl.exe` > `cppwinrt.exe` 工具鏈，其中從 IDL 產生 Windows 執行階段中繼資料 (`.winmd`)，然後從 Windows Runtime 中繼資料產生實作及標頭虛設常式。

IDL 檔案中，在您的元件、其預設的介面，以及任何其實作的其他介面中，定義執行階段類別。 建置一次專案產生 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您元件中的執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

將已建置的 Windows 執行階段元件二進位與其 `.winmd` 和使用它們的 UWP 應用程式搭配一起。

## <a name="earlier-versions-of-the-vsix-extension"></a>VSIX 擴充功能的較舊版本
我們建議您安裝 （或更新） [VSIX 擴充功能](https://aka.ms/cppwinrt/vsix)的最新版本。 它會自行更新預設設定。 如果這樣做，而且您有使用 VSIX 延伸模組比 1.0.190128.4，則本節稍早的版本所建立的專案包含這些專案以使用新版本的升級的重要資訊。 如果您沒有更新，然後您會仍然發現資訊在本節中非常有用。

數支援的 Windows SDK 與 Visual Studio 版本與 Visual Studio 設定中的資訊[Visual Studio 支援 C + + WinRT、 XAML、 VSIX 延伸模組，以及 NuGet 套件](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)一節適用於較舊版本的 VSIX擴充功能。 下方資訊描述的行為的重要差異，並設定的專案以建立 （或升級搭配） 稍早的版本。

### <a name="created-earlier-than-101810022"></a>比 1.0.181002.2 稍早建立
如果您的專案建立之版本的 VSIX 延伸模組早於 1.0.181002.2，則 C + + WinRT 建置支援已內建於該版本的 VSIX 延伸模組。 您的專案有`<CppWinRTEnabled>true</CppWinRTEnabled>`屬性中設定`.vcxproj`檔案。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

您可以藉由手動安裝**Microsoft.Windows.CppWinRT** NuGet 套件升級您的專案。 之後安裝 （或升級至） 最新版的 VSIX 延伸模組，在 Visual Studio 中開啟您的專案，按一下 [**專案**] \> **管理 NuGet 套件...** \> **瀏覽**，請輸入或貼上**Microsoft.Windows.CppWinRT** ，在搜尋方塊中，在搜尋結果中選取的項目，然後按一下 [安裝套件，為您的專案中**安裝**。

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>使用建立 （或升級至） 1.0.181002.2 和 1.0.190128.3 之間
如果使用 VSIX 延伸模組 1.0.181002.2 和 1.0.190128.3 之間的版本建立您的專案，（含），然後**Microsoft.Windows.CppWinRT** NuGet 套件已安裝在專案中自動專案範本所。 您可能也升級較舊的專案，這個範圍內使用 VSIX 延伸模組的版本。 如果您未，然後&mdash;建置支援以來也仍會出現在這個範圍內 VSIX 延伸模組的版本&mdash;可能會升級您的專案，或可能沒有**Microsoft.Windows.CppWinRT** NuGet 套件安裝。

若要升級您的專案，請依照上一節中的指示，並確保您的專案沒有**Microsoft.Windows.CppWinRT** NuGet 套件安裝。

### <a name="invalid-upgrade-configurations"></a>無效的升級設定
VSIX 擴充功能的最新版本，它不是有效值專案有`<CppWinRTEnabled>true</CppWinRTEnabled>`屬性，如果它也不會有安裝**Microsoft.Windows.CppWinRT** NuGet 套件。 使用此設定中的專案會產生組建錯誤訊息中，「 C + + /winrt VSIX 不再提供專案建置的支援。  請請將專案參考加入 Microsoft.Windows.CppWinRT Nuget 套件。 」

如前面所述，C + + /winrt 專案現在需要有在其中安裝 NuGet 套件。

因為`<CppWinRTEnabled>`項目現已過時，您可以選擇性地編輯您`.vcxproj`，並刪除項目。 它不是絕對必要，但它是一個選項。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自訂類型
在您 C + + /winrt 程式設計時，您可以使用標準 c + + 語言功能和[標準 c + + 資料類型與 C + + WinRT](std-cpp-data-types.md)&mdash;包括部分 c + + 標準程式庫資料類型。 但也您會注意到投影中的某些自訂資料類型，且您可以選擇使用它們。 例如，我們會使用[開始使用 C++/WinRT](get-started.md) 中快速程式碼範例的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

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
