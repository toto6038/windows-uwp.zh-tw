---
description: 本主題會逐步引導您完成使用 C++/WinRT 建立簡單自訂控制項。 您可以根據這裡的資訊，替自己建立功能豐富且可自訂的 UI 控制項。
title: 使用 C++/WinRT 的 XAML 自訂 (範本化) 控制項
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 XAML、 自訂、 樣板化控制項
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce4f7eea074233c625a2cc92ef773f0b06c2be9f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635143"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>使用 C++/WinRT 的 XAML 自訂 (範本化) 控制項

> [!IMPORTANT]
> 基本概念和詞彙，讓您更瞭解如何取用，並撰寫與執行階段類別的[C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，請參閱[取用的 Api，使用 C + + /cli WinRT](consume-apis.md)和[作者 Api，使用 C + +WinRT](author-apis.md)。

其中一個最強大的功能的通用 Windows 平台 (UWP) 是使用者介面 (UI) 堆疊提供建立 XAML 為基礎的自訂控制項的彈性[**控制**](/uwp/api/windows.ui.xaml.controls.control)型別。 XAML UI 架構提供的功能，例如[自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)並[附加屬性](/windows/uwp/xaml-platform/custom-attached-properties)，並[控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)，這讓您輕鬆建立功能豐富且可自訂的控制項。 本主題將逐步引導您逐步完成建立自訂 （範本） 控制項，使用 C + + /cli WinRT。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>建立空白的應用程式 (BgLabelControlApp)
在 Microsoft Visual Studio 中，從建立新的專案開始。 建立**Visual c + +** > **Windows Universal** > **空白應用程式 (C + + /cli WinRT)** 專案，並將它命名*BgLabelControlApp*. 在本主題稍後的章節，您會將您導向來建置您的專案 （不在那之前建置）。

我們要撰寫新的類別來代表自訂 （範本） 控制項。 我們在相同的編譯單位裡撰寫和使用此類別。 但我們想要能夠執行個體化這個類別以及從 XAML 標記，因此它將會執行階段類別。 且我們會使用 C++/WinRT 撰寫和使用它。

撰寫新執行階段類別的第一個步驟是將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。 將它名為 `BgLabelControl.idl`。 刪除 `BgLabelControl.idl`的預設內容，並在此執行階段類別宣告中貼上。

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

上述清單中的會顯示宣告相依性屬性 (DP) 時，您所遵循的模式。 有兩個到每個 DP。 首先，您可以在此宣告類型的唯讀靜態屬性[ **DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty)。 它有名稱加上您 DP*屬性*。 在您的實作中，您將使用這個靜態屬性。 第二，您會使用您 DP 的名稱與型別宣告讀寫執行個體屬性。 如果您想要撰寫*附加屬性*（而不是 DP)，然後請參閱中的程式碼範例[自訂附加屬性](/windows/uwp/xaml-platform/custom-attached-properties)。

> [!NOTE]
> 如果您想使用浮點類型的 DP，然後將其設`double`(`Double`中[MIDL 3.0](/uwp/midl-3/))。 宣告和實作型別的 DP `float` (`Single` MIDL 中)，並產生錯誤，然後設定的值在 XAML 標記中，該 DP*無法從文字建立 'Windows.Foundation.Single' '<NUMBER>'*.

儲存檔案並建置專案。 在建置程序期間，執行 `midl.exe` 工具建立描述執行階段類別的 Windows 執行階段中繼資料檔案 (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)。 然後，執行 `cppwinrt.exe` 工具產生原始碼檔案在撰寫和使用執行階段類別中支援您。 這些檔案包含可協助您開始實作 stub **BgLabelControl**您在您的 IDL 中宣告的執行階段類別。 這些虛設常式為 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` 與 `BgLabelControl.cpp`。

從 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` 將虛設常式檔案 `BgLabelControl.h` 和 `BgLabelControl.cpp` 複製到專案資料夾中，也就是 `\BgLabelControlApp\BgLabelControlApp\`。 在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 **\[加入至專案\]**。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>實作**BgLabelControl**自訂控制項類別
現在，我們開啟 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` 與 `BgLabelControl.cpp` 並實作我們的執行階段類別。 在`BgLabelControl.h`，變更設定的預設樣式的索引鍵，實作建構函式**標籤**並**LabelProperty**，加入名為靜態事件處理常式**OnLabelChanged**至處理相依性屬性的值變更，並新增私用成員，才能儲存支援欄位，如**LabelProperty**。

