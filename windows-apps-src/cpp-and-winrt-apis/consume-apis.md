---
author: stevewhims
description: 本主題示範如何使用 C++/WinRT API，無論 Windows、第三方元件廠商或您自己是否實作它們。
title: '使用 C++/WinRT 使用 API '
ms.author: stwhi
ms.date: 04/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影的、投影、實作、執行階段類別、啟用
ms.localizationpriority: medium
ms.openlocfilehash: e777aca8f1d9f3892f67b10c785c056b14c8070c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832242"
---
# <a name="consume-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 使用 API 
> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

本主題示範如何使用 C++/WinRT API，不管是否為 Windows 的一部分，由第三方元件廠商或您自己來實作它們。

## <a name="if-the-api-is-in-a-windows-namespace"></a>如果 API 在 Windows 命名空間中
這是您使用 Windows 執行階段 API 最常見的案例。 以下是一個簡單的程式碼範例。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
}
```

包含的標頭 `winrt/Windows.Foundation.h` 屬於 SDK 的一部分，可在資料夾 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 中找到。 資料夾中的標頭包含投影到 C++/WinRT 的 Windows API。 每當您要使用從 Windows 命名空間投影到 C++/WinRT，請包含對應到該命名空間的 C++/WinRT 投影標頭。 `using namespace` 指示詞是選擇性的，但很便利。

在此範例中，`winrt/Windows.Foundation.h`包含適用於執行階段類別 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 的投影類型。

> [!TIP]
> *投影類型*是以使用其 API 為目的透過執行階段類別的包裝程式。 *投影介面*是透過 Windows 執行階段介面的包裝程式。

上述的程式碼範例中，初始化 C++/WinRT 後，我們透過其公開記載於文件的建構函式之一 (在此範例中，[**Uri(String)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_)) 建構**Uri** 投影類型。 針對這點，最常見的使用案例就是您需要執行的。

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>如果在 Windows 執行階段元件中實作 API
不論您是自己撰寫元件，或由廠商提供，本節皆適用。

> [!NOTE]
> 如需有關目前可用的 C++/WinRT Visual Studio 擴充功能 (VSIX) (其提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請查閱 [適用於 C++/WinRT 的 Visual Studio 支援，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

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

## <a name="important-apis"></a>重要 API
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>相關主題
* [在 C++/WinRT 中撰寫事件 ](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [XAML 控制項；繫結至一個 C++/WinRT 屬性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
