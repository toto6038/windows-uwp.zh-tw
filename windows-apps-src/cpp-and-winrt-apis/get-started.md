---
description: 為了加快使用 C + + / WinRT，本主題逐步解說一個簡單的程式碼範例。
title: 開始使用 C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 取得, 取得, 開始
ms.localizationpriority: medium
ms.openlocfilehash: c058a727e09f00e01664c314d8c198f3f25e841e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "74255136"
---
# <a name="get-started-with-cwinrt"></a>開始使用 C++/WinRT

為了讓您快速上手 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，本主題會根據新的 **Windows 主控台應用程式 (C++/WinRT)** 專案，逐步解說簡單程式碼範例。 本主題也會示範如何[將 C++/WinRT 支援新增至 Windows 傳統型應用程式專案](#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

> [!NOTE]
> 雖然我們建議您使用最新版本的 Visual Studio 和 Windows SDK 來進行開發，但如果您使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，並將 Windows SDK 版本 10.0.17134.0 (Windows 10 版本 1803) 設定為目標，則新建立的 C++/WinRT 專案可能無法完成編譯，並出現「error C3861: 'from_abi': identifier not found」  錯誤，以及其他源自 *base.h* 的錯誤。 解決辦法是將 Windows SDK 的較新 (更一致) 版本設定為目標，或將專案屬性設定為 [C/C++]   > [語言]   > [一致性模式:  否] (此外，如果 **/permissive-** 顯示在 [其他選項]  底下的專案屬性 [C/C++]   > [語言]   > [命令列]  中，請加以刪除)。

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 快速入門

> [!NOTE]
> 如需安裝和為 C++/WinRT 開發設定 Visual Studio 的相關資訊&mdash;包括如何安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援)&mdash;，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

建立新的 **Windows 主控台應用程式 (C++/WinRT)** 專案。

編輯 `pch.h` 和 `main.cpp`，外觀如下。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
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
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

在使用預設專案設定時，所含的標頭會來自 Windows SDK 的資料夾 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 內。 Visual Studio 在其 IncludePath  巨集裡包括該路徑。 但其與 Windows SDK 沒有嚴格的相依關係，因為您的專案 (透過 `cppwinrt.exe` 工具) 會將相同的標頭產生到專案的 $(GeneratedFilesDir)  資料夾內。 如果在其他位置找不到這些標頭，或如果您變更了專案設定，就會從該資料夾來載入。

標頭包含投影到 C++/WinRT 的 Windows API。 也就是說，C++/WinRT 針對每個 Windows 類型定義 C++- 適用的對等項目 (稱為「投影類型」  )。 投影類型有相同的完整名稱做為 Windows 類型，但它放在 C++ **winrt** 命名空間。 在預先編譯的標頭中包含這些標頭，會減少增量的建置時間。

> [!IMPORTANT]
> 每當您要使用 Windows 命名空間的類型，請包含對應的 C++/WinRT Windows 命名空間標頭檔案，如上所示。 「對應」  標頭的名稱和類型命名空間的名稱相同。 例如，使用適用於 [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) 執行階段類別 `#include <winrt/Windows.Foundation.Collections.h>` 的 C++/WinRT 投影。 如果您包含 `winrt/Windows.Foundation.Collections.h`，則不必再  包含 `winrt/Windows.Foundation.h`。 每個 C++/WinRT 投影標頭自動包含其父系命名空間標頭檔案；讓您不需要  明確包含它。 不過，如果您這麼做，也不會有任何錯誤。

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

`using namespace` 指示詞是選擇性的，但很便利。 上述針對此類指示詞所顯示的模式 (允許對 **winrt** 命名空間中任何項目進行不完整名稱查詢) 適用於當您開始一個新投影，而 C++/WinRT 是在該投影中所使用的唯一語言投影。 也就是說，如果您混合 C++/WinRT 程式碼與 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和/或 SDK 應用程式二進位介面 (ABI) 的程式碼 (您正在從那些模型的其中一或兩個移植，或與其交互作業)，然後查看主題 [C++/WinRT 和 C++/CX 之間的互通性](interop-winrt-cx.md)、[從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md)，以及 [C++/WinRT 和 ABI 之間互通性](interop-winrt-abi.md)。