新增之後, 您`BgLabelControl.h`看起來像這樣。

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

在  `BgLabelControl.cpp`，定義靜態成員，就像這樣。

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

在本逐步解說中，我們將不會使用**OnLabelChanged**。 但它有讓您可以了解如何註冊相依性屬性的屬性變更的回呼。 實作**OnLabelChanged**也會示範如何從基底投影的型別取得衍生投影的型別 (基底投影的型別是**DependencyObject**，在此情況下)。 並示範如何取得實作投影的型別類型的指標。 第二項作業將自然只會在實作投影的型別 （也就是實作的執行階段類別的專案） 的專案中。

> [!NOTE]
> 如果您尚未安裝 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年)，或更新版本中，然後您必須呼叫[ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi)相依性屬性已變更的事件處理常式，而不是[ **winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)。

## <a name="design-the-default-style-for-bglabelcontrol"></a>設計之預設樣式**BgLabelControl**

在其建構函式， **BgLabelControl**本身設定的預設樣式索引鍵。 那*是*預設樣式？ 自訂 （範本） 的控制項必須有預設樣式&mdash;包含預設控制項範本&mdash;它可以用來呈現其本身與萬一控制項的取用者不會設定樣式及/或範本。 在本節中，我們會標記檔案加入包含我們的預設樣式的專案中。

在您的專案節點下建立新的資料夾並將它命名為 「 主題 」。 底下`Themes`，加入新項目型別的**Visual c + +** > **XAML** > **XAML 檢視**，並將它命名為"Generic.xaml"。 資料夾和檔案名稱必須以尋找預設樣式為自訂控制項的 XAML 架構的順序會像這樣。 刪除的預設內容`Generic.xaml`，並貼上以下的標記。

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

在此情況下，唯一的預設樣式設定的屬性是 [控制項] 範本。 範本是由方格所組成 (其背景會繫結至**背景**屬性，XAML 的所有執行個體[**控制**](/uwp/api/windows.ui.xaml.controls.control)類型有)，和文字項目 （其文字的繫結至**BgLabelControl::Label**相依性屬性)。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>新增的執行個體**BgLabelControl**主要 UI 頁面

開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 之後立即** 按鈕**項目 (內**StackPanel**)，加入下列標記。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，，新增下列 include 指示詞，以`MainPage.h`以便**MainPage**型別 （編譯 XAML 標記和命令式的程式碼的組合） 並知道**BgLabelControl**自訂控制項類型。 如果您想要使用**BgLabelControl**從另一個 XAML 頁面，然後將此新增相同也包含該頁面上，標頭檔的指示詞。 或者，或者，放入單一 include 指示詞先行編譯標頭檔中。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

現在建置並執行專案。 您會看到的預設控制項範本所繫結的背景筆刷，以及標籤的**BgLabelControl**在標記中的執行個體。

本逐步解說會示範自訂 （範本） 控制項的簡單範例在 C + + /cli WinRT。 任意豐富且功能完整，您可以讓您自己的自訂控制項。 例如，自訂控制項可以達到這種東西一樣複雜的可編輯的資料格、 影片的播放程式或 3D 幾何圖形的視覺化檢視。

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>實作*可覆寫*函式，例如**MeasureOverride**和**OnApplyTemplate**

衍生的自訂控制項[**控制**](/uwp/api/windows.ui.xaml.controls.control)執行階段類別，而其本身進一步衍生自基底的執行階段類別。 也有一些可覆寫方法的**控制**， [ **FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement)，以及[ **UIElement** ](/uwp/api/windows.ui.xaml.uielement)您可以覆寫衍生類別中。 以下是程式碼範例，示範如何執行該動作。

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

*可覆寫*函式沒有問題以不同的方式不同的語言投影中。 在C#，例如，可覆寫的函式通常會顯示為受保護的虛擬函式。 在 C + + /cli WinRT，它們是不虛擬也不受保護，但還是可以覆寫它們，如上所示，提供您自己的實作。

## <a name="important-apis"></a>重要 API
* [控制項類別](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty 類別](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement 類別](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement 類別](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>相關主題
* [控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)
* [自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)
