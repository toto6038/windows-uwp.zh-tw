---
title: 使用 C++/WinRT 建立 "Hello, World!" 應用程式
description: 本主題將逐步引導您使用 C++/WinRT 建立 Windows 10 UWP "Hello, World!" 應用程式。 應用程式的 UI 是使用可延伸應用程式標記語言 (XAML) 定義的。
ms.date: 07/11/2020
ms.topic: article
keywords: windows 10, uwp, cppwinrt, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 706b0aceb203fe6a333c281dfd2b691eb35b65b2
ms.sourcegitcommit: 98ca28fd0b5d306d35f3919fe9dd4d5a0222235e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102029831"
---
# <a name="create-a-hello-world-app-using-cwinrt"></a>使用 C++/WinRT 建立 "Hello, World!" 應用程式

本主題將逐步引導您使用 C++/WinRT 建立 Windows 10 通用 Windows 平台 (UWP) "Hello, World!" 應用程式。 應用程式的使用者介面 (UI) 是使用可延伸應用程式標記語言 (XAML) 定義的。

C++/WinRT 是 Windows 執行階段 (WinRT) API 的完全標準現代 C++17 語言投影。 如需詳細資訊以及更多逐步解說和程式碼範例，請參閱 [C++/WinRT](../cpp-and-winrt-apis/index.md) 文件。 [開始使用 C++/WinRT](../cpp-and-winrt-apis/get-started.md)是很好的起始主題。

## <a name="set-up-visual-studio-for-cwinrt"></a>設定適用於 C++/WinRT 的 Visual Studio

