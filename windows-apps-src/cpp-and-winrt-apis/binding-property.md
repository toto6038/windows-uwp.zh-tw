---
author: stevewhims
description: 可有效地繫結至 XAML 控制項屬性稱為*可觀察的*屬性。 本主題顯示實作和使用可觀察屬性的方法，以及如何將 XAML 控制項繫結至它。
title: XAML 控制項；繫結至一個 C++/WinRT 屬性
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 控制, 繫結, 屬性
ms.localizationpriority: medium
ms.openlocfilehash: 25ea4c4caf5135b13b88eeea6f43bb36bd691c11
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2018
ms.locfileid: "1863214"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>XAML 控制項；繫結至一個 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 屬性
可有效地繫結至 XAML 控制項屬性稱為*可觀察的*屬性。 這個主意是以軟體設計模式為基礎稱為*觀察者模式*。 本主題顯示在 C++/WinRT 中實作和可觀察屬性的方法，以及如何將 XAML 控制項繫結至它們。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-property"></a>對一個屬性來說，*可觀察*的意義是什麼？
比方說一個名為 **BookSku** 的執行階段類別有個名為 **Title** 的屬性。 如果 **BookSku** 選擇引發 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，每當變更 **Title** 的值時，則 **Title** 是可觀察的屬性。 它是 **BookSku** 的行為 (引發或不引發事件)，判斷是哪一個，如果有的話，其屬性則為可觀察的。

XAML 文字元素或控制項藉由擷取更新的值並且更新其本身以顯示新的值，可繫結至並處理這些事件。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="create-a-blank-app-bookstore"></a>建立空白的應用程式 (Bookstore)
在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立 **Visual C++ 空白的應用程式 (C++/WinRT)** 專案，並將它命名為 *Bookstore*。

我們撰寫新的類別，代表擁有可觀察到標題屬性的一本書。 我們在相同的編譯單位裡撰寫和使用此類別。 但是，我們希望能從 XAML 繫結至此類別，且基於這個原因，它會是一個執行階段類別。 且我們會使用 C++/WinRT 撰寫和使用它。

撰寫新執行階段類別的第一個步驟是將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。 將它名為 `BookSku.idl`。 刪除 `BookSku.idl`的預設內容，並在此執行階段類別宣告中貼上。

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.DependencyObject, Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!IMPORTANT]
> 針對通過 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)測試，並成功內嵌到 Microsoft Store 的應用程式，每個執行階段類別的最終基底類別*在應用程式中宣告*必須是來自 Windows.* 命名空間的類型。

若要滿足該需求，從 [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject) 衍生您的檢視模型類別。 或者，宣告一個從 **DependencyObject** 衍生的可繫結基底類別，並從其中衍生您的檢視模式。 您可以宣告資料模型為 C++ 結構；不需要在 MIDL 中宣告它們 (只要將您只從檢視模型使用它們且不直接將 XAML 繫結至它們；在此情況下根據定義，它們可說是檢視模型)。

儲存檔案並建置專案。 在建置程序期間，執行 `midl.exe` 工具建立描述執行階段類別的 Windows 執行階段中繼資料檔案 (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)。 然後，執行 `cppwinrt.exe` 工具產生原始碼檔案在撰寫和使用執行階段類別中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的 **BookSku** 執行階段類別。 這些虛設常式為 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` 與 `BookSku.cpp`。

從 `\Bookstore\Bookstore\Generated Files\sources\` 將虛設常式檔案 `BookSku.h` 和 `BookSku.cpp` 複製到專案資料夾中，也就是 `\Bookstore\Bookstore\`。 在 **\[方案總管\]** 中，確定 **\[顯示所有檔案\]** 已切換成開啟。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 **\[加入至專案\]**。

## <a name="implement-booksku"></a>執行 **BookSku**
現在，我們開啟 `\Bookstore\Bookstore\BookSku.h` 與 `BookSku.cpp` 並實作我們的執行階段類別。 在 `BookSku.h` 中，新增一個採用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 的建構函式，一個儲存標題字串的私用成員，以及另一個用於標題變更時，我們會引發的事件。 新增之後, 您的 `BookSku.h`外觀如下。

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(hstring const& title);

        hstring Title();
        void Title(hstring const& value);
        event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(event_token const& token);
    
    private:
        hstring title;
        event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> propertyChanged;
    };
}
```

