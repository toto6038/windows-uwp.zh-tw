---
description: 本主題示範如何直接或間接使用 **winrt::implements** 基底結構撰寫 C++/WinRT API。
title: 使用 C++/WinRT 撰寫 API
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projected, projection, implementation, implement, runtime class, activation, 標準, 投影的, 投影, 實作, 可實作, 執行階段類別, 啟用
ms.localizationpriority: medium
ms.openlocfilehash: 74d15b517c5ec6547115bc8ffdb44a2b742c68d6
ms.sourcegitcommit: a7a1e27b04f0ac51c4622318170af870571069f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717667"
---
# <a name="author-apis-with-cwinrt"></a>使用 C++/WinRT 撰寫 API

本主題示範如何直接或間接使用 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基底結構撰寫 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) API。 在此內容中「撰寫」  的同義字有「產生」  或「實作」  。 本主題以此順序涵蓋下列 C++/WinRT 類型的實作 API 案例。

> [!NOTE]
> 本主題與 Windows 執行階段元件的主體有關，但僅限與 C++/WinRT 環境有關的部分。 如果您想尋找的內容是涵蓋所有 Windows 執行階段語言的 Windows 執行階段元件，請參閱 [Windows 執行階段元件](/windows/uwp/winrt-components/)。

- 您「不」  撰寫 Windows 執行階段類別 (執行階段類別)；您只想針對您應用程式內的區域取用，實作一或多個 Windows 執行階段介面。 在此案例中，您直接從 **winrt::implements** 衍生並實作函式。
- 您「正」  撰寫執行階段類別。 您可能在撰寫供應用程式取用的元件。 或您可能在撰寫供 XAML 使用者介面 (UI) 取用的類型，在此情況下，您在相同編譯單位中，同時實作和取用執行階段類別。 在這些案例中，您讓工具為您產生從 **winrt::implements** 衍生的類別。

在這兩個案例中，實作您的 C++/WinRT API 的類型又稱為「實作類型」  。

> [!IMPORTANT]
> 請務必區分實作類型和投影類型的概念。 投影類型在[使用 C++/WinRT 取用 API](consume-apis.md) 中描述。

## <a name="if-youre-not-authoring-a-runtime-class"></a>如果您「不」  撰寫執行階段類別

最簡單的案例是，您在實作供區域取用的 Windows 執行階段介面。 您不需要執行階段類別；只需要一般 C++ 類別。 例如，您可能在撰寫以 [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication) 為基礎的應用程式。

> [!NOTE]
> 如需安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

在 Visual Studio 中，[Visual C++]   > [Windows 通用]   > [Core App (C++/WinRT)]  專案範本說明 **CoreApplication** 模式。 此模式以傳遞 [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 的實作給 [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run)，作為開始。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** 使用介面建立應用程式的第一個檢視。 在概念上，**IFrameworkViewSource** 看起來像這樣。

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

同樣在概念上，**CoreApplication::Run** 的實作會執行此動作。

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

因此，身為開發人員，您要實作 **IFrameworkViewSource** 介面。 C++/WinRT 有基底結構範本 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)，讓實作一個介面 (或多個) 變容易，而不需要 COM 樣式的程式設計。 您只是從 **implements** 衍生您的類型，然後實作介面的功能。 方法如下。

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

上述是處理 **IFrameworkViewSource**。 下一個步驟是傳回實作 **IFrameworkView** 介面的物件。 您也可以選擇在 **App** 上實作該介面。 下一個程式碼範例代表至少會在電腦上開啟視窗並執行的最簡應用程式。

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

因為您的 **App** 類型「是」  一個**IFrameworkViewSource**，所以您可以只將一個傳遞至 **Run**。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>如果您正在 Windows 執行階段元件中撰寫執行階段類別

如果您的類型為了取用一個應用程式，已封裝在 Windows 執行階段元件中，則它必須是執行階段類別。

> [!TIP]
> 為了在您編輯 IDL 檔案時最佳化建置效能，以及將 IDL 檔案邏輯對應到其產生的原始程式碼檔案，建議您在個別的介面定義語言 (IDL) (.idl) 檔案中宣告每個執行階段類別。 Visual Studio 會合併所有產生的 `.winmd` 檔案為單一檔案，檔案名稱與根命名空間相同。 最終 `.winmd` 檔案將會是您的元件取用者將參照的檔案。

