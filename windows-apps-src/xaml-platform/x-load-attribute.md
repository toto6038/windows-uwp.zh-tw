---
title: xLoad 屬性
description: xLoad 允許元素和其子項目的動態建立與破壞，並可降低啟動時間和記憶體使用量。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b110091c1a1f208ad06ea52f5c84dc7daad56c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161682"
---
# <a name="xload-attribute"></a>x:Load 屬性

您可以使用 **x:Load** 最佳化您 XAML 應用程式的啟動、視覺化樹狀結構的建立，以及記憶體使用量 。 使用 **x:Load** 與 **Visibility** 有類似的效果，但其差異在於當元素沒有載入時，記憶體將會釋出，並且會在其內部使用一個小型的預留位置標記其在視覺化樹狀結構中的位置。

帶有 x:Load 屬性的 UI 元素可透過程式碼載入或解除載入，或是使用 [x:Bind](x-bind-markup-extension.md) 運算式。 這在降低不常顯示或只在特定條件下需要顯示之元素的成本時很有用。 當您在一個容器 (例如 Grid 或 StackPanel) 上使用 x:Load 時，容器及其所有的子項目都會作為一個群組載入或取消載入。

XAML 架構為了追蹤延遲元素，會針對每一個帶有 x:Load 屬性的元素增加 600 位元組的記憶體使用量，作為其預留位置使用。 因此，過度使用此屬性可能反而會導致效能降低。 我們建議您只針對需要隱藏的項目使用此屬性。 若您在一個容器上使用 x:Load，則系統只需要針對帶有 x:Load 屬性的元素承擔額外負荷。

> [!IMPORTANT]
> 從 Windows 10，1703版 (建立者更新) 開始提供 x:Load 屬性。 Visual Studio 專案設為目標的版本必須至少是 *Windows 10 Creators Update (10.0，組建 15063)*，才能使用 x:Load。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>載入元素

有幾種不同的方式可用來載入元素：

- 使用 [x:Bind](x-bind-markup-extension.md) 運算式指定載入狀態。 運算式應該會傳回 **true** 以進行載入，以及 **false** 以取消載入。
- 使用您在元素上定義的名稱呼叫 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)。
- 使用您在元素上定義的名稱呼叫 [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild)。
- 在 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) 中，使用以 x:Load 元素為目標的 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 或 **Storyboard** 動畫。
- 在任何一個 **Storyboard** 中以取消載入的元素做為目標。

> 注意：元素在開始具現化後，就會建立在 UI 執行緒上，因此如果同時建立太多，就可能造成 UI 間斷。

以任何前述方法建立延遲的元素後，將會發生幾件事：

- 引發元素的 [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) 事件。
- 設定 x:Name 欄位。
- 任何正繫結在元素上的 x:Bind 都會受到評估。
- 如果您已登錄而會接收包含延遲元素之屬性的屬性變更通知，通知將會引發。

## <a name="unloading-elements"></a>取消載入元素

若要取消載入元素：

- 使用 x:Bind 運算式指定載入狀態。 運算式應該會傳回 **true** 以進行載入，以及 **false** 以取消載入。
- 在 Page 或 UserControl 中，呼叫 **UnloadObject** 並傳入物件參考
- 呼叫 **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** 並傳入物件參考

在物件取消載入時，一個預留位置將會取代其原先在樹狀結構中的位置。 物件執行個體將會繼續保存在記憶體中，直到所有的參考都獲得釋出。 Page/UserControl 上的 UnloadObject API，其設計的目的為了要釋出 Codegen 為 x:Name 和 x:Bind 保留的參考。 若您在應用程式程式碼中保留了額外的參考，您也需要將這些參考釋出。

當一個元素取消載入時，與該元素相關聯的所有狀態都會捨棄，因此若要將 x:Load 作為 Visibility 的最佳化版本使用，請務必確認所有的狀態都已透過繫結套用，或是當 Loaded 事件發出時透過程式碼重新套用。

## <a name="restrictions"></a>限制

**x:Load** 的使用限制如下：

- 您必須為專案定義[x：Name](x-name-attribute.md)   ，因為稍後需要有方法來尋找元素。
- 您只能在 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 或 [**FlyoutBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase) 衍生的類型上使用 x:Load。
- 您不能在 [**Page**](/uwp/api/windows.ui.xaml.controls.page)、[**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)，或 [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) 中的根元素上使用 x:Load。
- 您不能在 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中的元素上使用 x:Load。
- 您無法在與 [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 一同載入的鬆散 XAML 上使用 x:Load。
- 移動父元素將會清除任何尚未載入的元素。

## <a name="remarks"></a>備註

您可以在巢狀元素上使用 x:Load，但必須從最外層元素開始辨識這些元素。 如果您嘗試在辨識父元素之前辨識子元素，將會引發例外狀況。

一般而言，我們建議您延後不會出現在第一個畫面中的元素。在尋找要延遲的候選項目時，建議您尋找以摺疊的 [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) 建立的元素。 此外，使用者互動所觸發的 UI 也是您尋找延遲元素的理想之處。

請留意 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 中的延遲元素，因為它雖然可減少您的啟動時間，但也可能隨著您所建立的項目而導致移動瀏覽效能下降。 如果您想要提升移動瀏覽效能，請參閱 [{x:Bind} 標記延伸](x-bind-markup-extension.md)和 [x:Phase 屬性](x-phase-attribute.md)文件。

如果 [x:Phase 屬性](x-phase-attribute.md)結合 **x:Load** 使用，則當元素或元素樹狀結構實現之後，繫結會向上套用且包含目前階段。 針對 **x:Phase** 指定的階段將會影響或控制元素的載入狀態。 當清單項目被視為移動瀏覽的一部分回收時，實現的元素會與其他作用中元素的行為相同，而已編譯的繫結 (**{x:Bind}** 繫結) 會使用相同的規則處理 (包括階段處理)。

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