```cppwinrt
winrt::init_apartment();
```

對 **winrt::init_apartment** 的呼叫會初始化 Windows 執行階段中的執行緒；預設是在多執行緒 Apartment 中。 此呼叫也會初始化 COM。

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

堆疊配置兩個物件：它們代表 Windows 部落格的 URI，以及同步用戶端。 我們建構具簡單寬字串常值的 URI (請參閱 [C++/WinRT 中的字串處理](strings.md)以了解更多使用字串的方式)。

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) 是一個非同步 Windows 執行階段函式的範例。 程式碼範例從 **RetrieveFeedAsync** 接收一個非同步作業物件，並在該物件上呼叫 **get**，以封鎖呼叫執行緒和等待結果 (在此情況中是同步發佈摘要)。 如需更多有關並行以及非封鎖技術，請參閱[使用 C++/WinRT 的並行和非同步作業](concurrency.md)。

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) 是由 **begin** 與 **end** 函式 (或其常數、反向，以及常數反向變體) 所傳回的 iterator 來加以定義的範圍。 因此，您可以使用以範圍為基礎的 `for` 陳述式或使用 **std::for_each** 範本函式來列舉 **Items**。 每當您逐一查看如下所示的 Windows 執行階段集合，您將需要 `#include <winrt/Windows.Foundation.Collections.h>`。

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

以 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 物件的形式來取得摘要的標題文字 (如需詳細資訊，請參閱 [C++/WinRT 中的字串處理](strings.md))。 然後，透過 **c_str** 函式輸出 **Hstring**，其反映搭配使用 C++ 標準程式庫字串的模式。

如您所見，C++/WinRT 鼓勵現代化、和類似類別、C++ 運算式例如 `syndicationItem.Title().Text()`。 這是與傳統 COM 程式設計不同，且更清楚的程式設計樣式。 您不需要直接初始化 COM，也不需要使用 COM 指標。

也不需要處理 HRESULT 傳回碼。 C++/WinRT 將錯誤 HRESULT 轉換為例外，例如 [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)，以擁有自然而現代化的程式設計樣式。 如需有關錯誤處理和程式碼範例的詳細資訊，請參閱[錯誤處理 C++/WinRT](error-handling.md)。

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>修改 Windows 傳統型應用程式專案來新增 C++/WinRT 支援

本主題也會示範如何將 C++/WinRT 支援新增至您可能擁有的 Windows 傳統型應用程式專案。 如果您還未擁有 Windows 傳統型應用程式專案，則可以先建立一個來照著下列步驟操作。 例如，開啟 Visual Studio 並建立 [Visual C++]  \>[Windows 傳統型]  \>[Windows 傳統型應用程式]  專案。

