---
author: mijacobs
description: 如何使用關聯式命令，盡可能在所有輸入類型上提供最佳體驗的方式來實作這類動作。
title: 關聯式命令功能
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.author: mijacobs
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f06d7015fcb208b55fe0cb57b96eaecbc99317cc
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5975059"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>集合和清單的關聯式命令功能



許多應用程式會包含形式為清單、格線和樹狀目錄的內容集合，使用者可以進行操縱。 例如，使用者可以對項目進行刪除、重新命名、旗標附加或重新整理。 本文說明如何使用關聯式命令，以盡可能在所有輸入類型上提供最佳體驗的方式來實作這類動作。  

> **重要 API**：[ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)、[UIElement.ContextFlyout 屬性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout)、[INotifyPropertyChanged 介面](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![使用各種不同輸入來執行 [我的最愛] 命令](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>建立所有輸入類型的命令

由於使用者可以使用[各式各樣的裝置和輸入](../devices/index.md)與 UWP app 進行互動，您的 App 應該透過不限輸入類型的操作功能表和限定輸入類型的快速操作方式來公開命令。 兼採上述兩種公開方式可讓使用者快速對內容叫用命令，無論使用的是輸入類型還是裝置類型。

下表顯示一些常見的集合命令以及公開這些命令的方式。 

| 命令          | 不限輸入類型 | 滑鼠快速操作 | 鍵盤快速操作 | 觸控快速操作 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 刪除項目      | 操作功能表   | 暫留按鈕      | DEL 鍵              | 撥動以刪除   |
| 為項目加上旗標        | 操作功能表   | 暫留按鈕      | Ctrl+Shift+G         | 撥動以加上旗標     |
| 重新整理資料     | 操作功能表   | 無               | F5 鍵               | 拖動以重新整理   |
| 將項目加入我的最愛 | 操作功能表   | 暫留按鈕      | F、Ctrl+S            | 撥動以加入我的最愛 |


* **一般而言，您應該在項目的[操作功能表](menus.md)中提供項目的所有命令。** 操作功能表不論輸入類型為何，皆可供使用者存取，而且應該包含使用者可以執行的所有關聯式命令。

* **使用經常存取的命令時，請考慮使用輸入快速操作。** 輸入快速操作可讓使用者依據其輸入裝置迅速執行動作。 輸入快速操作包括：
    - 撥動以執行動作 (觸控快速操作)
    - 拖動以重新整理資料 (觸控快速操作)
    - 鍵盤快速鍵 (鍵盤快速操作)
    - 便捷鍵 (鍵盤快速操作)
    - 滑鼠和手寫筆暫留按鈕 (指標快速操作)

> [!NOTE]
> 使用者必須能夠透過任何類型的裝置來存取所有命令。 舉例說，如果只能透過暫留按鈕指標的快速操作公開 App 的命令，那麼觸控使用者便無法存取這些命令。 您至少要使用操作功能表，讓使用者可以存取所有的命令。  

## <a name="example-the-podcastobject-data-model"></a>範例：PodcastObject 資料模型

為了示範我們的命令功能建議，本文製作了一份 [播客] App 的播客清單。 範例代碼示範如何讓使用者可以將特定播客從清單中加入 [我的最愛]。

以下是我們將要使用的播客物件定義： 

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

請注意，PodcastObject 會實作 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) 來回應使用者切換 IsFavorite 屬性時的屬性變更。

## <a name="defining-commands-with-the-icommand-interface"></a>使用 ICommand 介面定義命令

[ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) 可協助您定義適用於多個輸入類型的命令。 例如，您不但不要在兩個不同事件處理常式 (一個在使用者按 Delete 鍵時使用，另一個在使用者以滑鼠右鍵按下操作功能表 [刪除] 時使用) 中撰寫相同的刪除命令程式碼，反而只要將刪除邏輯實作為 [ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) 一次，即可提供給不同的輸入類型使用。

我們需要定義表示「加入我的最愛」動作的 ICommand。 我們會使用命令的 [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) 方法，將播客加入我的最愛。 將會透過命令參數提供特定播客給 execute 方法，而這個參數可以使用 CommandParameter 屬性來繫結。

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

若要使用相同命令處理多個集合和元素，您可以將命令儲存為頁面或 App 上的資源。

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


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>建立 UserControl 以回應各種輸入

當您有項目清單，而這其中每一個項目都要回應多個輸入時，您可以定義項目的 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl)，再將其用於定義項目的操作功能表及事件處理常式，來簡化程式碼。 

