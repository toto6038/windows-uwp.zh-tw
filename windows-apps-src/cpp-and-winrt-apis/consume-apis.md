---
description: 本主題示範如何取用 C++/WinRT API，無論 Windows、第三方元件廠商或您自己是否實作它們。
title: 使用 C++/WinRT 取用 API
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 已投影, 投影, 實作, 執行階段類別, 啟用
ms.localizationpriority: medium
ms.openlocfilehash: 5a3d4b554fafeb2053e4e6af831c224b5eacd151
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328875"
---
# <a name="consume-apis-with-cwinrt"></a>使用 C++/WinRT 取用 API

本主題示範如何取用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) API，不管是否為 Windows 的一部分，皆由第三方元件廠商或您自己來實作。

## <a name="if-the-api-is-in-a-windows-namespace"></a>如果 API 位在 Windows 命名空間中
這是取用 Windows 執行階段 API 最常見的案例。 對於中繼資料中定義的 Windows 命名空間的每種類型，C++/WinRT 定義 C++ 適用的對等項目 (稱為*投影類型*)。 投影類型有相同的完整名稱做為 Windows 類型，但它放在使用 C++ 語法的 C++ **winrt** 命名空間。 例如，將 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 投影到 C++/WinRT 做為 **winrt::Windows::Foundation::Uri**。

以下是簡單的程式碼範例。 如果您想要將下列程式碼範例直接複製並貼到 **Windows 主控台應用程式 (C++/WinRT)** 專案的主要原始程式碼檔，請先在專案屬性中設定 [不使用預先編譯的標頭]  。

```cppwinrt
// main.cpp
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
> 每當您要使用來自 Windows 命名空間的類型時，請包含對應到該命名空間的 C++/WinRT 標頭。 `using namespace` 指示詞是選擇性的，但很便利。

上述的程式碼範例中，初始化 C++/WinRT 後，我們透過其公開記載於文件的建構函式之一 (在此範例中，[**Uri(String)** ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_)) 堆疊配置 **winrt::Windows::Foundation::Uri** 的值投影類型。 針對這點，最常見的使用案例就是您通常需要執行的。 一旦您取得 C++/WinRT 投影類型值，您可以將其視為如同實際 Windows 執行階段類型的執行個體，因為其有相同的成員。

事實上，該投影的值是 proxy；它基本上只是支援物件的智慧型指標。 投影值的建構函式呼叫 [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) 來建立支援 Windows 執行階段類別的執行個體 (此案例中為 **Windows.Foundation.Uri**)，並在新投影值中儲存該物件的預設介面。 如下所示，您投影值成員的呼叫透過智慧型指標確實委派至支援物件；其為狀態發生變更的位置。

![投影 Windows::Foundation::Uri 類型](images/uri.png)

當 `contosoUri` 值超出範圍時，會解構並將其參考資料發行至預設介面。 如果該參考是支援 Windows 執行階段 **Windows.Foundation.Uri** 物件的最後一個參考，則支援物件也會解構。

> [!TIP]
> *投影類型*是一種透過執行階段類別為媒介的包裝函式，目的是取用其 API。 *投影介面*是一種透過 Windows 執行階段介面為媒介的包裝函式。

## <a name="cwinrt-projection-headers"></a>C++/WinRT 投影標頭
若要從 C++/WinRT 取用 Windows 命名空間 API，請包含 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 資料夾的標頭。 附屬命名空間中的類型通常在其直接父系命名空間中參考類型。 因此，每個 C++/WinRT 投影標頭自動包含其父系命名空間標頭檔案；讓您不「需要」  明確包含它。 不過，如果您這麼做，也不會有任何錯誤。

例如，針對 [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates) 命名空間，對等項目 C++/WinRT 類型定義位於 `winrt/Windows.Security.Cryptography.Certificates.h`。 **Windows::Security::Cryptography::Certificates** 中的類型需要父系 **Windows::Security::Cryptography** 命名空間中的類型；且該命名空間中的類型可能需要其本身父系中的類型，**Windows::Security**。

因此，當您包含 `winrt/Windows.Security.Cryptography.Certificates.h` 時，該檔案依序包含 `winrt/Windows.Security.Cryptography.h`；以及 `winrt/Windows.Security.Cryptography.h` 包含 `winrt/Windows.Security.h`。 這是記錄停止的位置，因為沒有 `winrt/Windows.h`。 此可轉移的包含處理序會在第二層命名空間停止。

此處理序會透過可轉移的方式包含標頭檔案，這些標頭檔案為父系命名空間中定義的類別提供必要的「宣告」  和「實作」  。

某個命名空間中的類型成員可以參考在其他無關的命名空間中一個或多個類型。 為了讓編譯器成功編譯這些成員定義，編譯器需要查看所有這些類型的關閉類型宣告。 因此，每個 C++/WinRT 投影標頭包含需要「宣告」  任何相關類型的命名空間標頭。 與父系命名空間不同，此處理序「未」  提取參考類型的「實作」  。

> [!IMPORTANT]
> 當您想要實際「使用」  在不相關的命名空間裡宣告的類型 (初始化、呼叫方法等)，必須包含該類型適當的命名空間標頭檔案。 僅自動包含「宣告」  ，而無「實作」  。

例如，如果只包含 `winrt/Windows.Security.Cryptography.Certificates.h`，則會造成從這些命名空間提取宣告 (以可轉移的方式進行，依此類推)。

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

亦即，在您包含的標頭中轉送宣告一些 API。 但其定義在您未包含的標頭裡。 因此，如果您接著呼叫 [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri)，則您會收到連結器錯誤，指出成員未定義。 解決方案是明確 `#include <winrt/Windows.Foundation.h>`。 一般而言，當您看到如下的連結器錯誤時，請包含為 API 命名空間命名的標頭並重建。

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>透過物件、透過介面，或透過 ABI 存取成員
使用 C++/WinRT 投影，Windows 執行階段類別的執行階段呈現方式是不超過基礎 ABI 介面。 不過，為了方便使用，您可以按照作者希望的方式對類別撰寫程式碼。 例如，您可以呼叫 [**Uri**](/uwp/api/windows.foundation.uri) 的 **ToString** 方法，就像類別的方法一樣 (事實上，藉由此做法，這是一種在個別 **IStringable** 介面上的方法)。

`WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

透過查詢適當的介面以實現此便利。 但是，您隨時可以控制。 您可以透過自我擷取 IStringable 介面並直接使用，來選擇放棄一點方便以獲得一點效能。 在下列程式碼範例中，您可以在執行階段取得實際 IStringable 介面指標 (透過一次性查詢)。 之後，您對 **ToString** 的呼叫是直接的，並且可避免任何進一步呼叫 **QueryInterface**。

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

如果您知道會在相同的介面呼叫數種方法，可以選擇這個技巧。

順帶一提，如果您想要存取 ABI 層級的成員，您便可以這麼做。 下列程式碼範例顯示執行的方式，而且 [C++/WinRT 和 ABI 之間的互通性](interop-winrt-abi.md)中有更多詳細資料和程式碼範例。

```cppwinrt
#include <Windows.Foundation.h>
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
using namespace winrt::Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };

    int port{ contosoUri.Port() }; // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri{
        contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>() };
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>延遲初始化

在 C++/WinRT 中，每個投影類型都有特殊的 C++/WinRT **std::nullptr_t** 建構函式。 其例外狀況是所有投影類型建構函式&mdash;包括預設處理常式&mdash;都會導致建立支援的 Windows 執行階段物件，並為您提供其智慧型指標。 所以，該規則適用於任何使用預設建構函式的位置，例如未初始化的區域變數、未初始化的全域變數，以及未初始化的成員變數。

另一方面，如果您想要建構投影類型的變數，而不要依序建構支援 Windows 執行階段物件 (以便可延遲到之後才執行該工作)，那麼您可以這麼做。 使用特殊 C++/WinRT **std::nullptr_t** 建構函式 (C++/WinRT 投影會插入每個執行階段類別中)，宣告您的變數或欄位。 我們在下面的程式碼範例中使用該特殊建構函式搭配 *m_gamerPicBuffer*。

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

#define MAX_IMAGE_SIZE 1024

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

int main()
{
    winrt::init_apartment();
    Sample s;
    // ...
    s.DelayedInit();
}
```

投影類型的所有建構函式 (除了 **std::nullptr_t**  建構函式以外) 皆會促使系統建立支援 Windows 執行階段物件。 **std::nullptr_t** 建構函式基本上是沒有選項。 預期之後會初始化投影物件。 因此，不管執行階段類別是否有預設建構函式，您都可以使用這項技術有效地延遲初始化。

此考量會影響您叫用預設建構函數的其他位置，例如向量和地圖。 考量此程式碼範例，您需要**空白的應用程式 (C++/WinRT)** 專案。

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

此指派會建立新的 **TextBlock**，然後立即使用 `value` 覆寫。 解決方式如下。

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

另請參閱[預設建構函式對於集合的影響](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#how-the-default-constructor-affects-collections)。

### <a name="dont-delay-initialize-by-mistake"></a>不要意外造成延遲初始化

請小心，不要意外叫用 **std::nullptr_t** 建構函式。 編譯器的衝突解決方法能透過處理站建構函式對其提供協助。 例如，請考慮下列兩個執行階段類別定義。

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox();
}

// Gift.idl
runtimeclass Gift
{
    Gift(GiftBox giftBox); // You can create a gift inside a box.
}
```

