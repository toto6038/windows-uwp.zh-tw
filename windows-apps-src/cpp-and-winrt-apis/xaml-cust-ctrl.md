---
description: 本主題會逐步引導您完成使用 C++/WinRT 建立簡單自訂控制項。 您可以根據這裡的資訊，替自己建立功能豐富且可自訂的 UI 控制項。
title: 使用 C++/WinRT 的 XAML 自訂 (範本化) 控制項
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 自訂, 範本, 控制
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6acbd62a8fa75eefb39598dd5bbb6ec1270388c4
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209093"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>使用 C++/WinRT 的 XAML 自訂 (範本化) 控制項

> [!IMPORTANT]
> 如需一些基本概念和詞彙，以協助了解如何以 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 使用及撰寫執行階段類別，請參閱[使用 C++/WinRT 來使用 API](consume-apis.md) 和[使用 C++/WinRT 撰寫 API](author-apis.md)。

通用 Windows 平台 (UWP) 最強大的功能之一，即是使用者介面 (UI) 堆疊提供的彈性，能夠按照 XAML [**控制項**](/uwp/api/windows.ui.xaml.controls.control)類型建立自訂控制項。 XAML UI 架構提供的功能包括[自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)和[附加屬性](/windows/uwp/xaml-platform/custom-attached-properties)，以及[控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)，可用來輕鬆建立功能豐富的自訂控制項。 本主題會逐步說明如何使用 C++/WinRT 建立自訂 (範本) 控制項。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>建立空白應用程式 (BgLabelControlApp)
請先在 Microsoft Visual Studio 中，建立新的專案。 先建立 **空白應用程式 (C++/WinRT)** 專案，並命名為 *BgLabelControlApp*。 在本主題稍後的章節中，會引導您建立專案 (請勿在進行到該小節前先行建立)。

> [!NOTE]
> 如需安裝和為 C++/WinRT 開發設定 Visual Studio 的相關資訊&mdash;包括如何安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援)&mdash;，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

我們會撰寫新的類別來代表自訂 (範本) 控制項。 會在相同的編譯單元內撰寫和使用此類別。 但是，我們希望能從 XAML 標記將此類別具現化，基於這個原因，這會是執行階段類別。 且將會使用 C++/WinRT 來撰寫和使用。

撰寫新執行階段類別的第一個步驟，要將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。 請命名為 `BgLabelControl.idl`。 接下來請刪除 `BgLabelControl.idl` 的預設內容，並貼到此執行階段類別宣告內。

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

上列清單顯示的是在宣告相依性屬性 (DP) 時所遵循的模式。 每個 DP 都有兩個物件。 首先要宣告 [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty) 類型的唯讀靜態屬性。 其名稱是您的 DP 加上*屬性*。 在實作中，要使用這個靜態屬性。 再來請使用您的 DP 類型和名稱，來宣告讀寫執行個體的屬性。 如果想要撰寫*附加屬性* (而非 DP)，請參閱[自訂附加屬性](/windows/uwp/xaml-platform/custom-attached-properties)中的程式碼範例。

> [!NOTE]
> 如果需要具有浮點類型的 DP，可將其設定為 `double` ([MIDL 3.0](/uwp/midl-3/) 中的 `Double`)。 若宣告並實作 `float` 類型的 DP (MIDL 中的 `Single`)，然後設定 XAML 標記內該 DP 的值，則會導致「Failed to create a 'Windows.Foundation.Single' from the text」 *<NUMBER>的錯誤*。

儲存檔案並建置專案。 在建置程序期間，會執行 `midl.exe` 工具，以建立 Windows 執行階段中繼資料檔案 (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)，其會描述執行階段類別。 然後，會執行 `cppwinrt.exe` 工具，以產生原始碼檔案，可再撰寫和使用執行階段類別時提供支援。 這些檔案包含虛設常式，可協助開始實作您在 IDL 中宣告的 **BgLabelControl** 執行階段類別。 這些虛設常式為 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` 與 `BgLabelControl.cpp`。

從 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` 將虛設常式檔案 `BgLabelControl.h` 和 `BgLabelControl.cpp` 複製到專案資料夾中，也就是 `\BgLabelControlApp\BgLabelControlApp\`。 在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 在您複製的虛設常式檔案上按右鍵，然後按一下 **[加入至專案]** 。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>實作 **BgLabelControl** 自訂控制項類別
現在要開啟 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` 與 `BgLabelControl.cpp`，並實作我們的執行階段類別。 在 `BgLabelControl.h` 中，請變更建構函式，以設定預設樣式索引鍵，並實作 **Label** 和 **LabelProperty**，新增名為 **OnLabelChanged** 的靜態事件處理常式，來處理相依性屬性值的變更，再新增私用成員以儲存 **LabelProperty** 的支援欄位。

