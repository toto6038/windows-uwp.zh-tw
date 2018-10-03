---
author: stevewhims
description: 本主題示範如何直接或間接使用 **winrt::implements** 基礎結構撰寫 C++/WinRT API。
title: '使用 C++/WinRT 撰寫 API '
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projected, projection, implementation, implement, runtime class, activation, 標準, 投影的, 投影, 實作, 可實作, 執行階段類別, 啟用
ms.localizationpriority: medium
ms.openlocfilehash: 2476161954c1d4d49fcf9f8f74cd1b7cf9180c0a
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2018
ms.locfileid: "4309268"
---
# <a name="author-apis-with-cwinrt"></a>使用 C++/WinRT 撰寫 API 

本主題示範如何撰寫[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Api 使用[**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements)基礎結構，直接或間接。 在此內容中適用於 *author* 的同義字有 *produce*，或 *implement*。 在此訂單中，本主題涵蓋下列 C++/WinRT 類型的實作 API 案例。

- 您 *不* 撰寫 Windows 執行階段類別 (執行階段類別) ；您只要為應用程式中的本機使用實作一或多個 Windows 執行階段介面。 您可以直接從此案例的 **winrt::implements** 衍生並實作函式。
- 您 *正* 撰寫執行階段類別。 您可能會撰寫使用應用程式的元件。 或您可能撰寫使用 XAML 使用者介面 (UI) 的類型，且在此情況下，您會同時實作與使用相同編譯單位中的執行階段類別。 在這些案例中，您讓工具為您產生類別，從 **winrt::implements** 衍生。

在這兩個案例中，實作您的 C++/WinRT API 的類型又稱為 *實作類型*。

> [!IMPORTANT]
> 請務必從投影類型的執行類型中區分出執行類型的概念。 在 [使用 C++/WinRT 使用 API](consume-apis.md) 中所描述的投影類型。

## <a name="if-youre-not-authoring-a-runtime-class"></a>如果您 *不* 撰寫執行階段類別。
您正為本機使用量實作 的Windows 執行階段介面，是最簡單的案例。 您不需要執行階段類別；只需要一般 C++ 類別。 例如，您可能會根據 [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication) 撰寫應用程式。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

在 Visual Studio 中， **Visual c + +** > **Windows 通用** > **核心應用程式 (C + + /winrt)** 專案範本說明**CoreApplication**模式。 此模式以傳遞 [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)的實作給 [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run)，做為開始。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication**使用介面建立 app 的第一個檢視。 在概念上，**IFrameworkViewSource** 如下所示。

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

在概念上，**CoreApplication::Run** 的實作也是如此。

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

因此，您身為開發人員，請實作 **IFrameworkViewSource** 介面。 C++/WinRT 有基礎結構範本[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)，讓實作一個介面 (或多個) 變容易，而不需要 COM 樣式的程式設計。 您只是從 **實作** 中衍生您的類型，然後執行介面的功能。 方法如下。

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

處理 **IFrameworkViewSource**。 下一個步驟是傳回實作 **IFrameworkView** 介面的物件。 您也可以選擇在 **App** 上實作該介面。 下一個程式碼範例代表至少取得在電腦上啟動並執行的視窗最小 app。

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

因為您的 **App** 類型 *是一個* **IFrameworkViewSource**，您只要 **執行** 一個。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App{});
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>如果您正在 Windows 執行階段元件中撰寫執行階段類別
如果您的類型為了使用一個應用程式，已封裝在 Windows 執行階段元件中，它必須是執行階段類別。

> [!TIP]
> 為了在您編輯 IDL 檔案時最佳化建置效能，以及將 IDL 檔案邏輯對應到其產生的原始碼檔案，建議您在其本身的介面定義語言 (IDL) (.idl) 檔案中宣告各個執行階段類別。 Visual Studio 會合併所有產生的 `.winmd` 檔案為單一檔案，檔案名稱與根命名空間相同。 最終 `.winmd` 檔案將會是您的元件消費者將參照的檔案。

範例如下。

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

這個 IDL 宣告 Windows 執行階段 (執行階段) 類別。 執行階段類別可透過現代化 COM 介面啟動與使用，一般經過可執行的界限。 當您新增 IDL 檔案至專案，並組建 C++/WinRT toolchain (`midl.exe`與`cppwinrt.exe`) 為您產生實作類型。 使用上述範例 IDL，實作類型是 C++ 結構虛設常式名為 **winrt::MyProject::implementation::MyRuntimeClass** 在原始程式碼檔案中名為 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 和 `MyRuntimeClass.cpp`。