若要在 Visual Studio 中建立 UserControl：
1. 在 [方案總管] 中，以滑鼠右鍵按一下專案。 操作功能表會出現。
2. 選取 **\[新增\] > \[新項目...\]** <br />**\[加入新項目\]** 對話方塊會出現。 
3. 從項目清單選取 UserControl。 提供您想要的名稱並按一下**\[新增\]**。 Visual Studio 將會為您產生虛設常式 UserControl。 

在我們的播客範例中，每個播客都會顯示於清單，這會公開各種將播客「加入我的最愛」的方式。 使用者可以執行下列動作將播客「加入我的最愛」：
- 叫用操作功能表
- 執行鍵盤快速鍵
- 顯示暫留按鈕
- 執行撥動手勢

為了封裝這些行為和使用 FavoriteCommand，我們來建立一個新的名為「PodcastUserControl」的 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl)，以表示清單中的播客。

PodcastUserControl 將 PodcastObject 的欄位顯示為 TextBlock，並回應各種使用者互動。 我們在這整篇文章中都會提到和詳述 PodcastUserControl。

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

請注意，PodcastUserControl 會以 DependencyProperty 類型來保留 PodcastObject 的參考。 這可讓我們將 PodcastObjects 繫結至 PodcastUserControl。

產生了一些 PodcastObjects 之後，您可以將 PodcastObjects 繫結至 ListView，來建立播客清單。 PodcastUserControl 物件描述 PodcastObjects 的視覺效果，因此會使用 ListView 的 ItemTemplate 來設定。

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

當使用者要求命令或選項時，操作功能表會顯示這些命令或選項的清單。 操作功能表提供與其附加元素相關的關聯式命令，通常會保留給該項目專用的次要動作。

![在項目上顯示操作功能表](images/ContextualCommand_RightClick.png)

使用者可以使用下列「操作動作」來叫用操作功能表：

| 輸入    | 操作動作                          |
| -------- | --------------------------------------- |
| 滑鼠    | 按一下滑鼠右鍵                             |
| 鍵盤 | Shift+F10、功能表按鈕                  |
| 觸控    | 在項目上長按                      |
| 手寫筆      | 筆身按鈕、在項目上長按 |
| 遊戲台  | 選項按鈕                             |

**由於無論輸入類型為何，使用者都可以開啟操作功能表，因此操作功能表應該包含清單項目可用的所有關聯式命令。**

### <a name="contextflyout"></a>ContextFlyout

UIElement 類別所定義的 [ContextFlyout 屬性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) 可讓您輕鬆建立適用於所有輸入類型的操作功能表。 您可以使用 MenuFlyout 提供表示操作功能表的飛出視窗，並在使用者執行以上定義的「操作動作」時，顯示對應於項目的 MenuFlyout。

我們會將 ContextFlyout 新增至 PodcastUserControl。 指定為 ContextFlyout 的 MenuFlyout 包含用來將播客加入我的最愛的單一項目。 請注意，此 MenuFlyoutItem 在 CommandParamter 繫結至 PodcastObject 的情況下，使用以上所定義的 favoriteCommand。

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

請注意，您也可以使用 [ContextRequested 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested) 來回應操作動作。 如果 ContextFlyout 已指定，則不會引發 ContextRequested 事件。

## <a name="creating-input-accelerators"></a>建立輸入快速操作

雖然集合中的每個項目應該有一個包含所有關聯式命令的操作功能表，但是您可能會想要讓使用者可以快速執行較小的一組常用命令。 例如，郵件 App 可能會有 [回覆]、[封存]、[移動到資料夾]、[設定標幟]、[刪除] 等次要命令出現在操作功能表中，但最常用的命令則是 [刪除] 和 [設定標幟]。 確認出哪些命令最常用之後，您可以使用輸入型快速操作，讓使用者更容易執行這些命令。

在 [播客] App 中，經常執行的命令是 [我的最愛] 命令。

### <a name="keyboard-accelerators"></a>鍵盤快速操作

#### <a name="shortcuts-and-direct-key-handling"></a>快速鍵和直接按鍵處理

![按 Ctrl 和 F 以執行動作](images/ContextualCommand_Keyboard.png)

視內容類型而定，您可能會辨識出要執行某個動作的特定按鍵組合。 例如在 電子郵件 App 中，DEL 可能會用來刪除已選取的電子郵件。 在 [播客] App 中，Ctrl+S 或 F 鍵則會將播客加入我的最愛以供稍後使用。 某些命令會有常用且眾所周知的鍵盤快速鍵 (像是 DEL 之於刪除)，而其他命令則有 App 或網域專用的快速鍵。 盡可能使用眾所周知的快速鍵，或考慮在工具提示中提供提醒文字，讓使用者了解快速鍵命令。