這裡提供一個範例。

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

此 IDL 宣告 Windows 執行階段 (執行階段) 類別。 執行階段類別是可透過現代化 COM 介面啟動與使用的類型 (通常是跨可執行檔邊界的)。 當您將 IDL 檔案新增至專案並建置時，C++/WinRT 工具鏈 (`midl.exe` 和 `cppwinrt.exe`) 會為您產生實作類型。 如需作用中的 IDL 檔案工作流程範例，請參閱 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md)。

使用上述範例 IDL，實作類型是 C++ 結構虛設常式名為 **winrt::MyProject::implementation::MyRuntimeClass**，在名為 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 和 `MyRuntimeClass.cpp` 的原始程式碼檔案中。

實作類型如下所示。

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

請注意正在使用的 F 繫結多型模式 (**MyRuntimeClass** 使用本身作為其基底 (**MyRuntimeClassT**) 的範本引數)。 這也稱為奇怪的週期性範本模式 (CRTP)。 如果您遵循繼承鏈結向上，您將會通過 **MyRuntimeClass_base**。

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

因此，在本案例中，在繼承階層根部的一樣是 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基底結構範本。

如需更多詳細資料、程式碼和 Windows 執行階段元件中撰寫 API 的逐步解說，請參閱[在 C++/WinRT 中撰寫事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)。

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>如果您正在撰寫要在 XAML UI 中參考的執行階段類別

如果您的類型是要由您的 XAML UI 參照，它則需要是一個執行階段類別，即使它與 XAML 在同一個專案中也是。 雖然執行階段類別通常是跨可執行檔邊界來啟動，但執行階段類別也可以在實作它的編譯單位中使用。

在此案例中，您同時撰寫「和」  取用 API。 實作您執行階段類別的程序，基本上與 Windows 執行階段元件的相同。 因此，請查閱上一節&mdash;[如果您正在 Windows 執行階段元件中撰寫執行階段類別](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)。 唯一不同的細節是，C + + / WinRT 工具鏈不僅從 IDL 產生執行類型，也產生投影類型。 請務必了解，在此案例中只說 "**MyRuntimeClass**" 可能會模稜兩可；有數個不同種類相同名字的實體。

- **MyRuntimeClass** 是執行階段類別的名稱。 但這只是抽象概念：在 IDL 中宣告，並以某些程式語言實作。
- **MyRuntimeClass** 是 C++ 結構 **winrt::MyProject::implementation::MyRuntimeClass** 的名字，也就是執行階段類別的 C++/WinRT 實作。 我們已經看過，如果有不同實作和取用專案，則此結構只存在實作專案中。 這是「實作類型」  ，或「實作」  。 此類型會在檔案 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 與 `MyRuntimeClass.cpp` 中產生 (藉由 `cppwinrt.exe` 工具)。
- **MyRuntimeClass** 是 C++ 結構 **winrt::MyProject::MyRuntimeClass** 形式投影類型的名稱。 如果有不同的實作並取用專案，則此結構只存在取用專案中。 這是「投影類型」  ，或「投影」  。 此類型會在檔案 `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` 中產生 (藉由 `cppwinrt.exe` 工具)。

![投影類型和實作類型](images/myruntimeclass.png)

以下是本與主題相關的投影類型部分。

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

如需執行階段類別中的實作 **INotifyPropertyChanged** 介面逐步說明範例，請參閱 [XAML 控制項；繫結至一個 C++/WinRT 屬性](binding-property.md)。

在此案例中使用您執行階段類別的程序，在 [C++/WinRT 使用 APS](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project) 中描述。

## <a name="runtime-class-constructors"></a>執行階段類別建構函式

上列內容的重點如下。