假設我們要建構不在盒子中的 **Gift** (以未初始化 **GiftBox** 建構的 **Gift**)。 首先，讓我們看看「錯誤」  的方法。 我們知道有一個採用 **GiftBox** 的 **Gift** 建構函式。 但是，如果我們想要傳遞 null 的 **GiftBox** (透過我們下方所執行的統一初始化叫用 **Gift** 建構函式)，則我們「不會」  得到想要的結果。

```cppwinrt
// These are *not* what you intended. Doing it in one of these two ways
// actually *doesn't* create the intended backing Windows Runtime Gift object;
// only an empty smart pointer.

Gift gift{ nullptr };
auto gift{ Gift(nullptr) };
```

您在此取得的是未初始化的 **Gift**。 您無法透過未初始化的 **GiftBox** 取得 **Gift**。 以下是「正確」  的方法。

```cppwinrt
// Doing it in one of these two ways creates an initialized
// Gift with an uninitialized GiftBox.

Gift gift{ GiftBox{ nullptr } };
auto gift{ Gift(GiftBox{ nullptr }) };
```

在不正確的範例中，傳遞 `nullptr` 常值可透過延遲初始化建構函式來解析。 若要透過處理站建構函式進行解析，則參數類型必須是 **GiftBox**。 您仍然可以選擇傳遞明確的延遲初始化 **GiftBox**，如正確的範例所示。

下一個範例「也是」  正確的，因為參數有 GiftBox 類型，而非 **std:: nullptr_t**。

```cppwinrt
GiftBox giftBox{ nullptr };
Gift gift{ giftBox }; // Calls factory constructor.
```

只有在您傳遞 `nullptr` 常值時，才會發生模稜兩可的情況。

## <a name="dont-copy-construct-by-mistake"></a>不要意外造成複製建構。

這個警告類似於上述[不要意外造成延遲初始化](#dont-delay-initialize-by-mistake)一節中的描述。

除了延遲初始化建構函式，C++/WinRT 投影也會將複製建構函式插入每個執行階段類別。 這是單一參數建構函式，可接受的類型與要建構的物件相同。 產生的智慧指標會指向其建構函式參數所指向的相同支援 Windows 執行階段物件。 結果是兩個智慧指標物件指向相同支援物件。

以下是我們在程式碼範例中使用的執行階段類別定義。

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox(GiftBox biggerBox); // You can place a box inside a bigger box.
}
```

假設我們想要在較大的 **GiftBox** 內建構 **GiftBox**。

```cppwinrt
GiftBox bigBox{ ... };

// These are *not* what you intended. Doing it in one of these two ways
// copies bigBox's backing-object-pointer into smallBox.
// The result is that smallBox == bigBox.

GiftBox smallBox{ bigBox };
auto smallBox{ GiftBox(bigBox) };
```

「正確」  的方法就是明確地呼叫啟用處理站。

```cppwinrt
GiftBox bigBox{ ... };

// These two ways call the activation factory explicitly.

GiftBox smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
auto smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>如果 API 是在 Windows 執行階段元件中實作
不論您是自己撰寫元件，或由廠商提供，本節皆適用。

> [!NOTE]
> 如需安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

在您的應用程式專案中，參考 Windows 執行階段元件的 Windows 執行階段中繼資料 (`.winmd`) 檔案，並建置。 在建置期間，`cppwinrt.exe` 工具產生標準的 C++ 程式庫，完整描述 &mdash; 或*投影*&mdash;適用於元件的 API 介面。 換言之，產生的程式庫包含適用於元件的投影類型。

然後，就像 Windows 命名空間類型一樣，您包含標頭並透過其中一個建構函式建構投影類型。 您的應用程式專案啟動程式碼註冊執行階段類別，且投影類別的建構函式呼叫 [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)，以從參考的元件啟動執行階段類別。

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

如需在 Windows 執行階段元件中實作取用 API 的更多詳細資料、程式碼和逐步解說，請參閱[以 C++/WinRT 撰寫事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)。

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>如果 API 是在取用的專案中實作
從 XAML UI 使用的類型必須是執行階段類別，即使它與 XAML 位於相同專案中。

