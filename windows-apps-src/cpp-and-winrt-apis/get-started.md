---
author: stevewhims
description: 為了加快使用 C + + / WinRT，本主題逐步解說一個簡單的程式碼範例。
title: 開始使用 C++/WinRT
ms.author: stwhi
ms.date: 10/19/2018
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 取得, 取得, 開始
ms.localizationpriority: medium
ms.openlocfilehash: 6cb8e18904f61976103689c8d83475ec248eb38b
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2018
ms.locfileid: "6282533"
---
# <a name="get-started-with-cwinrt"></a>開始使用 C++/WinRT

若要取得您即可加快搭配使用[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，此主題將逐步解說一個簡單的程式碼範例，根據新**Windows 主控台應用程式 (C + + WinRT)** 專案。 本主題也示範如何[新增 C + + /winrt 支援 Windows 傳統型應用程式專案](#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

> [!IMPORTANT]
> 如果您使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，並且針對 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803年)，則新建立 C + + WinRT 專案可能會失敗來編譯錯誤 「*錯誤 C3861: 'from_abi': 識別碼不找到*」，並使用來自*base.h*其他錯誤。 解決方案是任一目標更新版本 （更多符合） 版本的 Windows SDK 中或設定專案屬性**C/c + +** > **語言** > **一致性模式： 否**(此外，如果 **/ 寬鬆-** 會出現在專案屬性**C/C++** > **語言** > **命令列**在**其他選項**，然後刪除它)。

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 快速入門

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

建立新的 **Windows 主控台應用程式 (C++/WinRT)** 專案。

編輯 `pch.h` 和 `main.cpp`，外觀如下。

```cppwinrt
// pch.h
...
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
...
```

```cppwinrt
// main.cpp
#include "pch.h"

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
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

我們來逐一檢視上述簡短的程式碼範例，並說明每個部分的情況。

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
```

包含的標頭  屬於 SDK 的一部分，可在資料夾  中找到 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`。 Visual Studio 在其*IncludePath*巨集裡包括該路徑。 標頭包含投影到 C++/WinRT 的 Windows API。 也就是說，C++/WinRT 針對每個 Windows 類型定義 C++- 適用的對等項目 (稱為*投影類型*)。 投影類型有相同的完整名稱做為 Windows 類型，但它放在 C++ **winrt** 命名空間。 將這些包含您預先編譯的標頭，會減少增量的建置時間。

> [!IMPORTANT]
> 每當您要使用 Windows 命名空間的類型，請包含對應的 C++/WinRT Windows 命名空間標頭檔案，如所示。 *對應*標頭會有和類型命名空間相同的名稱。 例如，使用適用於[**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset)執行階段類別`#include <winrt/Windows.Foundation.Collections.h>`的 C++/WinRT 投影。

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

`using namespace` 指示詞是選擇性的，但很便利。 上面針對此類指示詞所顯示的圖樣 (允許對 **winrt**命名空間中任何項目進行不完整名稱查詢) 適用於當您開始一個新投影，而 C++/WinRT 是您在使用的唯一語言投影，而不是那個投影。 也就是說，如果您混合 C++/WinRT 程式碼與 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和/或 SDK 應用程式二進位介面 (ABI) 的程式碼 (您正在從那些模型的其中一或兩個移植，或與其相互操作)，然後查看主題 [C++/WinRT 和 C++/CX 之間的互通性](interop-winrt-cx.md)、[從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md)，以及 [C++/WinRT 和 ABI 之間互通性](interop-winrt-abi.md)。

```cppwinrt
winrt::init_apartment();
```

對 **winrt::init_apartment** 的呼叫初始化 COM；預設是在多執行緒 Apartment 中。

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

堆疊配置兩個物件：它們代表 Windows 部落格的 uri，以及同步用戶端。 我們建構具簡單寬字串常值的 uri (請參閱 [在 C++/WinRT 中字串處理](strings.md) 以了解更多使用字串的方式)。

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) 是一個非同步 Windows 執行階段函式的範例。 程式碼範例從 **RetrieveFeedAsync** 接收一個非同步作業物件，並在該物件上呼叫**get**，以封鎖呼叫執行緒和等待結果 (在此情況中是同步發佈摘要)。 如需更多有關並行以及非封鎖技術，請參閱 [使用 C++/WinRT 的並行和非同步作業](concurrency.md)。

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items)是一個範圍，由從**begin**與**end**函式 (或其常數、反向，以及常數反向變體) 傳回的 iterators 定義。 因此，您可以使用以範圍為基礎的 `for` 陳述式，或使用 **std::for_each** 範本函式，列舉**Items**。

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