您可以選擇性地安裝 [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 和 NuGet 套件。 如需詳細資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="set-project-properties"></a>設定專案屬性

移至專案屬性 [一般]  \>[Windows SDK 版本]  ，然後選取 [所有組態]  和 [所有平台]  。 請確認 [Windows SDK 版本]  已設為 10.0.17134.0 (Windows 10 版本 1803) 或更新版本。

確認您未受[我的新專案為什麼不會編譯？](/windows/uwp/cpp-and-winrt-apis/faq)影響。

因為 C++/ WinRT 使用來自 C++17 標準的功能，因此請將專案屬性 [C/C++]   > [語言]   > [語言標準]  設為 [ISO C++17 標準 (/std:c++17)]  。

### <a name="the-precompiled-header"></a>預先編譯的標頭

預設專案範本會為您建立名為 `framework.h` 或 `stdafx.h` 的預先編譯標頭。 請將該標頭重新命名為 `pch.h`。 如果您有 `stdafx.cpp` 檔案，則請將該檔案重新命名為 `pch.cpp`。 將專案屬性 [C/C++]   > [預先編譯的標頭]   > [預先編譯的標頭]  設為 [建立 (/Yc)]  ，並將 [預先編譯的標頭檔案]  設為 [pch.h]  。

尋找所有 `#include "framework.h"` (或 `#include "stdafx.h"`) 並使用 `#include "pch.h"` 加以取代。

在 `pch.h` 中包含 `winrt/base.h`。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>連結

C++/WinRT 語言投影取決於某些 Windows 執行階段的可用 (非成員) 函式，以及需要連結至 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 傘文件庫的進入點。 本節會說明符合連結器的三種方式。

第一個選項是在 Visual Studio 專案中新增所有 C++WinRT MSBuild 屬性和目標。 若要這麼做，請將 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 套件安裝到您的專案中。 在 Visual Studio 中開啟專案，按一下 [專案]  \>[管理 NuGet 套件...]  \>[瀏覽]  、在搜尋方塊中輸入或貼上 **Microsoft.Windows.CppWinRT**、在搜尋結果中選取項目，然後按一下 [安裝]  以安裝適用於該專案的套件。

您也可以使用專案連結設定來明確地連結 `WindowsApp.lib`。 或者，您也可以在原始程式碼中這麼做 (例如，在 `pch.h` 中)，如下所示。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

您現在可以編譯和連結，並將 C++/WinRT 程式碼新增至您的專案 (例如，類似上面 [C++/WinRT 快速入門](#a-cwinrt-quick-start)一節所示的程式碼)。

## <a name="the-three-main-scenarios-for-cwinrt"></a>C++/WinRT 的三個主要案例

當您使用並熟悉 C++/WinRT，以及詳細閱讀此處的文件時，您可能會注意到三個主要案例，如下列各節所述。

### <a name="consuming-windows-runtime-apis-and-types"></a>使用 Windows 執行階段 API 和類型

換句話說，「使用」  或「呼叫」  API。 例如，進行 API 呼叫以使用藍牙進行通訊、串流和呈現影片、與 Windows Shell 整合等等。 C++/WinRT 全然支援這類案例。 如需詳細資訊，請參閱[使用 C++/WinRT 取用 API](/windows/uwp/cpp-and-winrt-apis/consume-apis)。

### <a name="authoring-windows-runtime-apis-and-types"></a>撰寫 Windows 執行階段 API 和類型

換句話說，「產生」  API 和類型。 例如，產生上一節所述的 API 類型，或圖形 API、儲存體和檔案系統 API、網路 API 等等。 如需詳細資訊，請參閱[使用 C++/WinRT 撰寫 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

使用 C++/WinRT 撰寫 API 比使用它們更複雜一點，因為您必須先使用 IDL 來定義 API 的外形，才能加以實作。 [XAML 控制項；繫結至 C++/WinRT 屬性](/windows/uwp/cpp-and-winrt-apis/binding-property)會提供這麼做的逐步解說。

### <a name="xaml-applications"></a>XAML 應用程式

此案例是關於在 XAML UI 架構上建置應用程式和控制項。 在 XAML 應用程式中運作等同結合使用和撰寫。 但由於 XAML 是現今 Windows 上的主要 UI 架構，而其對 Windows 執行階段的影響成比例，因此應有自己的案例類別。

請注意，XAML 最適合用於提供反映的程式設計語言。 在 C++/WinRT 中，您有時必須執行一些額外的工作，才能與 XAML 架構相交互操作。 這些案例全都涵蓋在本文件中。 建議從 [XAML 控制項；繫結至 C++/WinRT 屬性](/windows/uwp/cpp-and-winrt-apis/binding-property)和[使用 C++/WinRT 的 XAML 自訂 (範本化) 控制項](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl)著手。

## <a name="important-apis"></a>重要 API
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items 屬性](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult-error 結構](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [使用 C++/WinRT 處理錯誤](error-handling.md)
* [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md)
* [C++/WinRT 與 ABI 之間的互通性](interop-winrt-abi.md)
* [從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md)
* [C++/WinRT 中的字串處理](strings.md)
