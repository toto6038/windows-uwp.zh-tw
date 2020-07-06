---
description: 可有效地繫結至 XAML 控制項屬性稱為「可觀察的」屬性。 本主題示範如何實作和使用可觀察屬性，以及如何將 XAML 控制項繫結至它。
title: XAML 控制項；繫結至一個 C++/WinRT 屬性
ms.date: 06/21/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 控制項, 繫結, 屬性
ms.localizationpriority: medium
ms.openlocfilehash: 12a20ae3df6ae83723550bf365aadab99b1b3b7b
ms.sourcegitcommit: 90fe7a9a5bfa7299ad1b78bbef289850dfbf857d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756525"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML 控制項；繫結至一個 C++/WinRT 屬性
可有效地繫結至 XAML 控制項屬性稱為「可觀察的」屬性。 這個主意是以軟體設計模式為基礎稱為「觀察者模式」。 本主題顯示在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中實作可觀察屬性，以及將 XAML 控制項繫結至這些屬性的方法 (如需背景資訊，請參閱[資料繫結](/windows/uwp/data-binding))。

> [!IMPORTANT]
> 如需一些基本概念和詞彙，以協助了解如何以 C++/WinRT 使用及撰寫執行階段類別，請參閱[使用 C++/WinRT 來使用 API](consume-apis.md) 和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-property"></a>對一個屬性來說，*可觀察*的意義是什麼？
比方說一個名為 **BookSku** 的執行階段類別有個名為 **Title** 的屬性。 如果 **BookSku** 選擇引發 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，每當變更 **Title** 的值時，則 **Title** 是可觀察的屬性。 它是 **BookSku** 的行為 (引發或不引發事件)，判斷是哪一個，如果有的話，其屬性則為可觀察的。

XAML 文字元素或控制項藉由擷取更新的值並且更新其本身以顯示新的值，可繫結至並處理這些事件。

> [!NOTE]
> 如需安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="create-a-blank-app-bookstore"></a>建立空白的應用程式 (Bookstore)
請先在 Microsoft Visual Studio 中，建立新的專案。 建立 **空白的應用程式 (C++/WinRT)** 專案，並將它命名為 *Bookstore*。

我們撰寫新的類別，代表擁有可觀察到標題屬性的一本書。 我們在相同的編譯單位裡撰寫和使用此類別。 但是，我們希望能從 XAML 繫結至此類別，且基於這個原因，它會是一個執行階段類別。 且我們會使用 C++/WinRT 撰寫和使用它。

