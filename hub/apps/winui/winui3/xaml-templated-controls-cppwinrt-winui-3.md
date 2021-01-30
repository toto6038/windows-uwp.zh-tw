---
description: 本文會逐步引導您使用 C++/WinRT 建立適用於 WinUI 3 的 XAML 樣板化控制項。
title: 使用 C++/WinRT 製作的適用於 WinUI 3 應用程式的樣板化 XAML 控制項
ms.date: 07/09/2020
ms.topic: article
keywords: windows 10, uwp, 自訂控制項, 樣板化控制項, winui, C++/WinRT
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: d319374791eeb0a02b0291c66f25f55e31bcbc4b
ms.sourcegitcommit: 6759309a3fbb6ede498c95c04c05f57a074ab070
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99069165"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-cwinrt"></a>使用 C++/WinRT 製作的適用於 WinUI 3 應用程式的樣板化 XAML 控制項

本文會逐步引導您使用 C++/WinRT 建立適用於 WinUI 3 的樣板化 XAML 控制項。 樣板化控制項繼承自 **Microsoft.UI.Xaml.Controls.Control**，並且具有可使用 XAML 控制項範本自訂的視覺效果結構和視覺效果行為。 本文描述與[使用 C++/WinRT 的 XAML 自訂 (樣板化) 控制項](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl)一文相同的案例，但是已調整為使用 WinUI 3。

