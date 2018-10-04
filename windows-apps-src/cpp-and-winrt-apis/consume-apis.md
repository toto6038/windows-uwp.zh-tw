---
author: stevewhims
description: 本主題示範如何使用 C++/WinRT API，無論 Windows、第三方元件廠商或您自己是否實作它們。
title: '使用 C++/WinRT 使用 API '
ms.author: stwhi
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影的、投影、實作、執行階段類別、啟用
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cd05f974bf9193e84919a5e679ef996746d7e
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "4357298"
---
# <a name="consume-apis-with-cwinrt"></a>使用 C++/WinRT 來使用 API

本主題示範如何使用[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Api，無論他們是否 Windows 的一部分，實作由第三方元件廠商或您自己來實作。

## <a name="if-the-api-is-in-a-windows-namespace"></a>如果 API 在 Windows 命名空間中
這是您使用 Windows 執行階段 API 最常見的案例。 適用於中繼資料中定義的 Windows 命名空間裡每一類型，C++/WinRT 定義 C++ 適用的對等項目 (稱為 *投影類型*)。 投影類型有相同的完整名稱做為 Windows 類型，但它放在使用 C++ syntax 的 C++ **winrt** 命名空間。 例如，將 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 投影到 C++/WinRT 做為 **winrt::Windows::Foundation::Uri**。

以下是一個簡單的程式碼範例。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
    Uri combinedUri = contosoUri.CombineUri(L"products");
}
```

包含的標頭 `winrt/Windows.Foundation.h` 屬於 SDK 的一部分，可在資料夾 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 中找到。 資料夾中的標頭包含投影到 C++/WinRT 的 Windows 命名空間類型。 在此範例中，`winrt/Windows.Foundation.h` 包含 **winrt::Windows::Foundation::Uri**，其適用於執行階段類別 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 的投影類型。

> [!TIP]
> 每當您要使用從 Windows 命名空間投影到 C++/WinRT，請包含對應到該命名空間的 C++/WinRT 標頭。 `using namespace` 的指示詞是選擇性的，但很便利。

上述的程式碼範例中，初始化 C++/WinRT 後，我們透過其公開記載於文件的建構函式之一 (在此範例中，[**Uri(String)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_)) 堆疊配置 **winrt::Windows::Foundation::Uri** 的值投影類型。 針對這點，最常見的使用案例就是您通常需要執行的。 一旦您取得 C++/WinRT 投影類型值，您可以將其視為如同實際 Windows 執行階段類型的執行個體，因為其有相同的成員。

事實上，該投影的值是 proxy；它基本上只是支援物件的智慧型指標。 投影值的建構函式呼叫 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) 來建立支援 Windows 執行階段類別的執行個體 (此案例中為，**Windows.Foundation.Uri**)，並在新投影值中儲存該物件的預設介面。 如下所示，您投影值成員的呼叫透過智慧型指標確實委派至支援物件；其為狀態發生變更的位置。

![投影 Windows::Foundation::Uri 類型](images/uri.png)

當 `contosoUri` 值超出範圍時，會解構並將其參考資料發行至預設介面。 如果該參考是支援 Windows 執行階段 **Windows.Foundation.Uri** 物件的最後一個參考，則支援物件也會解構。

> [!TIP]
> *投影類型*是以使用其 API 為目的透過執行階段類別的包裝程式。 *投影介面*是透過 Windows 執行階段介面的包裝程式。

## <a name="cwinrt-projection-headers"></a>C++/WinRT 投影標頭
若要從 C++/WinRT 使用 Windows 命名空間 API，請您包含 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 資料夾的標頭。 附屬命名空間中的類型通常在其直接父系命名空間中參考類型。 因此，每個 C++/WinRT 投影標頭自動包含其父系命名空間標頭檔案；讓您不 *需要* 明確包含它。 不過，如果您這麼做，也不會有任何錯誤。

例如，針對 [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates) 命名空間，對等項目 C++/WinRT 類型定義位於 `winrt/Windows.Security.Cryptography.Certificates.h`。 **Windows::Security::Cryptography::Certificates** 中的類型需要父系 **Windows::Security::Cryptography** 命名空間中的類型；且命名空間中的類型可能需要其本身父系中的類型，**Windows::Security**。

因此，當您包含 `winrt/Windows.Security.Cryptography.Certificates.h`，該檔案依序包含 `winrt/Windows.Security.Cryptography.h`；以及 `winrt/Windows.Security.Cryptography.h` 包含 `winrt/Windows.Security.h`。 便是記錄停止的位置，因為沒有 `winrt/Windows.h`。 此轉移包含程序會在第二層級命名空間停止。

此程序間接包括提供在父系命名空間裡定義的必要類別*宣告*和*實作*標頭檔案。

在一個命名空間中的類型成員可以參考在其他、無關的命名空間中一個或多個類型。 為了讓編譯器成功編譯這些成員定義，需要看到適用於所有這些類型關閉的類型宣告。 因此，每個 C++/WinRT 投影標頭包含需要*宣告*任何相關類型的命名空間標頭。 與父系命名空間不同，此程序*未*引進參考類型的*實作*。

> [!IMPORTANT]
> 當您想要實際*使用*在不相關的命名空間裡宣告的類型（初始化、呼叫方法等）時，您必須包含該類型適當的命名空間標頭檔案。 僅自動包含*宣告*，而無*實作*。

例如，如果您只包含 `winrt/Windows.Security.Cryptography.Certificates.h`，則會導致要從這些命名空間（等項目，轉移性的）引進宣告。

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

亦即，在您包含的標頭中轉送宣告一些 API。 但其定義在您未包含的標頭裡。 因此，如果您接著呼叫 [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri)，則您會收到連結器錯誤，指出成員未定義。 解決方案是明確地 `#include <winrt/Windows.Foundation.h>`。 一般而言，當您看到如下的連結器錯誤時，請包含為 API 命名空間命名的標頭並重建。

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>透過物件、或透過介面，或透過 ABI 存取成員
使用 C++/WinRT 投影，Windows 執行階段類別的執行階段呈現方式是不超過基礎 ABI 介面。 不過，為了方便使用，您可以按照作者希望的方式對類別進行編碼。 例如，您可以呼叫 [**Uri**](/uwp/api/windows.foundation.uri) 的 **ToString** 方法，就像類別的方法一樣（事實上，背後的運作方式，這是一種在獨立 **IStringable** 介面上的方法）。

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

