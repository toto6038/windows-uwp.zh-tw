---
title: 在 通用 Windows 平台 (UWP) 應用程式的命令
description: 如何使用 XamlUICommand 和 StandardUICommand 類別 （連同 ICommand 介面） 來共用及管理跨各種控制項的型別，不論裝置和所使用的輸入的類型的命令。
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 11/01/2018
ms.author: kbridge
ms.openlocfilehash: 32d5005f9965b14d5080344832eb185f0e711689
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646523"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>使用 StandardUICommand、 XamlUICommand 和 ICommand 的通用 Windows 平台 (UWP) 應用程式的命令執行

本主題中，我們會說明在通用 Windows 平台 (UWP) 應用程式中的命令。 具體來說，我們會討論如何使用[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)並[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)共用及管理不同的控制項類型，不論命令類別 （連同 ICommand 介面）裝置和所使用的輸入型別。

![圖表，代表共用的命令的一般使用方式： 使用 '最愛' 命令的多個 UI 呈現](images/commanding/generic-commanding.png)

*共用命令，跨各種控制項，而不論裝置與輸入型別*

## <a name="important-apis"></a>重要 API

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)和[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>概觀

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

可以叫用命令，直接透過 UI 互動，例如按一下按鈕，或從操作功能表中選取項目。 它們也會叫用間接透過將輸入的裝置，例如鍵盤對應鍵、 手勢、 語音辨識或自動化/協助工具的工具。 一旦叫用，命令可以接著處理由控制項 （編輯控制項中文字導覽，） 視窗 （向後巡覽） 或應用程式 （結束）。

命令可以作用於特定的內容，在您的應用程式，例如刪除文字，或復原動作，或者可以是內容免，例如靜音音訊或調整亮度。

下圖顯示兩個命令介面 ( [CommandBar](app-bars.md)及內容浮動[CommandBarFlyout](command-bar-flyout.md)) 共用許多相同的命令。

![命令介面的範例](images/commanding/command-interface-example.png)

## <a name="command-interactions"></a>命令的互動

由於有各式各樣的裝置、 輸入的類型，以及 UI 介面可能會影響如何叫用命令，我們建議盡可能公開您的命令，透過多個的命令介面。 這些可以包含多種[撥動](swipe.md)， [MenuBar](menus.md)， [CommandBar](app-bars.md)， [CommandBarFlyout](command-bar-flyout.md)，和傳統[操作功能表](menus.md)。

**如需重要的命令，使用輸入特定加速器。** 輸入的加速器可讓使用者執行更快速地根據他們所使用的輸入裝置的動作。

以下是一些常見的輸入的加速器，各種不同的輸入類型：

- **指標**-滑鼠 （& p） 將滑鼠移至按鈕
- **鍵盤**-快速鍵 （便捷鍵和快速鍵）
- **觸控**-撥動
- **觸控**-拖動以重新整理資料

您必須考量，以便將您的應用程式功能都可存取的輸入型別和使用者體驗。 例如，集合 （尤其是使用者可編輯的項目） 通常會包含各種不同的執行方式則相當不同的輸入裝置的特定命令。

下表顯示一些一般集合命令和公開這些命令的方法。

| 命令          | 不限輸入類型 | 滑鼠快速操作 | 鍵盤快速操作 | 觸控快速操作 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 刪除項目      | 操作功能表   | 暫留按鈕      | DEL 鍵              | 撥動以刪除   |
| 為項目加上旗標        | 操作功能表   | 暫留按鈕      | Ctrl+Shift+G         | 撥動以加上旗標     |
| 重新整理資料     | 操作功能表   | 無               | F5 鍵               | 拖動以重新整理   |
| 將項目加入我的最愛 | 操作功能表   | 暫留按鈕      | F、Ctrl+S            | 撥動以加入我的最愛 |

**一定要提供內容功能表**我們建議您在傳統的操作功能表或 CommandBarFlyout，包括所有相關的內容命令，兩者都針對支援的所有輸入類型。 比方說，如果指標暫留事件期間，只公開命令時，它不能在觸控式裝置上。

## <a name="commands-in-uwp-applications"></a>在 UWP 應用程式中的命令

有幾個方式，您可以共用及管理在 UWP 應用程式中的命令體驗。 您可以定義標準的互動，例如按一下滑鼠，事件處理常式程式碼後置中 （這可以是非常沒有效率，視您的 UI 的複雜度而定），您可以將標準互動的事件接聽程式繫結至共用的處理常式，或您可以將控制項繫結描述命令邏輯的 ICommand 實作的命令屬性。

若要有效率地與最少的程式碼重複，請跨命令介面提供豐富且完整的使用者體驗，我們建議使用的繫結功能，本主題中所述的命令 （標準事件處理，請參閱個別的事件主題）。

將控制項繫結至共用的命令的資源，您可以自行實作 ICommand 介面，或您可以建置您的命令，從[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)基底類別或其中一個所定義的平台命令[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)衍生的類別。

- ICommand 介面 ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)或是[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)) 可讓您建立完全自訂，可重複使用的命令在您的應用程式。
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)也提供這項功能，但簡化開發作業藉由公開一組內建的命令屬性，例如命令行為、 鍵盤快速鍵 （便捷鍵和快速鍵）、 圖示、 標籤和描述。
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)可以再進一步簡化可讓您選擇從一組標準的平台命令，使用預先定義的屬性。