如需安裝和為 C++/WinRT 開發設定 Visual Studio 的相關資訊&mdash;包括如何安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援)&mdash;，請參閱 [C++/WinRT 的 Visual Studio 支援](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

若要下載 Visual Studio，請參閱[下載](https://visualstudio.microsoft.com/downloads/)。

如需 XAML 簡介，請參閱 [XAML 概觀](../xaml-platform/xaml-overview.md)

## <a name="create-a-blank-app-helloworldcppwinrt"></a>建立空白應用程式 (HelloWorldCppWinRT)

我們的第一個應用程式是 "Hello, World!" 應用程式，其會示範互動功能、配置及樣式的某些基本功能。

請先在 Microsoft Visual Studio 中，建立新的專案。 建立 **空白應用程式 (C++/WinRT)** 專案，並將其命名為 *HelloWorldCppWinRT*。 確定已取消核取 [將方案和專案放置於同一個目錄]。 以 Windows SDK 最新的正式推出版本 (即非預覽版本) 為目標。

在本主題稍後的章節中，會引導您建立專案 (但請勿在進行到該小節前先行建立)。

### <a name="about-the-project-files"></a>關於專案檔案

一般來說，在專案資料夾中，每個 `.xaml` (XAML 標記) 檔案都有對應的 `.idl`、`.h` 和 `.cpp` 檔案。 這些檔案會一起編譯成 XAML 頁面類型。

您可以修改 XAML 標記檔案來建立 UI 元素，也可以將這些元素繫結至資料來源 (這項工作稱為 [資料繫結](../data-binding/index.md))。 例如，您可以修改 `.h` 和 `.cpp` 檔案 (有時也會修改 `.idl` 檔案)，為您的 XAML 頁面事件處理常式新增自訂邏輯。

讓我們查看一些專案檔案。

- `App.idl`、`App.xaml`、`App.h` 和 `App.cpp`。 這些檔案代表您應用程式特製的 [**Windows::UI::Xaml::Application**](/uwp/api/windows.ui.xaml.application) 類別，其中包括您應用程式的進入點。 `App.xaml` 不包含任何頁面特定的標記，但您可以在該處加入使用者介面元素樣式，以及您想要從所有頁面存取的任何其他元素。 `.h` 和 `.cpp` 檔案包含各種應用程式生命週期事件的處理常式。 通常，您會在該處加入自訂程式碼，以在您的應用程式啟動時初始化您的應用程式，並在您的應用程式暫停或終止時執行清理。
- `MainPage.idl`、`MainPage.xaml`、`MainPage.h` 和 `MainPage.cpp`。 包含應用程式中預設主要 (啟動) 頁面類型 (即 **MainPage** 執行階段類別) 的 XAML 標記和實作。 **MainPage** 沒有瀏覽支援，但其會提供一些預設 UI 和一個事件處理常式，讓您開始使用。
- `pch.h` 和 `pch.cpp`。 這些檔案代表您專案的先行編譯標頭檔。 在 `pch.h` 中，包含任何不常變更的標頭檔，然後在專案的其他檔案中包含 `pch.h`。

## <a name="a-first-look-at-the-code"></a>初窺程式碼

### <a name="runtime-classes"></a>執行階段類別

如您所知，在以 C# 撰寫的通用 Windows 平台 (UWP) 應用程式中，其所有類別都是 Windows 執行階段類型。 但是當您在 C++/WinRT 應用程式中撰寫類型時，可以選擇該類型為 Windows 執行階段類型，還是一般 C++ 類別/結構/列舉。

您專案中的任何 XAML 頁面類型都必須是 Windows 執行階段類型。 因此，**MainPage** 是 Windows 執行階段類型。 具體而言，這是 *執行階段類別*。 XAML 頁面所取用的任何類型也必須是 Windows 執行階段類型。 當您撰寫 [Windows 執行階段元件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)，且想要撰寫可從另一個應用程式取用的類型時，將會撰寫 Windows 執行階段類型。 在其他情況下，您的類型可以是一般 C++ 類型。 一般來說，您可以使用任何 Windows 執行階段語言來取用 Windows 執行階段類型。

若要適當地指出類型為 Windows 執行階段類型，請在介面定義語言 (`.idl`) 檔案內的 [Microsoft 介面定義語言 (MIDL)](/uwp/midl-3/) 中定義該類型。 讓我們以 **MainPage** 做為範例。

```idl
// MainPage.idl
namespace HelloWorldCppWinRT
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Int32 MyProperty;
    }
}
```

以下是 **MainPage** 執行階段類別及其啟用處理站的基本實作結構，如 `MainPage.h` 中所見。

```cppwinrt
// MainPage.h
...
namespace winrt::HelloWorldCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        int32_t MyProperty();
        void MyProperty(int32_t value);
        ...
    };
}

namespace winrt::HelloWorldCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```    

如需有關是否要撰寫特定類別之執行階段類別的詳細資訊，請參閱[使用 C++/WinRT 撰寫 API](../cpp-and-winrt-apis/author-apis.md) 主題。 如需有關執行階段類別與 IDL (`.idl` 檔案) 之間連線的詳細資訊，請閱讀並遵循 [XAML 控制項；繫結至一個 C++/WinRT 屬性](../cpp-and-winrt-apis/binding-property.md)主題。 該主題會逐步解說撰寫新執行階段類別的程序，其第一步是要將一個新的 **Midl 檔案 (.idl)** 項目新增至專案。

現在讓我們新增一些功能至 **HelloWorldCppWinRT** 專案。

## <a name="step-1-modify-your-startup-page"></a>步驟 1. 修改啟動頁面

在 [方案總管]中，開啟 `MainPage.xaml`，讓您可以撰寫構成使用者介面 (UI) 的控制項。

刪除已在其中的 **StackPanel**，以及其內容。 在其所在之處，貼上下列 XAML。

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock HorizontalAlignment="Left" Text="Hello, World!" FontSize="36"/>
    <TextBlock Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
    </StackPanel>
    <TextBlock x:Name="greetingOutput"/>
</StackPanel>
```

此新的 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) 具有會提示使用者名稱的 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、會接受使用者名稱的 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)、一個 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)，以及另一個 **TextBlock** 元素。

因為我們已刪除名為 **myButton** 的 *Button*，所以必須從程式碼中移除其參考。 因此，在 `MainPage.cpp` 中，刪除 **MainPage::ClickHandler** 函式內的程式碼行。

到目前為止，您已建立一個非常基本的 Windows 通用應用程式。 若要查看 UWP 應用程式的樣子，請建置並執行此應用程式。

![包含控制項的 UWP 應用程式畫面](images/xaml-hw-app2.png)

在應用程式中，您可以在文字方塊中鍵入文字。 但是按一下按鈕尚不會執行任何動作。

## <a name="step-2-add-an-event-handler"></a>步驟 2. 新增事件處理常式

在 `MainPage.xaml` 中，尋找名為 *inputButton* 的 **Button**，然後為其 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件宣告事件處理常式。 **Button** 的標記現在看起來應該像這樣。

```xaml
<Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="inputButton_Click"/>
```

像這樣實作事件處理常式。

```cppwinrt
// MainPage.h
struct MainPage : MainPageT<MainPage>
{
    ...
    void inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e);
};

// MainPage.cpp
namespace winrt::HelloWorldCppWinRT::implementation
{
    ...
    void MainPage::inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e)
    {
        greetingOutput().Text(L"Hello, " + nameInput().Text() + L"!");
    }
}
```

如需詳細資訊，請參閱[使用委派來處理事件](../cpp-and-winrt-apis/handle-events.md)。

實作會從文字方塊中擷取使用者的名稱、將其用來建立問候語，然後在 *greetingOutput* 文字區塊中顯示此問候語。

建置並執行應用程式。 在文字方塊中鍵入您的名稱，然後按一下按鈕。 應用程式會顯示個人化的問候語。

![顯示訊息的應用程式畫面](images/xaml-hw-app3.png)

## <a name="step-3-style-the-startup-page"></a>步驟 3. 設定啟動頁面的樣式

### <a name="choose-a-theme"></a>選擇佈景主題

自訂您 app 的外觀和操作方式非常容易。 根據預設，您的應用程式會使用淺色樣式的資源。 系統資源也包含深色佈景主題。

若要試用深色佈景主題，請編輯 `App.xaml`，並為 [**Application::RequestedTheme**](/uwp/api/windows.ui.xaml.application.requestedtheme)新增一值。

```xaml
<Application
    ...
    RequestedTheme="Dark">

