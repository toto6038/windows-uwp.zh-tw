---
author: Jwmsft
Description: Use the pull-to-refresh control to get new content into a list.
title: 拖動以重新整理
label: Pull-to-refresh
template: detail.hbs
ms.author: jimwalk
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8b1dd6bd1bc165a79ba123c94e63e1dcfa58ec21
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5564885"
---
# <a name="pull-to-refresh"></a>拖動以重新整理

「拖動以重新整理」讓使用者以觸控方式向下拖動資料清單來擷取更多資料。 「拖動以重新整理」已在有觸控式螢幕的裝置上廣泛使用。 您可以使用這裡顯示的 API，實作您的應用程式中的「拖動以重新整理」。

> **重要 API**：[RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer)、[RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![拖動以重新整理 gif](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您提供使用者可能需要定期重新整理的資料清單或方格，且您的應用程式可能要在以觸控為主的裝置上執行時，請使用「拖動以重新整理」。

您也可以使用 [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)，建立以其他方式 (例如透過 [重新整理] 按鈕) 叫用的一致重新整理體驗。

## <a name="refresh-controls"></a>重新整理控制項

「拖動以重新整理」由 2 個控制項啟用。

- **RefreshContainer**：提供「拖動以重新整理」體驗包裝函式的 ContentControl。 這會處理觸控互動，並管理其內部重新整理視覺化檢視的狀態。
- **RefreshVisualizer**：封裝重新整理視覺效果 (下一節會說明這點)。

主要的控制項是 **RefreshContainer**，您要當做使用者所拖動觸發重新整理之內容的包裝函式來放置。 RefreshContainer 只對觸控有作用，因此建議您也應該為沒有觸控介面的使用者提供 [重新整理] 按鈕。 您可以將 [重新整理] 按鈕放置在應用程式中的適當位置，而這若不是在命令列上，就是在靠近要重新整理之介面的位置。

## <a name="refresh-visualization"></a>重新整理視覺效果

預設重新整理視覺效果是圓形進度環，用來傳達有關何時會進行重新整理，以及重新整理起始後進度的資訊。 重新整理視覺化檢視有 5 個狀態。

 使用者必須向下拖動清單才能起始重新整理的距離，稱為_閾值_。 視覺化檢視[狀態](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State)取決於拖動狀態，因為此狀態與這個閾值有關。 可能的值包含在 [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate) 列舉中。

### <a name="idle"></a>Idle

視覺化檢視的預設狀態是 **Idle**。 使用者未透過觸控與 RefreshContainer 互動，而且沒有重新整理在進行中。

在視覺上，沒有任何重新整理視覺化檢視的跡象。

### <a name="interacting"></a>Interacting

當使用者依 PullDirection 屬性所指定的方向拖動清單時，在到達閾值之前，視覺化檢視所處在的狀態即是 **Interacting**。

- 如果使用者在這種狀態時放開控制項，控制項會回復到 **Idle**。

    ![到達閾值之前的拖動以重新整理](images/ptr-prethreshold.png)

    在視覺上，圖示會顯示為已停用 (60% 不透明度)。 此外，圖示還會隨著捲動動作旋轉完整一圈。

- 如果使用者拖動清單超過閾值，視覺化檢視就會從 **Interacting** 轉換到 **Pending**。

    ![到達閾值的拖動以重新整理](images/ptr-atthreshold.png)

    在視覺上，圖示會切換為 100% 不透明度，而大小則在轉換期間跳上 150% 後又回到 100%。

### <a name="pending"></a>Pending

當使用者有拖動清單超過閾值時，視覺化檢視處於 **Pending** 狀態。

- 如果使用者將清單移回閾值以內但不放開，則會回到 **Interacting** 狀態。
- 如果使用者放開清單，則會起始重新整理要求，並轉換到 **Refreshing** 狀態。

![到達閾值之後的拖動以重新整理](images/ptr-postthreshold.png)

在視覺上，圖示的大小和不透明度都是 100%。 在這種狀態下，圖示會繼續隨著捲動動作向下移動，但不再旋轉。

### <a name="refreshing"></a>Refreshing

當使用者在超過閾值後放開視覺化檢視，則會處於 **Refreshing** 狀態。

進入此狀態後，會引發 **RefreshRequested** 事件。 這是開始進行應用程式內容重新整理的訊號。 事件引數 ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) 包含 [Deferral](/uwp/api/windows.foundation.deferral) 物件，您應該在事件處理常式中取得其控制代碼。 然後在完成了執行重新整理的程式碼時，您應該將此延遲 (Deferral) 標示為已完成。

重新整理完成時，視覺化檢視會回到 **Idle** 狀態。

在視覺上，圖示會重新回到閾值位置，並在重新整理期間旋轉。 這個旋轉動作是用來顯示重新整理進度，並且會由傳入內容的動畫取代。

### <a name="peeking"></a>Peeking

當使用者依重新整理方向從不允許重新整理的起始位置拖動時，視覺化檢視會進入 **Peeking** 狀態。 當 ScrollViewer 不在使用者開始拖動時的位置 0 時，通常會發生這種情況。

- 如果使用者在這種狀態時放開控制項，控制項會回復到 **Idle**。

## <a name="pull-direction"></a>拖動方向

使用預設會由上而下拖動清單來起始重新整理。 如果您的清單或方格使用不同的方向，您應該變更重新整理容器的拖動方向來配合。

[PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) 屬性會接受下列其中一個 [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection) 值：**BottomToTop**、**TopToBottom**、**RightToLeft** 或 **LeftToRight**。

當您變更拖動方向時，視覺化檢視進度環的開始位置會自動旋轉，使箭頭開始指到拖動方向的適當位置。 如有需要，您可以變更 [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation) 屬性來覆寫此自動行為。 在大部分情況下，我們建議您保留 **Auto** 的預設值。

## <a name="implement-pull-to-refresh"></a>實作拖動以重新整理

若要將「拖動以重新整理」功能新增至清單，只需要幾個步驟。

1. 將您的清單包裝在 **RefreshContainer** 控制項中。
1. 處理 **RefreshRequested** 事件以重新整理您的內容。
1. 或者，呼叫 **RequestRefresh** (例如，從按鈕按一下動作中) 以起始重新整理。

> [!NOTE]
> 您可以讓 RefreshVisualizer 獨自具現化。 不過，我們建議您將內容包裝在 RefreshContainer 中，並使用 RefreshContainer.Visualizer 屬性所提供的 RefreshVisualizer，即使對非觸控案例，也是這樣。 在本文中，我們假設視覺化檢視一律是從重新整理容器中取得。

> 此外，為了方便，還要使用重新整理容器的 RequestRefresh 和 RefreshRequested 成員。 `refreshContainer.RequestRefresh()` 相當於 `refreshContainer.Visualizer.RequestRefresh()`，而任何一個都會引發 RefreshContainer.RefreshRequested 事件和 RefreshVisualizer.RefreshRequested 事件。

### <a name="request-a-refresh"></a>要求重新整理

重新整理容器會處理觸控互動，讓使用者透過觸控重新整理內容。 我們建議您為非觸控介面提供其他能供性，例如 [重新整理] 按鈕或語音控制。

若要起始重新整理，請呼叫 [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh) 方法。

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

當您呼叫 RequestRefresh 時，視覺化檢視狀態會直接從 **Idle** 進入 **Refreshing**。

### <a name="handle-a-refresh-request"></a>處理重新整理要求

若要在需要時取得重新整理內容，請處理 RefreshRequested 事件。 在事件處理常式中，您將需要應用程式的專屬程式碼來取得重新整理內容。

事件引數 ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) 包含 [Deferral](/uwp/api/windows.foundation.deferral) 物件。 在事件處理常式中取得此延遲 (Deferral) 的控制代碼。 然後在完成了執行重新整理的程式碼時，將此延遲 (Deferral) 標示為已完成。