> [!Important]
> 在 UWP 應用程式中，命令是實作任一[Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) （c + +） 或[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#) 介面，根據您選擇的語言架構。

## <a name="command-experiences-using-the-standarduicommand-class"></a>命令可讓您體驗使用 StandardUICommand 類別

衍生自[XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (衍生自[Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) c + + 或[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)的C#)， [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)類別會公開一組標準的平台命令，使用預先定義的屬性，例如圖示、 鍵盤對應鍵，以及描述。

A [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)提供快速且一致的方式，來定義常用的命令，例如`Save`或`Delete`。 您必須執行的就是提供 execute 和 canExecute 的函式。

### <a name="example"></a>範例

![StandardUICommand 範例](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

在此範例中，我們會示範如何加強基本[ListView](listview-and-gridview.md)以刪除項目命令透過實作[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)類別，同時最佳化使用者經驗的各種不同的輸入類型使用[MenuBar](menus.md)，[撥動](swipe.md)控制項、 動態顯示按鈕，並[快顯功能表](menus.md)。

> [!NOTE]
> 此範例需要 Microsoft.UI.Xaml.Controls NuGet 封裝的一部分[Microsoft Windows 的 UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

**Xaml:**

範例 UI 會包含[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)的五個項目。 刪除[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)繫結至[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)，則[SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem)，則[AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)，和[ContextFlyout 功能表](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)。

``` xaml
<Page
    x:Class="StandardUICommandSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:StandardUICommandSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="60"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                StandardUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the StandardUICommand class to 
                share a platform command and consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a standard delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1" Padding="10">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True" 
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered" 
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer" >
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem" 
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left" 
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. 首先，我們會定義`ListItemData`我們 ListView 中每個 ListViewItem 包含文字字串和 ICommand 的類別。

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. 在 MainPage 類別中，我們會定義的集合`ListItemData`的物件[DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)的[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)。 我們再填入它的初始集合 5 個項目 (使用文字和相關聯[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)刪除)。

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = deleteCommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. 接下來，我們會定義我們用來實作項目刪除命令的 ICommand ExecuteRequested 處理常式。

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 最後，我們會定義各種 ListView 事件，包括處理常式[PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)， [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)，並[SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件。 指標事件處理常式用來顯示或隱藏每個項目的 [刪除] 按鈕。

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>命令可讓您體驗使用 XamlUICommand 類別

如果您需要建立一個不所定義的指令[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)類別，或您想要更充分掌控命令外觀[XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)類別衍生自[ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)介面，將各種 UI （例如圖示、 標籤、 描述和鍵盤快速鍵） 的屬性、 方法和事件，以快速定義 UI 和行為的自訂命令。

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)可讓您透過控制項繫結，例如圖示，指定 UI 標籤、 描述和鍵盤快速鍵 （便捷鍵和鍵盤對應鍵），而不需要設定個別的屬性。

### <a name="example"></a>範例

![XamlUICommand 範例](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

此範例中共用的刪除功能，於先前[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)範例，但會顯示如何[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)類別可讓您定義自訂的 delete 命令，以您自己的字型圖示標籤，鍵盤對應鍵和描述。 像是[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)範例中，我們會增強基本[ListView](listview-and-gridview.md)以刪除項目是透過實作的命令[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)類別，同時最佳化各種使用的輸入類型的使用者體驗[MenuBar](menus.md)，[撥動](swipe.md)控制項、 動態顯示按鈕和[操作功能表](menus.md)。

平台的許多控制項使用 XamlUICommand 屬性實際上，就像我們在上一節中的 StandardUICommand 範例一樣。 

> [!NOTE]
> 此範例需要 Microsoft.UI.Xaml.Controls NuGet 封裝的一部分[Microsoft Windows 的 UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

**Xaml:**

範例 UI 會包含[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)的五個項目。 自訂[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)刪除繫結至[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)，則[SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem)，則[AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)，和[ContextFlyout 功能表](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)。

``` xaml
<Page
    x:Class="XamlUICommand_Sample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:XamlUICommand_Sample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <XamlUICommand x:Name="CustomXamlUICommand" ExecuteRequested="DeleteCommand_ExecuteRequested" Description="Custom XamlUICommand" Label="Custom XamlUICommand">
            <XamlUICommand.IconSource>
                <FontIconSource FontFamily="Wingdings" Glyph="&#x4D;"/>
            </XamlUICommand.IconSource>
            <XamlUICommand.KeyboardAccelerators>
                <KeyboardAccelerator Key="D" Modifiers="Control"/>
            </XamlUICommand.KeyboardAccelerators>
        </XamlUICommand>

        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="70"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
        
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded" Name="MainGrid">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                XamlUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the XamlUICommand class to 
                share a custom command with consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a custom delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" Command="{StaticResource CustomXamlUICommand}"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True"
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered"
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer">
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem"
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left"       
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. 首先，我們會定義`ListItemData`我們 ListView 中每個 ListViewItem 包含文字字串和 ICommand 的類別。

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. 在 MainPage 類別中，我們會定義的集合`ListItemData`的物件[DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)的[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)。 我們再填入它的初始集合 5 個項目 (使用文字和相關聯[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand))。

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. 接下來，我們會定義我們用來實作項目刪除命令的 ICommand ExecuteRequested 處理常式。

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 最後，我們會定義各種 ListView 事件，包括處理常式[PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)， [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)，並[SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件。 指標事件處理常式用來顯示或隱藏每個項目的 [刪除] 按鈕。

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>命令可讓您體驗使用 ICommand 介面

標準 UWP 控制項 （按鈕、 清單、 選取項目、 行事曆、 預測文字） 提供許多相同的命令使用體驗的基礎。 如需控制項類型的完整清單，請參閱 <<c0> [ 控制項和 UWP 應用程式模式](index.md)。

支援結構化的命令經驗最基本的方式是定義的 ICommand 介面實作 ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)用於 c + + 或[System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)的C#)。  這個的 ICommand 執行個體則會以如按鈕等控制項繫結。

> [!NOTE]
> 在某些情況下，它可能只是效率來繫結至資料的 IsEnabled 屬性的屬性和資料的 Click 事件的方法。

#### <a name="example"></a>範例

![命令介面的範例](images/commanding/icommand.gif)

*ICommand 範例*
 
在此範例中，我們會示範如何可以按鈕叫用單一命令按一下、 鍵盤對應鍵並旋轉滑鼠滾輪。

我們使用兩個[Listview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)、 一個填入五個項目和其他空的與兩個按鈕，一個用於將項目從 ListView 的資料在左側移到右邊，ListView 和由右至左的另一個則用於移動項目。 每個按鈕繫結至對應的命令 (ViewModel.MoveRightCommand 和 ViewModel.MoveLeftCommand，分別)，並會啟用和停用 自動根據其相關聯的 ListView 中的項目數。

**下列 XAML 程式碼會定義我們的範例中的 UI。**

```xaml
<Page
    x:Class="UICommand1.View.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:vm="using:UICommand1.ViewModel"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <vm:OpacityConverter x:Key="opaque" />
    </Page.Resources>

    <Grid Name="ItemGrid"
          Background="AliceBlue"
          PointerWheelChanged="Page_PointerWheelChanged">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" VerticalAlignment="Center"
                  x:Name="CommandListView" 
                  ItemsSource="{x:Bind Path=ViewModel.ListItemLeft}" 
                  SelectionMode="None" IsItemClickEnabled="False" 
                  HorizontalAlignment="Right">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <Grid Grid.Column="1" Margin="0,0,5,0"
              HorizontalAlignment="Center" 
              VerticalAlignment="Center">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel Grid.Row="1">
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE893;" 
                          Opacity="{x:Bind Path=ViewModel.listItemLeft.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton" ToolTipService.ToolTip="Tooltip"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand, Mode=OneWay}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Add" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Next"/>
                        <TextBlock>Move item right</TextBlock>
                    </StackPanel>
                </Button>
                <Button Name="MoveItemLeftButton" 
                            Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand, Mode=OneWay}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Subtract" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Previous"/>
                        <TextBlock>Move item left</TextBlock>
                    </StackPanel>
                </Button>
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE892;"
                          Opacity="{x:Bind Path=ViewModel.listItemRight.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
            </StackPanel>
        </Grid>
        <ListView Grid.Column="2" 
                  x:Name="CommandListViewRight" 
                  VerticalAlignment="Center" 
                  IsItemClickEnabled="False" 
                  SelectionMode="None"
                  ItemsSource="{x:Bind Path=ViewModel.ListItemRight}" 
                  HorizontalAlignment="Left">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**以下是上述的 ui 程式碼後置。**

在程式碼後置，我們會連接到我們的檢視模型，其中包含我們的指令碼。 此外，我們會定義從滑鼠滾輪，也連線到我們的指令碼的輸入處理常式。

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Controls;
using UICommand1.ViewModel;
using Windows.System;
using Windows.UI.Core;

namespace UICommand1.View
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Reference to our view model.
        public UICommand1ViewModel ViewModel { get; set; }

        // Initialize our view and view model.
        public MainPage()
        {
            this.InitializeComponent();
            ViewModel = new UICommand1ViewModel();
        }

        /// <summary>
        /// Handle mouse wheel input and assign our
        /// commands to appropriate direction of rotation.
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void Page_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            var props = e.GetCurrentPoint(sender as UIElement).Properties;

            // Require CTRL key and accept only vertical mouse wheel movement 
            // to eliminate accidental wheel input.
            if ((Window.Current.CoreWindow.GetKeyState(VirtualKey.Control) != 
                CoreVirtualKeyStates.None) && !props.IsHorizontalMouseWheel)
            {
                bool delta = props.MouseWheelDelta < 0 ? true : false;

                switch (delta)
                {
                    case true:
                        ViewModel.MoveRight();
                        break;
                    case false:
                        ViewModel.MoveLeft();
                        break;
                    default:
                        break;
                }
            }
        }
    }
}
```

**以下是我們的檢視模型的程式碼**

我們的檢視模型是我們在我們的應用程式中定義的兩個命令的執行詳細資料，填入一個 ListView，而隱藏或顯示某些額外的每個 ListView 項目計數為基礎的 UI 中提供的不透明度值轉換器。

```csharp
using System;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using System.ComponentModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Data;

namespace UICommand1.ViewModel
{
    /// <summary>
    /// UI properties for our list items.
    /// </summary>
    public class ListItemData
    {
        /// <summary>
        /// Gets and sets the list item content string.
        /// </summary>
        public string ListItemText { get; set; }
        /// <summary>
        /// Gets and sets the list item icon.
        /// </summary>
        public Symbol ListItemIcon { get; set; }
    }

    /// <summary>
    /// View Model that sets up a command to handle invoking the move item buttons.
    /// </summary>
    public class UICommand1ViewModel : INotifyPropertyChanged
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand moveLeftCommand;
        public RelayCommand MoveLeftCommand { get => moveLeftCommand; private set { } }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand moveRightCommand;
        public RelayCommand MoveRightCommand { get => moveRightCommand; private set { } }

        // Item collections
        public ObservableCollection<ListItemData> listItemLeft;
        public ObservableCollection<ListItemData> ListItemLeft { get => listItemLeft; private set { } }
        public ObservableCollection<ListItemData> listItemRight;
        public ObservableCollection<ListItemData> ListItemRight { get => listItemRight; private set { } }

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            moveLeftCommand = new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            moveRightCommand = new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            listItemLeft = new ObservableCollection<ListItemData>();
            listItemRight = new ObservableCollection<ListItemData>();

            LoadItems();
        }

        /// <summary>
        ///  Populate our list of items.
        /// </summary>
        public void LoadItems()
        {
            for (var x = 0; x <= 4; x++)
            {
                listItem = new ListItemData();
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return listItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return listItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (listItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                listItemRight.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemLeft.RemoveAt(listItemLeft.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (listItemRight.Count > 0)
            {
                listItem = new ListItemData();
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemRight.RemoveAt(listItemRight.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
            }
        }
    }

    /// <summary>
    /// Convert a collection count to an opacity value of 0.0 or 1.0.
    /// </summary>
    public class OpacityConverter : IValueConverter
    {
        /// <summary>
        /// Converts a collection count to an opacity value of 0.0 or 1.0.
        /// </summary>
        /// <param name="value">The bool passed in</param>
        /// <param name="targetType">Ignored.</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns>1.0 if count > 0, otherwise returns 0.0</returns>
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            return ((int)value > 0 ? 1.0 : 0.0);
        }

        /// <summary>
        /// Not used, converter is not intended for two-way binding. 
        /// </summary>
        /// <param name="value">Ignored</param>
        /// <param name="targetType">Ignored</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns></returns>
        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

**最後，以下是我們的 ICommand 介面的實作**

我們在這裡，定義命令可實作[ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)介面，並只會轉送它對其他物件的功能。

```csharp
using System;
using System.Windows.Input;

namespace UICommand1
{
    /// <summary>
    /// A command whose sole purpose is to relay its functionality 
    /// to other objects by invoking delegates. 
    /// The default return value for the CanExecute method is 'true'.
    /// <see cref="RaiseCanExecuteChanged"/> needs to be called whenever
    /// <see cref="CanExecute"/> is expected to return a different value.
    /// </summary>
    public class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool> _canExecute;

        /// <summary>
        /// Raised when RaiseCanExecuteChanged is called.
        /// </summary>
        public event EventHandler CanExecuteChanged;

        /// <summary>
        /// Creates a new command that can always execute.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        public RelayCommand(Action execute)
            : this(execute, null)
        {
        }

        /// <summary>
        /// Creates a new command.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        /// <param name="canExecute">The execution status logic.</param>
        public RelayCommand(Action execute, Func<bool> canExecute)
        {
            if (execute == null)
                throw new ArgumentNullException("execute");
            _execute = execute;
            _canExecute = canExecute;
        }

        /// <summary>
        /// Determines whether this <see cref="RelayCommand"/> can execute in its current state.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        /// <returns>true if this command can be executed; otherwise, false.</returns>
        public bool CanExecute(object parameter)
        {
            return _canExecute == null ? true : _canExecute();
        }

        /// <summary>
        /// Executes the <see cref="RelayCommand"/> on the current command target.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        public void Execute(object parameter)
        {
            _execute();
        }

        /// <summary>
        /// Method used to raise the <see cref="CanExecuteChanged"/> event
        /// to indicate that the return value of the <see cref="CanExecute"/>
        /// method has changed.
        /// </summary>
        public void RaiseCanExecuteChanged()
        {
            var handler = CanExecuteChanged;
            if (handler != null)
            {
                handler(this, EventArgs.Empty);
            }
        }
    }
}
```

## <a name="summary"></a>摘要

通用 Windows 平台提供強大且富彈性的命令系統，可讓您建置應用程式，共用及管理整個控制項類型、 裝置與輸入的類型的命令。

建立 UWP 應用程式的命令時，請使用下列方法：

- 接聽並處理中 XAML/程式碼後置的事件
- 繫結至事件處理常式方法，例如按一下
- 定義您自己的 ICommand 的實作
- 使用您自己的一組預先定義的屬性的值建立 XamlUICommand 物件
- 使用一組預先定義的平台屬性和值建立 StandardUICommand 物件

## <a name="next-steps"></a>後續步驟

如需完整的範例，示範[XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)並[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)實作，請參閱[XAML 控制項陳列庫](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)範例。

## <a name="see-also"></a>請參閱

[控制項和 UWP 應用程式模式](index.md)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->