取得摘要的標題文字，做為 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)物件 (詳細在 [C++/WinRT 的字串處理](strings.md)中)。 然後，透過 **c_str** 函式輸出 **Hstring**，其反映搭配使用 C++ 標準程式庫字串的模式。

如您所見，C++/WinRT 鼓勵現代化、和類似類別、C++ 運算式例如 `syndicationItem.Title().Text()`。 這是與傳統 COM 程式設計不同，且更清楚的程式設計樣式。 您不需要直接初始化 COM，使用 COM 指標。

也不需要處理 HRESULT 傳回碼。 C++/WinRT 將錯誤 HRESULT 轉換為例外，例如 [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)，以擁有自然而現代化的程式設計樣式。 如需有關錯誤處理和程式碼範例的詳細資訊，請參閱[錯誤處理 C++/WinRT](error-handling.md)。

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>修改 Windows 傳統型應用程式專案新增 C + + /winrt 的支援

本節說明如何新增 C + + /winrt 支援您可能會有一個 Windows 傳統型應用程式專案。 如果您沒有現有的 Windows 傳統型應用程式專案，則您可以依照這些步驟所建立的第一個。 例如，開啟 Visual Studio 及建立**Visual c + +** \> **的 Windows 桌面** \> **Windows 傳統型應用程式**專案。

### <a name="set-project-properties"></a>設定專案屬性

前往專案屬性**一般** \> **Windows SDK 版本**，並選取**全部的設定**和**所有平台**。 請確定**Windows SDK 版本**設定為 10.0.17134.0 (Windows 10，版本 1803年) 或更高。

確認您在不影響所[為何要將無法我的新專案編譯？](/windows/uwp/cpp-and-winrt-apis/faq)。

因為 C + + WinRT 從 C + + 17 標準使用功能，將專案屬性**C/c + +** > **語言** > **標準 c + + 語言**來*ISO C + + 17 標準 (/ /std: c + + 17)*。

### <a name="the-precompiled-header"></a>先行編譯標頭

重新命名您`stdafx.h`和`stdafx.cpp`到`pch.h`和`pch.cpp`分別。 將專案屬性**C/c + +** > **先行編譯標頭** > 來*pch.h***先行編譯標頭檔案**。

尋找和取代所有`#include "stdafx.h"`與`#include "pch.h"`。

在`pch.h`，包括`winrt/base.h`。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>連結

C + + /winrt 語言投影取決於特定的 Windows 執行階段免費 （非成員） 函式和進入點，需要的連結到[WindowsApp.lib](/uwp/win32-and-com/win32-apis)傘程式庫。 本章節描述了三種方法可以滿足連結器。

第一個選項是將新增至您的 Visual Studio 專案所有的 C + + /winrt MSBuild 屬性和目標。 編輯您`.vcxproj`檔案、 尋找`<PropertyGroup Label="Globals">`和裡該屬性群組中，設定屬性`<CppWinRTEnabled>true</CppWinRTEnabled>`。

或者，您可以使用專案連結設定來明確地連結`WindowsApp.lib`。

或者，您可以在原始程式碼中執行它 (在`pch.h`，例如) 像這樣。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

您可以立即編譯和連結，並新增 C + + /winrt 程式碼到您的專案 (例如，在顯示的程式碼[A C + WinRT 快速入門](#a-cwinrt-quick-start)區段，上述)

## <a name="important-apis"></a>重要 API
* [Syndicationclient:: Retrievefeedasync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items 屬性](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [hresult-error 結構](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [使用 C++/WinRT 的錯誤處理](error-handling.md)
* [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md)
* [C++/WinRT 與 ABI 之間的互通性](interop-winrt-abi.md)
* [從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md)
* [C++/WinRT 中的字串處理](strings.md)
