---
author: jwmsft
title: "xDeferLoadStrategy 屬性"
description: "xDeferLoadStrategy 會延遲建立元素及其子系而縮短啟動時間，但記憶體使用量會略為增加。 每個受影響的元素會增加約 600 個位元組的記憶體使用量。"
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 0fd1e58549ba19397948864fe5fe0b31fcaf01d7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xdeferloadstrategy-attribute"></a>x:DeferLoadStrategy 屬性

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**x:DeferLoadStrategy="Lazy"** 是可用來最佳化 XAML 應用程式啟動效能或樹狀目錄建立案例的一項功能。 使用 **x:DeferLoadStrategy="Lazy"** 會延遲建立元素及它的子項，藉由不需要建立元素來減少啟動時間及記憶體的成本。 這在降低不常需要或只在特定條件下需要之元素的成本時很有用。 當從程式碼或 VisualStateManager 參考元素時，元素將會具現化。

不過，處理延遲的作業會針對每個受影響的元素，增加大約 600 個位元組的記憶體使用量。 延遲的元素樹狀結構愈大，節省的啟動時間就愈多，但代價是記憶體使用量較大。 因此，過度使用此屬性可能導致效能降低。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>備註

**x:DeferLoadStrategy** 的使用限制如下：

-   必須定義 [x:Name](x-name-attribute.md)，因為後續必須要有尋找元素的方法。
-   只有 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 可標示為延遲，但衍生自 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249) 的類型例外。
-   根元素不可在 [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page)、[**UserControls**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol) 或 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) 中延遲。
-   [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中的元素不可延遲。
-   不使用透過 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 載入的鬆散 XAML。
-   移動父元素將會清除任何尚未辨識的元素。

有幾種不同的方式可用來辨識延遲的元素：

-   使用在元素上定義的名稱呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
-   使用在元素上定義的名稱呼叫 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
-   在 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) 中，使用以延遲的元素為目標的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 或 **Storyboard** 動畫。
-   在 **Storyboard** 中以任何延遲的元素做為目標。
-   使用以延遲的元素為目標的繫結。
-   注意：元素在開始具現化後，就會建立在 UI 執行緒上，因此如果同時建立太多，就可能造成 UI 間斷。

以任何前述方法建立延遲的元素後，將會發生幾件事：

-   將會引發元素的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件。
-   將會評估元素的任何繫結。
-   如果應用程式已登錄而會接收包含延遲元素之屬性的屬性變更通知，就會引發通知。

您可以內嵌延遲的元素，但必須從最外層元素開始辨識這些元素。  如果您嘗試在辨識父元素之前辨識子元素，將會引發例外狀況。

一般而言，建議延遲無法在第一個畫面格中檢視的項目。  在尋找要延遲的候選項目時，建議您尋找以摺疊的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 建立的元素。  附帶性 UI (也就是使用者互動所觸發的 UI) 也是您尋找延遲元素的理想之處。  

請留意 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 案例中的延遲元素，因為它雖然可減少您的啟動時間，但也可能隨著您所建立的項目而導致移動瀏覽效能下降。  如果您想要提升移動瀏覽效能，請參閱 [{x:Bind} 標記延伸](x-bind-markup-extension.md)和 [x:Phase 屬性](x-phase-attribute.md)文件。

如果 [x:Phase 屬性](x-phase-attribute.md)結合 **x:DeferLoadStrategy** 使用，則當元素或元素樹狀結構實現之後，繫結會向上套用且包含目前階段。 針對 **x:Phase** 指定的階段將不會影響或控制元素的延遲。 當清單項目被視為移動瀏覽的一部分回收時，實現的元素會與其他作用中元素的行為相同，而已編譯的繫結 (**{x:Bind}** 繫結) 將會使用相同的規則處理 (包括階段處理)。

一般會建議您在之前或之後測量您的應用程式，以確保您得到想要的效能。

## <a name="example"></a>範例

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    this.FindName("DeferredGrid"); // This will realize the deferred grid
}
```