- 您在 IDL 中宣告的每個建構函式，都導致在實作類型與投影類型上產生一個建構函示。 從「不同」  編譯單位使用 IDL 已宣告的建構函式取用執行階段類型。
- 不論您是否有 IDL 已宣告的建構函式，都會在您的投影類型上產生接受 **std::nullptr_t** 的建構函式多載。 呼叫 **std::nullptr_t** 建構函式是從「相同」  編譯單位取用執行階段類別的「首兩個步驟」  。 如需詳細資料和程式碼範例，請參閱[使用 C++/WinRT 取用 API](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)。
- 如果您正從「相同」  編譯單位取用執行階段類別，您也可以直接在實作類型 (請記住，是在 `MyRuntimeClass.h`) 上實作非預設建構函式。

> [!NOTE]
> 如果您希望從不同編譯單位取用您的執行階段類別 (這是常見的)，然後在您的 IDL 中包含建構函式 (至少一個預設建構函式)。 這樣一來，您也會隨您的實作類型取得原廠實作。
> 
> 如果您想要只在相同的編譯單位撰寫及取用執行階段類別，則不可以在您的 IDL 中宣告任何建構函式。 您不需要原廠實作，且不會產生一個。 系統將會刪除您實作類型的預設建構函式，但您可以輕鬆地編輯並改為預設值。
> 
> 如果您想要只在相同的編譯單位中撰寫與取用您的執行階段類別，且您需要建構函式參數，則直接在您的實作類型上直接撰寫您需要的建構函式。

## <a name="runtime-class-methods-properties-and-events"></a>執行階段類別方法、屬性和事件

我們已了解工作流程會使用 IDL 宣告您的執行階段類別和其成員，然後由工具為您產生原型和虛設常式實作。 至於這些針對您執行階段類別成員自動產生的原型，您「可以」  對其進行編輯，讓其傳遞您在 IDL 中宣告的不同類型。 但是，若要這麼做，您在 IDL 中宣告的類型必須要能夠轉送到您在實作版本中宣告的類型。

以下是一些範例。

- 您可以放寬參數類型的限制。 例如，如果您的方法在 IDL 中採用 **SomeClass**，則在實作中，您可以選擇將其變更成 **IInspectable**。 之所以可以這麼做，是因為任何 **SomeClass** 都可轉送到 **IInspectable** (但反過來則不行)。
- 您可以接受值形式的可複製參數，而不是參考形式。 例如，將 `SomeClass const&` 變更為 `SomeClass const&`。 如果您需要避免將參考擷取到協同程式，則這是必要的 (請參閱[參數傳遞](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing))。
- 您可以放寬傳回值的限制。 例如，您可以將 **void** 變更為 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)。

最後兩項非常適合用來撰寫非同步的事件處理常式。

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>初始化並傳回實作類型與介面

本節中，作為範例的執行類型名為 **MyType**，實作 [**IStringable**](/uwp/api/windows.foundation.istringable) 和 [**IClosable**](/uwp/api/windows.foundation.iclosable) 介面。

您可以直接從 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) (不是執行階段類別) 衍生 **MyType**。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

或者您可以從 IDL (是執行階段類別) 產生它。

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

若要從 **MyType** 移至 **IStringable** 或 **IClosable** 物件 (您可以使用或傳回作為您投影的一部分)，您應呼叫 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 函式範本。 **make** 傳回實作類型的預設介面。

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> 不過，如果您是從 XAML UI 參考您的類型，則在相同的專案中將會同時有執行類型和投影類型。 在該情況下，**make** 傳回投影類型的執行個體。 如需該案例的程式碼範例，請參閱 [XAML 控制項；繫結至一個 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

我們可以只使用 `istringable` (在上述的程式碼範例中) 呼叫 **IStringable** 介面的成員。 但 C++/WinRT 介面 (這是投影介面) 是從 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) 衍生的。 因此，您可以在該介面上呼叫 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (或 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function))，查詢其他投影類型或介面，您也可以使用查詢結果或將它們傳回。

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

如果您需要存取實作的所有成員，然後再傳回介面給呼叫者，則使用 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 功能範本。 **make_self** 傳回一個 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 包裝實作類型。 您可以存取其所有介面 (使用箭號運算子) 的成員，您可將它依原狀傳回給呼叫者，或您可以在它上面呼叫 **as**，並將結果介面物件傳回給呼叫者。

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