透過查詢適當的介面達成此便利。 但是，您隨時可以控制。 您可以透過自我擷取 IStringable 介面並直接使用，來選擇放棄一點方便以獲得一點效能。 在下列程式碼範例中，您可以在執行階段取得實際 IStringable 介面指標（透過一次性查詢）。 之後，您便直接呼叫 **ToString**，並避免任何進一步呼叫 **QueryInterface**。

```cppwinrt
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

如果您知道會在相同的介面呼叫幾種方法，您可以選擇這個技巧。

順帶一提，如果您想要存取 ABI 層級的成員，接著您便可以存取。 下列程式碼範例顯示執行的方式，以及 [C++/WinRT 和 ABI 間的互通性](interop-winrt-abi.md) 裡的更多詳細資料和程式碼範例。

```cppwinrt
int port = contosoUri.Port(); // Access the Port "property" accessor via C++/WinRT.

winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri = contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>();
HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
```

## <a name="delayed-initialization"></a>延遲初始化
即使投影類型的預設建構函式導致建立支援 Windows 執行階段物件。 如果您想要建構投影類型的變數，而不要依序建構 Windows 執行階段物件（讓您可以延遲之後才執行該工作），那麼您可以這麼做。 宣告您的變數或使用投影類型特殊 C++/WinRT `nullptr_t` 建構函式的欄位。

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

所有投影類型的預設建構函式*除了* `nullptr_t` 建構函式，皆導致建立支援 Windows 執行階段物件。 `nullptr_t` 建構函式基本上是沒有選項。 它預期在後續次數會初始化投影物件。 因此，不管執行階段類別是否有預設建構函式，您都可以使用這項技術有效地延遲初始化。

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>如果在 Windows 執行階段元件中實作 API
不論您是自己撰寫元件，或由廠商提供，本節皆適用。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

您的應用程式專案中，參考 Windows 執行階段元件的 Windows 執行階段中繼資料 (`.winmd`) 檔案，並建置。 在建置期間，`cppwinrt.exe` 工具產生完整描述的標準 C++ 程式庫&mdash;或 *投影*&mdash;適用於元件的 API 介面。 換言之，產生的程式庫包含適用於元件的投影類型。

然後，就像 Windows 命名空間類型一樣，您包含標頭並透過其中一個建構函式建構投影類型。 您的應用程式專案啟動程式碼註冊執行階段類別，且投影類別的建構函式呼叫 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646)，從參考的元件啟動執行階段類別。

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

如需更多詳細資料、程式碼和使用 Windows 執行階段元件中實作的 API 的逐步解說，請查閱 [C++/WinRT 中的撰寫事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)。

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>如果在使用的專案中實作 API
從 XAML UI 使用的類型必須是執行階段類別，即使是在像 XAML 一樣的專案。

針對這個案例，您從執行階段類別的 Windows 執行階段中繼資料 (`.winmd`) 產生一個投影類別。 同樣地，您包含標頭，但這次透過其 `nullptr` 建構函式建構投影類型。 該建構函式不執行任何初始設定，因此您接下來必須透過 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 協助程式函式將一個值指派給執行個體，傳遞任何必要的建構函式引數。 在如同使用程式碼相同的專案中實作執行階段類別不需要註冊，也不用透過 Windows 執行階段/COM 啟用初始化。

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

如需更多詳細資料、程式碼以及在使用專案中實做使用執行階段類別的逐步解說，請參閱 [XAML 控制項。繫結至 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>初始化並傳回投影類型與介面
以下是在您使用專案中的投影類型和介面可能會有的外觀範例。

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** 是一個投影類型；投影介面包含 **IMyRuntimeClass**、**IStringable**，與 **IClosable**。 本主題示範您可以初始化投影類型的不同方式。 以下是使用 **MyRuntimeClass** 做為範例的提醒和摘要。

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- 您可以存取所有投影類型介面的成員。
- 您可以將投影類型傳回給呼叫者。
- 投影類型與介面從 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) 衍生。 因此，您可以在投影類型或介面上呼叫 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，查詢其他投影介面，您可以也可以使用或將其傳回給呼叫者。 **as** 成員函式如同 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) 一樣運作。

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>啟用 Factory
建立 C++/WinRT 物件直接方便的方法如下。

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

但有時，您可能會想要自己建立啟用 Factory，並在適當時從其建立物件。 以下是顯示如何操作的範例，請使用 [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory) 函式範本。

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
auto factory = winrt::get_activation_factory<CurrencyFormatter, ICurrencyFormatterFactory>();
CurrencyFormatter currency = factory.CreateCurrencyFormatterCode(L"USD");
```

```cppwinrt
using namespace winrt::Windows::Foundation;
...
auto factory = winrt::get_activation_factory<Uri, IUriRuntimeClassFactory>();
Uri account = factory.CreateUri(L"http://www.contoso.com");
```

上述兩個範例中的類別是 Windows 命名空間的類型。 在下一個範例中，**BankAccountWRC::BankAccount** 是 Windows 執行階段元件中實作的自訂類型。

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="important-apis"></a>重要 API
* [QueryInterface 介面](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [RoActivateInstance 函式](https://msdn.microsoft.com/library/br224646)
* [Windows::Foundation::Uri 類別](/uwp/api/windows.foundation.uri)
* [winrt::get_activation_factory 函式範本](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown 結構](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>相關主題
* [在 C++/WinRT 中撰寫事件 ](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [C++/WinRT 與 ABI 之間的互通性](interop-winrt-abi.md)
* [C++/WinRT 的簡介](intro-to-using-cpp-with-winrt.md)
* [XAML 控制項；繫結一個 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
