---
author: stevewhims
description: 可有效地繫結至 XAML 控制項屬性稱為*可觀察的*屬性。 本主題顯示實作和使用可觀察屬性的方法，以及如何將 XAML 控制項繫結至它。
title: XAML 控制項；繫結至一個 C++/WinRT 屬性
ms.author: stwhi
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 控制, 繫結, 屬性
ms.localizationpriority: medium
ms.openlocfilehash: f2b9d342e775b2834c6b3e7eb02a8b2e3d71728d
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2018
ms.locfileid: "4309218"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML 控制項；繫結至一個 C++/WinRT 屬性
可有效地繫結至 XAML 控制項屬性稱為*可觀察的*屬性。 這個主意是以軟體設計模式為基礎稱為*觀察者模式*。 本主題示範如何實作中的可觀察屬性[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，以及如何將 XAML 控制項繫結至它們。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-property"></a>對一個屬性來說，*可觀察*的意義是什麼？
比方說一個名為 **BookSku** 的執行階段類別有個名為 **Title** 的屬性。 如果 **BookSku** 選擇引發 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，每當變更 **Title** 的值時，則 **Title** 是可觀察的屬性。 它是 **BookSku** 的行為 (引發或不引發事件)，判斷是哪一個，如果有的話，其屬性則為可觀察的。

XAML 文字元素或控制項藉由擷取更新的值並且更新其本身以顯示新的值，可繫結至並處理這些事件。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="create-a-blank-app-bookstore"></a>建立空白的應用程式 (Bookstore)
在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立**Visual c + +** > **Windows 通用** > **空白的應用程式 (C + + /winrt)** 專案，並將它命名為*Bookstore*。

我們撰寫新的類別，代表擁有可觀察到標題屬性的一本書。 我們在相同的編譯單位裡撰寫和使用此類別。 但是，我們希望能從 XAML 繫結至此類別，且基於這個原因，它會是一個執行階段類別。 且我們會使用 C++/WinRT 撰寫和使用它。

撰寫新執行階段類別的第一個步驟是將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。 將它名為 `BookSku.idl`。 刪除 `BookSku.idl`的預設內容，並在此執行階段類別宣告中貼上。

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> 您的檢視模型類別&mdash;事實上，在您的應用程式中宣告的任何執行階段類別&mdash;不需要是衍生自基底類別。 上述宣告的**BookSku**類別是該範例。 它實作的介面，但它不會從任何基底類別衍生。
>
> 您在應用程式中宣告的任何執行階段類別*會*衍生自基底類別稱為*可組合*類別。 也可組合類別周圍的限制式。 應用程式要通過[Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)測試使用 Visual studio 和 Microsoft store 來驗證提交 (因而成功內嵌到 Microsoft Store 應用程式)，可組合類別必須最終從 Windows 的基底類別衍生。 這表示在非常根目錄的類別繼承階層的必須來自 windows.* 命名空間的類型。 如果您真的是一個執行階段類別衍生自基底類別&mdash;例如，若要實作的檢視模型是衍生自所有的**Quizgame**類別&mdash;，然後您可以從[**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)衍生。
>
> 檢視模型是檢視的抽象概念，因此它會繫結，直接到檢視 （在 XAML 標記）。 資料模型的抽象概念的資料，而且它取用只從檢視模型，以及不直接繫結至 XAML。 因此，您可以宣告資料模型，不是執行階段類別，但為 c + + 結構或類別。 不需要在 MIDL 中進行宣告，您可以隨意使用任何您喜歡的繼承階層決定。

儲存檔案並建置專案。 在建置程序期間，執行 `midl.exe` 工具建立描述執行階段類別的 Windows 執行階段中繼資料檔案 (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)。 然後，執行 `cppwinrt.exe` 工具產生原始碼檔案在撰寫和使用執行階段類別中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的 **BookSku** 執行階段類別。 這些虛設常式為 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` 與 `BookSku.cpp`。

從 `\Bookstore\Bookstore\Generated Files\sources\` 將虛設常式檔案 `BookSku.h` 和 `BookSku.cpp` 複製到專案資料夾中，也就是 `\Bookstore\Bookstore\`。 在 **\[方案總管\]** 中，確定 **\[顯示所有檔案\]** 已切換成開啟。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 **\[加入至專案\]**。

## <a name="implement-booksku"></a>執行 **BookSku**
現在，我們開啟 `\Bookstore\Bookstore\BookSku.h` 與 `BookSku.cpp` 並實作我們的執行階段類別。 在 `BookSku.h` 中，新增一個採用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 的建構函式，一個儲存標題字串的私用成員，以及另一個用於標題變更時，我們會引發的事件。 進行這些變更之後, 您`BookSku.h`看起來像這樣。

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
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
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

在**標題**更動子函式中，我們會檢查是否設定的值，此地址不同於目前的值。 而且，如果是的話，我們更新標題並也會引發[**inotifypropertychanged:: Propertychanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)事件已變更的屬性名稱相同的引數。 這是為了讓使用者介面 (UI) 知道要重新查詢哪些屬性的值。

## <a name="declare-and-implement-bookstoreviewmodel"></a>宣告和實作 **BookstoreViewModel**
我們的主要 XAML 頁面會繫結至主要檢視模型。 且檢視模型會有幾個屬性，包括類型之一的 **BookSku**。 在此步驟，我們會宣告並實作主要檢視模型執行階段類別。

新增一個 **Midl 檔案 (.idl)** 項目名為 `BookstoreViewModel.idl`。

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

儲存並建置。 從 `Generated Files` 資料夾將 `BookstoreViewModel.h` 與 `BookstoreViewModel.cpp` 複製到專案資料夾，在專案中包含它們。 打開那些檔案，並實作執行階段類別，如下所示。 附註解，請在`BookstoreViewModel.h`，我們要加入`BookSku.h`，其宣告實作類型 (**winrt::Bookstore::implementation::BookSku**)。 我們正在將預設建構函式還原藉由移除`= delete`。

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel final : BookstoreViewModelT<BookstoreViewModel>
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
開放 `MainPage.idl`，其宣告代表我們主要 UI 頁面的執行階段類別。 新增一個匯入陳述式匯入 `BookstoreViewModel.idl`，並新增一個名為類型 **BookstoreViewModel** MainViewModel 的唯讀屬性。 同時移除**MyProperty**屬性。 另請注意`import`指示詞，在下列清單中。

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

儲存檔案。 完成時，將不會建置專案，但現在建置很有用，是以執行步驟，因為它會重新產生原始碼檔案，在程式實作**MainPage**執行階段類別 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`和`MainPage.cpp`)。 因此，請繼續進行，現在建置。 若要查看在這個階段，您可以預期的建置錯誤是 **'MainViewModel': 不是 'winrt::Bookstore::implementation::MainPage' 隸屬**。

