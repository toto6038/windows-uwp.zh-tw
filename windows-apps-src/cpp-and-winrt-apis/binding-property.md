---
description: 可有效地繫結至 XAML 控制項屬性稱為*可觀察的*屬性。 本主題示範如何實作和使用可觀察屬性，以及如何將 XAML 控制項繫結至它。
title: XAML 控制項；繫結一個 C++/WinRT 屬性
ms.date: 08/21/2018
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 控制, 繫結, 屬性
ms.localizationpriority: medium
ms.openlocfilehash: 9bdbfef54b799f8dff23ad739007cec9fef98af8
ms.sourcegitcommit: c315ec3e17489aeee19f5095ec4af613ad2837e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2019
ms.locfileid: "58921724"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML 控制項；繫結一個 C++/WinRT 屬性
可有效地繫結至 XAML 控制項屬性稱為*可觀察的*屬性。 這個主意是以軟體設計模式為基礎稱為*觀察者模式*。 本主題說明如何實作可觀察的屬性，在[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，以及如何在 XAML 控制項繫結至它們。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-property"></a>對一個屬性來說，*可觀察*的意義是什麼？
比方說一個名為 **BookSku** 的執行階段類別有個名為 **Title** 的屬性。 如果 **BookSku** 選擇引發 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，每當變更 **Title** 的值時，則 **Title** 是可觀察的屬性。 它是 **BookSku** 的行為 (引發或不引發事件)，判斷是哪一個，如果有的話，其屬性則為可觀察的。

XAML 文字元素或控制項藉由擷取更新的值並且更新其本身以顯示新的值，可繫結至並處理這些事件。

> [!NOTE]
> 如需安裝和使用的資訊C++WinRT Visual Studio 擴充功能 (VSIX) 和 NuGet 套件 （其同時提供專案範本，並建置支援），請參閱[Visual Studio 支援C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="create-a-blank-app-bookstore"></a>建立空白的應用程式 (Bookstore)
在 Microsoft Visual Studio 中，從建立新的專案開始。 建立**Visual C++**   >  **Windows Universal** > **空白應用程式 (C++/WinRT)** 專案，並將它命名*Bookstore*。

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
> 您的檢視模型類別&mdash;事實上，您在您的應用程式中宣告的任何執行階段類別&mdash;需要不是衍生自基底類別。 **BookSku**上面所宣告的類別是範例。 它會實作一個介面，但它不是衍生自任何基底類別。
>
> 您在應用程式中宣告的任何執行階段類別的*並未*衍生自基底類別稱為*組合*類別。 並且有條件約束可組合的類別。 將應用程式[Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)測試來驗證提交由 Visual Studio 和 Microsoft Store (因此成功擷取到 Microsoft Store 應用程式)、可組合的類別必須最終衍生自 Windows 的基底類別。 這表示繼承階層架構類別非常的根目錄必須源自 windows.* 命名空間中的類型。 如果您需要衍生自基底類別的執行階段類別&mdash;例如，若要實作**BindableBase**類別衍生自您檢視模型的所有&mdash;則可衍生自[ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。
>
> 檢視模型是抽象的檢視，並因此它直接繫結至檢視 （XAML 標記）。 資料模型是抽象的資料，以及它使用只能從您的檢視模型，並不直接繫結至 XAML。 因此，宣告您的資料模型，而不是執行階段類別，C++結構或類別。 它們不需要宣告於 MIDL，而且您可以隨意使用任何您喜歡的繼承階層架構。

儲存檔案並建置專案。 在建置程序期間，執行 `midl.exe` 工具建立描述執行階段類別的 Windows 執行階段中繼資料檔案 (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)。 然後，執行 `cppwinrt.exe` 工具產生原始碼檔案在撰寫和使用執行階段類別中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的 **BookSku** 執行階段類別。 這些虛設常式為 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` 與 `BookSku.cpp`。

以滑鼠右鍵按一下專案節點，然後按一下 [**在檔案總管] 中開啟資料夾**。 這會開啟檔案總管 中的專案資料夾。 Stub 檔案，複製`BookSku.h`並`BookSku.cpp`從`\Bookstore\Bookstore\Generated Files\sources\`資料夾到專案資料夾中，這是和`\Bookstore\Bookstore\`。 在 [**方案總管] 中**，選取專案節點，請確定**顯示所有檔案**上切換。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 **\[加入至專案\]**。

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

在  **Title** mutator 函式中，我們會檢查是否為其設定值，不同於目前的值。 而且，如果是，我們更新的標題，也會引發[ **INotifyPropertyChanged::PropertyChanged** ](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)事件引數等於已變更之屬性的名稱。 這是為了讓使用者介面 (UI) 知道要重新查詢哪些屬性的值。

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

儲存並建置。 從 `Generated Files` 資料夾將 `BookstoreViewModel.h` 與 `BookstoreViewModel.cpp` 複製到專案資料夾，在專案中包含它們。 開啟這些檔案，並實作的執行階段類別，如下所示。 請注意如何，請在`BookstoreViewModel.h`，我們加入`BookSku.h`，其中宣告的實作類型 (**winrt::Bookstore::implementation::BookSku**)。 我們要還原預設建構函式，藉由移除和`= delete`。

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
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> 型別`m_bookSku`是投影的型別 (**winrt::Bookstore::BookSku**)，以及您使用的範本參數[ **winrt::make** ](/uwp/cpp-ref-for-winrt/make)是實作類型 (**winrt::Bookstore::implementation::BookSku**)。 即便如此，**make** 傳回投影類型的執行個體。

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>新增類型的屬性 **BookstoreViewModel** 至 **MainPage**
開放 `MainPage.idl`，其宣告代表我們主要 UI 頁面的執行階段類別。 新增一個匯入陳述式匯入 `BookstoreViewModel.idl`，並新增一個名為類型 **BookstoreViewModel** MainViewModel 的唯讀屬性。 也會移除**MyProperty**屬性。 也請注意`import`下方清單中的指示詞。

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

儲存檔案。 專案不會建置目前的完成，但現在建置是有用的事情，因為它會重新產生所在的原始程式碼檔**MainPage**執行階段類別實作 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`和`MainPage.cpp`)。 因此請繼續並立即建置。 若要查看在這個階段，您可以預期的建置錯誤是 **'MainViewModel': 不是成員的 'winrt::Bookstore::implementation::MainPage'**。

如果您省略的 include `BookstoreViewModel.idl` (查看的清單`MainPage.idl`上方)，則您會看到錯誤**預期\<附近"MainViewModel"**。 請確定您將所有類型都保留在相同的命名空間中為另一個秘訣： 所示的程式碼清單的命名空間。

若要解決我們預期會看見此錯誤，您現在必須將複製的存取子虛設常式**MainViewModel**屬性從產生的檔案 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`並`MainPage.cpp`)，並`\Bookstore\Bookstore\MainPage.h`和`MainPage.cpp`。

在  `\Bookstore\Bookstore\MainPage.h`，包括`BookstoreViewModel.h`，其中宣告的實作類型 (**winrt::Bookstore::implementation::BookstoreViewModel**)。 加入私用成員，才能儲存檢視的模型。 請注意，按照 **Bookstore::BookstoreViewModel**，即為投影類型，實作屬性存取子函式 (以及 member m_mainViewModel)。 實作類型是在相同的專案 （編譯單位） 為應用程式，因此我們建構 m_mainViewModel 透過建構函式多載， `nullptr_t`。 也會移除**MyProperty**屬性。

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

在  `\Bookstore\Bookstore\MainPage.cpp`，呼叫[ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) （與實作型別） 將投影類型的新執行個體指派給 m_mainViewModel。 指派一個初始值給本書的標題。 適用於 MainViewModel 屬性的實作存取子。 最後，更新按鈕事件處理常式中的本書標題。 也會移除**MyProperty**屬性。

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
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
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
開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 下列清單所示，在按鈕中，移除名稱，並變更其**內容**常值從繫結運算式的屬性值。 注意繫結運算式上的 `Mode=OneWay` (單向從檢視模型到 UI)。 沒有該屬性，UI 不會回應變更事件的屬性。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

現在建置並執行專案。 按一下按鈕執行 **按一下** 事件處理常式。 該處理常式呼叫本書標題更動子函式；該更動子引發一個事件，讓 UI 知道 **Title** 屬性已變更；且按鈕重新查詢該屬性的值，更新其自身的 **Content** 值。

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>使用 {Binding} 標記延伸模組搭配C++/WinRT
目前發行版本的C++/WinRT，為了能夠使用 {Binding} 標記延伸模組必須實作[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)並[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)介面。

## <a name="important-apis"></a>重要 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 來使用 API](consume-apis.md)
* [使用 C++/WinRT 撰寫 API](author-apis.md)
