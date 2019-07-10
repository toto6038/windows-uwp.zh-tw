---
description: 如何使用關聯式命令來實作這類動作，盡可能在所有輸入類型上提供最佳體驗。
title: 關聯式命令功能
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1d520f811c9929721bfcb9d1c83fbff6a4891091
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "63801180"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>集合和清單的關聯式命令功能



許多應用程式含有內容的集合，以表單、格線和樹狀目錄這類可由使用者操縱的形式存在。 例如，使用者可以刪除、重新命名或重新整理項目，也可以加上旗標。 本文會說明如何使用關聯式命令來實作這類動作，盡可能在所有輸入類型上提供最佳體驗。  

> **重要 API**：[ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)、[UIElement.ContextFlyout 屬性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout)、[INotifyPropertyChanged 介面](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![使用各種不同的輸入方式來執行 [我的最愛] 命令](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>建立所有輸入類型的命令

由於使用者可使用[各式各樣的裝置和輸入方式](../devices/index.md)與 UWP 應用程式進行互動，建議同時採用不限輸入類型的操作功能表，以及限定輸入類型的快速操作方式，讓您的應用程式公開命令。 兼採上述兩種公開方式，使用者就能迅速對內容叫用命令，無論使用何種輸入類型或裝置類型皆可。

下表列出了一些常見的集合命令，以及公開這些命令的方式。 

| 命令          | 不限輸入類型 | 滑鼠快速操作 | 鍵盤快速操作 | 觸控快速操作 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 刪除項目      | 操作功能表   | 暫留按鈕      | DEL 鍵              | 撥動以刪除   |
| 為項目加上旗標        | 操作功能表   | 暫留按鈕      | Ctrl+Shift+G         | 撥動以加上旗標     |
| 重新整理資料     | 操作功能表   | 不適用               | F5 鍵               | 拖動以重新整理   |
| 將項目加入我的最愛 | 操作功能表   | 暫留按鈕      | F、Ctrl+S            | 撥動以加入我的最愛 |


* **一般而言，某個項目的[操作功能表](menus.md)中，應要提供該項目的所有命令。** 無論使用者所用的是何種輸入類型，都要得以使用操作功能表，且應備齊使用者可執行的所有關聯式命令。

* **對於常用命令，建議使用輸入快速操作。** 透過輸入快速操作，使用者可依據所用的輸入裝置，迅速執行動作。 輸入快速操作包括：
    - 撥動以執行動作 (觸控快速操作)
    - 拖動以重新整理資料 (觸控快速操作)
    - 鍵盤快速鍵 (鍵盤快速操作)
    - 便捷鍵 (鍵盤快速操作)
    - 滑鼠和手寫筆暫留按鈕 (指標快速操作)

> [!NOTE]
> 應該要讓使用者在任何一種裝置上，都能使用所有命令。 舉例來說，如果要公開應用程式命令時，只能使用暫留按鈕指標的快速操作功能，觸控使用者就無法使用這些命令了。 至少請使用操作功能表，讓使用者可使用所有命令。  

## <a name="example-the-podcastobject-data-model"></a>範例：PodcastObject 資料模型

為了示範命令功能的相關建議，本文製作了一份播客應用程式的播客節目清單。 在範例代碼中，示範了如何讓使用者能夠將清單中的某個播客節目加進 [我的最愛]。

以下是所要使用的播客物件定義： 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

請注意，PodcastObject 會實作 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)，以回應使用者切換 IsFavorite 屬性時的屬性變化。

## <a name="defining-commands-with-the-icommand-interface"></a>以 ICommand 介面來定義命令

[ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) 可協助定義適用於多種輸入類型的命令。 例如，兩個不同事件處理常式內並不需要編寫相同的代碼 (一個處理常式是於使用者按下 Delete 鍵時使用，另一個則是用於使用者以滑鼠右鍵按下操作功能表中的 [刪除] 時)，而只需要將您的刪除邏輯實作為 [ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) 一次，再提供給不同的輸入類型使用。

我們需要定義代表「加入我的最愛」動作的 ICommand。 接下來會使用命令的 [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) 方法，將播客節目加入我的最愛。 將會透過該命令的參數，將特定播客提供給 execute 方法，而這個參數可以使用 CommandParameter 屬性來繫結。

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

若要使用相同命令處理多個集合和元素，可將命令儲存為頁面或應用程式上的資源。

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

若要執行命令，請呼叫 [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) 方法。

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>建立 UserControl 以回應各種輸入類型

如果有個項目清單，而其中每個項目都需要回應多種輸入類型時，只要定義該項目的 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl)，再用來定義項目的操作功能表及事件處理常式，就能簡化程式碼。 

