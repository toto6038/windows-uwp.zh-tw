---
ms.assetid: 79CF3927-25DE-43DD-B41A-87E6768D5C35
title: 最佳化您的 XAML 配置
description: 配置可說是 XAML 應用程式中高度耗費資源的一部分，包括在 CPU 使用量與記憶體負荷方面。 您可以採取下列一些簡單步驟來提升 XAML app 的版面配置效能。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 92dca27a4cfb02f5d1bcb722683eca89ec16a6d6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "66362216"
---
# <a name="optimize-your-xaml-layout"></a>最佳化您的 XAML 配置


**重要 API**

-   [**面板**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel)

版面配置是為 UI 定義視覺化結構的程序。 用來說明 XAML 版面配置的主要機制是透過面板，這類面板是讓您能夠在其中放置與排列 UI 元素的容器物件。 在 CPU 使用量與記憶體負荷方面，版面配置可說是 XAML app 中高度耗費資源的一部分。 您可以採取下列一些簡單步驟來提升 XAML app 的版面配置效能。

## <a name="reduce-layout-structure"></a>減少版面配置結構

簡化 UI 元素樹狀結構的階層結構，就能獲得最大的版面配置效能。 面板存在於視覺化樹狀結構中，但它們是結構化元素，而不是像 [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 或 a [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的「像素產生的元素」  。 藉由減少非像素產生的元素來簡化樹狀結構，通常可大幅提升效能。

許多 UI 都是透過巢狀面板來實作，因而產生了既深且複雜的面板與元素樹狀結構。 巢串面板是非常簡便的，但在許多情況下，您可以利用更複雜的單一面板來達成相同的 UI。 使用單一面板可提供較佳的效能。

### <a name="when-to-reduce-layout-structure"></a>減少版面配置結構的時機

使用簡單的方式來減少版面配置結構—例如，從最上層頁面減少一個巢狀面板—並不會有顯著的效果。

最大的效能提升是來自減少 UI 中重複的版面配置結構，例如 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 或 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)。 這些 [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 元素會使用 [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) 來定義 UI 元素的樹狀子目錄 (其已多次具現化)。 當同一個樹狀子目錄在您的 app 中多次重複出現時，任何對於該樹狀子目錄的效能提升，都會使對 app 整體效能所產生的影響倍增。

### <a name="examples"></a>範例

考量下列 UI。

![表單版面配置範例](images/layout-perf-ex1.png)

下列範例示範 3 種實作同一個 UI 的方式。 每個實作選項都會在畫面上產生幾乎完全相同的像素，但在實作細節上卻大不相同。

選項 1：巢狀 [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) 元素

雖然這是最簡單的模型，但它使用了 5 個面板元素，並產生了顯著的負荷。

```xml
  <StackPanel>
  <TextBlock Text="Options:" />
  <StackPanel Orientation="Horizontal">
      <CheckBox Content="Power User" />
      <CheckBox Content="Admin" Margin="20,0,0,0" />
  </StackPanel>
  <TextBlock Text="Basic information:" />
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Name:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Email:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Password:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <Button Content="Save" />
</StackPanel>
```

選項 2：單一 [**方格**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)

[  **Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) 增加了一些複雜度，但只使用單一面板元素。

```xml
<Grid>
  <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
      <ColumnDefinition Width="Auto" />
      <ColumnDefinition Width="Auto" />
  </Grid.ColumnDefinitions>
  <TextBlock Text="Options:" Grid.ColumnSpan="2" />
  <CheckBox Content="Power User" Grid.Row="1" Grid.ColumnSpan="2" />
  <CheckBox Content="Admin" Margin="150,0,0,0" Grid.Row="1" Grid.ColumnSpan="2" />
  <TextBlock Text="Basic information:" Grid.Row="2" Grid.ColumnSpan="2" />
  <TextBlock Text="Name:" Width="75" Grid.Row="3" />
  <TextBox Width="200" Grid.Row="3" Grid.Column="1" />
  <TextBlock Text="Email:" Width="75" Grid.Row="4" />
  <TextBox Width="200" Grid.Row="4" Grid.Column="1" />
  <TextBlock Text="Password:" Width="75" Grid.Row="5" />
  <TextBox Width="200" Grid.Row="5" Grid.Column="1" />
  <Button Content="Save" Grid.Row="6" />
</Grid>
```

選項 3：單一 [**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)：

比起使用巢狀面板，此單一面板同樣較為複雜，但可能會比 [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) 更容易了解和維護。

```xml
<RelativePanel>
  <TextBlock Text="Options:" x:Name="Options" />
  <CheckBox Content="Power User" x:Name="PowerUser" RelativePanel.Below="Options" />
  <CheckBox Content="Admin" Margin="20,0,0,0" 
            RelativePanel.RightOf="PowerUser" RelativePanel.Below="Options" />
  <TextBlock Text="Basic information:" x:Name="BasicInformation"
           RelativePanel.Below="PowerUser" />
  <TextBlock Text="Name:" RelativePanel.AlignVerticalCenterWith="NameBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="NameBox"               
           RelativePanel.Below="BasicInformation" />
  <TextBlock Text="Email:"  RelativePanel.AlignVerticalCenterWith="EmailBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="EmailBox"
           RelativePanel.Below="NameBox" />
  <TextBlock Text="Password:" RelativePanel.AlignVerticalCenterWith="PasswordBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="PasswordBox"
           RelativePanel.Below="EmailBox" />
  <Button Content="Save" RelativePanel.Below="PasswordBox" />
</RelativePanel>
```

如下列範例所示，有許多方式可用來達成相同的 UI。 您應該謹慎地進行全面考量 (包括效能、可讀性和可維護性)，然後做出取捨。

## <a name="use-single-cell-grids-for-overlapping-ui"></a>針對重疊的 UI 使用單一儲存格格線

常見的 UI 需求是讓元素彼此重疊的版面配置。 通常會使用這種方式，利用邊框間距、邊界、對齊和轉換來放置元素。 XAML [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) 控制項已最佳化，可改善重疊元素的版面配置效能。

**重要**  若要查看改進功能，請使用單一儲存格 [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)。 請勿定義 [**RowDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) 或 [**ColumnDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.columndefinitions)。

### <a name="examples"></a>範例

```xml
<Grid>
    <Ellipse Fill="Red" Width="200" Height="200" />
    <TextBlock Text="Test" 
               HorizontalAlignment="Center" 
               VerticalAlignment="Center" />
</Grid>
```

![圓圈上重疊的文字](images/layout-perf-ex2.png)

```xml
<Grid Width="200" BorderBrush="Black" BorderThickness="1">
    <TextBlock Text="Test1" HorizontalAlignment="Left" />
    <TextBlock Text="Test2" HorizontalAlignment="Right" />
</Grid>
```

![格線內的兩個文字區塊](images/layout-perf-ex3.png)

## <a name="use-a-panels-built-in-border-properties"></a>使用面板內建的框線屬性

[**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)、[**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)、[**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)、[**ContentPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter) 控制項具有內建的框線屬性，可讓您沿著控制項繪製框線，而不需要在 XAML 中新增額外的 [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) 元素。 支援內建框線的新屬性如下：**BorderBrush**、**BorderThickness**、**CornerRadius**、**Padding**。 這其中每一個都是 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)，因此您可以將它們與繫結和動畫搭配使用。 它們是設計來完全取代個別的 **Border** 元素。

如果您的 UI 在這些面板周圍具有 [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) 元素，請改用內建的框線，以便在 app 的版面配置結構中省下額外的元素。 如先前所述，這樣能夠大幅節省，特別是在重複 UI 的情況下。

### <a name="examples"></a>範例

```xml
<RelativePanel BorderBrush="Red" BorderThickness="2" CornerRadius="10" Padding="12">
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

## <a name="use-sizechanged-events-to-respond-to-layout-changes"></a>使用 **SizeChanged** 事件來回應版面配置變更

[**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 類別公開兩個可用以回應版面配置變更的類似事件：[**LayoutUpdated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) 和 [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)。 您可能會在進行版面配置期間調整元素大小時，使用這其中一個事件來接收通知。 這兩個事件的語意不同，而且在它們之間進行選擇時有一些重要的效能考量。

如需良好的效能，[**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 幾乎一向是正確的選擇。 **SizeChanged** 具備直覺式語意。 它會在進行版面配置期間更新 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 的大小時引發。

[**LayoutUpdated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) 也會在配置期間引發，但它具備全域語意—其會在更新任何元素時，於每個元素上引發。 通常只會在事件處理常式中進行本機處理，在此情況下，程式碼執行的頻率會比所需的更頻繁。 只有在您需要知道何時將重新放置元素而不會改變其大小 (這不常見) 時，才能使用 **LayoutUpdated**。

## <a name="choosing-between-panels"></a>選擇面板

在選擇個別面板時，通常不會將效能納入考量。 通常是藉由考量哪一個面板可提供最接近您正在實作之 UI 的版面配置行為來選擇。 例如，如果您在 [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)、[**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) 和 [**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) 之間進行選擇，則應該選擇最能對應到您在腦中實作之模型的面板。

每個 XAML 面板都已針對良好的效能進行最佳化，而所有的面板都可為類似 UI 提供類似的效能。