實作類型如下所示。

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        hstring Name();
        void Name(hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

注意正在使用的 F 繫結多型模式 (**MyRuntimeClass** 使用本身做為其基本的範本引數，**MyRuntimeClassT**)。 這也稱為奇怪的週期性範本模式 (CRTP)。 如果您遵循繼承鏈結向上，您將會通過 **MyRuntimeClass_base**。

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

因此，在本案例中，在繼承根階層根部的是再一次的 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基礎結構範本。

如需更多詳細資料、程式碼和 Windows 執行階段元件中撰寫 API 的逐步解說，請查閱 [C++/WinRT 中的撰寫事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)。

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>如果您正在撰寫在 XAML UI 中參照的執行階段類別
如果 XAML UI 參照您的類型，它則需要是一個執行階段類別，即使是在 XAML 相同的專案中。 雖然其通常經過可執行的界限來啟動，但執行階段類別可以在實作它的實作單位中使用。

在本案例中，您同時撰寫*與*使用此 API。 實作您執行階段類別的程序，基本上與 Windows 執行階段元件相同。 因此，請查閱前一章節&mdash;[如果您在 Windows 執行階段元件中撰寫執行階段類別](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)。 唯一不同的詳細資料是，從 IDL C + + / WinRT toolchain 不僅產生執行類型，也產生投影的類型。 還好，在本案例中，只有 **MyRuntimeClass** 可能會模擬兩可；有數個不同種類相同名字的實體。

- **MyRuntimeClass** 是執行階段類別的名稱。 但這是非常抽象：在 IDL 中宣告，並以某些程式語言實作。
- **MyRuntimeClass**是 C++ 結構 **winrt::MyProject::implementation::MyRuntimeClass** 的名字，也就是執行階段類別的 C++/WinRT 實作。 我們已經看過，如果有不同的實作和使用專案，則此結構只存在實作專案中。 這是*實作類型*，或*實作*。 在檔案 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 與 `MyRuntimeClass.cpp` 會產生 (藉由 `cppwinrt.exe` 工具) 此類型。 
- **MyRuntimeClass**是 C++ 結構 **winrt::MyProject::MyRuntimeClass** 表單中投影類型的名稱。 如果有不同的實作和使用專案，則此結構只存在使用專案中。 這是*投影類型*，或*投影*。 在檔案 `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` 中 (由 `cppwinrt.exe`) 產生此類型。

![投影的類型和實作類型](images/myruntimeclass.png)

以下是本主題相關的投影類型部分。

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

如需執行階段類別中的實作 **INotifyPropertyChanged** 介面逐步說明範例，請參閱 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md)。

在 [使用 C++/WinRT 使用 APS](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)中說明本案例中使用您執行階段類別的程序。

## <a name="runtime-class-constructors"></a>執行階段類別建構函式
以下是幾個重點，從我們上述看到的清單中取走。

- 您在 IDL 中宣告的每個建構函式，導致在實作類型與投影類型上產生一個建構函示。 從*不同的*編譯單位使用 IDL 已宣告的建構函式使用執行階段類型。
- 不論您是否有 IDL 已宣告的建構函式，會在您的投影類型上產生採用 `nullptr_t` 的建構函式多載。 呼叫 `nullptr_t` 建構函式是從 *相同* 編譯單位耗用執行階段類別中的 *首兩個步驟*。 如需詳細資訊和程式碼範例，請參閱 [使用 C++/WinRT 使用 API](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)。
- 如果您正從 *相同* 編譯單位耗用執行階段類別，您也可以直接在實作類別 (請記住，是在 `MyRuntimeClass.h`) 上實作非預設建構函式。

> [!NOTE]
> 如果您希望從不同編譯單位使用您的執行階段類別 (這是常見的)，然後在您的 IDL (至少預設一個建構函式) 中包含建構函式。 這樣一來，您也會與您的實作類型一起收到原廠實作。
> 
> 如果您想要只在相同的編譯單位撰寫及使用執行階段類別，則不可以在您的 IDL 中宣告任何建構函式。 您不需要的原廠實作，且不會產生一個。 將刪除您實作類型的預設建構函式，但您可以輕鬆編輯並改為預設值。
> 
> 如果您想要只在相同的編譯單位中撰寫與使用您的執行階段類別，且您需要建構函式參數，則直接在您的實作類型上直接撰寫您需要的建構函式。

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>初始化並傳回實作類型與介面
本節中，做為範例的執行類型名為 **MyType**，實作 [**IStringable**](/uwp/api/windows.foundation.istringable) 和 [**IClosable**](/uwp/api/windows.foundation.iclosable)介面。

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

