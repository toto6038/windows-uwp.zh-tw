---
author: stevewhims
description: 這個主題會帶領您完成的步驟建立簡單的自訂控制項使用 C + + WinRT。 您可以建立以建立您自己的豐富功能和可自訂 UI 控制項的以下的資訊。
title: XAML 自訂 （樣板） 的控制項與 C + + WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 預測、 XAML、 自訂、 樣板、 控制項
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2795460"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>XAML 自訂 （樣板） 如果控制項具有[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

其中一個最強大功能的萬用 Windows 平台 (UWP) 為使用者介面 (UI) 堆疊提供建立 XAML[控制項](/uwp/api/windows.ui.xaml.controls.control)類型為基礎的自訂控制項的彈性。 XAML UI 架構提供的功能，例如[自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)與附加的屬性和[控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)，以輕鬆建立功能豐富及可自訂的控制項。 本主題會帶領您完成的步驟建立的自訂 （樣板） 控制項與 C + + WinRT。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>建立空白的應用程式 (BgLabelControlApp)
在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立**Visual c + + 空白應用程式 (C + + WinRT)** 專案，並將其命名為*BgLabelControlApp*。

我們將製作新的類別來代表自訂 （樣板化） 的控制項。 我們在相同的編譯單位裡撰寫和使用此類別。 但是我們希望能夠將它要執行階段類別分組的原因這個類別從 XAML 標記及執行個體化。 且我們會使用 C++/WinRT 撰寫和使用它。

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

上述清單中的會顯示您依照宣告相依性屬性 (DP) 時的圖樣。 有兩個部分至每個 DP。 首先，您宣告類型[DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)唯讀靜態屬性。 它具有 DP 外加*屬性*的名稱。 您將使用此靜態屬性中實作。 其次，您宣告為可讀寫的執行個體屬性的類型與您 DP 的名稱。

> [!NOTE]
> 如果您想 DP 浮點類型，然後讓它`double`(`Double` [MIDL 3.0](/uwp/midl-3/)中)。 宣告及實作 DP 類型的`float`(`Single` MIDL 中)，且會產生錯誤值設定為在 XAML 標記該 DP*無法建立 'Windows.Foundation.Single' 文字 '<NUMBER>'*。

儲存檔案並建置專案。 在建置程序期間，執行 `midl.exe` 工具建立描述執行階段類別的 Windows 執行階段中繼資料檔案 (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)。 然後，執行 `cppwinrt.exe` 工具產生原始碼檔案在撰寫和使用執行階段類別中支援您。 這些檔案包含讓您開始實作您在您 IDL 中宣告的**BgLabelControl** runtime 類別的 stub。 這些虛設常式為 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` 與 `BgLabelControl.cpp`。

從 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` 將虛設常式檔案 `BgLabelControl.h` 和 `BgLabelControl.cpp` 複製到專案資料夾中，也就是 `\BgLabelControlApp\BgLabelControlApp\`。 在 **\[方案總管\]** 中，確定 **\[顯示所有檔案\]** 已切換成開啟。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 **\[加入至專案\]**。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>實作**BgLabelControl**自訂控制項類別
現在，我們開啟 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` 與 `BgLabelControl.cpp` 並實作我們的執行階段類別。 在`BgLabelControl.h`、 變更要設定預設樣式機碼、 實作**標籤**和**LabelProperty**建構函式、 新增名為**OnLabelChanged**處理相依性屬性的值變更靜態事件處理常式及新增私用成員若要儲存**LabelProperty**備份] 欄位。

在這個逐步解說，我們將不會使用**OnLabelChanged**。 但是很有，讓您可以查看如何變更屬性的回呼中註冊的相依性屬性。

之後，請新增您`BgLabelControl.h`看起來像這樣。

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

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

在`BgLabelControl.cpp`，定義如下 static 成員。

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

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>設計之預設樣式的**BgLabelControl**

在其建構函式， **BgLabelControl**設定預設樣式按鍵本身擷取。 但*是*什麼預設樣式？ 自訂 （樣板） 控制項需要有預設樣式&mdash;包含預設的控制項範本&mdash;讓它可以用來轉譯本身具有以防控制項的用戶不會將樣式及 （或） 範本。 本節中我們將標記將檔案新增至包含我們預設樣式的專案。

在 [專案] 節點下建立新的資料夾，並命名 」 佈景主題 」。 下`Themes`、 新增新的項目類型**Visual c + +** 的 > **XAML** > **XAML 檢視**，並將其命名為"Generic.xaml"。 資料夾和檔案名稱必須類似 XAML framework 尋找自訂控制項的預設樣式的順序。 刪除預設內容`Generic.xaml`，並貼入下列的標記中。

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

在此例中之預設樣式設定的唯一屬性是控制項範本。 範本包含的方形 （其背景繫結至 XAML[控制項](/uwp/api/windows.ui.xaml.controls.control)類型的所有執行個體有**Background**屬性），以及文字項目 （其文字繫結至**BgLabelControl::Label**相依性屬性）。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>主要 UI] 頁面上新增**BgLabelControl**執行個體

開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 後面 （內部**StackPanel**) 的**按鈕**元素，新增下列標記。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，新增 [下列包含指示詞以`MainPage.h`使**MainPage**類型 （編譯 XAML 標記和必要的程式碼的組合） 時會知曉**BgLabelControl**自訂控制項類型。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

現在建置並執行專案。 您會看到的預設控制項範本已繫結至背景筆刷和 label、 **BgLabelControl**執行個體中標記。

此逐步解說說明自訂 （化） 控制項中的簡單範例會在 C + + WinRT。 您可以讓您自己自訂控制項的任意位置豐富又全功能。 例如，自訂控制項可採取某些項目為可編輯的資料格、 影片播放程式或的 3D 幾何視覺化檢視為複雜的表單。

## <a name="important-apis"></a>重要 API
* [控制項](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>相關主題
* [控制項範本](/windows/uwp/design/controls-and-patterns/control-templates)
* [自訂相依性屬性](/windows/uwp/xaml-platform/custom-dependency-properties)
