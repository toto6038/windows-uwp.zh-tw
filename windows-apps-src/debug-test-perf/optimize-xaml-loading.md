---
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: 最佳化您的 XAML 標記
description: 剖析 XAML 標記以在記憶體建構物件，對複雜 UI 而言很耗費時間。 以下是一些您可以執行的動作，以針對您的應用程式改善 XAML 標記剖析和載入時間及記憶體效率。
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ec88af01e46788ea9f24760af7f9a3b81281ba8d
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7715998"
---
# <a name="optimize-your-xaml-markup"></a>最佳化您的 XAML 標記


剖析 XAML 標記以在記憶體建構物件，對複雜 UI 而言很耗費時間。 以下是一些您可執行來改善 XAML 標記剖析與載入時間及 App 記憶體效率的動作。

在 app 啟動時，限制 XAML 標記僅針對您需要的初始 UI 載入。 檢查初始頁面中的標記 (包括頁面資源)，並確認您沒有載入不是立即需要的額外元素。 這些元素可能來自各種不同的來源，例如資源字典、最初既已摺疊的元素，以及繪製在其他元素上的元素。

針對效率最佳化您的 XAML 需要做出取捨；不一定會有各種情況都適用的單一解決方案。 我們來看看一些常見的問題，並提供您可用來為 App 進行適當權衡取捨的指導方針。

## <a name="minimize-element-count"></a>最小化元素計數

雖然 XAML 平台能夠顯示大量的元素，您可以讓您的 app 配置和轉譯速度更快，方法是使用最少量的元素來達成您想要的視覺效果。

您對如何配置 UI 控制項所做的選擇會影響 App 啟動時所建立的 UI 元素數目。 如需有關最佳化版面配置的詳細資訊，請參閱[最佳化您的 XAML 版面配置](optimize-your-xaml-layout.md)。

元素計數在資料範本中極其重要，因為會針對每個資料項目重新建立各個元素。 如需有關減少清單或方格中元素計數的詳細資訊，請參閱 [ListView 與 GridView UI 最佳化](optimize-gridview-and-listview.md)文章中的*每個項目的元素減少*。

我們來看看一些可以減少 App 參數在啟動時載入之元素數目的其他方式。

### <a name="defer-item-creation"></a>延遲建立項目

如果您的 XAML 標記包含不會立即顯示的元素，您可以將這些元素延遲到要顯示時再載入。 比如說，您可以延遲建立不可見內容，例如索引標籤式 UI 中的次要索引標籤。 或者，可以預設將項目顯示在方格檢視中，但提供使用者可改為在清單中檢視資料的選項。 您可以將清單延遲到需要時再載入。

使用 [x:Load 屬性](../xaml-platform/x-load-attribute.md) 而非 [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.Visibility) 屬性來控制項顯示元素的時機。 當元素的可見度設定為 **Collapsed** 時，系統會在轉譯階段跳過該元素，但仍然要付出物件執行個體佔用記憶體的代價。 改用 x:Load 時，除非必要，否則架構不會建立物件執行個體，因此記憶體使用量甚至更低。 缺點是未載入 UI 時，會產生少量的記憶體額外負荷 (約為 600 個位元組)。

> [!NOTE]
> 您可以使用 [x:Load](../xaml-platform/x-load-attribute.md) 或 [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md) 屬性來延遲載入元素。 x:Load 屬性是在 Windows 10 Creators Update (版本 1703，SDK 組建 15063) 中開始提供。 Visual Studio 專案設為目標的版本必須至少是 *Windows 10 Creators Update (10.0，組建 15063)*，才能使用 x:Load。 若要以舊版為目標，請使用 x:DeferLoadStrategy。

下列範例顯示元素計數及記憶體使用量在使用不同技術隱藏 UI 元素時的差異。 將包含完全相同項目的 ListView 和 GridView 放置在頁面的根方格中。 隱藏 ListView，但顯示 GridView。 這其中每個範例的 XAML 都會在螢幕上產生相同的 UI。 我們使用 Visual Studio [分析和效能的工具](tools-for-profiling-and-performance.md)來檢查元素計數和記憶體使用量。

#### <a name="option-1---inefficient"></a>選項 1 - 效率低