```csharp
// See the Examples section for the full code.
private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
{
    // Respond to a request by performing a refresh and using the deferral object.
    using (var RefreshCompletionDeferral = args.GetDeferral())
    {
        // Do some async operation to refresh the content

         await FetchAndInsertItemsAsync(3);

        // The 'using' statement ensures the deferral is marked as complete.
        // Otherwise, you'd call
        // RefreshCompletionDeferral.Complete();
        // RefreshCompletionDeferral.Dispose();
    }
}
```

### <a name="respond-to-state-changes"></a>回應狀態變更

您可以視需要回應視覺化檢視狀態的變更。 例如，若要防止多個重新整理要求，您可以在視覺化檢視正在重新整理時停用 [重新整理] 按鈕。

```csharp
// See the Examples section for the full code.
private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
{
    // Respond to visualizer state changes.
    // Disable the refresh button if the visualizer is refreshing.
    if (args.NewState == RefreshVisualizerState.Refreshing)
    {
        RefreshButton.IsEnabled = false;
    }
    else
    {
        RefreshButton.IsEnabled = true;
    }
}
```

## <a name="examples"></a>範例

### <a name="using-a-scrollviewer-in-a-refreshcontainer"></a>使用 RefreshContainer 中的 ScrollViewer

此範例示範如何搭配捲動檢視器使用「拖動以重新整理」。

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>將「拖動以重新整理」新增至 ListView