**MyType** 類別不是投影的一部分。它是實作。 但這樣您可以直接呼叫其實作方法，而不需要虛擬函式呼叫的額外負荷。 在上述的範例中，雖然 **MyType::ToString** 與 **IStringable** 上的投影方法使用相同特徵標記，我們仍直接呼叫非虛擬方法，而不需要通過應用程式二進位介面 (ABI)。 **Com_ptr** 只持有指向 **MyType** 結構的指標，所以您也可以透過 `myimpl` 變數和箭號運算子，存取 **MyType** 的任何其他內部詳細資訊。

如果您有介面物件，且知道它是在您實作上的介面，則您可以使用 [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) 函式範本返回實作。 同樣地，這是避免虛擬函式呼叫的技術，並讓您直接在實作中取得。

> [!NOTE]
> 如果您尚未安裝 Windows SDK 10.0.17763.0 版 (Windows 10 1809 版) 或更新版本，則您必須呼叫 [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi)，而不是呼叫 [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)。

這裡提供一個範例。 這是[實作 **BgLabelControl** 自訂控制項類別](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class)的另一個範例。

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

但僅有原始介面物件保留參考。 如果「您」  想要保留它，則您可以呼叫 [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)。

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

實作類型本身不是從 **winrt::Windows::Foundation::IUnknown** 衍生的，因此它沒有 **as** 函式。 即使如此，您可以具現化一個並存取所有介面的成員。 但若您這樣做，則不要將原始實作類型執行個體傳回給呼叫者。 反之，請使用上述的其中一個技術並傳回投影介面，或 **com_ptr**。

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

如果您有實作類型的執行個體，且您需要將它傳遞給預期對應投影類型的函式，則您可以進行。 因為轉換運算子存在於您的實作類型 (實作類型必須是由 `cppwinrt.exe` 工具產生的) 上，所以您可以這麼做。 您可以將實作類型值直接傳遞給預期該對應投影類型值的方法。 您可以從實作類型成員函式，將 `*this` 傳遞給預期該對應投影類型值的方法。