撰寫新執行階段類別的第一個步驟，要將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。 請命名為 `BookSku.idl`。 刪除 `BookSku.idl`的預設內容，並在此執行階段類別宣告中貼上。

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
> 您的檢視模型類別 &mdash; 事實上，您在應用程式中宣告的任何執行階段類別&mdash; 不需要衍生自基底類別。 上面所宣告的 **BookSku** 類別是其範例。 它會實作一個介面，但它不會衍生自任何基底類別。
>
> 您在從基底類別衍生的應用程式中宣告的任何執行階段類別，都稱為「可組合」類別。 而且有以可組合的類別為主的條件約束。 若要讓應用程式通過 Visual Studio 和 Microsoft Store 所使用的 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)測試來驗證提交 (因而讓應用程式成功擷取到 Microsoft Store 中)，可組合的類別必須最終衍生自 Windows 基底類別。 這表示位於繼承階層根目錄的類別必須是源自 Windows.* 命名空間的類型。 如果您需要從基底類別衍生執行階段類別&mdash;例如，若要針對要衍生自的所有檢視模型實作 **BindableBase** 類別&mdash;則可衍生自 [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。
>
> 檢視模型是檢視的抽取，所以會直接繫結至檢視 (XAML 標記)。 資料模型是資料的抽取，而且只能從您的檢視模型取用，並不會直接繫結至 XAML。 因此，您可以宣告您的資料模型不是執行階段類別，而是 C++ 結構或類別。 它們不需在 MIDL 中宣告，而且您可以隨意使用您喜歡的任何繼承階層。

儲存檔案並建置專案。 在建置程序期間，會執行 `midl.exe` 工具，以建立 Windows 執行階段中繼資料檔案 (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)，其會描述執行階段類別。 然後，執行 `cppwinrt.exe` 工具產生原始碼檔案在撰寫和使用執行階段類別中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的 **BookSku** 執行階段類別。 這些虛設常式為 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` 與 `BookSku.cpp`。

以滑鼠右鍵按一下專案節點，然後按一下 [在檔案總管中開啟資料夾]。 這會在檔案總管中開啟專案資料夾。 在那裏，從 `\Bookstore\Bookstore\Generated Files\sources\` 資料夾將虛設常式檔案 `BookSku.h` 和 `BookSku.cpp` 複製到專案資料夾中，也就是 `\Bookstore\Bookstore\`。 在 [方案總管] 中，選取專案資料夾後，確定 [顯示所有檔案] 已切換成開啟。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 [加入至專案]。

## <a name="implement-booksku"></a>執行 **BookSku**
現在要開啟 `\Bookstore\Bookstore\BookSku.h` 與 `BookSku.cpp`，並實作我們的執行階段類別。 在 `BookSku.h` 中，進行下列變更。

- 新增採用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 值的函式。 此值是標題字串。
- 新增私人成員以儲存標題字串。
- 在標題變更時，為我們將引發的事件新增另一個私人成員。

進行這些變更之後，您的 `BookSku.h` 看起來像這樣。

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
#include "BookSku.g.cpp"

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

在 **Title** 更動子函式裡，我們檢查是否設定了與目前值不同的值。 如果是，則更新標題並使用與變更過的屬性名稱相同的引數引發 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件。 這是為了讓使用者介面 (UI) 知道要重新查詢哪些屬性的值。

## <a name="declare-and-implement-bookstoreviewmodel"></a>宣告和實作 **BookstoreViewModel**
我們的主要 XAML 頁面會繫結至主要檢視模型。 且檢視模型會有幾個屬性，包括類型之一的 **BookSku**。 在此步驟，我們會宣告並實作主要檢視模型執行階段類別。

新增一個 **Midl 檔案 (.idl)** 項目名為 `BookstoreViewModel.idl`。 另請參閱[將執行階段類別分解成 Midl 檔案 (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)。

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

儲存並建置。 從 `Generated Files\sources` 資料夾將 `BookstoreViewModel.h` 與 `BookstoreViewModel.cpp` 複製到專案資料夾，在專案中包含它們。 開啟那些檔案，並實作如下所示的執行階段類別。 請注意，我們在 `BookstoreViewModel.h` 中如何宣告 `BookSku.h`，其會宣告 **BookSku** 的實作類型 (也就是 **winrt::Bookstore::implementation::BookSku**)。 我們會從預設的建構函式中移除 `= default`。

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
#include "BookstoreViewModel.g.cpp"

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
> `m_bookSku` 的類型是投影類型 (**winrt::Bookstore::BookSku**)，且您搭配 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 使用的範本參數是實作類型 (**winrt::Bookstore::implementation::BookSku**)。 即便如此，**make** 傳回投影類型的執行個體。

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>將類型的屬性 **BookstoreViewModel** 新增至 **MainPage**
開放 `MainPage.idl`，其宣告代表我們主要 UI 頁面的執行階段類別。 新增一個匯入陳述式以匯入 `BookstoreViewModel.idl`，並新增一個名為類型 **BookstoreViewModel** MainViewModel 的唯讀屬性。 也會移除 **MyProperty** 屬性。 也請注意下面清單中的 `import` 指示詞。

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

儲存檔案。 此專案此刻不會建置完成，但立即建置是實用的做法，因為它會重新產生原始程式碼檔案，而 **MainPage** 執行階段類別會在其中實作 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 和 `MainPage.cpp`)。 因此繼續執行並立即建置。 您預期在這個階段看見的建置錯誤為 **'MainViewModel': 不是 'winrt::Bookstore::implementation::MainPage' 的成員**。

如果您省略包含 `BookstoreViewModel.idl` (請查看上面的 `MainPage.idl` 清單)，則會看到 **\<"MainViewModel" 附近**的預期錯誤。 確定您將所有類型都留在相同的命名空間中的另一個秘訣：未顯示在程式碼清單中的命名空間。

若要解決我們預期會看見的錯誤，您現在必須從所產生的檔案 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`和 `MainPage.cpp`) 複製 **MainViewModel** 屬性的存取子虛設常式，並貼到 `\Bookstore\Bookstore\MainPage.h` 和 `MainPage.cpp` 中。 接下來說明這麼做的步驟。

在 `\Bookstore\Bookstore\MainPage.h` 中包含 `BookstoreViewModel.h`，其會宣告 **BookstoreViewModel** 的實作類型 (也就是 **winrt::Bookstore::implementation::BookstoreViewModel**)。 新增私用成員以儲存檢視模型。 請注意，按照 **BookstoreViewModel** (這是 **Bookstore::BookstoreViewModel**) 的投影類型，實作屬性存取子函式 (以及成員 m_mainViewModel)。 實作類型是在與應用程式相同的專案 (編譯單位) 中，因此我們透過採用 **std::nullptr_t** 的建構函式多載建構 m_mainViewModel。 也會移除 **MyProperty** 屬性。

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

