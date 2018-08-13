---
author: jwmsft
title: xLoad 屬性
description: xLoad 可讓動態建立與損毀的項目及其子項，會降低啟動時間及記憶體使用量。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f34ea43b981de65c81e9cce2b8a896c3577e595
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "610783"
---
# <a name="xload-attribute"></a>x:Load 屬性

您可以使用**X： 負載**最佳化啟動、 視覺樹狀目錄中建立和 XAML 應用程式的記憶體使用量。 時未載入元素、 發行其記憶體而內部小型的版面配置區用來標示對開視覺化樹狀結構中的使用**X： 負載**具有**可見性**、 類似視覺效果。

X： 負載對應的 UI 元素可以載入和卸載透過程式碼，或使用[X： 繫結](x-bind-markup-extension.md)運算式。 這在降低不常顯示或只在特定條件下需要顯示之元素的成本時很有用。 當您使用 x： 負載如方格或 StackPanel 容器上、 容器與它所有的子項會載入或未載入以群組方式。

XAML framework 延期元素的追蹤會新增對應 x： 負載，以便版面配置區的每個元素的記憶體使用狀況約 600 個位元組。 因此，則有可能您效能實際縮小的過度使用此屬性。 我們建議您僅使用它需要為隱藏的元素。 如果您使用 x： 負載容器上，額外負荷為付費僅針對 x： 負載屬性的元素。

> [!IMPORTANT]
> X： 負載屬性是可啟動第 10 Windows 版本 1703 （建立者更新）。 Visual Studio 專案設為目標的版本必須至少是 *Windows 10 Creators Update (10.0，組建 15063)*，才能使用 x:Load。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>載入元素

有數種不同的方式載入元素：

- 使用[X： 繫結](x-bind-markup-extension.md)運算式來指定負載狀態。 運算式應傳回**true**載入及**false**卸載項目。
- 使用您在元素上定義的名稱呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
- 使用您在元素上定義的名稱呼叫 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
- 在[**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)中，使用為目標的 x： 負載元素[**set 存取子**](https://msdn.microsoft.com/library/windows/apps/br208817)或**腳本**動畫。
- 目標任何**腳本**中的卸載項目。

> 注意：元素在開始具現化後，就會建立在 UI 執行緒上，因此如果同時建立太多，就可能造成 UI 間斷。

以任何前述方法建立延遲的元素後，將會發生幾件事：

- 引發元素的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件。
- 設定 x： 名稱] 欄位。
- 項目上的任何 x： 繫結繫結進行評估。
- 如果您已登錄而會接收包含延遲元素之屬性的屬性變更通知，通知將會引發。

## <a name="unloading-elements"></a>卸載的元素

若要卸載元素：

- 使用 x： 繫結運算式來指定負載狀態。 運算式應傳回**true**載入及**false**卸載項目。
- 在頁面或 UserControl 中，呼叫**UnloadObject**並傳遞的物件參考 （英文）
- 呼叫**Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject**並傳遞的物件參考 （英文）

卸載物件時，它將會取代樹狀目錄中的預留位置。 物件執行個體將都維持在記憶體中已經發行的所有參考。 在頁面/UserControl UnloadObject API 的設計版本所 codegen x: Name 與 x： 繫結的參照。 如果您保留其他參考他們也需要要發行的應用程式碼中。

元素卸載時，元素相關聯的所有狀態則會捨棄，因此如果使用 x： 負載為可見性、 最佳化版本然後確定所有狀態透過繫結，就會套用或重新套用的程式碼時引發載入的事件時。

## <a name="restrictions"></a>限制

使用**X： 負載**的限制是：

- 您必須針對元素定義 [x:Name](x-name-attribute.md)，因為程式必須至少要能在稍後找到該元素。
- 您只能使用 x： 負載衍生自[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)或[**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)的類型。
- 您無法使用 x： 載入[**頁面**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page)、 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)或[**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)中的根元素。
- 您無法使用 x： 負載[**資源字典**](https://msdn.microsoft.com/library/windows/apps/br208794)中的元素。
- 您不可以載入含有[**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)寬鬆 XAML x： 負載。
- 移動父項目將會清除出未載入任何元素。

## <a name="remarks"></a>備註

您可以使用 x： 負載巢狀元素然而他們有實現從最外部的元素中。  如果您嘗試在辨識父元素之前辨識子元素，將會引發例外狀況。

一般而言，我們建議您延後不會出現在第一個畫面中的元素。 在尋找要延遲的候選項目時，建議您尋找以摺疊的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 建立的元素。 此外，使用者互動所觸發的 UI 也是您尋找延遲元素的理想之處。

請留意 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 中的延遲元素，因為它雖然可減少您的啟動時間，但也可能隨著您所建立的項目而導致移動瀏覽效能下降。 如果您想要提升移動瀏覽效能，請參閱 [{x:Bind} 標記延伸](x-bind-markup-extension.md)和 [x:Phase 屬性](x-phase-attribute.md)文件。

如果[x： 階段屬性](x-phase-attribute.md)一起使用**X： 負載**然後當項目或項目樹狀結構實現，向上，且包含目前的階段套用繫結。 指定**X： 階段**的階段不會影響或控制項之元素的載入狀態。 回收一部分的 panning 清單項目時，實現元素的行為相同的方式為其他作用中的元素，以及使用相同的規則，包括 phasing 處理編譯繫結 （**{X： 繫結}** 繫結）。

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