新增之後，您的 `BgLabelControl.h` 如下所示。

```cppwinrt
// BgLabelControl.h
...
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

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

在 `BgLabelControl.cpp` 中，請定義像這樣的靜態成員。

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // Call members of the projected type via theControl.

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

在本逐步解說中，不會使用 **OnLabelChanged**。 但其仍然存在，所以您可以看出如何註冊具有屬性變更回呼的相依性屬性。 **OnLabelChanged** 的實作，同樣也會顯示出如何從基底投影類型取得衍生投影類型 (在此範例中，基底投影類型是 **DependencyObject**)。 也會顯示如何取得用於實作投影類型的指標。 第二項作業自然只能在實作投影類型的專案 (也就是實作執行階段類別的專案) 中進行。

> [!NOTE]
> 如果您尚未安裝 Windows SDK 10.0.17763.0 版 (Windows 10 1809 版) 或更新版本，則必須呼叫上述相依性屬性變更事件處理常式中的 [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi)，而不是呼叫 [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)。

## <a name="design-the-default-style-for-bglabelcontrol"></a>設計 **BgLabelControl** 的預設樣式

在 **BgLabelControl** 的建構函式中，其會設定本身的預設樣式索引鍵。 不過預設樣式是  什麼？ 自訂 (範本) 控制項需要有&mdash;包含預設控制項範本&mdash;的預設樣式，以便在控制項的取用者不設定樣式及/或範本的情況下，用來自行轉譯。 在本節中，我們會將標記檔案新增到包含預設樣式的專案。

請在您的專案節點下，建立新的資料夾 (不是篩選器而是資料夾)，並將其命名為「Themes」。 在 `Themes` 底下，新增類別 **Visual C++**  > **XAML** > **XAML View** 的新項目，並命名為 "Generic.xaml"。 資料夾和檔案名稱必須類似如此，XAML 架構才能找到自訂控制項的預設樣式。 刪除 `Generic.xaml` 的預設內容，並且貼到下列標記中。

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

在此情況下，預設樣式所設定的唯一屬性是控制項範本。 該範本是由方形 (其背景繫結至XAML [**控制項**](/uwp/api/windows.ui.xaml.controls.control)類型的所有執行個體具有的**背景**屬性) 和文字元素 (其文字繫結至 **BgLabelControl::Label** 相依性屬性) 組成。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>將 **BgLabelControl** 的執行個體新增至主要 UI 頁面

開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 緊接在 (**StackPanel** 內的) **Button** 元素之後，新增下列標記。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，請將下列 include 指示詞新增到 `MainPage.h`，以讓 **MainPage** 類型 (編譯 XAML 標記和命令式程式碼的組合) 得知 **BgLabelControl** 自訂控制項類型。 如果您想要使用其他 XAML 頁面的**BgLabelControl**，可以將同一個 include 指示詞新增到標頭檔。 或者，也可以在先行編譯的標頭檔中放入單個 include 指示詞。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

現在請建置並執行專案。 您會看到預設控制項範本繫結到標記中 **BgLabelControl** 執行個體本身的背景筆刷和標籤。

本結會逐步介紹 C++/WinRT 內自訂 (範本) 控制項的簡單範例。 您可以自行設定功能豐富完整的自訂控制項。 例如，自訂控制項可以呈現為可供編輯的資料格、影片播放程式或 3D 幾何圖形視覺特效播放器等複雜的形式。

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>實作 *overridable* 函式，例如**MeasureOverride** 和 **OnApplyTemplate**

您可以從[**控制項**](/uwp/api/windows.ui.xaml.controls.control)執行階段類別衍生自訂控制項，該類別本身則是衍生自基底執行階段類別。 **Control**、[**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement) 和 [**UIElement**](/uwp/api/windows.ui.xaml.uielement) 也有一些可覆寫方法，您可以在衍生類別中覆寫。 以下程式碼範例會示範如何進行。

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

*Overridable* 函式本身在不同的語言投影中有不同的形態。 例如，在 C# 中，可覆寫函式一般會以受保護的虛擬函式呈現。 在C++/WinRT 中，這些並非虛擬，也不受保護，但仍然可以予以覆寫並提供您自己的實作，如上所示。

## <a name="important-apis"></a>重要 API
* [Control 類別](/uwp/api/windows.ui.xaml.controls.control) (英文)
* [DependencyProperty 類別](/uwp/api/windows.ui.xaml.dependencyproperty) (英文)
* [FrameworkElement 類別](/uwp/api/windows.ui.xaml.frameworkelement) (英文)
* [UIElement 類別](/uwp/api/windows.ui.xaml.uielement) (英文)

## <a name="related-topics"></a>相關主題
* [控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)
* [自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)
