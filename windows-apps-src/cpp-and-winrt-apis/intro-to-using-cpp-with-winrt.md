---
author: stevewhims
description: C++/ WinRT&mdash;Windows 執行階段 API 的標準 C++ 語言投影的簡介。
title: C++/WinRT 的簡介
ms.author: stwhi
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影
ms.localizationpriority: medium
ms.openlocfilehash: 968afd6fdad1e7bf6b3c38d929ab79eefa71819a
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832322"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 的簡介
> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

版本 10.0.17134.0 (Windows 10，版本 1803 ) 中引進，現在 Windows SDK 包含 C++/WinRT。

> [!IMPORTANT]
> 要注意最重要的 C++/WinRT 項目，在本節中會說明[C++/WinRT 的 SDK 支援](#sdk-support-for-cwinrt) 和 [C++/WinRT 的支援，以及 VSIX](#visual-studio-support-for-cwinrt-and-the-vsix)。

C++/WinRT 是完全標準現代 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅在標頭檔案中實作，以及設計用來提供您現代化 Windows API 的第一級存取。 使用 C++/WinRT，您可以撰寫及取用使用任何符合標準 C++17 編譯器的 Windows 執行階段 API。

## <a name="language-projections"></a>語言投影
Windows 執行階段根據元件物件模型 (COM) API，且設計它透過*語言投影*來存取。 投影隱藏 COM 的詳細資訊，並針對特定語言提供更自然的程式設計體驗。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 參考資料內容中的 C++/WinRT 語言投影
當您在瀏覽[Windows UWP Api](https://docs.microsoft.com/uwp/api/)，按一下右上方下拉式方塊中的 [**語言**]，然後選取**C++/WinRT**，當它們在 C++/WinRT 語言投影中顯示時，檢視 API 語法區塊。

## <a name="sdk-support-for-cwinrt"></a>適用於 C++/WinRT 的 SDK 支援
從版本 10.0.17134.0 (Windows 10，版本 1803) 開始，Windows SDK 包含 C++/WinRT 投影 Windows 命名空間標頭和工具。 一個重要的工具為 `cppwinrt.exe`其從一個 `.winmd`，產生將中繼資料投影到 C++/WinRT 的原始碼檔案。 Windows 執行階段中繼資料 (`.winmd`) 檔案，提供描述 Windows 執行階段 API 介面的正式方式。 在 Windows 執行階段中繼資料中，`cppwinrt.exe`產生完整描述&mdash;或*投影*&mdash;API 介面的標準 C++ 程式庫；不論是否為 Windows API 或第三方 Windows 執行階段元件 API。 `Cppwinrt.exe` 在您使用第一和第三方 API 以及撰寫您自己的 API 元件的開發工作流程中，扮演很重要的角色。

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>C++/WinRT 的 Visual Studio 支援，以及 VSIX
適用於 Visual Studio 中的 C++/WinRT 專案範本，以及 C++/WinRT MSBuild 屬性及目標，從 [Visual Studio Marketplace](https://marketplace.visualstudio.com/) 下載並安裝 [C + + / WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)。

您會需要 Visual Studio 2017 版本 15.6，或更新版本，以及 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803)。 然後您可以在 Visual Studio 中建立新專案，或您可以藉由將 `<CppWinRTProject>true</CppWinRTProject>`屬性新增至其專案 > PropertyGroup 中的  `.vcxproj` 檔案來轉換現有的專案。 一旦您新增該屬性，會收到適用於專案的 C++/WinRT MSBuild 支援，包含叫用 `cppwinrt.exe` 工具。

因為 C++/ WinRT 從 C++17 標準使用功能，它必須投影屬性**C/C++** > **語言** > **ISO C++17 標準 (/std: c++17)**。 您可能還需設定**一致性模式：是 (/已獲授權-)**，其進一步限制您的程式碼才能標準相容。

另一個要注意的專案屬性是**C/C++** > **一般** > **警告為錯誤**。 將此設定為**Yes(/WX)** 或**No(/WX-)** 來品味。 有時候，`cppwinrt.exe`工具產生的來源檔案，會產生警告，直到您將實作新增至其中。

這些是 VSIX 所提供的 Visual Studio 專案範本。

### <a name="windows-console-application-cwinrt"></a>Windows 主控台應用程式 (C++/WinRT)
適用於 Windows 電腦的 C++/WinRT 用戶端應用程式專案範本，具有主控台使用者介面。

### <a name="blank-app-cwinrt"></a>空白的應用程式 (C++/WinRT)
適用於擁有 XAML 使用者介面的通用 Windows 平台 (UWP) 的應用程式範本。

Visual Studio 提供 XAML 編譯器支援，並從位於每個 XAML 標記檔案後面的介面定義語言 (IDL) (`.idl`) 產生實作與標頭虛設常式。 IDL 檔案中，定義任何您想要在應用程式 XAML 網頁中參考的本機執行階段類別，然後見一次專案，在 `Generated Files` 中產生實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您的本機執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

### <a name="core-app-cwinrt"></a>核心應用程式 (C++/WinRT)
適用於不使用 XAML 的通用 Windows 平台 (UWP) 的應用程式範本。

但是，它使用適用於 Windows.ApplicationModel.Core 命名空間的 C++/WinRT 投影 Windows 命名空間標頭。 在建置與執行後，請按一下空白空間新增彩色方格；然後再按一下彩色方格拖曳它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 執行階段元件 (C++/WinRT)
元件的專案範本；通常適用於通用 Windows 平台 (UWP) 的使用量。

此範本示範 `midl.exe` > `cppwinrt.exe` 工具鏈，其中從 IDL 產生 Windows 執行階段中繼資料 (`.winmd`)，然後從 Windows Runtime 中繼資料產生實作及標頭虛設常式。

IDL 檔案中，在您的元件、其預設的介面，以及任何其實作的其他介面中，定義執行階段類別。 建置一次專案產生 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的實作範本，以及 `Generated Files\sources` 中的虛設常式類型定義。 然後使用這些適用於參考資料的虛設類型定義，實作您元件中的執行階段類別。 我們建議您在其自身的 IDL 檔案中，宣告每個執行階段類別。

將已建置的 Windows 執行階段元件二進位與其 `.winmd` 和使用它們的 UWP 應用程式搭配一起。

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 快速入門
建立新的 **Windows 主控台應用程式 (C++/WinRT)** 專案。 編輯 `main.cpp` 外觀如下。

```cppwinrt
// main.cpp

#include "pch.h"
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

已包含的標頭 `winrt/Windows.Foundation.h` 與 `winrt/Windows.Web.Syndication.h` 皆在資料夾 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 中的 SDK 裡。 Visual Studio 在其*IncludePath*巨集裡包括該路徑。 標頭包含投影到 C++/WinRT 的 Windows API。 當您要使用從 Windows 命名空間投影到 C++/WinRT，包含對應的 C++/WinRT 投影 Windows 命名空間標頭如下所示。 `using namespace`的指示詞是選擇性的，但很便利。

所有投影的類型皆位於 C++/WinRT 根命名空間**winrt**。 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)以及根命名空間 **Windows** 的 Windows SDK 宣告類型。 這些不同的命名空間，可讓您以自己的速度從 C++/CX 移轉至 C++/WinRT。

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)是一個非同步 Windows 執行階段函式的範例。 程式碼範例從 **RetrieveFeedAsync** 接收一個非同步作業物件，並在該物件上呼叫**get**，封鎖呼叫執行緒且等待結果。 如需更多有關並行以及非封鎖技術，請參閱 [使用 C++/WinRT 的並行和非同步作業](concurrency.md)。

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items)是一個範圍，由從**begin**與**end**函式 (或其常數、反向，以及常數反向變體) 傳回的 iterators 定義。 因此，您可以使用以範圍為基礎的 `for` 陳述式，或使用 **std::for_each** 範本函式，列舉**Items**。

然後程式碼取得摘要的標題文字，做為 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)物件 (請參閱 [C++/WinRT 的字串處理](strings.md))。 然後 **Hstring** 透過 **c_str** 輸出，這對您來說看起來很熟悉，如果您已經從 C++ 標準程式庫使用字串。

如您所見，C++/WinRT 鼓勵現代化、和類似類別、C++ 運算式例如 `syndicationItem.Title().Text()`。 這是與傳統 COM 程式設計不同，且更清楚的程式設計樣式。 您不需要明確初始化 COM (**winrt::init_apartment** 會為您執行)、使用 COM 指標，也不需要處理 HRESULT 傳回程式碼。 C++/WinRT 將錯誤 HRESULT 轉換至自然且現代化程式設計樣式的例外。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自訂類型
您可以使用標準 C++ 語言功能和 [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)&mdash;包括在您 C++/WinRT 程式設計中的部分 C++ 標準程式庫資料類型&mdash;。 但也您會注意到投影中的某些自訂資料類型，且您可以選擇使用它們。 例如，我們在上方的快速入門中使用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array)是您在某些時候可能會使用的另一種類型。 但是您比較不會直接使用例如[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view)的類型。 或者，您可以選擇不想使用它，如果且當 C++ 標準程式庫中出現對等項目類型時，才不會變更任何程式碼。

如果您仔細研究 C++/WinRT 投影 Windows 命名空間標頭，您也會看到這類類型。 有一個範例是**winrt::param::hstring**。 這些只為了效率而存在，且您不應該在程式碼中使用它們。

## <a name="important-apis"></a>重要 API
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [Winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT 中的字串處理](strings.md)
* [Visual Studio Marketplace](https://marketplace.visualstudio.com/)
* [Windows UWP APIs](https://docs.microsoft.com/uwp/api/)