如果您省略所包含的`BookstoreViewModel.idl`(看到的清單`MainPage.idl`上方)，則您會看到錯誤**預期 \ < 附近 」 MainViewModel 」**。 另一個提示是確定您在同一個命名空間中保留所有類型： 會顯示在程式碼清單中的命名空間。

若要解決這個錯誤，我們會看見，您將現在需要複製存取子虛設常式適用於**MainViewModel**屬性，從產生的檔案 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`和`MainPage.cpp`) 到`\Bookstore\Bookstore\MainPage.h`和`MainPage.cpp`。

在`\Bookstore\Bookstore\MainPage.h`，包括`BookstoreViewModel.h`，其宣告實作類型 (**winrt::Bookstore::implementation::BookstoreViewModel**)。 加入私用成員儲存檢視模型。 請注意，按照 **Bookstore::BookstoreViewModel**，即為投影類型，實作屬性存取子函式 (以及 member m_mainViewModel)。 實作類型是在同一個專案 （編譯單位） 中為應用程式，因此我們建構 m_mainViewModel 的建構函式多載，透過`nullptr_t`。 同時移除**MyProperty**屬性。

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

在`\Bookstore\Bookstore\MainPage.cpp`，呼叫[**winrt:: make**](/uwp/cpp-ref-for-winrt/make) （使用實作類型），將投影類型的新執行個體指派給 m_mainViewModel。 指派一個初始值給本書的標題。 適用於 MainViewModel 屬性的實作存取子。 最後，更新按鈕事件處理常式中的本書標題。 同時移除**MyProperty**屬性。

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>將按鈕繫結至 **Title** 屬性
開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 在下面清單中所示，從按鈕移除名稱，並將其**內容**的屬性值從一個常變更繫結運算式。 注意繫結運算式上的 `Mode=OneWay` (單向從檢視模型到 UI)。 沒有該屬性，UI 不會回應變更事件的屬性。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

現在建置並執行專案。 按一下按鈕執行 **按一下** 事件處理常式。 該處理常式呼叫本書標題更動子函式；該更動子引發一個事件，讓 UI 知道 **Title** 屬性已變更；且按鈕重新查詢該屬性的值，更新其自身的 **Content** 值。

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>使用 {Binding} 標記延伸搭配 C + + /winrt
目前已發行版本的 C + + /winrt，才能使用 {Binding} 標記延伸，您將需要實作的[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)和[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)介面。

## <a name="important-apis"></a>重要 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [使用 C++/WinRT 撰寫 API ](author-apis.md)