如何在 Visual Studio 中建立 UserControl：
1. 在 [方案總管] 中，以滑鼠右鍵按一下專案。 操作功能表即會出現。
2. 選取 [新增] > [新項目...]  <br />[加入新項目]  對話方塊會出現。 
3. 在項目清單中選取 UserControl。 命名後，請按一下 [新增]  。 Visual Studio 會幫您產生虛設常式 UserControl。 

在播客的範例中，每個播客節目都會顯示在清單上，如此就會出現各種可將播客節目「加入我的最愛」的方式。 使用者可以執行下列動作，將播客節目「加入我的最愛」：
- 叫用操作功能表
- 執行鍵盤快速鍵
- 顯示暫留按鈕
- 執行撥動手勢

為了封裝這些行為並使用 FavoriteCommand，我們要來建立一個名為「PodcastUserControl」的新 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl)，用來表示清單中的播客節目。

PodcastUserControl 會將 PodcastObject 的欄位顯示為 TextBlock，並回應各種不同的使用者互動。 本篇文章會時常參照 PodcastUserControl 並加以說明。

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

請注意，PodcastUserControl 會以 DependencyProperty 類型來保留對 PodcastObject 的參考。 如此可將 PodcastObjects 繫結至 PodcastUserControl。

產生了一些 PodcastObjects 之後，可以將 PodcastObjects 繫結至 ListView，以此方式建立播客清單。 PodcastUserControl 物件描述的是 PodcastObjects 的視覺效果，因此會使用 ListView 的 ItemTemplate 加以設定。

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>建立操作功能表

當使用者要求取得命令或選項時，操作功能表會顯示命令或選項的清單。 操作功能表會提供與其附加元素相關的關聯式命令，通常會保留給該項目專用的次要動作。

![在項目上顯示操作功能表](images/ContextualCommand_RightClick.png)

使用者可以使用下列「操作動作」來叫用操作功能表：

| Input    | 操作動作                          |
| -------- | --------------------------------------- |
| 滑鼠    | 按一下滑鼠右鍵                             |
| 鍵盤 | Shift+F10、功能表按鈕                  |
| 觸控    | 長按項目                      |
| 手寫筆      | 筆身按鈕、長按項目 |
| 遊戲台  | 功能表按鈕                             |

**由於無論輸入類型為何，使用者都可以開啟操作功能表，因此操作功能表應該包含清單項目可用的所有關聯式命令。**

### <a name="contextflyout"></a>ContextFlyout

UIElement 類別所定義的 [ContextFlyout 屬性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout)，可輕鬆建立適用於所有輸入類型的操作功能表。 可使用 MenuFlyout 來提供用於顯示操作功能表的飛出視窗，並在使用者執行上述的「操作動作」時，顯示與該項目對應的 MenuFlyout。

我們會將 ContextFlyout 新增至 PodcastUserControl。 指定為 ContextFlyout 的 MenuFlyout 內有一個項目，可用來將播客節目加入我的最愛。 請注意，此 MenuFlyoutItem 會使用定義如上的 favoriteCommand，並將 CommandParamter 繫結至 PodcastObject。

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

請注意，您也可以使用 [ContextRequested 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested)來回應操作動作。 如果已指定 ContextFlyout，則不會引發 ContextRequested 事件。

## <a name="creating-input-accelerators"></a>建立輸入快速操作

雖然集合中的每個項目都應該要具有操作功能表，其中需包含所有的關聯式命令，但建議讓使用者能夠迅速執行一組數量較少的常用命令。 舉例來說，在郵件應用程式的操作功能表中，可能會出現 [回覆]、[封存]、[移到資料夾]、[設定標幟]、[刪除] 等次要命令，但最常用的命令則是 [刪除] 和 [設定標幟]。 確認哪些命令最常使用後，可以利用輸入型的快速操作功能，讓使用者更容易執行這些命令。

在播客應用程式中，經常會執行 [我的最愛] 命令。

### <a name="keyboard-accelerators"></a>鍵盤快速操作

#### <a name="shortcuts-and-direct-key-handling"></a>快速鍵和直接按鍵的操作方式

![按 Ctrl 和 F 以執行動作](images/ContextualCommand_Keyboard.png)

視內容類型的不同，您可能會發現，有特定的鍵盤快速鍵應會執行某個動作。 例如，在電子郵件應用程式中，可能會使用 DEL 鍵來刪除選取的電子郵件。 在播客應用程式中，Ctrl+S 或 F 鍵則會將播客加入我的最愛，以便稍後觀看。 某些命令具有眾所周知的常用鍵盤快速鍵 (像是 DEL 可執行刪除)，而其他命令則有在應用程式專用或領域專用的快速鍵。 請盡可能使用眾所周知的快速鍵，或考慮在工具提示中加上提醒文字，協助使用者認識快速鍵命令。

