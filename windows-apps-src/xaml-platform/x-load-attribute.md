---
title: xLoad 屬性
description: xLoad 可讓元素及其子系，減少啟動時間和記憶體使用量的動態建立和解構。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1fa0f12779ad56d57c92f667443644851dc3d5e5
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7976087"
---
# <a name="xload-attribute"></a>x:Load 屬性

您可以使用**X:load**來最佳化啟動時，視覺化樹狀結構建立和 XAML 應用程式的記憶體使用量。 使用**X:load**有其**可見性**，以類似的視覺效果，差別只在於不載入項目時，會釋放它的記憶體，而且小型預留位置內部用於標記其視覺化樹狀結構中的位置。

使用 X:load 屬性化的 UI 元素可以是載入與卸載透過程式碼，或使用[X:bind](x-bind-markup-extension.md)運算式。 這在降低不常顯示或只在特定條件下需要顯示之元素的成本時很有用。 當您使用 X:load 例如方格或 StackPanel 容器上時，在容器及所有子系會載入或卸載為群組。

追蹤延遲元素，由 XAML 架構會增加約 600 個位元組記憶體使用量每個元素使用屬性使用 X:load，針對預留位置。 因此，就可能效能實際降低，過度使用此屬性。 我們建議您只使用它需要隱藏的項目上。 如果您在容器上使用 X:load，額外負荷會支付僅適用於具有 X:load 屬性的項目。

> [!IMPORTANT]
> X:load 屬性是在 Windows 10 版本 1703 (Creators Update) 中開始提供。 Visual Studio 專案設為目標的版本必須至少是 *Windows 10 Creators Update (10.0，組建 15063)*，才能使用 x:Load。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>載入元素

有幾種不同的方式來載入元素：

- 您可以使用[X:bind](x-bind-markup-extension.md)運算式來指定載入狀態。 Expression 應該會傳回**true**來載入和**false**解除載入元素。
- 使用您在元素上定義的名稱呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
- 使用您在元素上定義的名稱呼叫 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
- 在[**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)中，使用 X:load 的元素為目標的[**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)或**腳本**動畫。
- 以任何**腳本**中的卸載的元素為目標。

> 注意：元素在開始具現化後，就會建立在 UI 執行緒上，因此如果同時建立太多，就可能造成 UI 間斷。

以任何前述方法建立延遲的元素後，將會發生幾件事：

- 引發元素的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件。
- 設定 X:name 的欄位。
- 會評估元素的任何 X:bind 繫結。
- 如果您已登錄而會接收包含延遲元素之屬性的屬性變更通知，通知將會引發。

## <a name="unloading-elements"></a>卸載的元素

若要卸載元素：

- 您可以使用 X:bind 運算式來指定載入狀態。 Expression 應該會傳回**true**來載入和**false**解除載入元素。
- 在頁面或使用者控制項中，呼叫**UnloadObject**並傳入的物件參考
- 呼叫**Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** ，然後傳入的物件參考

卸載物件時，它用來取代樹狀目錄中的預留位置。 直到已發行的所有參考，將會維持在記憶體中的物件執行個體。 在頁面/UserControl UnloadObject API 被設計來釋出保有 codegen X:name 和 X:bind 的參照。 如果您在他們將也需要發行的應用程式程式碼中保留額外的參考。

卸載元素時，所有項目相關聯的狀態將會被捨棄，因此如果使用 X:load 的可見性，最佳化版本為然後確保所有狀態套用透過繫結，或重新套用程式碼所引發 Loaded 的事件時。

## <a name="restrictions"></a>限制

使用**X:load**的限制是：

- 您必須定義一個[X:name](x-name-attribute.md)元素，因為程式必須至少要能在稍後找到該元素。
- 您只能使用 X:load 上從[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)或[**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)衍生的類型。
- 您無法使用 X:load[**頁面**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page)、 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)或在[**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)中的根元素上。
- 您無法在[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)中的項目上使用 X:load。
- 您無法使用 X:load 上使用[**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)載入的鬆散 XAML。
- 移動父元素將會清除任何尚未載入的項目。

## <a name="remarks"></a>備註

不過，他們可以從在最外層元素開始辨識，您可以巢狀在元素上，使用 X:load。 如果您嘗試在辨識父元素之前辨識子元素，將會引發例外狀況。

一般而言，我們建議您延後不會出現在第一個畫面中的元素。在尋找要延遲的候選項目時，建議您尋找以摺疊的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 建立的元素。 此外，使用者互動所觸發的 UI 也是您尋找延遲元素的理想之處。

請留意 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 中的延遲元素，因為它雖然可減少您的啟動時間，但也可能隨著您所建立的項目而導致移動瀏覽效能下降。 如果您想要提升移動瀏覽效能，請參閱 [{x:Bind} 標記延伸](x-bind-markup-extension.md)和 [x:Phase 屬性](x-phase-attribute.md)文件。

如果[X:phase 屬性](x-phase-attribute.md)使用**X:load**搭配然後，當元素或元素樹狀結構實現之後，繫結會向上套用且包含目前階段。 **X:phase**所指定的階段不會影響或控制元素的載入狀態。 當清單項目視為移動瀏覽的一部分回收時，實現的元素的行為相同的方式的其他作用中的元素，以及已編譯的繫結 （**{X:bind}** 繫結） 會使用相同的規則，包括分段處理。

一般會建議您在之前或之後測量您應用程式的執行效能，以確保您得到想要的效能。

## <a name="example"></a>範例

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```