在 `BookSku.cpp` 中，如下實作函式。

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(hstring const& title)
    {
        Title(title);
    }

    hstring BookSku::Title()
    {
        return title;
    }

    void BookSku::Title(hstring const& value)
    {
        if (title != value)
        {
            title = value;
            propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(event_token const& token)
    {
        propertyChanged.remove(token);
    }
}
```

在 **Title** 更動子函式裡，我們檢查是否設定了不同的值，如果是，更新標題並使用與變更過的屬性名稱相同的引數引發 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件。 這是為了讓使用者介面 (UI) 知道要重新查詢哪些屬性的值。

## <a name="declare-and-implement-bookstoreviewmodel"></a>宣告和實作 **BookstoreViewModel**
我們的主要 XAML 頁面會繫結至主要檢視模型。 且檢視模型會有幾個屬性，包括類型之一的 **BookSku**。 在此步驟，我們會宣告並實作主要檢視模型執行階段類別。

新增一個 **Midl 檔案 (.idl)** 項目名為 `BookstoreViewModel.idl`。

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel : Windows.UI.Xaml.DependencyObject
    {
        BookSku BookSku{ get; };
    }
}
```

儲存並建置。 從 `Generated Files` 資料夾將 `BookstoreViewModel.h` 與 `BookstoreViewModel.cpp` 複製到專案資料夾，在專案中包含它們。 打開那些檔案，並實作執行階段類別如下。

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();
        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> `m_bookSku` 的類型是投影類型 (**winrt::Bookstore::BookSku**)，且您與 **make** 搭配使用的範本參數是實作類型 (**winrt::Bookstore::implementation::BookSku**)。 即便如此，**make** 傳回投影類型的執行個體。

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>新增類型的屬性 **BookstoreViewModel** 至 **MainPage**
開放 `MainPage.idl`，其宣告代表我們主要 UI 頁面的執行階段類別。 新增一個匯入陳述式匯入 `BookstoreViewModel.idl`，並新增一個名為類型 **BookstoreViewModel** MainViewModel 的唯讀屬性。

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace BookstoreCPPWinRT
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

重新建置專案產生原始碼檔案，在此其中執行 **MainPage** 執行階段類別 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 和 `MainPage.cpp`)。 將 ViewModel 屬性的存取子虛設常式，從產生的檔案複製出來並複製到 `\Bookstore\Bookstore\MainPage.h` 與 `MainPage.cpp`。

至 `\Bookstore\Bookstore\MainPage.h`，新增私用成員儲存檢視模型。 請注意，按照 **Bookstore::BookstoreViewModel**，即為投影類型，實作屬性存取子函式 (以及 member m_mainViewModel)。 實作類型是在同一個專案 (編譯單位) 裡，因此我們透過採用 `nullptr_t` 的建構函式多載建構 m_mainViewModel。

```cppwinrt
// MainPage.h
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

    private:
        void ClickHandler(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args);

        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };

        friend class MainPageT<MainPage>;
    };
}
...
```

在 `\Bookstore\Bookstore\MainPage.cpp` 中，包括 `BookstoreViewModel.h`，其宣告實作類型。 呼叫 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (使用實作類型) 將一個投影類型的新執行個體指派給 m_mainViewModel。 指派一個初始值給本書的標題。 適用於 MainViewModel 屬性的實作存取子。 最後，更新按鈕事件處理常式中的本書標題。

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "BookstoreViewModel.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }

    void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>將按鈕繫結至 **Title** 屬性
開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 從按鈕移除名稱，然後從一個常值變更其 **Content** 屬性為繫結運算式。 注意繫結運算式上的 `Mode=OneWay` (單向從檢視模型到 UI)。 沒有該屬性，UI 不會回應變更事件的屬性。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

現在建置並執行專案。 按一下按鈕執行 **按一下** 事件處理常式。 該處理常式呼叫本書標題更動子函式；該更動子引發一個事件，讓 UI 知道 **Title** 屬性已變更；且按鈕重新查詢該屬性的值，更新其自身的 **Content** 值。

## <a name="important-apis"></a>重要 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [使用 C++/WinRT 撰寫 API ](author-apis.md)