此範例示範如何搭配清單檢視使用「拖動以重新整理」。

```xaml
<StackPanel Margin="0,40" Width="280">
    <CommandBar OverflowButtonVisibility="Collapsed">
        <AppBarButton x:Name="RefreshButton" Click="RefreshButtonClick"
                      Icon="Refresh" Label="Refresh"/>
        <CommandBar.Content>
            <TextBlock Text="List of items" 
                       Style="{StaticResource TitleTextBlockStyle}"
                       Margin="12,8"/>
        </CommandBar.Content>
    </CommandBar>

    <RefreshContainer x:Name="RefreshContainer">
        <ListView x:Name="ListView1" Height="400">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <Grid Height="80">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <TextBlock Text="{x:Bind Path=Header}"
                                   Style="{StaticResource SubtitleTextBlockStyle}"
                                   Grid.Row="0"/>
                        <TextBlock Text="{x:Bind Path=Date}"
                                   Style="{StaticResource CaptionTextBlockStyle}"
                                   Grid.Row="1"/>
                        <TextBlock Text="{x:Bind Path=Body}"
                                   Style="{StaticResource BodyTextBlockStyle}"
                                   Grid.Row="2"
                                   Margin="0,4,0,0" />
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RefreshContainer>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<ListItemData> Items { get; set; } 
        = new ObservableCollection<ListItemData>();

    public MainPage()
    {
        this.InitializeComponent();

        Loaded += MainPage_Loaded;
        ListView1.ItemsSource = Items;
    }

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        Loaded -= MainPage_Loaded;
        RefreshContainer.RefreshRequested += RefreshContainer_RefreshRequested;
        RefreshContainer.Visualizer.RefreshStateChanged += Visualizer_RefreshStateChanged;

        // Add some initial content to the list.
        await FetchAndInsertItemsAsync(2);
    }

    private void RefreshButtonClick(object sender, RoutedEventArgs e)
    {
        RefreshContainer.RequestRefresh();
    }

    private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
    {
        // Respond to a request by performing a refresh and using the deferral object.
        using (var RefreshCompletionDeferral = args.GetDeferral())
        {
            // Do some async operation to refresh the content

            await FetchAndInsertItemsAsync(3);

            // The 'using' statement ensures the deferral is marked as complete.
            // Otherwise, you'd call
            // RefreshCompletionDeferral.Complete();
            // RefreshCompletionDeferral.Dispose();
        }
    }

    private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
    {
        // Respond to visualizer state changes.
        // Disable the refresh button if the visualizer is refreshing.
        if (args.NewState == RefreshVisualizerState.Refreshing)
        {
            RefreshButton.IsEnabled = false;
        }
        else
        {
            RefreshButton.IsEnabled = true;
        }
    }

    // App specific code to get fresh data.
    private async Task FetchAndInsertItemsAsync(int updateCount)
    {
        for (int i = 0; i < updateCount; ++i)
        {
            // Simulate delay while we go fetch new items.
            await Task.Delay(1000);
            Items.Insert(0, GetNextItem());
        }
    }

    private ListItemData GetNextItem()
    {
        return new ListItemData()
        {
            Header = "Header " + DateTime.Now.Second.ToString(),
            Date = DateTime.Now.ToLongDateString(),
            Body = DateTime.Now.ToLongTimeString()
        };
    }
}

public class ListItemData
{
    public string Header { get; set; }
    public string Date { get; set; }
    public string Body { get; set; }
}
```

## <a name="related-articles"></a>相關文章

- [觸控互動](../input/touch-interactions.md)
- [清單檢視和方格檢視](listview-and-gridview.md)
- [項目容器與範本](item-containers-templates.md)
- [Expression 動畫](../../composition/composition-animation.md)