ListView 在此處載入，但因寬度為 0 而看不到。 ListView 及其每個子元素已建立於視覺化樹狀結構中，並已載入記憶體中。

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <ListView x:Name="List1" Width="0">
        <ListViewItem>Item 1</ListViewItem>
        <ListViewItem>Item 2</ListViewItem>
        <ListViewItem>Item 3</ListViewItem>
        <ListViewItem>Item 4</ListViewItem>
        <ListViewItem>Item 5</ListViewItem>
        <ListViewItem>Item 6</ListViewItem>
        <ListViewItem>Item 7</ListViewItem>
        <ListViewItem>Item 8</ListViewItem>
        <ListViewItem>Item 9</ListViewItem>
        <ListViewItem>Item 10</ListViewItem>
    </ListView>

    <GridView x:Name="Grid1">
        <GridViewItem>Item 1</GridViewItem>
        <GridViewItem>Item 2</GridViewItem>
        <GridViewItem>Item 3</GridViewItem>
        <GridViewItem>Item 4</GridViewItem>
        <GridViewItem>Item 5</GridViewItem>
        <GridViewItem>Item 6</GridViewItem>
        <GridViewItem>Item 7</GridViewItem>
        <GridViewItem>Item 8</GridViewItem>
        <GridViewItem>Item 9</GridViewItem>
        <GridViewItem>Item 10</GridViewItem>
    </GridView>
</Grid>
```

載入 ListView 後的即時視覺化樹狀結構。 頁面的元素總計數為 89。

![包含清單檢視的視覺化樹狀結構](images/visual-tree-1.png)

ListView 及其子系已載入記憶體中。

![包含清單檢視的視覺化樹狀結構](images/memory-use-1.png)

#### <a name="option-2---better"></a>選項 2 - 較佳

ListView 的可見度在此處設定為已摺疊 (其他 XAML 與原先完全相同)。 已於視覺化樹狀結構中建立 ListView，但未建立其子元素。 不過，這些子元素是載入記憶體中，因此記憶體使用量與上一個範例的相同。

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

ListView 已摺疊的即時視覺化樹狀結構。 頁面的元素總計數為 46。

![包含已摺疊清單檢視的視覺化樹狀結構](images/visual-tree-2.png)

ListView 及其子系已載入記憶體中。

![包含清單檢視的視覺化樹狀結構](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>選項 3 - 最具效率

ListView 在此處將 x:Load 屬性設定為 **False** (其他 XAML 與原先完全相同)。 啟動時並未在視覺化樹狀結構中建立 ListView，也未將其載入記憶體中。

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

未載入 ListView 的即時視覺化樹狀結構。 頁面的元素總計數為 45。

![未載入清單檢視的即時視覺化樹狀結構](images/visual-tree-3.png)

ListView 及其子系未載入記憶體中。

![包含清單檢視的視覺化樹狀結構](images/memory-use-3.png)

> [!NOTE]
> 這些範例的元素計數和記憶體使用量都很小，只是顯示來示範概念。 在這些範例中，使用 x:Load 的額外負荷大於節省的記憶體，因此應用程式並未受益。 您應該對您的 App 使用分析工具，以判斷 App 是否因為延遲載入而受益。

### <a name="use-layout-panel-properties"></a>使用版面配置面板屬性

版面配置面板具有 [Background](https://msdn.microsoft.com/library/windows/apps/BR227512) 屬性，因此不需要只是為了上色而將 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 放在面板最前面。

**效率低**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid>
    <Rectangle Fill="Black"/>
</Grid>
```

**有效率**

```xaml
<Grid Background="Black"/>
```

版面配置面板還有內建的框線屬性，因此不需要在版面配置面板周圍放置 [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) 元素。 如需詳細資訊和範例，請參閱[最佳化您的 XAML 版面配置](optimize-your-xaml-layout.md)。

### <a name="use-images-in-place-of-vector-based-elements"></a>使用影像來取代向量元素