在遵循本文中的步驟之前，您應該確定您的開發環境已設定為建立 WinUI 3 應用程式。 如需設定資訊，請參閱[開始使用適用於桌面應用程式的 WinUI 3](./get-started-winui3-for-desktop.md)。 您也必須從 [Visual Studio Marketplace](https://marketplace.visualstudio.com)下載並安裝最新版的 [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>建立空白應用程式 (BgLabelControlApp)

請先在 Microsoft Visual Studio 中，建立新的專案。 在 [`Create a new project`] 對話方塊中，選取 [空白應用程式 (UWP 中的 WinUI)] 專案範本，並務必選取 C++ 語言版本。 將專案名稱設定為 "BgLabelControlApp"，讓檔案名稱與下列範例中的程式碼一致。 將 [目標版本] 設定為 Windows 10 版本 1903 (組建 18362)，並將 [最低版本] 設定為 Windows 10 版本 1803 (組建 17134)。 本逐步解說也適用於使用 **已封裝的空白應用程式 (WinUI in Desktop)** 專案範本所建立的桌面應用程式，只是請務必執行 **BgLabelControlApp (Desktop)** 專案中的所有步驟。

![空白應用程式專案範本](images/WinUI-cpp-newproject-UWP.png)

## <a name="add-a-templated-control-to-your-app"></a>將樣板化控制項新增至您的應用程式

若要新增樣板化控制項，請按一下工具列中的 [專案] 功能表，或以滑鼠右鍵按一下 [方案總管] 中的專案，然後選取 [新增項目]。 在 [Visual C++ -> WinUI] 下選取 [自訂控制項 (WinUI)] 範本。 將新控制項命名為 "BgLabelControl"，然後按一下 [新增]。 這會將三個新檔案新增至您的專案。 `BgLabelControl.h` 是包含控制項宣告的標頭，而 `BgLabelControl.cpp` 包含控制項的 C++/WinRT 實作。 `BgLabelControl.idl` 是介面定義檔，可讓控制項具現化為執行階段類別。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>實作 BgLabelControl 自訂控制項類別

在下列步驟中，您將會更新專案目錄中 `BgLabelControl.idl`、`BgLabelControl.h` 和 `BgLabelControl.cpp` 檔案中的程式碼，以實作執行階段類別。 


樣板化控制項類別會從 XAML 標記具現化，基於這個原因，這會是執行階段類別。 當您建置完成的專案時，MIDL 編譯器 (midl.exe) 會使用 `BgLabelControl.idl` 檔案來產生控制項的 Windows 執行階段中繼資料檔案 (.winmd)，元件的取用者會參考此檔案。 如需建立執行階段類別的詳細資訊，請參閱[使用 C++/WinRT 撰寫 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

我們所建立的樣板化控制項將會公開單一屬性，這是字串，將會當做控制項的標籤使用。 以下列程式碼取代 `BgLabelControl.idl` 的內容。

```cppwinrt
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Microsoft.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Microsoft.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

上列清單顯示的是在宣告相依性屬性 (DP) 時所遵循的模式。 每個 DP 都有兩個物件。 首先要宣告 DependencyProperty 類型的唯讀靜態屬性。 其名稱是您的 DP 加上屬性。 在實作中，要使用這個靜態屬性。 再來請使用您的 DP 類型和名稱，來宣告讀寫執行個體的屬性。 如果想要撰寫附加屬性 (而非 DP)，請參閱[自訂附加屬性](/windows/uwp/xaml-platform/custom-attached-properties)中的程式碼範例。

請注意，上述程式碼中所參考的 XAML 類別是在 Microsoft.UI.Xaml 命名空間中。 如此便得以將其區分為 WinUI 控制項與 UWP XAML 控制項，這會在 Windows.UI.XAML 命名空間中定義。

將 BgLabelControl.h 的內容取代為下列程式碼。

```cppwinrt
// BgLabelControl.h
#pragma once
#include "BgLabelControl.g.h"

namespace winrt::BgLabelControlApp::implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl>
    {
        BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

        winrt::hstring Label()
        {
            return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
        }

        void Label(winrt::hstring const& value)
        {
            SetValue(m_labelProperty, winrt::box_value(value));
        }

        static Microsoft::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const&, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Microsoft::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

上面顯示的程式碼會實作 **Label** 和 **LabelProperty** 屬性，新增名為 **OnLabelChanged** 的靜態事件處理常式，來處理相依性屬性值的變更，再新增私用成員以儲存 **LabelProperty** 的支援欄位。 同樣地請注意，標頭檔中參考的 XAML 類別是在屬於 WinUI 3 架構的 Microsoft.UI.Xaml 命名空間中，而不是 UWP UI 架構所使用的 Windows.UI.Xaml 命名空間。


接下來，將 BgLabelControl.cpp 的內容取代為下列程式碼。

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#if __has_include("BgLabelControl.g.cpp")
#include "BgLabelControl.g.cpp"
#endif

namespace winrt::BgLabelControlApp::implementation
{
    Microsoft::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Microsoft::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Microsoft::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Microsoft::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const& d, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
    {
        if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
        {
            // Call members of the projected type via theControl.

            BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
            // Call members of the implementation type via ptr.
        }
    }
}
```
此逐步解說不會使用 **OnLabelChanged** 回呼，但是會提供，讓您可以查看如何使用屬性變更的回呼來註冊相依性屬性。 **OnLabelChanged** 的實作，同樣也會顯示出如何從基底投影類型取得衍生投影類型 (在此範例中，基底投影類型是 DependencyObject)。 也會顯示如何取得用於實作投影類型的指標。 第二項作業自然只能在實作投影類型的專案 (也就是實作執行階段類別的專案) 中進行。

[xaml_typename](/uwp/cpp-ref-for-winrt/xaml-typename) 函式是由 WinUI 3 專案範本中預設未包含的 Windows.UI.Xaml.Interop 命名空間所提供。 將一行新增至專案的預先編譯標頭檔 (`pch.h`)，以包含與此命名空間相關聯的標頭檔。

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Xaml.Interop.h>
...
```



## <a name="define-the-default-style-for-bglabelcontrol"></a>定義 BgLabelControl 的預設樣式

在 **BgLabelControl** 的建構函式中，其會設定本身的預設樣式索引鍵。 樣板化控制項需要有包含預設控制項範本的預設樣式，以便在控制項的取用者不設定樣式及/或範本的情況下，用來自行轉譯。 在本節中，我們會將標記檔案新增到包含預設樣式的專案。

確定 [顯示所有檔案] 仍切換成開啟 (在 [方案總管] 中)。 請在您的專案節點下，建立新的資料夾 (不是篩選器而是資料夾)，並將其命名為「Themes」。 在 `Themes` 底下，新增 **Visual C++ > WinUI > 資源字典 (WinUI)** 類型的新項目，並且命名為 "Generic.xaml"。 資料夾和檔案名稱必須類似如此，XAML 架構才能找到樣板化控制項的預設樣式。 刪除 Generic.xaml 的預設內容，並且貼到下列標記中。

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

在此情況下，預設樣式所設定的唯一屬性是控制項範本。 該範本是由方形 (其背景繫結至 XAML **控制項** 類型的所有執行個體具有的 **背景** 屬性) 和文字元素 (其文字繫結至 **BgLabelControl::Label** 相依性屬性) 組成。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>將 BgLabelControl 的執行個體新增至主要 UI 頁面

開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 緊接在 (**StackPanel** 內的) **Button** 元素之後，新增下列標記。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，請將下列 include 指示詞新增到 `MainPage.h`，以讓 **MainPage** 類型 (編譯 XAML 標記和命令式程式碼的組合) 得知 **BgLabelControl** 樣板化控制項類型。 如果您想要使用其他 XAML 頁面的 **BgLabelControl**，可以將同一個 include 指示詞新增到標頭檔。 或者，也可以在先行編譯的標頭檔中放入單個 include 指示詞。

```cppwinrt
//MainPage.h
...
#include "BgLabelControl.h"
...
```

現在請建置並執行專案。 您會看到預設控制項範本繫結到標記中 **BgLabelControl** 執行個體本身的背景筆刷和標籤。

![樣板化控制項結果](images/winui-templated-control-result.png)

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>實作可覆寫函式，例如 **MeasureOverride** 和 **OnApplyTemplate**

您可以從 **控制項** 執行階段類別衍生樣板化控制項，該類別本身則是衍生自基底執行階段類別。 **Control**、**FrameworkElement** 和 **UIElement** 也有一些可覆寫方法，您可以在衍生類別中覆寫。 以下程式碼範例會示範如何進行。

```cppwinrt
// Control overrides.
void OnPointerPressed(Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

// FrameworkElement overrides.
Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
void OnApplyTemplate() const { ... };

// UIElement overrides.
Microsoft::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
```

*Overridable* 函式本身在不同的語言投影中有不同的形態。 例如，在 C# 中，可覆寫函式一般會以受保護的虛擬函式呈現。 在C++/WinRT 中，這些並非虛擬，也不受保護，但仍然可以予以覆寫並提供您自己的實作，如上所示。



## <a name="generating-the-control-source-files-without-using-a-template"></a>在不使用範本的情況下產生控制項來源檔案。

本節說明如何產生必要的來源檔案來建立自訂控制項，而不需使用 **自訂控制項** 項目範本。 

首先，將新的 Midl 檔案 (.idl) 項目新增至專案。 從 [專案] 功能表中，選取 [新增新項目...]，並且在搜尋方塊中輸入 "MIDL" 以尋找 .idl 檔案項目。 將新檔案命名為 `BgLabelControl.idl`，讓名稱與本文中的步驟一致。 刪除 `BgLabelControl.idl` 的預設內容，並貼到上述步驟中所示的執行階段類別宣告。


在儲存新的 .idl 檔案之後，下一個步驟是產生 Windows 執行階段中繼資料檔案 (.winmd) 和 .cpp 與 .h 實作檔案的虛設常式，您可以用來實作樣板化控制項。 藉由建置解決方案來產生這些檔案，這樣會導致 MIDL 編譯器 (midl.exe) 編譯您所建立的 .idl 檔案。 請注意，解決方案將不會成功建置，而且 Visual Studio 會在輸出視窗中顯示建置錯誤，但是會產生必要的檔案。

將虛設常式檔案 BgLabelControl.h 和 BgLabelControl.cpp 從 \BgLabelControlApp\BgLabelControlApp\Generated Files\sources\ 複製到專案資料夾。 在 [方案總管] 中，確定 [顯示所有檔案] 已切換成開啟。 在您複製的虛設常式檔案上按右鍵，然後按一下 **[加入至專案]** 。

編譯器會將 static_assert 一行放在 BgLabelControl.h 和 BgLabelControl.cpp 的頂端，以防止產生的檔案進行編譯。 在實作控制項時，您應該從已放在專案目錄中的檔案中移除這些程式碼行。 針對此逐步解說，您可以使用以上提供的程式碼，來覆寫檔案的整個內容。