</Application>
```

對於主要顯示影像或視訊的應用程式，建議您使用深色佈景主題；對於包含大量文字的應用程式，則建議使用淺色佈景主題。 如果您要使用自訂色彩配置，則使用與您應用程式外觀及操作方式最搭配的佈景主題。

> [!NOTE]
> 當您的應用程式啟動時，即會套用佈景主題。 當應用程式正在執行時，無法變更佈景主題。

### <a name="use-system-styles"></a>使用系統樣式

在本節中，我們將變更文字的外觀 (例如，讓字型大小變大)。

在 `MainPage.xaml` 中，尋找 "What's your name?" **TextBlock**。 將其 [**樣式**](/uwp/api/windows.ui.xaml.style)屬性設定為參考 *BaseTextBlockStyle* 系統資源索引鍵。

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

*BaseTextBlockStyle* 是資源的索引鍵，而此資源定義在 `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml` 的 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中。 以下是該樣式所設定的屬性值。

```xaml
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="XamlAutoFontFamily" />
    <Setter Property="FontWeight" Value="SemiBold" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="TextTrimming" Value="None" />
    <Setter Property="TextWrapping" Value="Wrap" />
    <Setter Property="LineStackingStrategy" Value="MaxHeight" />
    <Setter Property="TextLineBounds" Value="Full" />
</Style>
```

此外，在 `MainPage.xaml` 中，尋找名為 `greetingOutput` 的 **TextBlock**。 將其 [樣式] 設定為 *BaseTextBlockStyle*。 如果您現在建置並執行應用程式，則會看到兩個文字區塊的外觀都已變更 (例如，字型大小現在更大)。

## <a name="step-4-have-the-ui-adapt-to-different-window-sizes"></a>步驟 4. 使 UI 隨著不同的視窗大小進行調整

現在，我們會讓 UI 以動態方式隨著變更的視窗大小進行調整，使其在具有小型顯示器的裝置上更美觀。 若要這樣做，請將 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) 區段新增至 `MainPage.xaml`。 您將針對不同的視窗大小定義不同的視覺狀態，然後設定要針對其中每個視覺狀態套用的屬性。

### <a name="adjust-the-ui-layout"></a>調整 UI 配置

將此 XAML 區塊新增為根 **StackPanel** 元素的第一個子元素。

```xaml
<StackPanel ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</StackPanel>
```

建置並執行應用程式。 請注意，除非調整大小後的視窗小於 641 裝置獨立畫素 (DIP)，否則 UI 看起來會和以前一樣。 此時，會套用 *narrowState* 視覺狀態，並連同針對該狀態定義的所有屬性 Setter。

名為 `wideState` 的 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) 有一個 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 的 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 屬性設為 641。 這表示只有在視窗寬度不小於最小值 641 DIP 時才會套用狀態。 您不需為此狀態定義任何 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 物件，因此其會使用您在 XAML 頁面內容中所定義的配置屬性。

第二個 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) (`narrowState`) 有一個 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 的 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 屬性設為 0。 當視窗寬度大於 0 但小於 641 DIP 時，會套用這個狀態。 只有 641 DIP `wideState` 有效。 在 `narrowState` 中，您定義 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 物件，以變更 UI 中控制項的配置屬性：

- 將 *contentPanel* 元素的左邊界從 120 減少至 20。
- 將 *inputPanel* 元素的 [**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) 從 **Horizontal** 變更為 **Vertical**。
- 新增 4 個 DIP 的上邊界至 *inputButton* 元素。

## <a name="summary"></a>摘要

本逐步解說向您展示了如何新增內容至 Windows 通用應用程式、如何新增互動功能，以及如何變更 UI 外觀。