針對這個案例，您可以從執行階段類別的 Windows 執行階段中繼資料 (`.winmd`) 產生投影類型。 同樣地，包含標頭，但這次透過其 **std::nullptr_t** 建構函式建構投影類型。 該建構函式不執行任何初始化，因此您接下來必須透過 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 輔助函式將一個值指派給執行個體，傳遞任何必要的建構函式引數。 在與取用程式碼相同的專案中實作的執行階段類別不需要登錄，也不需要透過 Windows 執行階段/COM 啟動來進行具現化。

您需要適用於此程式碼範例的**空白應用程式 (C++/WinRT)** 專案。

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
    m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

如需在取用專案中實作取用執行階段類別的更多詳細資料、程式碼，以及逐步解說，請參閱 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>具現化並傳回投影類型與介面
以下是在您使用專案中的投影類型和介面可能會有的外觀範例。 請記住，投影的類型 (例如此範例中的類型) 是由工具產生，而不是您自己撰寫的。

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** 是投影類型；投影介面包含 **IMyRuntimeClass** **IStringable**，以及 **IClosable**。 本主題示範您可以具現化投影類型的不同方式。 以下是使用 **MyRuntimeClass** 做為範例的提醒和摘要。

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
- 投影類型與介面衍生自 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)。 因此，您可以在投影類型或介面上呼叫 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，以查詢其他投影介面，您也可以使用或將其傳回給呼叫者。 **as** 成員函式的運作方式類似 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))。

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
建立 C++/WinRT 物件的簡便方法如下。

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

但有時，您可能會想要自己建立啟用 Factory，並在適當時從中建立物件。 以下範例示範如何使用 [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory) 函式範本。

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

## <a name="membertype-ambiguities"></a>成員/類型意義不明確

當成員函式具有與類型相同的名稱時，意義就會不明確。 成員函式中 C++ 不合格名稱查閱的規則會使它在命名空間中搜尋之前，先搜尋類別。 「替代失敗不是錯誤」  (SFINAE) 規則不適用 (它適用於函式樣板的多載解析期間)。 因此如果類別內的名稱沒有意義，則編譯器不會持續尋找更好的相符項目&mdash;只會回報錯誤。

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // This doesn't compile. You get the error
        // "'winrt::Windows::Foundation::IUnknown::as':
        // no matching overloaded function found".
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Style>() };
    }
}
```

在上述情況下，編譯器會認為您要將 [**FrameworkElement.Style()** ](/uwp/api/windows.ui.xaml.frameworkelement.style) (在 C++/WinRT 中為成員函式) 當作範本參數傳遞至 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)。 解決方法是強制將名稱 `Style` 解譯為類型 [**Windows::UI::Xaml::Style**](/uwp/api/windows.ui.xaml.style)。

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // One option is to fully-qualify it.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Windows::UI::Xaml::Style>() };

        // Another is to force it to be interpreted as a struct name.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<struct Style>() };

        // If you have "using namespace Windows::UI;", then this is sufficient.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Xaml::Style>() };

        // Or you can force it to be resolved in the global namespace (into which
        // you imported the Windows::UI::Xaml namespace when you did
        // "using namespace Windows::UI::Xaml;".
        auto style = Application::Current().Resources().
            Lookup(L"MyStyle").as<::Style>();
    }
}
```

如果名稱後面接著 `::`，不合格的名稱查閱會有特殊的例外狀況，在此情況下，它會忽略函式、變數和列舉值。 這可讓您執行這類工作。

```cppwinrt
struct MyPage : Page
{
    void DoSomething()
    {
        Visibility(Visibility::Collapsed); // No ambiguity here (special exception).
    }
}
```

對 `Visibility()` 的呼叫會解析為 [**UIElement.Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) 成員函式名稱。 但是參數 `Visibility::Collapsed` 在 `Visibility` 後面加上 `::`，因此會忽略方法名稱，而編譯器會找到列舉類別。

## <a name="important-apis"></a>重要 API
* [QueryInterface 介面](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [RoActivateInstance 函式](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Windows::Foundation::Uri 類別](/uwp/api/windows.foundation.uri)
* [winrt::get_activation_factory 函式範本](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown 結構](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>相關主題
* [以 C++/WinRT 撰寫事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [C++/WinRT 與 ABI 之間的互通性](interop-winrt-abi.md)
* [C++/WinRT 的簡介](intro-to-using-cpp-with-winrt.md)
* [XAML 控制項；繫結至一個 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
