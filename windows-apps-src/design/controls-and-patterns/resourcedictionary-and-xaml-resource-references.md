---
Description: Explains how to define a ResourceDictionary element and keyed resources, and how XAML resources relate to other resources that you define as part of your app or app package.
MS-HAID: dev\_ctrl\_layout\_txt.resourcedictionary\_and\_xaml\_resource\_references
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: ResourceDictionary 與 XAML 資源參考
ms.assetid: E3CBFA3D-6AF5-44E1-B9F9-C3D3EA8A25CE
label: ResourceDictionary and XAML resource references
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 51461df47fe92c296fee198a6f2ed1c34e833cd7
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8743914"
---
# <a name="resourcedictionary-and-xaml-resource-references"></a>ResourceDictionary 與 XAML 資源參考

 

您可以使用 XAML 來定義您 app 的 UI 或資源。 資源通常是一些您預期會多次使用之物件的定義。 若稍後要參考 XAML 資源，您可以為資源指定像做為名稱來使用的索引鍵。 您可以在整個應用程式或從其中的任一個 XAML 頁面，參考某個資源。 您可以使用來自「Windows 執行階段 XAML」的 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 元素來定義您的資源。 接著，您可以使用 [StaticResource 標記延伸](../../xaml-platform/staticresource-markup-extension.md)或 [ThemeResource 標記延伸](../../xaml-platform/themeresource-markup-extension.md)來參考資源。

您最常宣告為 XAML 資源的 XAML 元素包括 [Style](https://msdn.microsoft.com/library/windows/apps/br208849)、[ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)、動畫元件以及 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 子類別。 我們會在此處說明如何定義 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 和索引資源，以及 XAML 資源如何與其他定義為 app 或 app 套件之一部分的資源相關。 我們也會說明資源字典進階功能，例如 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 和 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807)。

**先決條件**

我們假設您了解 XAML 標記並已閱讀 [XAML 概觀](https://msdn.microsoft.com/library/windows/apps/mt185595)。

## <a name="define-and-use-xaml-resources"></a>定義和使用 XAML 資源

XAML 資源是從標記參考多次的物件。 資源是在 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中定義的，通常是在個別檔案或在標記頁面的頂端，如下所示。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
        <x:String x:Key="goodbye">Goodbye world</x:String>
    </Page.Resources>

    <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
</Page>
```

在此範例中：

-   `<Page.Resources>…</Page.Resources>` - 定義資源字典。
-   `<x:String>` - 使用 "greeting" 索引鍵來定義資源。
-   `{StaticResource greeting}` - 查詢具有 "greeting" 索引鍵的資源，此索引鍵會指派給 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 的 [Text](https://msdn.microsoft.com/library/windows/apps/br209676) 屬性。

> **注意**&nbsp;&nbsp;請不要將 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 的相關概念，與在產生應用程式套件的程式碼專案結構形成內容中所討論的 **\[資源\]** 建置動作、資源 (.resw) 檔案或其他「資源」混為一談。

資源不一定要是字串；它們可以是任何可共用的物件，例如樣式、範本、筆刷和色彩。 不過，控制項、形狀和其他 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 無法共用，因此無法宣告為可重複使用的資源。 如需有關共用的詳細資訊，請參閱本主題稍後的 [XAML 資源必須是可共用的](#xaml-resources-must-be-shareable)一節。

在這裡，筆刷和字串都宣告為資源，並且供頁面中的控制項使用。

```XAML
<Page
    x:Class="SpiderMSDN.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <SolidColorBrush x:Key="myFavoriteColor" Color="green"/>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource myFavoriteColor}" Text="{StaticResource greeting}" VerticalAlignment="Top"/>
    <Button Foreground="{StaticResource myFavoriteColor}" Content="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

所有資源都必須有一個索引鍵。 該索引鍵通常是以 `x:Key=”myString”` 定義的字串。 不過，指定索引鍵有其他幾個方式：

-   如果未指定 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)，[Style](https://msdn.microsoft.com/library/windows/apps/br208849) 和 [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391) 需要 **TargetType**，而且將會使用 **TargetType** 做為索引鍵。 在此情況下，索引鍵是實際的 Type 物件，而不是字串。 (請參閱以下範例)
-   如果未指定 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)，具有 **TargetType** 的 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/br242348) 資源將會使用 **TargetType** 做為索引鍵。 在此情況下，索引鍵是實際的 Type 物件，而不是字串。
-   可以使用 [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788) 代替 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)。 不過，x:Name 也會產生資源的程式碼後置欄位。 因此，x:Name 比 x:Key 沒有效率，因為載入頁面時，必須初始化該欄位。