```cppwinrt
// MyProject::MyType is the projected type; the implementation type would be MyProject::implementation::MyType.

void MyOtherType::DoWork(MyProject::MyType const&){ ... }

...

void FreeFunction(MyProject::MyOtherType const& ot)
{
    MyType myimpl;
    ot.DoWork(myimpl);
}

...

void MyType::MemberFunction(MyProject::MyOtherType const& ot)
{
    ot.DoWork(*this);
}
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>從有非預設建構函式的類型衍生

[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_)是非預設建構函式的範例。 它沒有預設建構函式，若要建構 **ToggleButtonAutomationPeer**，您必須傳遞 *owner*。 因此，如果您從 **ToggleButtonAutomationPeer** 衍生，則您需要提供一個接受 *owner* 的建構函式，並將它傳遞給基底。 讓我們看看實際上看起來如何。

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

為您實作類型產生的建構函式看起來像這樣。

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

唯一遺失的部分，是您需要將該建構函式參數傳遞至基底類別。 還記得我們上述的 F 繫結多型模式嗎？ 一旦您熟悉 C++/WinRT 所使用模式的細節，您便可以找出基底類別的名稱 (或您可以直接查看您實作類別的標頭檔)。 在此情形下，這是呼叫基底類別建構函式的方式。

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

基底類別建構函式預期一個 **ToggleButton**。 而且 **MySpecializedToggleButton**「是」  一個**ToggleButton**。

除非您進行上述的編輯 (將建構函式參數傳遞給基底類別)，否則編譯器會標幟您的建構函式，並指出在稱為 (此案例中) **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** 的類型中，無法取得適當的預設建構函式。 這是您實作類型的基底類別的實際基底類別。

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>命名空間：投影的類型、實作類型及處理站

如您先前在本主題中看到的，C++/WinRT 執行階段類別會以多個 C++ 類別的形式存在於多個命名空間中。 因此，**MyRuntimeClass** 名稱在 **winrt::MyProject** 命名空間中有一個意義，而在 **winrt::MyProject::implementation** 命名空間中則有不同的意義。 請留意您目前環境中具有的命名空間，並在需要從不同命名空間中取得名稱時，使用命名空間前置詞。 讓我們更仔細地看看這些命名空間。

- **winrt::MyProject**。 此命名空間包含投影類型。 投影類型的物件是 Proxy；其本質是支援物件的智慧指標，而該支援物件可能會在此實作到您的專案中，或可能實作到另一個編譯單位中。
- **winrt::MyProject::implementation**。 此命名空間包含實作類型。 實作類型的物件不是指標；而是值 &mdash; 完整的 C++ 堆疊物件。 請勿直接建構實作類型，而是呼叫 [**winrt::make**](/uwp/cpp-ref-for-winrt/make)，將實作類型作為範本參數來傳遞。 在此主題中，我們先前已示範作用中的 **winrt::make** 範例，而另一個範例位在 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)中。
- **winrt::MyProject::factory_implementation**。 此命名空間包含處理站。 此命名空間中的物件支援 [**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory)。

下表列出在不同環境中必須使用的最低命名空間條件。

|環境中的命名空間|指定投影類型|指定投影類型|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> 如果您想要從實作中傳回投影類型，請不要透過撰寫 `MyRuntimeClass myRuntimeClass;` 來具現化實作類型。 適用於該案例的正確技術和程式碼位在本主題前面的[具現化並傳回實作類型與介面](#instantiating-and-returning-implementation-types-and-interfaces)一節。
>
> 在該案例中，`MyRuntimeClass myRuntimeClass;` 的問題是其在堆疊上建立了 **winrt::MyProject::implementation::MyRuntimeClass** 物件。 該物件 (屬於實作類型) 的行為在某些方面類似投影類型 &mdash; 您可以透過相同方式在其中叫用方法，其甚至可以轉換成投影類型。 但物件會在有範圍限制時遭到解構 (根據一般的 C++ 規則)。 因此，如果您已將投影類型 (以智慧指標的形式) 傳回給該物件，則該指標現在是懸空狀態。
>
> 這種記憶體損毀的錯誤類型很難診斷。 因此，針對偵錯組建，C++/WinRT 判斷提示會使用堆疊偵測器協助您找出這種錯誤。 但協同程式會配置到堆積上，因此，如果將其放到協同程式中，則無法取得此錯誤的協助。

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>搭配各種 C++/WinRT 功能使用投影類型與實作類型

以下是各種位置和其中預期有類型的 C++/WinRT 功能，以及這些功能預期的類型種類 (投影類型、實作類型或兩者皆是)。

|功能|接受|附註|
|-|-|-|
|`T` (代表智慧指標)|投影|請參閱[命名空間：投影類型、實作類型及處理站](#namespaces-projected-types-implementation-types-and-factories)中有關意外使用實作類型的注意事項。|
|`agile_ref<T>`|兩者|如果您使用實作類型，則建構函式引數必須是 `com_ptr<T>`。|
|`com_ptr<T>`|實作|使用投影類型會產生錯誤：`'Release' is not a member of 'T'`。|
|`default_interface<T>`|兩者|如果您使用實作類型，則會傳回第一次實作的介面。|
|`get_self<T>`|實作|使用投影類型會產生錯誤：`'_abi_TrustLevel': is not a member of 'T'`。|
|`guid_of<T>()`|兩者|傳回預設介面的 GUID。|
|`IWinRTTemplateInterface<T>`<br>|投影|使用實作類型編譯，但這是錯誤的 &mdash; 請參閱[命名空間：投影類型、實作類型及處理站](#namespaces-projected-types-implementation-types-and-factories)中的注意事項。|
|`make<T>`|實作|使用投影類型會產生錯誤：`'implements_type': is not a member of any direct or indirect base class of 'T'`|
| `make_agile(T const&amp;)`|兩者|如果您使用實作類型，則引數必須是 `com_ptr<T>`。|
| `make_self<T>`|實作|使用投影類型會產生錯誤：`'Release': is not a member of any direct or indirect base class of 'T'`|
| `name_of<T>`|投影|如果您使用實作類型，您會取得轉換為字串 (stringify) 的預設介面 GUID。|
| `weak_ref<T>`|兩者|如果您使用實作類型，則建構函式引數必須是 `com_ptr<T>`。|

## <a name="important-apis"></a>重要 API
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from 函式](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [winrt::from_abi 函式範本](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self 函式範本](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 函式範本](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown::as 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [XAML 控制項；繫結至一個 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