從 **MyType** 移至 **IStringable** 或 **IClosable** 物件，您可以使用或傳回做為您投影的一部分，您可以呼叫 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 函式範本。 **請**傳回實作類型的預設介面。

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> 不過，如果您正在從 XAML UI 參考您的類型，則在相同的專案中將會同時有執行類型和投影類型。 在此情況下，**讓**傳回投影類型的執行個體。 如需該案例的程式碼範例，請參閱 [XAML 控制項。繫結至 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

我們可以只使用 `istringable` (在上述的程式碼範例中) 撥打電話給 **IStringable** 介面的成員。 但 C++/WinRT 介面 (這是一個投影介面) 從 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) 衍生。 因此，您可以呼叫[**iunknown:: As**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) （或[**iunknown:: Try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)） 在其上查詢其他投影的類型或介面，您可以也使用或傳回。

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

如果您需要存取所有實作的成員，則稍後再傳回介面給來電者，然後使用 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 功能範本。 **make_self** 傳回一個 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 包裝實作類型。 您可以存取所有介面 (使用箭號運算子) 的成員，您可以將其傳回給呼叫端 as-is，或您可以在其上撥打電話 **as**，及將結果介面物件傳回給呼叫端。

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

**MyType** 類別並不是投影的一部分。它是實作。 但這種方式您可以直接撥打其實作方法，而不需要虛擬功能通話的額外負擔。 在上述的範例中，即使 **MyType::ToString** 在 **IStringable** 使用相同簽章做為投影方法，我們仍直接呼叫非 虛擬方法，而不需要通過應用程式二進位介面 (ABI)。 **Com_ptr** 簡單的保留 **MyType** 結構的指標，讓您也可以透過 [`myimpl` 變數和箭號運算子存取的任何其他內部 **MyType** 的詳細資訊。

在案例中，您有介面物件，並知道這是您實作上的介面，然後您可以回到使用[**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)函式範本的實作。 再試一次，這是一種技術，避免虛擬功能通話，並讓您直接在實作中取得。

> [!NOTE]
> 如果您在安裝 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年)，或更新版本，則您需要呼叫[**winrt:: from_abi**](/uwp/cpp-ref-for-winrt/from-abi)而不是[**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)。

範例如下。 [實作**BgLabelControl**自訂控制項類別](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class)沒有另一個範例。

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

但僅有原始介面物件保留參考。 如果 *您* 想要保留，則您可以撥打電話 [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)。

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

實作類型本身不會從 **winrt::Windows::Foundation::IUnknown** 衍生，因此它沒有 **as** 功能。 即使如此，您可以初始化一個並存取所有的介面的成員。 但若您這樣做，則不要將原始實作類型執行個體傳回給呼叫者。 而是，使用上述的其中一種技術並傳回投影介面，或 **com_ptr**。

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

如果您有實作類型的執行個體，且您需要將它傳遞給預期對應投影類型的函式，則您可以進行。 轉換運算子存在於您實作類型 (但前提是所產生的實作類型`cppwinrt.exe`工具)，讓這能夠進行。

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>從有一個非預設建構函式衍生
[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)**](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_)是一個非預設建構函式的範例。 因此，沒有預設建構函式建構 **ToggleButtonAutomationPeer**，您需要傳遞 *owner*。 因此，如果您從 **ToggleButtonAutomationPeer** 衍生，則您需要提供取得一個 *owner* 的建構函式，並將它傳遞給基礎。 讓我們看看實際上看起來如何。

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

所產生您實作類型的建構函式如下所示。

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

唯一遺失的部分，是您需要將該建構函式參數傳遞給此基礎類別。 還記得我們上述的 F 繫結多型模式嗎？ 一旦您熟悉 C++/WinRT 使用的該模式細節，您便可以找出基礎類別的名稱 (或您可以只查看實作類別的標頭檔案)。 在這種情形下，這是呼叫基礎類別建構函式的方式。

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

基礎類別建構函式期待一個 **ToggleButton**。 以及 **MySpecializedToggleButton** *是一個***ToggleButton**。

直到您進行上述的編輯 (將該建構函式參數傳遞給此基礎類別)，編譯器會標幟您的建構函式並指出在稱為 (此範例中) **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** 的類型中沒有適當的預設建構函式。 這是您實作類型的基礎類別的實際基礎類別。

## <a name="important-apis"></a>重要 API
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from 函式](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)
* [winrt::from_abi 函式範本](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self 函式範本](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 函式範本](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown::as 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [XAML 控制向；繫結一個 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