當使用者按下按鍵時，您的應用程式可以使用 [KeyDown](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent) 事件來回應。 使用者通常希望應用程式在第一次按下按鍵時，就會做出回應，而不是等到放開按鍵才回應。

此範例會逐步解說如何將 KeyDown 處理常式新增至 PodcastUserControl，以在使用者按下 Ctrl+S 或 F 時，將播客加入我的最愛。範例使用的命令與之前所述相同。

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>滑鼠快速操作

![將滑鼠指標暫留在項目上，以顯示按鈕](images/ContextualCommand_HovertoReveal.png)

使用者很習慣在操作功能表上按右鍵，但建議讓使用者按一下滑鼠就能執行常用命令。 要達到這種效果，可以在集合項目的畫布上納入專用按鈕。 若要讓使用者可以使用滑鼠迅速執行動作，又想要盡可能降低畫面的雜亂度，在使用者將滑鼠指標停留在特定清單項目上時，建議選擇只顯示這些按鈕。

在此範例中，[我的最愛] 命令是由 PodcastUserControl 中直接定義的按鈕來呈現。 請注意，此範例中的按鈕使用的是 FavoriteCommand 命令，與之前相同。 若要切換此按鈕的可見度，您可以使用 VisualStateManager，在指標進入和離開控制項時，切換視覺狀態。

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

暫留按鈕應會在滑鼠指標滑鼠進入和離開項目時出現和消失。 若要回應滑鼠事件，您可以使用 PodcastUserControl 上的 [PointerEntered](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) 和 [PointerExited](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) 事件。

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

暫留狀態下顯示的按鈕，只能透過滑鼠指標輸入類型加以存取。 由於這類按鈕僅限於指標輸入類型，可選擇盡量縮小或移除按鈕圖示周圍的邊框間距，讓指標輸入功能達到最佳效果。 若要這樣做，按鈕所佔大小請至少為 20x20px，才方便手寫筆和滑鼠使用。

### <a name="touch-accelerators"></a>觸控快速操作

#### <a name="swipe"></a>Swipe

![撥動項目以顯示命令](images/ContextualCommand_Swipe.png)

撥動命令功能是一種觸控快速操作，可讓使用者在觸控裝置上以觸控方式執行常用的次要動作。 撥動的動作，可讓觸控使用者使用「撥動以刪除」或「撥動以叫用」等常見動作，與內容進行快速且自然的互動。 若要深入了解，請參閱[撥動命令功能](swipe.md)一文。

為了將撥動整合到集合之中，需要兩個元件：SwipeItems (裝載命令) 和 SwipeControl (包裝項目，並允許撥動互動)。

SwipeItems 可以定義為 PodcastUserControl 中的資源。 在此範例中，SwipeItems 包含 [將項目加入我的最愛] 的命令。

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

SwipeControl 會包裝項目，並允許使用者使用撥動手勢與其進行互動。 請注意，SwipeControl 包含做為其 RightItems 的 SwipeItems 的參考。 [我的最愛] 項目會在使用者由右至左撥動時顯示。

```xaml
<SwipeControl x:Name="swipeContainer" RightItems="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeControl, since the SwipeControl wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</SwipeControl>
```

當使用者撥動以叫用 [我的最愛] 命令時，即會呼叫 Invoked 方法。

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>拖動以重新整理

拖動以重新整理的動作，可讓使用者以觸控方式向下拖動資料集合，以擷取更多資料。 若要深入了解，請參閱[撥動以重新整理](pull-to-refresh.md)一文。

### <a name="pen-accelerators"></a>手寫筆快速操作

手寫筆輸入類型可精準地以指標輸入。 使用者可以使用手寫筆快速操作來各種常用動作，例如開啟操作功能表等。 若要開啟操作功能表，使用者可按下筆身按鈕並輕觸螢幕，或是長按內容。 使用者也可以使用手寫筆暫留在內容上方，以更深入了解 UI (例如顯示工具提示)，或顯示類似於滑鼠的次要暫留動作。

若要針對手寫筆輸入使應用程式最佳化，請參閱[手寫筆互動](../input/pen-and-stylus-interactions.md)一文。


## <a name="dos-and-donts"></a>可行與禁止事項

* 請確定使用者可以用所有類型的 UWP 裝置存取全部的命令。
* 請務必加入操作功能表，讓使用者得以存取集合項目的所有可用命令。 
* 請務必提供常用命令的輸入快速操作功能。 
* 請務必使用 [ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)來實作命令。 

## <a name="related-topics"></a>相關主題
* [ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) (英文)
* [功能表和操作功能表](menus.md)
* [Swipe](swipe.md)
* [拖動以重新整理](pull-to-refresh.md)
* [手寫筆互動](../input/pen-and-stylus-interactions.md)
* [針對 Xbox 和電視進行設計](../devices/designing-for-tv.md)