[StaticResource 標記延伸](../../xaml-platform/staticresource-markup-extension.md)僅能以字串名稱 ([x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) 或 [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788)) 擷取資源。 不過，當 XAML 架構決定尚未設定 [Style](https://msdn.microsoft.com/library/windows/apps/br208743) 和 [ContentTemplate](https://msdn.microsoft.com/library/windows/apps/br209369) 或 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/br242830) 屬性的控制項要使用的樣式與範本時，也會尋找隱含的樣式資源 (使用 **TargetType** 而不是 x:Key 或 x:Name 的資源)。

在這裡，[Style](https://msdn.microsoft.com/library/windows/apps/br208849) 有一個 **typeof(Button)** 隱含的索引鍵，而且由於頁面底部的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 未指定 [Style](https://msdn.microsoft.com/library/windows/apps/br208743) 屬性，因此它會尋找索引鍵為 **typeof(Button)** 的樣式：

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button">
            <Setter Property="Background" Value="Red"/>
        </Style>
    </Page.Resources>
    <Grid>
       <!-- This button will have a red background. -->
       <Button Content="Button" Height="100" VerticalAlignment="Center" Width="100"/>
    </Grid>
</Page>
```

如需有關隱含樣式及其運作方式的詳細資訊，請參閱[設定控制項的樣式](xaml-styles.md)和[控制項範本](control-templates.md)。

## <a name="look-up-resources-in-code"></a>查詢程式碼中的資源

您可以像存取任何其他字典一樣存取資源字典的成員。

> [!WARNING]
> 當您在程式碼中，只有在資源中執行資源查詢`Page.Resources`字典會被查看。 不同於 [StaticResource 標記延伸](../../xaml-platform/staticresource-markup-extension.md)，此程式碼如果在第一個字典中找不到資源，並不會退而使用 `Application.Resources` 字典。

 

此範例示範如何從頁面的資源字典抓取 `redButtonStyle` 資源：

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button" x:Key="redButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources>
</Page>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style redButtonStyle = (Style)this.Resources["redButtonStyle"];
        }
    }
```

若要從程式碼中查詢整個 app 的資源，使用 **Application.Current.Resources** 取得 app 的資源字典，如下所示。

```XAML
<Application
    x:Class="MSDNSample.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SpiderMSDN">
    <Application.Resources>
        <Style TargetType="Button" x:Key="appButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Application.Resources>

</Application>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style appButtonStyle = (Style)Application.Current.Resources["appButtonStyle"];
        }
    }
```

您也可以在程式碼中加入應用程式資源。

以下是執行此動作時應該牢記的兩件事：

-   首先，您需要在任何頁面嘗試使用資源之前先加入資源。
-   第二，您無法在 app 的建構函示中加入資源。

如果您使用如下所示的 [Application.OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335) 方法加入資源，可以同時避免這兩個問題。

```CSharp
// App.xaml.cs
    
sealed partial class App : Application
{
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;
        if (rootFrame == null)
        {
            SolidColorBrush brush = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 0, 255, 0)); // green
            this.Resources["brush"] = brush;
            // … Other code that VS generates for you …
        }
    }
}
```

## <a name="every-frameworkelement-can-have-a-resourcedictionary"></a>每個 FrameworkElement 都可以有一個 ResourceDictionary

[FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 是控制項可從中繼承的一個基底類別，而且具備 [Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 屬性。 因此，您可以將本機資源字典新增至任何 **FrameworkElement**。

在這裡，[Page](https://msdn.microsoft.com/library/windows/apps/br227503) 和 [Border](https://msdn.microsoft.com/library/windows/apps/br209250) 都有資源字典，而且它們都有稱為 "greeting" 的資源。 名為 'textBlock2' [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)是內部**框線**，因此其資源查詢會先**框線**資源，然後在**頁面**的資源，然後在[應用程式](https://msdn.microsoft.com/library/windows/apps/br242324)資源。 **TextBlock** 將會讀取 "Hola mundo"。

若要從程式碼存取該元素的資源，請使用該元素的 [Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 屬性。 存取程式碼而不是 XAML 中 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 的資源時，只會在該字典中查看，而不會在父元素的字典中查看。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>
    
    <StackPanel>
        <!-- Displays "Hello world" -->
        <TextBlock x:Name="textBlock1" Text="{StaticResource greeting}"/>

        <Border x:Name="border">
            <Border.Resources>
                <x:String x:Key="greeting">Hola mundo</x:String>
            </Border.Resources>
            <!-- Displays "Hola mundo" -->
            <TextBlock x:Name="textBlock2" Text="{StaticResource greeting}"/>
        </Border>

        <!-- Displays "Hola mundo", set in code. -->
        <TextBlock x:Name="textBlock3"/>
    </StackPanel>
</Page>

```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            textBlock3.Text = (string)border.Resources["greeting"];
        }
    }
```

## <a name="merged-resource-dictionaries"></a>合併的資源字典

「合併的資源字典」** 是將某個資源字典結合到另一個資源字典中，通常是在另一個檔案中。

> **提示**&nbsp;&nbsp;您可以使用 **\[專案\]** 功能表的 **\[新增\] &gt; \[新增項目\] &gt; \[資源字典\]** 選項，在 Microsoft Visual Studio 中建立資源字典檔案。

在這裡，您會在另一個名為 Dictionary1.xaml 的 XAML 檔案中，定義資源字典。

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

```

若要使用該字典，您要將該字典與您頁面的字典合併：

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
            </ResourceDictionary.MergedDictionaries>

            <x:String x:Key="greeting">Hello world</x:String>

        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

以下是此範例所發生的情況。 在 `<Page.Resources>` 中，您要宣告 `<ResourceDictionary>`。 當您將資源加入到 `<Page.Resources>` 時，XAML 架構會為您隱含地建立一個資源字典；不過，在此案例中，您想要的不是任一資源字典，而是要一個包含合併字典的資源字典。

因此，您需宣告 `<ResourceDictionary>`，然後將資源加入到其 `<ResourceDictionary.MergedDictionaries>` 集合中。 這些項目中的每一個都採用 `<ResourceDictionary Source="Dictionary1.xaml"/>` 的形式。 若要加入多個字典，只要在第一個項目之後加入 `<ResourceDictionary Source="Dictionary2.xaml"/>` 項目即可。

在 `<ResourceDictionary.MergedDictionaries>…</ResourceDictionary.MergedDictionaries>` 之後，您可以視需要在您的主要字典中放入其他資源。 使用合併字典中的資源就像使用一般字典中的資源一樣。 在以上的範例中，`{StaticResource brush}` 會在子字典/合併字典中找到資源 (Dictionary1.xaml)，而 `{StaticResource greeting}` 則會在主頁面字典中找到其資源。

在資源查詢序列中，只有在檢查該 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 的其他所有索引資源之後，才會檢查 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 字典。 搜尋該層級之後，就會查詢合併字典，並檢查 **MergedDictionaries** 中的每個項目。 如果有多個合併字典，會按照在 **MergedDictionaries** 屬性中宣告的相反順序檢查它們。 在以下的範例中，如果 Dictionary2.xaml 與 Dictionary1.xaml 宣告了相同的索引鍵，則會先使用 Dictionary2.xaml 的索引鍵，因為它是這組 **MergedDictionaries** 中的最後一個項目。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
                <ResourceDictionary Source="Dictionary2.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="greetings!" VerticalAlignment="Center"/>
</Page>
```

在任一 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 的範圍內，會檢查字典的索引鍵唯一性。 不過，這個範圍不會延伸到不同 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 檔案中的不同項目。

您可以在合併字典範圍上使用查詢序列且不使用唯一索引鍵強制，建立 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 資源的後援值順序。 例如，您可以將特定筆刷色彩的使用者喜好設定儲存在順序的最後一個合併資源字典中，而使用同步至 app 狀態和使用者喜好設定資料的資源字典。 不過，如果還沒有使用者喜好設定，您可以在初始 [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801) 檔案中，為 ResourceDictionary 資源定義同一個索引鍵字串，就可以做為後援值。 請記得，您在主要資源字典中提供的值，一律會比合併字典優先檢查，所以如果您要使用後援技術，請不要在主要資源字典中定義該資源。

## <a name="theme-resources-and-theme-dictionaries"></a>佈景主題資源和佈景主題字典

[ThemeResource](../../xaml-platform/themeresource-markup-extension.md) 與 [StaticResource](../../xaml-platform/staticresource-markup-extension.md) 類似，但佈景主題變更時，會重新評估資源查詢。

在這個範例中，您可以將 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 的前景從目前的佈景主題設定為一個值。

```XAML
<TextBlock Text="hello world" Foreground="{ThemeResource FocusVisualWhiteStrokeThemeBrush}" VerticalAlignment="Center"/>
```

「佈景主題字典」是一種特殊類型的合併字典，可根據使用者目前在裝置上使用的佈景主題保留不同的資源。 例如，「淺色」佈景主題可能會使用白色筆刷，而「深色」佈景主題則可能使用深色筆刷。 筆刷會變更它所解析成的資源，但除此之外，使用筆刷做為資源的控制項組合仍可維持不變。 若要在您自己的範本和樣式中重新產生佈景主題切換行為，請使用 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 屬性取代將項目合併到主要字典時使用的 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 屬性。

[ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 中的每個 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 元素都必須有一個 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) 值。 這個值是為相關佈景主題命名的字串，例如 "Default"、"Dark"、"Light" 或 "HighContrast"。 `Dictionary1` 和 `Dictionary2` 通常會定義名稱相同但值不同的資源。

在這裡，您針對淺色佈景主題使用紅色文字，針對深色佈景主題使用藍色文字。

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

<!-- Dictionary2.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="blue"/>

</ResourceDictionary>
```

在這個範例中，您可以將 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 的前景從目前的佈景主題設定為一個值。

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml" x:Key="Light"/>
                <ResourceDictionary Source="Dictionary2.xaml" x:Key="Dark"/>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </Page.Resources>
    <TextBlock Foreground="{StaticResource brush}" Text="hello world" VerticalAlignment="Center"/>
</Page>
```

以佈景主題字典來說，每當使用 [ThemeResource 標記延伸](../../xaml-platform/themeresource-markup-extension.md)來建立參考且系統偵測到佈景主題變更時，要用於資源查詢的使用中字典就會動態變更。 系統是根據將使用中的佈景主題對應到特定佈景主題字典的 [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)，來完成查詢行為。

檢查預設 XAML 設計資源中佈景主題字典的建構方式 (與「Windows 執行階段」預設用於其控制項的範本相似) 可能會相當有用。 使用文字編輯器或 IDE 在 \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic 中開啟 XAML 檔案。 請注意 generic.xaml 中首先定義佈景主題字典的方式，以及每個佈景主題字典定義相同索引鍵的方式。 接著佈景主題字典外部的各種索引鍵元素的元素組合會參考這些每個索引鍵，並稍後在 XAML 中加以定義。 另外，也有針對只包含佈景主題資源和預設控制項範本以外之其他範本設計的個別 themeresources.xaml 檔案。 佈景主題區域與您在 generic.xaml 中看到的一樣。

當您使用 XAML 設計工具編輯樣式和範本的複本時，設計工具會從 XAML 設計資源字典擷取區段，然後當成您 app 和專案中的 XAML 字典元素的本機複本。

如需應用程式可用的佈景主題專用資源和系統資源詳細資訊和清單，請參閱 [XAML 佈景主題資源](xaml-theme-resources.md)。

## <a name="lookup-behavior-for-xaml-resource-references"></a>XAML 資源參考的查詢行為

「查詢行為」** 是描述 XAML 資源系統如何嘗試尋找 XAML 資源的詞彙。 從應用程式 XAML 中的某個位置以 XAML 資源參考的方式參考某個索引鍵時，就會發生查詢。 首先，資源系統針對將在何處依據範圍檢查資源是否存在方面，具有可預期的行為。 如果在初始範圍中找不到某項資源，則會擴充範圍。 查詢行為會繼續在可能由應用程式或系統定義 XAML 資源的所有位置與範圍進行。 如果所有可能的資源查詢嘗試都失敗，通常會產生錯誤。 在開發過程中，通常可以移除這些錯誤。

XAML 資源參考的查詢行為是從套用實際用法的物件和它本身的 [Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 屬性開始。 如果 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 存在，會檢查該 **ResourceDictionary** 是否有項目含有要求的索引鍵。 這個第一層查詢通常沒有什麼相關性，因為您通常不會在定義物件上的資源之後，接著參考該相同物件上的資源。 實際上，這裡通常不會有 **Resources** 屬性。 您幾乎可以在 XAML 中的任何一處建立 XAML 資源參考；不限於 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 子類別的屬性。

查詢序列會接著檢查 app 執行階段物件樹狀目錄中的下一個父物件。 如果 [FrameworkElement.Resources](https://msdn.microsoft.com/library/windows/apps/br208740) 存在並包含 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)，便會要求包含指定索引鍵字串的字典項目。 如果找到資源，查詢序列會停止，並將物件放置在參考的位置。 否則，查詢行為會朝著物件樹狀根目錄繼續前進到下一個父層級。 此搜尋會一直持續向上遞迴到 XAML 的根元素為止，以搜尋完所有可能的直接資源位置。

> **注意**&nbsp;&nbsp;常見的做法是在頁面的根層級定義所有直接資源，這樣可以同時利用這個資源查詢行為並使用 XAML 標記樣式慣例。

 

如果在直接資源中找不到要求的資源，下一個查詢步驟就是檢查 [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338) 屬性。 **Application.Resources** 是放置 app 特定資源最佳的位置，可以讓多個頁面在 app 瀏覽結構參考這些資源。

控制項範本在參考查詢中還有另一個可能的位置：佈景主題字典。 佈景主題字典是單一 XAML 檔案，而 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 元素則是它的根目錄。 佈景主題字典可能是來自 [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338) 的合併字典。 佈景主題字典也可能是範本化自訂控制項的控制項特定佈景主題字典。

最後，還會針對平台資源進行資源查詢。 平台資源包含針對每個系統 UI 佈景主題定義的控制項範本，這些範本會針對 Windows 執行階段應用程式，定義您在 UI 中使用的所有控制項的預設外觀。 平台資源也包含一組與整個系統外觀及佈景主題相關的具名資源。 技術上來說，這些資源是 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) 項目，所以在載入 app 後可以從 XAML 或程式碼查詢。 例如，系統佈景主題資源包含稱為 "SystemColorWindowTextColor" 的資源，它提供 [Color](https://msdn.microsoft.com/library/windows/apps/hh673723) 定義，能夠讓 app 文字色彩與來自作業系統和使用者喜好設定的系統視窗文字色彩相符。 app 的其他 XAML 樣式也可以參考此樣式，或者程式碼可以取得資源查詢值 (並在範例中將它轉換為 **Color**)。

如需詳細資訊，以及可供採用 XAML 之 UWP app 使用的佈景主題專用資源與系統資源清單，請參閱 [XAML 佈景主題資源](xaml-theme-resources.md)。

如果仍然無法在這些位置中找到要求的索引鍵，就會發生 XAML 剖析錯誤/例外狀況。 在某些情況下，XAML 剖析例外狀況可能是 XAML 標記編譯動作或 XAML 設計環境沒有偵測到的執行階段例外狀況。

因為資源字典的分層查詢行為，您可以特意定義使用相同字串做為索引鍵的多個資源項目，但前提是每個資源要在不同的層級定義。 也就是說，雖然每個特定 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 內的索引鍵都必須是唯一的，但是這種唯一性不會延伸到整個查詢行為序列。 在查詢期間，只有順利擷取的第一個物件才會用於 XAML 資源參考，然後查詢就會停止。 您可以使用這種行為，在 App 的 XAML 內的不同位置上，依索引鍵要求相同的 XAML 資源，但會取回不同的資源，這取決於 XAML 資源參考建立的來源範圍，以及該特定查詢的運作方式。

##  <a name="forward-references-within-a-resourcedictionary"></a>ResourceDictionary 內的向前參考


特定資源字典內的 XAML 資源參考必須參考已經使用索引鍵定義的資源，而且在語彙上該資源必須出現在資源參考之前。 XAML 資源參考無法解析向前參考。 因此，如果您使用來自另一個資源內的 XAML 資源參考，就必須設計您的資源字典結構，將其他資源使用的資源定義在資源字典的開頭。

在應用程式層級定義的資源不能參考直接資源。 這就等同於嘗試向前參考，因為實際上會先處理應用程式資源 (在應用程式首次啟動時，以及載入任何瀏覽頁面內容之前)。 不過，任何直接資源都可以參考應用程式資源，這是避免向前參考情況的一種很實用的技術。

## <a name="xaml-resources-must-be-shareable"></a>XAML 資源必須是可共用的


物件若要存在於 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)，就必須 *「可共用」*。

可共用是必要的特性，因為建構應用程式物件樹狀目錄並在執行階段使用時，物件不可以存在於樹狀目錄的多個位置。 要求每個 XAML 資源時，資源系統會在內部建立資源值複本，以在應用程式的物件圖形中使用。

[ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 和 Windows 執行階段 XAML 通常支援這些物件進行共用：

-   樣式和範本 ([Style](https://msdn.microsoft.com/library/windows/apps/br208849) 和衍生自 [FrameworkTemplate](https://msdn.microsoft.com/library/windows/apps/br208753) 的類別)
-   筆刷和色彩 (衍生自 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 的類別以及 [Color](https://msdn.microsoft.com/library/windows/apps/hh673723) 值)
-   包含 [Storyboard](https://msdn.microsoft.com/library/windows/apps/br210490) 的動畫類型
-   轉換 (衍生自 [GeneralTransform](https://msdn.microsoft.com/library/windows/apps/br210034) 的類別)
-   [Matrix](https://msdn.microsoft.com/library/windows/apps/br210127) 和 [Matrix3D](https://msdn.microsoft.com/library/windows/apps/br243266)
-   [Point](https://msdn.microsoft.com/library/windows/apps/br225870) 值
-   某些其他 UI 相關結構，如 [Thickness](https://msdn.microsoft.com/library/windows/apps/br208864) 和 [CornerRadius](https://msdn.microsoft.com/library/windows/apps/br242343)
-   [XAML 內建資料類型](https://msdn.microsoft.com/library/windows/apps/mt186448)

如果您遵循必要的實作模型，也可以使用自訂類型當成可共用的資源。 您可在支援程式碼 (或您包含的執行階段元件) 中定義這些類別，然後在 XAML 中，以資源方式具現化這些類別。 範例包括物件資料來源，以及資料繫結的 [IValueConverter](https://msdn.microsoft.com/library/windows/apps/br209903) 實作。

自訂類型必須具有預設建構函式，因為 XAML 剖析器會用它來具現化類別。 當成資源使用的自訂類型不能在其繼承中擁有 [UIElement](https://msdn.microsoft.com/library/windows/apps/br208911) 類別，因為 **UIElement** 永遠不能共用 (唯一目的是代表您的執行階段 app 的物件圖形中，單一位置的單一 UI 元素)。

## <a name="usercontrol-usage-scope"></a>UserControl 用法範圍


[UserControl](https://msdn.microsoft.com/library/windows/apps/br227647) 元素有一種資源查詢行為的特殊情況，因為它具有定義範圍及用法範圍的固有概念。 從定義範圍建立 XAML 資源參考的 **UserControl** 必須能夠支援在自己的定義範圍查詢序列內查詢該資源—也就是說，它不能存取 app 資源。 在 **UserControl** 用法範圍，資源參考被視為在朝用法頁面根目錄進行的查詢序列內 (就像是已載入物件樹狀目錄中物件的任何其他資源參考) 並可存取 app 資源。

## <a name="resourcedictionary-and-xamlreaderload"></a>ResourceDictionary 和 XamlReader.Load

您可以使用 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 做為 [XamlReader.Load](https://msdn.microsoft.com/library/windows/apps/br228048) 方法的根目錄或 XAML 輸入的一部分。 如果提交來進行載入的 XAML 中的所有 XAML 資源參考都是完全獨立的，則您也可以在該 XAML 中包含這類參考。 **XamlReader.Load** 會在不知道有任何其他 **ResourceDictionary** 物件 (甚至是 [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338)) 的內容中剖析 XAML。 此外，請勿在已提交到 **XamlReader.Load** 的 XAML 內使用 `{ThemeResource}`。

## <a name="using-a-resourcedictionary-from-code"></a>從程式碼使用 ResourceDictionary

大部分適用於 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 的案例都是專門使用 XAML 來處理。 您可以在 XAML 檔案內或 UI 定義檔案的一組 XAML 節點中宣告 **ResourceDictionary** 容器和資源。 然後，您可以使用 XAML 資源參考，從 XAML 的其他部分要求這些資源。 儘管如此，還是會有一些特定狀況，是您的 app 想要在執行時使用執行的程式碼來調整 **ResourceDictionary** 的內容，或者至少查詢 **ResourceDictionary** 的內容來查看是否已經定義某個資源。 這些程式碼是在 **ResourceDictionary** 執行個體上進行呼叫的，所以您必須先擷取一個 (透過取得 [**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) 擷取物件樹狀目錄某處的直接 ResourceDictionary，或 `Application.Current.Resources`)。

在 C\# 或 Microsoft Visual Basic 程式碼中，可以使用索引子 ([Item](https://msdn.microsoft.com/library/windows/apps/jj603134)) 在指定的 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中參考資源。 **ResourceDictionary** 是字串索引鍵字典，因此索引子會使用字串索引鍵而不是整數索引。 在 VisualC + + 元件延伸 (C + + /CX) 程式碼中，使用[查詢](https://msdn.microsoft.com/library/windows/apps/br208800)。

使用程式碼來檢查或變更 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 時，[Lookup](https://msdn.microsoft.com/library/windows/apps/br208800) 或 [Item](https://msdn.microsoft.com/library/windows/apps/jj603134) 這類 API 的行為不會從直接資源周遊到 app 資源；那是只有在載入 XAML 頁面時才會發生的 XAML 剖析器行為。 在執行階段，索引鍵的範圍與您當時使用的 **ResourceDictionary** 執行個體完全無關。 但是該範圍卻會延伸到 [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801)。

此外，如果您要求的索引鍵不存在於 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 中，可能不會出現錯誤；可能只是會提供 **null** 做為傳回值。 不過，如果您嘗試使用傳回的 **null** 做為值，則您還是會收到錯誤。 錯誤會來自屬性的 setter，並非您的 **ResourceDictionary** 呼叫。 如果屬性接受 **null** 為有效值，是唯一可以避免錯誤的的方法。 請注意，這個行為與 XAML 剖析期間的 XAML 查詢行為恰好相反；無法在剖析期間解析從 XAML 提供的索引鍵會造成 XAML 剖析錯誤，即使在屬性可以接受 **null** 的情況中也是一樣。

合併的資源字典包含在主要資源字典的索引範圍內，而主要資源字典會在執行階段參考合併字典。 換句話說，您可以使用主要字典的 **Item** 或 [Lookup](https://msdn.microsoft.com/library/windows/apps/br208800) 來尋找合併字典中實際定義的任何物件。 在這個情況下，查詢行為會與剖析階段 XAML 查詢行為類似：如果合併字典中有多個物件，而且每個物件都有相同的索引鍵，就會傳回最後新增的字典中的物件。

您可以透過呼叫 **Add** (C\# 或 Visual Basic) 或 [Insert](https://msdn.microsoft.com/library/windows/apps/br208799) (C++/CX)，將項目新增到現有的 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)。 您可以將項目新增到直接資源或 app 資源。 這兩種 API 呼叫都需要索引鍵，這符合 **ResourceDictionary** 中每個項目都必須具有索引鍵的需求。 不過，您在執行階段新增到 **ResourceDictionary** 的項目與 XAML 資源參考無關。 XAML 資源參考所需的查詢會在 app 載入時 (或者在偵測到佈景主題變更時)，進行第一次剖析 XAML 的時候發生。 在執行階段新增到集合的資源則無法使用，而更改 **ResourceDictionary** 不會已經從中抓取的資源中失效，即使您變更了該資源的值也一樣。

在執行階段，您也可以從 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 移除項目，建立部分或所有項目的複本，或執行其他操作。 **ResourceDictionary** 的成員清單指示可用的 API。 請注意，因為 **ResourceDictionary** 具有預計的 API 來支援它的基礎集合介面，所以 API 選項會根據您是使用 C\#、Visual Basic 或 C++/CX 而有所差異。

## <a name="resourcedictionary-and-localization"></a>ResourceDictionary 和當地語系化


XAML [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) 最初可能包含需要當地語系化的字串。 如果是這樣，請將這些字串儲存為專案資源，而不要儲存在 **ResourceDictionary** 中。 將字串從 XAML 抽取出來，並改為對擁有元素指定一個 [x:Uid 指示詞](https://msdn.microsoft.com/library/windows/apps/mt204791)值。 接著，在資源檔案中定義資源。 使用 *XUIDValue*.*PropertyName* 格式提供資源名稱，以及提供應該當地語系化的字串資源值。

## <a name="custom-resource-lookup"></a>自訂資源查詢

對於進階案例，您可以實作一個行為與本主題所述之 XAML 資源參考查詢行為不同的類別。 若要這樣做，您需要實作 [CustomXamlResourceLoader](https://msdn.microsoft.com/library/windows/apps/br243327) 類別，然後就可以使用 [CustomResource 標記延伸](https://msdn.microsoft.com/library/windows/apps/mt185580) (而不是使用 [StaticResource](../../xaml-platform/staticresource-markup-extension.md) 或 [ThemeResource](../../xaml-platform/themeresource-markup-extension.md)) 進行資源參考，以存取該行為。 大部分的 app 都不包含要求此作業的案例。 如需詳細資訊，請參閱 [CustomXamlResourceLoader](https://msdn.microsoft.com/library/windows/apps/br243327)。

 
## <a name="related-topics"></a>相關主題

* [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)
* [XAML 概觀](https://msdn.microsoft.com/library/windows/apps/mt185595)
* [StaticResource 標記延伸](../../xaml-platform/staticresource-markup-extension.md)
* [ThemeResource 標記延伸](../../xaml-platform/themeresource-markup-extension.md)
* [XAML 佈景主題資源](xaml-theme-resources.md)
* [設定控制項的樣式](xaml-styles.md)
* [x:Key 屬性](https://msdn.microsoft.com/library/windows/apps/mt204787)

 

 



