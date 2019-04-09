---
description: C++/ WinRT&mdash;Windows 執行階段 API 的標準 C++ 語言投影的簡介。
title: C++/WinRT 的簡介
ms.date: 04/02/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, introduction, 標準, 投影, 撰寫, 事件, 簡介
ms.localizationpriority: medium
ms.openlocfilehash: e9e84370f0503a8f361df9b43b60a2870be745a3
ms.sourcegitcommit: 940645c705865ba9635ccae2da9d917420faf608
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2019
ms.locfileid: "58812597"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 的簡介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 使用 C++/WinRT，您可以撰寫及取用使用任何符合標準 C++17 編譯器的 Windows 執行階段 API。 Windows SDK 包含 C++/WinRT；其在版本 10.0.17134.0 (Windows 10，版本 1803 ) 中引進。

C++/ WinRT 是 Microsoft 的建議的替代[ C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)語言推演，而[Windows 執行階段C++範本程式庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)。 完整清單[有關的主題C++/WinRT](index.md#topics-about-cwinrt)有關交互操作，與從，移植的資訊C++/CX 及 WRL。

> [!IMPORTANT]
> 一些最重要的部分的C++/WinRT 要注意的幾節所述[SDK 支援C++/WinRT](#sdk-support-for-cwinrt)並[Visual Studio 支援C++/WinRT、 XAML、 VSIX 擴充功能和 NuGet封裝](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="language-projections"></a>語言投影
Windows 執行階段根據元件物件模型 (COM) API，且設計它透過*語言投影*來存取。 投影隱藏 COM 的詳細資訊，並針對特定語言提供更自然的程式設計體驗。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 參考資料內容中的 C++/WinRT 語言投影
當您在瀏覽[Windows UWP Api](https://docs.microsoft.com/uwp/api/)，按一下右上方下拉式方塊中的 [**語言**]，然後選取**C++/WinRT**，當它們在 C++/WinRT 語言投影中顯示時，檢視 API 語法區塊。

## <a name="sdk-support-for-cwinrt"></a>適用於 C++/WinRT 的 SDK 支援
自版本 10.0.17134.0 (Windows 10，版本 1803) 開始，Windows SDK 包含標頭檔案式的標準 C++ 程式庫，以取用 Windows 命名空間中第一方 Windows API (Windows 執行階段 API)。

相容性，Windows SDK 也隨附`cppwinrt.exe`工具。 不過，我們建議您改為安裝並使用最新版本`cppwinrt.exe`，這是隨附**Microsoft.Windows.CppWinRT** NuGet 套件。 該套件，以及`cppwinrt.exe`下, 一節所述。

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio 支援 C + /cli WinRT、 XAML，VSIX 擴充功能，以及 NuGet 封裝
如需 Visual Studio 支援，除了最小的 Windows SDK 目標版本的 10.0.17134.0 (Windows 10 1803年版)，您將需要 Visual Studio 2019 或 Visual Studio 2017 (至少版本 15.6; 我們建議至少 15.7)。 如果您尚未安裝它，您必須安裝**C++通用 Windows 平台工具**在 Visual Studio 安裝程式選項。 與中 Windows**設定** > **更新\&安全性** > **適用於開發人員**，選擇**開發人員模式**選項而非**側載應用程式**選項。

您必須下載並安裝最新版[ C++WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)從[Visual Studio Marketplace](https://marketplace.visualstudio.com/)。

- VSIX 擴充功能可讓您C++/WinRT 專案和項目範本，在 Visual Studio 中，以便您可以開始使用C++/WinRT 開發。
- 此外，它可讓您 Visual Studio 原生偵錯視覺效果 (.natvis) 的C++/WinRT 投射類型;提供的體驗類似於C#偵錯。 Natvis 會自動偵錯組建。 您可以透過定義符號 WINRT_NATVIS 選擇加入到發行組建。

Visual Studio 專案範本，C + /cli WinRT 如下所述。 當您建立新C++安裝的 VSIX 延伸模組的最新版本的 /WinRT 專案，新C++/WinRT 專案會自動安裝[Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 **Microsoft.Windows.CppWinRT** NuGet 套件提供C++/WinRT 建置支援 （MSBuild 屬性和目標），使您的專案之間的開發電腦和組建代理程式可攜 (在其上只有 NuGet套件，以及不 VSIX 擴充功能，已安裝）。

> [!IMPORTANT]
> 如果您有與建立 （或升級為使用） 的專案 VSIX 擴充功能稍早的版本比 1.0.190128.4，然後看到[VSIX 擴充功能的舊版](#earlier-versions-of-the-vsix-extension)。 該區段包含您的專案，您必須知道要將它們升級為使用最新版的 VSIX 擴充功能組態的相關重要資訊。

> [!NOTE]
> 因為C++/WinRT 使用 C + + 17 標準功能，NuGet 套件設定專案屬性**C /C++** > **語言** >   **C++語言標準** > **ISO c++17 標準 (/ /std: c + + 17)** Visual Studio 中。 它也會新增[/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file)編譯器選項。
> 
> 您也可以在設定**一致性模式：[是] (/permissive--)**，它進一步限制為符合標準的程式碼。 另一個要注意的專案屬性是**C/C++** > **一般** > **警告為錯誤**。 將此設定為**Yes(/WX)** 或**No(/WX-)** 來品味。 有時候，`cppwinrt.exe` 工具產生的來源檔案，會產生警告，直到您將實作新增至其中。

您與您最多如上面所述設定的系統，是能夠建立和建置，或開啟時， C++/WinRT 專案在 Visual Studio 中，並將它部署。

或者，您可以將現有的專案轉換藉由手動安裝**Microsoft.Windows.CppWinRT** NuGet 套件。 之後安裝 （或更新） 最新版的 VSIX 擴充功能，在 Visual Studio 中開啟現有的專案，請按一下**專案** \> **管理 NuGet 套件...**\> **瀏覽**中，輸入或貼上**Microsoft.Windows.CppWinRT**在 [搜尋] 方塊中，選取項目在搜尋結果中，然後按一下**安裝**安裝適用於該專案的套件。 一旦您已新增封裝，您將取得C++專案，包括叫用的 WinRT MSBuild 支援`cppwinrt.exe`工具。 自 2.0 版起**Microsoft.Windows.CppWinRT** NuGet 套件包含`cppwinrt.exe`工具。

您可以指向`cppwinrt.exe`工具在 Windows 執行階段中繼資料 (`.winmd`) 來產生標頭檔案為基礎的標準檔案C++程式庫，*專案*從耗用量的中繼資料中所述的 Api C++/ WinRT 程式碼。 Windows 執行階段中繼資料 (`.winmd`) 檔案，提供描述 Windows 執行階段 API 介面的正式方式。 透過在您可以產生程式庫的中繼資料指出 `cppwinrt.exe`，以在第二方或第三方 Windows 執行階段元件中搭配使用任何執行階段類別實作，或在您自己的應用程式中實作。 如需詳細資訊，請參閱 [使用 C++/WinRT 取用 API](consume-apis.md)。

透過使用 C++/WinRT，您也可以使用標準 C++ 實作自己的執行階段類別，而不用求助於 COM 樣式程式設計。 對於執行階段類別，您只要在 IDL 檔案中描述您的類型，而 `midl.exe` 與 `cppwinrt.exe` 會為您產生實作重複使用的原始程式碼檔案。 或者您可以只要實作衍生自 C++/WinRT 基底類別的介面。 如需詳細資訊，請參閱 [使用 C++/WinRT 撰寫 API](author-apis.md)。

您可以識別專案使用C++的存在 WinRT MSBuild 的支援**Microsoft.Windows.CppWinRT**專案中安裝的 NuGet 套件。

以下是 VSIX 擴充功能所提供的 Visual Studio 專案範本。

### <a name="windows-console-application-cwinrt"></a>Windows 主控台應用程式 (C++/WinRT)
適用於 Windows 電腦的 C++/WinRT 用戶端應用程式專案範本，具有主控台使用者介面。

### <a name="blank-app-cwinrt"></a>空白的應用程式 (C++/WinRT)
適用於擁有 XAML 使用者介面的通用 Windows 平台 (UWP) 的應用程式範本。

Visual Studio 提供 XAML 編譯器支援，並從位於每個 XAML 標記檔案後面的介面定義語言 (IDL) (`.idl`) 產生實作與標頭虛設常式。 IDL 檔案中，定義任何您想要在應用程式 XAML 網頁中參考的本機執行階段類別，然後見一次專案，在 `Generated Files` 中產生實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您的本機執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

Visual Studio 的 XAML 設計介面支援C++/WinRT 即將使用的同位檢查C#。 唯一例外的是**事件**索引標籤**屬性**視窗。 使用C#專案中，您可以使用該索引標籤上新增事件處理常式;使用C++/WinRT 專案、 設備不存在。 但請參閱[使用中的委派處理事件C++/WinRT](handle-events.md)如需如何將事件處理常式新增至您的程式碼的資訊。

### <a name="core-app-cwinrt"></a>核心應用程式 (C++/WinRT)
適用於不使用 XAML 的通用 Windows 平台 (UWP) 的應用程式範本。

但是，它使用適用於 Windows.ApplicationModel.Core 命名空間的 C++/WinRT Windows 命名空間標頭。 在建置與執行後，請按一下空白空間新增彩色方格；然後再按一下彩色方格拖曳它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 執行階段元件 (C++/WinRT)
元件的專案範本；通常適用於通用 Windows 平台 (UWP) 的使用量。

此範本示範 `midl.exe` > `cppwinrt.exe` 工具鏈，其中從 IDL 產生 Windows 執行階段中繼資料 (`.winmd`)，然後從 Windows Runtime 中繼資料產生實作及標頭虛設常式。

IDL 檔案中，在您的元件、其預設的介面，以及任何其實作的其他介面中，定義執行階段類別。 建置一次專案產生 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您元件中的執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

將已建置的 Windows 執行階段元件二進位與其 `.winmd` 和使用它們的 UWP 應用程式搭配一起。

## <a name="earlier-versions-of-the-vsix-extension"></a>舊版的 VSIX 擴充功能
我們建議您安裝 （或更新） 的最新版本[VSIX 擴充功能](https://aka.ms/cppwinrt/vsix)。 它會設定為預設情況下更新自己。 如果這麼做，而且您使用的版本早於 1.0.190128.4，則此區段的 VSIX 擴充功能所建立的專案包含升級這些專案，以使用新版本的重要資訊。 如果您未更新，然後您仍然覺得資訊這一節很有用。

形式支援 Windows SDK 和 Visual Studio 版本，以及 Visual Studio 的組態中的資訊[Visual Studio 支援 C + /cli WinRT、 XAML，VSIX 擴充功能，以及 NuGet 封裝](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)上一節適用於先前VSIX 擴充功能的版本。 下列資訊描述的行為的重要差異和組態的專案建立 （或升級為使用） 稍早的版本。

### <a name="created-earlier-than-101810022"></a>早於 1.0.181002.2 建立
如果您的專案以版本建立的 VSIX 擴充功能之前 1.0.181002.2，然後C++/WinRT 建置支援內建在該版本的 VSIX 擴充功能。 您的專案具有`<CppWinRTEnabled>true</CppWinRTEnabled>`屬性中設定`.vcxproj`檔案。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

您可以藉由手動安裝來升級您的專案**Microsoft.Windows.CppWinRT** NuGet 套件。 之後安裝 （或升級至） 最新版的 VSIX 擴充功能，在 Visual Studio 中開啟您的專案，請按一下**專案** \> **管理 NuGet 套件...**\> **瀏覽**中，輸入或貼上**Microsoft.Windows.CppWinRT**在 [搜尋] 方塊中，選取項目在搜尋結果中，然後按一下**安裝**安裝套件，為您的專案。

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>使用建立 （或升級至） 1.0.181002.2 和 1.0.190128.3 之間
如果您專案已建立版本的 VSIX 擴充功能之間 1.0.181002.2 和 1.0.190128.3，內含，則**Microsoft.Windows.CppWinRT**專案會自動由專案中已安裝 NuGet 套件範本。 您可能會一併升級舊的專案，以用於此範圍中的 VSIX 擴充功能的版本。 如果您這樣做，然後&mdash;因為建置支援也仍會出現在 VSIX 擴充功能，在此範圍中的新版&mdash;升級的專案可能會或可能沒有**Microsoft.Windows.CppWinRT** NuGet 套件安裝。

若要升級您的專案，遵循上一節中的指示，並確保您的專案沒有**Microsoft.Windows.CppWinRT**安裝的 NuGet 套件。

### <a name="invalid-upgrade-configurations"></a>無效的升級設定
使用 VSIX 擴充功能的最新版本，就不適用的專案已`<CppWinRTEnabled>true</CppWinRTEnabled>`屬性，如果也沒有**Microsoft.Windows.CppWinRT**安裝的 NuGet 套件。 此組態的專案會產生組建錯誤訊息: 「 C++WinRT VSIX 不再提供專案的建置支援。  請新增 Microsoft.Windows.CppWinRT Nuget 套件的專案參考。 」

如上所述， C++/WinRT 專案現在必須已安裝的 NuGet 套件。

由於`<CppWinRTEnabled>`項目現在已過時，您可以選擇性地編輯您`.vcxproj`，和刪除項目。 它不是絕對必要，但這是一個選項。

此外，如果您`.vcxproj`包含`<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`，然後您可以將它移除，因此您可以建置而不需要C++安裝的 WinRT VSIX 擴充功能。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自訂類型
在您C++/WinRT 程式設計，您可以使用標準C++語言功能以及[標準C++資料類型和C++/WinRT](std-cpp-data-types.md)&mdash;包括一些C++標準程式庫的資料類型。 但也您會注意到投影中的某些自訂資料類型，且您可以選擇使用它們。 例如，我們會使用[開始使用 C++/WinRT](get-started.md) 中快速程式碼範例的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array** ](/uwp/cpp-ref-for-winrt/com-array)很可能會在某個時間點所使用的另一種類型。 但是您比較不會直接使用例如[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view)的類型。 或者，您可以選擇不想使用它，如果且當 C++ 標準程式庫中出現對等項目類型時，才不會變更任何程式碼。

> [!WARNING]
> 如果您仔細研究 C++/WinRT Windows 命名空間標頭，您也會看到這類類型。 一個範例是 **winrt::param::hstring**，但有也會收集範例。 這些會存在只是要最佳化輸入參數的繫結，並且它們會產生大幅的效能提升，讓大部分呼叫模式「只適用於」相關的標準 C++ 類型和容器。 這些類型僅由投影在新增大多數值時使用。 它們會高度最佳化，它們不用於一般用途，切勿冒險自行使用它們。 您也不應該使用 `winrt::impl` 命名空間的任何項目，因為這些是實作類型，因此會隨時變更。 您應該繼續使用標準類型，或來自 [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)的類型。

## <a name="important-apis"></a>重要 API
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/ WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)
* [開始使用 C++/WinRT](get-started.md)
* [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)
* [字串處理 C + /cli WinRT](strings.md)
* [Windows UWP Api](https://docs.microsoft.com/uwp/api/)