當使用者按下按鍵時，您的 App 可以使用 [KeyDown](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent) 事件來回應。 使用者通常希望 App 在他們第一次按下按鍵時即做出回應，而不是等到放開按鍵才回應。

此範例逐步解說如何將 KeyDown 處理常式新增至 PodcastUserControl，以便在使用者按 Ctrl+S 鍵或 F 時將播客加入我的最愛。範例使用如前所述的相同命令。

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

![將滑鼠指標暫留在項目以顯示按鈕](images/ContextualCommand_HovertoReveal.png)

使用者很熟悉以滑鼠右鍵按一下操作功能表的動作，但您可能會想要讓使用者只用單一滑鼠點選就能執行常用命令。 若要啟用這個體驗，您可以在您的集合項目的畫布上加入專用的按鈕。 若要既可讓使用者使用滑鼠快速執行動作，又能盡量使畫面的雜亂度減至最低，您可以選擇只有在使用者將其指標進入特定清單項目時才顯示這些按鈕。

在此範例中，[我的最愛] 命令是由 PodcastUserControl 中直接定義的按鈕來表示。 請注意，此範例中的按鈕使用跟之前一樣的命令 FavoriteCommand。 若要切換此按鈕的可見度，您可以使用 VisualStateManager，在指標進入和離開控制項時切換視覺狀態。

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

暫留按鈕應該在滑鼠指標滑鼠進入或離開項目時出現和消失。 若要回應滑鼠事件，您可以使用 PodcastUserControl 上的 [PointerEntered](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) 和 [PointerExited](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) 事件。

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

暫留狀態下顯示的按鈕只能透過指標輸入類型來存取。 由於這些按鈕僅限於指標輸入，您可以選擇盡量縮小或移除按鈕圖示周圍的邊框間距，使指標輸入最佳化。 如果您選擇這樣做，請務必使按鈕所佔大小至少為 20x20px，才能持續搭配手寫筆和滑鼠使用。

### <a name="touch-accelerators"></a>觸控快速操作

#### <a name="swipe"></a>撥動

![撥動項目以顯示命令](images/ContextualCommand_Swipe.png)

撥動命令功能是可讓使用者以觸控方式在觸控裝置上執行次要動作的觸控快速操作。 「撥動」讓觸控使用者能夠使用「撥動以刪除」或「撥動以叫用」等常見動作，來與內容進行快速且自然的互動。 請參閱[撥動命令功能](swipe.md)文章，以深入了解。

為了將撥動整合到集合中，您需要兩個元件：裝載命令的 SwipeItems，以及包裝項目和允許撥動互動的 SwipeControl。

SwipeItems 可以定義為 PodcastUserControl 中的資源。 在此範例中，SwipeItems 包含 [將項目加入我的最愛] 的命令。

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

SwipeControl 會包裝項目，並允許使用者使用撥動手勢與其進行互動。 請注意，SwipeControl 包含做為其 RightItems 的 SwipeItems 的參考。 我的最愛項目會在使用者由右至左撥動時顯示。

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

當使用者撥動以叫用 [我的最愛] 命令時，會呼叫 Invoked 方法。

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>拖動以重新整理

「拖動以重新整理」讓使用者以觸控方式向下拖動資料集合來擷取更多資料。 請參閱[撥動以重新整理](pull-to-refresh.md)文章，以深入了解。

### <a name="pen-accelerators"></a>手寫筆快速操作

手寫筆輸入類型提供指標輸入的精確度。 使用者可以使用手寫筆快速操作來執行一般動作，例如開啟操作功能表。 若要開啟操作功能表，使用者可以在按下筆身按鈕時輕觸螢幕，或長按內容。 使用者也可以使用手寫筆暫留在內容上方，以更深入了解 UI (例如顯示工具提示)，或顯示類似於滑鼠的次要暫留動作。

若要針對手寫筆輸入使 App 最佳化，請參閱[畫筆和手寫筆互動](../input/pen-and-stylus-interactions.md)文章。


## <a name="dos-and-donts"></a>可行與禁止事項

* 確定使用者可以從各種類型的 UWP 裝置存取所有的命令。
* 務必加入可讓使用者存取集合項目所有可用命令的操作功能表。 
* 務必提供常用命令的輸入快速操作。 
* 務必使用 [ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)來實作命令。 

## <a name="related-topics"></a>相關主題
* [ICommand 介面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [功能表和操作功能表](menus.md)
* [撥動](swipe.md)
* [拖動以重新整理](pull-to-refresh.md)
* [畫筆和手寫筆互動](../input/pen-and-stylus-interactions.md)
* [針對遊戲台和 Xbox 量身打造您的 App](../devices/designing-for-tv.md)
