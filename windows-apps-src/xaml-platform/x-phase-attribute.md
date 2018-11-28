---
title: x:Phase 屬性
description: 搭配使用 xPhase 與 xBind 標記延伸，可用遞增方式轉譯 ListView 和 GridView 項目，並改善移動瀏覽體驗。
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6def088b3e7f6410f12d1b2e411bcb547c90a09a
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7971834"
---
# <a name="xphase-attribute"></a>x:Phase 屬性


搭配使用 **x:Phase** 與 [{x:Bind} 標記延伸](x-bind-markup-extension.md)，可用遞增方式轉譯 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 項目，並改善移動瀏覽體驗。 **x:Phase** 可讓您以宣告方式，達成與使用 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) 事件手動控制清單項目的呈現相同的效果。 另請參閱[以遞增方式更新 ListView 與 GridView 項目](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally)。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>XAML 值


| 詞彙 | 說明 |
|------|-------------|
| PhaseValue | 一個數值，指出元素的處理階段。 預設是 0。 | 

## <a name="remarks"></a>備註

如果使用觸控或滑鼠滾輪時可以快速移動瀏覽清單，清單轉譯項目的速度有可能會跟不上捲動速度，視資料範本的複雜度而定。 特別是使用省電 CPU 的可攜式裝置 (例如手機或平板電腦)，更是如此。

分段可以讓資料範本以遞增方式轉譯，以訂出內容的優先順序，讓最重要的元素最先轉譯。 如此，清單將可在移動瀏覽速度較快時顯示每個項目的部分內容，並在時間允許時轉譯各個範本的較多元素。

## <a name="example"></a>範例

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

資料範本說明 4 個階段：

1.  顯示 DisplayName 文字區塊。 所有未指定階段的控制項，都將隱含地被視為階段 0 的一部分。
2.  顯示 prettyDate 文字區塊。
3.  顯示 prettyFileSize 和 prettyImageSize 文字區塊。
4.  顯示影像。

分段是 [{x:Bind}](x-bind-markup-extension.md) 的功能之一，可與衍生自 [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) 的控制項搭配運作，並可用遞增方式處理資料繫結的項目範本。 轉譯清單項目時，**ListViewBase** 會在單一階段中轉譯檢視中的所有項目，再移至下一個階段。 轉譯工作會以分時段的批次執行，因此在清單捲動時可以重新評估所需的工作，且對於不再顯示的項目將不會執行轉譯。

**x:Phase** 屬性可以對資料範本中任何使用 [{x:Bind}](x-bind-markup-extension.md) 的元素指定。 當元素的階段不是 0 時，該元素將會隱藏於檢視中 (透過 **Opacity**，而非 **Visibility**)，直到該階段處理完成且繫結更新為止。 [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) 衍生的控制項在捲動時，將會從已不在畫面上的項目回收項目範本，以轉譯新的可見項目。 範本中的 UI 元素會保留其舊值，直到再次完成資料繫結為止。 分段會造成資料繫結步驟延遲，因此在分段時必須隱藏 UI 元素，以免它們過時。

每個 UI 元素可能只有一個指定的階段。 若是如此，將會套用到元素的所有繫結。 若未指定階段，將會使用階段 0。

階段號碼不需要是連續的，且會與 [**ContainerContentChangingEventArgs.Phase**](https://msdn.microsoft.com/library/windows/apps/dn298493) 的值相同。 在處理 **x:Phase** 繫結之前，將會為每個階段引發 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) 事件。

分段只會對 [{x:Bind}](x-bind-markup-extension.md) 繫結造成影響，不會影響到 [{Binding}](binding-markup-extension.md) 繫結。

只有使用可辨識分段功能的控制項轉譯項目範本時，才可使用分段。 Windows 10，這是指[**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)和[**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)。 分段不會套用至其他項目控制項中使用的資料範本，或是其他案例 (如 [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/br209369) 或 [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 區段)，而在大多數的情況下，所有的 UI 元素會同時進行資料繫結。