如果您重複使用相同向量元素的次數多到一定程度，改用 [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 元素會更有效率。 因為 CPU 必須個別建立每個元素，向量元素可能會耗費較多資源。 影像檔只需要解碼一次。

## <a name="optimize-resources-and-resource-dictionaries"></a>最佳化資源和資源字典

您通常會使用[資源字典](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)，在有點像全域的層級上儲存要在 App 中多個地方參考的資源。 例如，樣式、筆刷、範本等等。

一般而言，我們已將 [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) 最佳化，除非有要求，否則不會將資源具現化。 但有些情況還是應該避免，不要讓資源進行無謂的具現化。

### <a name="resources-with-xname"></a>包含 x:Name 的資源

使用 [x:Key 屬性](../xaml-platform/x-key-attribute.md)來參考您的資源。 任何含有 [x:Name 屬性](../xaml-platform/x-name-attribute.md)的資源都無法從平台最佳化受益，反而是在 ResourceDictionary 一經建立之後，就會立即進行具現化。 這是因為 x:Name 會告知平台您的應用程式需要此資源的欄位存取權，因此平台需要建立可供建立參考的內容。

### <a name="resourcedictionary-in-a-usercontrol"></a>UserControl 中的 ResourceDictionary

在 [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) 內定義的 ResourceDictionary 會受到不利影響。 平台會針對 UserControl 的每個執行個體建立這類 ResourceDictionary 的複本。 如果您有一個使用頻率很高的 UserControl，則請將 ResourceDictionary 從 UserControl 中移出，然後將它放到頁面層級。

### <a name="resource-and-resourcedictionary-scope"></a>Resource 和 ResourceDictionary 領域

如果頁面參照不同檔案中定義的使用者控制項或資源，則架構也會剖析該檔案。

這裡的 _InitialPage.xaml_ 使用來自 _ExampleResourceDictionary.xaml_ 的某個資源，因此必須在啟動時剖析整個 _ExampleResourceDictionary.xaml_。

**InitialPage.xaml。**

```xaml
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml。**

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
        are used in the app, but are not needed during startup.-->
</ResourceDictionary>
```

如果您會在 App 的許多頁面上使用資源，則將資料儲存在 _App.xaml_ 是不錯做法，而且可以避免重複。 但是 _App.xaml_ 會在 app 啟動時進行剖析，因此任何只有一個頁面使用的資源 (除非該頁面是初始頁面) 都應該放在頁面的本機資源中。 此範例顯示包含僅由一個頁面 (不是初始頁面) 使用的資源的 _App.xaml_。 這會無端地增加 App 啟動時間。

**App.xaml**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Application ...>
     <Application.Resources>
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/>
    </Application.Resources>
</Application>
```

**InitialPage.xaml。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <StackPanel>
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/>
    </StackPanel>
</Page>
```

**SecondPage.xaml。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.SecondPage" ...>
    <StackPanel>
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" />
    </StackPanel>
</Page>
```

為了讓這些範例更有效率，請將 `SecondPageTextBrush` 移入 _SecondPage.xaml_，並將 `ThirdPageTextBrush` 移入 _ThirdPage.xaml_。 `InitialPageTextBrush` 可以保留在 _App.xaml_ 中，因為在任何情況下，應用程式資源都必須在 App 啟動時進行剖析。

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>將看起來相同的多個筆刷合併成一個資源

XAML 平台會嘗試快取常用的物件，這樣就可以盡可能地重複使用這些物件。 但是，XAML 無法清楚分辨某個標記中宣告的筆刷與另一個標記中宣告的筆刷是否相同。 此處的範例使用 [SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962) 進行示範，但是使用 [GradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210068) 的案例會更適合且更重要。 此外，還會檢查是否有使用預先定義色彩的筆刷；例如 `"Orange"` 和 `"#FFFFA500"` 是相同的色彩。

**效率低。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBox>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

若要修正重複，將筆刷定義為資源。 如果其他頁面中的控制項使用相同的筆刷，請將其移至 _App.xaml_。

**有效率。**

```xaml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>

    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## <a name="minimize-overdrawing"></a>盡量減少過度繪製

過度繪製是在以相同螢幕像素繪製多個物件的情況下發生。 請注意，有時需要在此指導方針與元素計數最小化願望之間進行權衡取捨。

使用 [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) 做為目視診斷方法。 您可能會發現原先不知道已在場景內的繪製中物件。

### <a name="transparent-or-hidden-elements"></a>透明或隱藏的元素

如果元素因為透明或隱藏在其他元素後面而看不見，並且不影響版面配置，則將其刪除。 如果元素在初始視覺狀態看不見，但是在其他視覺狀態中可見，請使用 x:Load 來控制其狀態，或者在元素本身將 [Visibility](https://msdn.microsoft.com/library/windows/apps/BR208992) 設為 **Collapsed**，並且在適當狀態中將值變更為 **Visible**。 這個啟發學習法有例外狀況：一般來說，屬性在大多數視覺狀態中的值最好是在本機對元素進行設定。

### <a name="composite-elements"></a>複合元素

使用複合元素，而不要分層放置多個元素來建立效果。 在這個範例中，結果是雙色調圖形，上半部是黑色 (從 [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) 的背景)，下半部是灰色 (從半透明白色的 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) alpha 混合 **Grid** 的黑色背景)。 這裡需要 150% 的像素以達成填滿的結果。

**沒有效率。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Black">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/>
</Grid>
```

**有效率。**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Fill="Black"/>
    <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
</Grid>
```

### <a name="layout-panels"></a>版面配置面板

配置面板可以有兩個用途：為區域上色和配置子元素。 如果進一步以 z 順序排序的元素已為區域上色，則前方的配置面板不需要繪製該區域：而是可以只專注於配置其子系。 這裡提供一個範例。

**沒有效率。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid Background="Blue"/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

**有效率。**

```xaml
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

如果 [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) 可以進行點擊測試，則在其上設定透明背景值。

### <a name="borders"></a>框線

使用 [Border](https://msdn.microsoft.com/library/windows/apps/BR209253) 元素以在物件周圍繪製框線。 在這個範例中，[Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) 是做為 [TextBox](https://msdn.microsoft.com/library/windows/apps/BR209683) 周圍的臨時框線。 但是會過度繪製中心儲存格中的所有像素。

**沒有效率。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Blue" Width="300" Height="45">
    <Grid.RowDefinitions>
        <RowDefinition Height="5"/>
        <RowDefinition/>
        <RowDefinition Height="5"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="5"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="5"/>
    </Grid.ColumnDefinitions>
    <TextBox Grid.Row="1" Grid.Column="1"></TextBox>
</Grid>
```

**有效率。**

```xaml
 <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
     <TextBox/>
</Border>
```

### <a name="margins"></a>邊界

請注意邊界。 如果負數邊界延伸到彼此轉譯的界限內，並且導致過度繪製，則兩個鄰近的元素會重疊 (可能會意外發生)。

### <a name="cache-static-content"></a>快取靜態內容

另一個過度繪製的來源是來自許多重疊元素的圖形。 如果您在 [UIElement](https://msdn.microsoft.com/library/windows/apps/BR208911)上將 [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084) 設為 **BitmapCache**，該元素包含組合圖形，則平台會將元素轉譯為點陣圖一次，然後在每個框架使用該點陣圖，而不會過度繪製。

**沒有效率。**

```xaml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![三個單色圓圈組成的文氏圖表](images/solidvenn.png)

上面的影像是結果，但以下是過度繪製區域的地圖。 紅色越深代表過度繪製的次數越多。

![顯示重疊區域的文氏圖表](images/translucentvenn.png)

**有效率。**

```xaml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

請注意使用 [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084)。 如果有任何子圖形具有動畫效果，請勿使用這項技巧，因為每個框架可能需要重新產生點陣快取，而導致破壞原本的目的。

## <a name="use-xbf2"></a>使用 XBF2

XBF2 是 XAML 標記的二進位表示法，可避免在執行階段產生的所有文字剖析成本。 它也會將您的二進位檔針對載入和樹狀目錄建立進行最佳化，並且允許針對 XAML 類型使用「快速路徑」以改善堆積和物件建立成本，例如 VSM、ResourceDictionary、樣式等等。 它完全採用記憶體對應，因此沒有用來載入和讀取 XAML 頁面的堆積磁碟使用量。 此外，它也可以減少 appx 中儲存的 XAML 頁面的磁碟使用量。 XBF2 是一個較精簡的表示法，與 XAML/XBF1 檔案相比，最多可以減少 50% 的磁碟使用量。 例如，內建的「相片」應用程式在轉換成 XBF2 之後的縮減程度大約為 60%，從大約 ~1 mb 的 XBF1 資產降低成 ~400kb 的 XBF2 資產。 我們也看過應用程式的受益情況是在 CPU 方面的縮減程度大約為 15% 到 20%，而在 Win32 堆積方面為 10% 到 15%。

XAML 內建控制項和架構提供的字典已經完全啟用 XBF2。 針對您自己的應用程式，請確定您專案檔宣告的是 TargetPlatformVersion 8.2 或更新版本。

若要檢查您是否有 XBF2，請在二進位編輯器中開啟您的應用程式；如果您有 XBF2，則第 12 和第 13 個位元組會是 00 02。

## <a name="related-articles"></a>相關文章

- [App 啟動效能的最佳做法](best-practices-for-your-app-s-startup-performance.md)
- [最佳化您的 XAML 配置](optimize-your-xaml-layout.md)
- [ListView 與 GridView UI 最佳化](optimize-gridview-and-listview.md)
- [分析和效能的工具](tools-for-profiling-and-performance.md)