在 `\Bookstore\Bookstore\MainPage.cpp` 中，呼叫 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (使用實作類型) 將一個投影類型的新執行個體指派給 m_mainViewModel。 指派一個初始值給本書的標題。 適用於 MainViewModel 屬性的實作存取子。 最後，更新按鈕事件處理常式中的本書標題。 也會移除 **MyProperty** 屬性。

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

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
開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 如以下清單所示，從按鈕移除名稱，然後從一個常值變更其 **Content** 屬性為繫結運算式。 注意繫結運算式上的 `Mode=OneWay` (單向從檢視模型到 UI)。 沒有該屬性，UI 不會回應變更事件的屬性。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

現在建置並執行專案。 按一下按鈕執行 [按一下] 事件處理常式。 該處理常式呼叫本書標題更動子函式；該更動子引發一個事件，讓 UI 知道 **Title** 屬性已變更；且按鈕重新查詢該屬性的值，更新其自身的 **Content** 值。

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>使用 {Binding} 標記延伸搭配 C++/WinRT
對於目前發行的 C++/WinRT 版本，為了能夠使用 {Binding} 標記延伸，您必須實作 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 和 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)介面。

## <a name="element-to-element-binding"></a>元素繫結

您可以將一個 XAML 元素的屬性繫結至另一個 XAML 元素的屬性。 以下是其在標記中的作法範例。

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

您必須將具名的 XAML 實體 `myTextBox` 宣告為 Midl 檔 (.idl) 中的唯讀屬性。

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

以下是這項必要性的原因。 所有 XAML 編譯器需要驗證的類型 (包括用於 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 的類型)，都會從 Windows 中繼資料 (WinMD) 進行讀取。 您只需要將唯讀屬性新增至 Midl 檔即可。 無須進行實作，因為自動產生的 XAML 後方程式碼會為您提供實作。

## <a name="consuming-objects-from-xaml-markup"></a>取用 XAML 標記中的物件

使用 XAML [ **{x:Bind} 標記延伸**](/windows/uwp/xaml-platform/x-bind-markup-extension) 取用的所有實體都必須公開於 IDL 中。 此外，如果 XAML 標記包含另一個也在標記中的元素參考，則該標記的 getter 必須存在於 IDL 中。

```xaml
<Page x:Name="MyPage">
    <StackPanel>
        <CheckBox x:Name="UseCustomColorCheckBox" Content="Use custom color"
             Click="UseCustomColorCheckBox_Click" />
        <Button x:Name="ChangeColorButton" Content="Change color"
            Click="{x:Bind ChangeColorButton_OnClick}"
            IsEnabled="{x:Bind UseCustomColorCheckBox.IsChecked.Value, Mode=OneWay}"/>
    </StackPanel>
</Page>
```

ChangeColorButton 元素會透過繫結參考 *UseCustomColorCheckBox* 元素。 因此，此頁面的 IDL 必須宣告名為 *UseCustomColorCheckBox* 的唯讀屬性，以便可供繫結存取。

*UseCustomColorCheckBox* 的 Click 事件處理常式委派會使用傳統 XAML 委派語法，因此不需要 IDL 中的項目；它只需在您的實作類別中是公用的。 另一方面，*ChangeColorButton* 也有 `{x:Bind}` Click 事件處理常式，其也必須放入 IDL 中。

```idl
runtimeclass MyPage : Windows.UI.Xaml.Controls.Page
{
    MyPage();

    // These members are consumed by binding.
    void ChangeColorButton_OnClick();
    Windows.UI.Xaml.Controls.CheckBox UseCustomColorCheckBox{ get; };
}
```

您不需要提供 **UseCustomColorCheckBox** 屬性的實作。 XAML 程式碼產生器會為您執行此動作。

### <a name="binding-to-boolean"></a>繫結至布林值

您可以在診斷模式下執行此動作。

<syntaxhighlight lang="xml">
<TextBlock Text="{Binding CanPair}"/>
</syntaxhighlight>

這會在 C++/CX 中顯示 `true` 或 `false`，但在 C++/WinRT 中顯示 **Windows.Foundation.IReference`1<Boolean>** 。

繫結至布林值時使用 `x:Bind`。

```xaml
<TextBlock Text="{x:Bind CanPair}"/>
```

## <a name="important-apis"></a>重要 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [使用 C++/WinRT 撰寫 API](author-apis.md)
