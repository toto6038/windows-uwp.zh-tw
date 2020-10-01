---
Description: TabView 可讓您在動態索引標籤中靈活組織多個文件
title: 索引標籤檢視
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 62e2156dc9988d458520648751c67cd71292f72c
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220481"
---
# <a name="tabview"></a>TabView

TabView 控制項是一種顯示索引標籤組合以及其各自內容的方式。 TabViews 適用於顯示內容的數個頁面 (或文件)，同時讓使用者能夠重新排列、開啟或關閉新的索引標籤。

![TabView 範例](images/tabview/tab-introduction.png)

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | **TabView** 控制項需要 Windows UI 程式庫，該程式庫是 NuGet 套件，其中包含適用於 UWP 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。 |

> **Windows UI 程式庫 API**：[TabView 類別](/uwp/api/microsoft.ui.xaml.controls.tabview)、[TabViewItem 類別](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

> [!TIP]
> 在這整份文件中，我們使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的 [Page](/uwp/api/windows.ui.xaml.controls.page) 元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在後方的程式碼中，我們也使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

在一般情況下，索引標籤式 UI 有兩個不同的樣式，其功能和外觀各有不同：**靜態索引標籤**是通常可在設定視窗中找到的索引標籤。 它們包含固定順序的數個頁面，這些頁面通常包含預先定義的內容。
**文件索引標籤**是可在瀏覽器 (例如 Microsoft Edge) 中找到的索引標籤。 使用者可以建立、移除和重新排列索引標籤；在視窗之間移動索引標籤；並變更索引標籤的內容。

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 為 UWP 應用程式提供文件索引標籤。 使用 TabView 的時機：

- 使用者將能夠動態開啟、關閉或重新排列索引標籤。
- 使用者將能夠直接在索引標籤中開啟文件或網頁。
- 使用者將能夠在視窗之間拖放索引標籤。

如果 TabView 不適合您的應用程式，請考慮使用 [Pivot](./pivot.md) 或[NavigationView](./navigationview.md) 等控制項。

## <a name="anatomy"></a>結構

下圖顯示 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 控制項的各個部分。 TabStrip 具有頁首和頁尾，但與文件不同的是，TabStrip 的頁首和頁尾分別位於索引標籤帶的最左邊和最右邊。

![TabView 控制項的結構](images/tabview/tab-view-anatomy.png)

接下來的圖顯示 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) 控制項的各個部分。 請注意，雖然內容顯示在 TabView 控制項內，但實際上是 TabViewItem 的一部分。

![TabViewItem 控制項的結構](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>建立索引標籤檢視

這個範例會建立簡單的 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 以及事件處理常式，以支援開啟和關閉索引標籤。

```xaml
 <muxc:TabView AddTabButtonClick="TabView_AddTabButtonClick"
               TabCloseRequested="TabView_TabCloseRequested"/>
```

```csharp
// Add a new Tab to the TabView
private void TabView_AddTabButtonClick(muxc.TabView sender, object args)
{
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void TabView_TabCloseRequested(muxc.TabView sender, muxc.TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>行為

有數種方式可利用或擴充 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 的功能。

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>將 TabItemsSource 繫結至 TabViewItemCollection

```xaml
<muxc:TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>在視窗標題列中顯示 TabView 索引標籤

您可以將兩者合併至相同的區域，而不是讓索引標籤在視窗標題列底下佔據自己的資料列。 這會節省內容的垂直空間，並讓您的應用程式具有現代化的風格。

由於使用者可以拖曳視窗的標題列來重新置放視窗，所以標題列不能完全填滿索引標籤，這點很重要。 因此，在標題列中顯示索引標籤時，您必須指定標題列的一部分，以保留為可拖曳的區域。 如果您沒有指定可拖曳的區域，整個標題列將會是可拖曳的，這會導致您的索引標籤無法接收輸入事件。 如果您的 TabView 會顯示在視窗的標題列中，則應該一律在 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 中包含 [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter)，並將其標示為可拖曳的區域。

如需詳細資訊，請參閱[標題列自訂](../shell/title-bar.md)

![標題列中的索引標籤](images/tabview/tab-extend-to-title.png)

```xaml
<muxc:TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
    <muxc:TabViewItem Header="Home" IsClosable="False">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Home" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 1">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 2">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 3">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>

    <muxc:TabView.TabStripHeader>
        <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
    </muxc:TabView.TabStripHeader>
    <muxc:TabView.TabStripFooter>
        <Grid x:Name="CustomDragRegion" Background="Transparent" />
    </muxc:TabView.TabStripFooter>
</muxc:TabView>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> 若要確保命令介面內容不會遮蔽標題列中的索引標籤，您必須考慮左右重疊。 在 LTR 版面配置中，右內凹包含標題按鈕和拖曳區域。 RTL 中的反轉為 True。 SystemOverlayLeftInset 和 SystemOverlayRightInset 值是以實際的左和右為依據，因此在 RTL 中也要將其反轉。

### <a name="control-overflow-behavior"></a>控制項溢位行為

當索引標籤列擠滿索引標籤時，您可以藉由設定 [TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode) 來控制索引標籤的顯示方式。

| TabWidthMode 值 | 行為                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 等於              | 新增索引標籤時，所有索引標籤會水平縮小，直到達到非常小的最小寬度為止。                                                       |
| SizeToContent      | 索引標籤一律會是其「自然大小」，也就是顯示其圖示和標題所需的最小大小。 當新增或關閉索引標籤時，它們不會展開或縮小。 |

無論您選擇任何值，最後都有可能會有太多索引標籤顯示在索引標籤區域中。 在此情況下將會顯示捲軸，讓使用者可向左或向右滾動 TabStrip。

### <a name="guidance-for-tab-selection"></a>索引標籤選項的指引

大部分的使用者都有使用網頁瀏覽器就能簡單使用文件索引標籤的體驗。 當使用者在您的應用程式中使用文件索引標籤時，先前的體驗會讓他們對索引標籤的行為有所期待。

無論使用者如何與文件索引標籤互動，都應該要有作用中的索引標籤。如果使用者關閉選取的索引標籤，或將選取的索引標籤分成另一個視窗，另一個索引標籤應該會變成作用中的索引標籤。[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 嘗試執行此動作時，會自動選取下一個索引標籤。如果您有很好的理由要讓應用程式允許具有未選取索引標籤的 TabView，則 TabView 的內容區域就會是空白。

## <a name="keyboard-navigation"></a>鍵盤瀏覽

根據預設，[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 支援許多常見的鍵盤瀏覽案例。 本節說明內建功能，並提供其他功能的建議，這些功能對某些應用程式可能有用。

### <a name="tab-and-cursor-key-behavior"></a>索引標籤和游標索引鍵行為

當焦點移至 _TabStrip_ 區域時，選取的 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) 會獲得焦點。 使用者接著可以使用向左鍵和向右鍵，將焦點 (而非選取項目) 移至 TabStrip 中的其他索引標籤。 箭號焦點會受困於索引標籤帶和新增索引標籤 (+) 按鈕 (如果有的話) 內。 若要將焦點移出 TabStrip 區域之外，使用者可以按下 Tab 鍵，將焦點移至下一個可設定的元素。

透過索引標籤移動焦點

![透過索引標籤移動焦點](images/tabview/tab-keyboard-behavior-1.png)

方向鍵不會迴圈焦點

![方向鍵不會迴圈焦點](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>選取索引標籤

當 TabViewItem 有焦點時，按下空格鍵或 Enter 會選取該 TabViewItem。

使用方向鍵來移動焦點，然後按空格鍵以選取索引標籤。

![按空格鍵以選取索引標籤](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>選取相鄰索引標籤的捷徑

Ctrl+Tab 會選取下一個 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)。 Ctrl+Shift+Tab 會選取上一個 TabViewItem。 基於這些目的，索引標籤清單會「產生迴圈」，因此選取下一個索引標籤時已選取上一個索引標籤，會選取第一個索引標籤。

### <a name="closing-a-tab"></a>關閉索引標籤

按 Ctrl + F4 將會引發 [TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested) 事件。 處理事件並關閉索引標籤 (如果適用)。

### <a name="keyboard-guidance-for-app-developers"></a>應用程式開發人員的鍵盤指引

有些應用程式可能需要更進階的鍵盤控制項。 如果適用於您的應用程式，請考慮執行下列快速鍵。

> [!WARNING]
> 如果要將 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 新增至現有的應用程式，您可能已經建立對應至建議 TabView 鍵盤快速鍵之按鍵組合的鍵盤快速鍵。 在此情況下，您必須考慮是否要保留現有的快速鍵，或為使用者提供直覺的索引標籤體驗。

- Ctrl + T 應會開啟新的索引標籤。一般而言，此索引標籤會使用預先定義的文件填入，或以簡單的方式建立空白索引標籤，以選擇其內容。 如果使用者必須選擇新索引標籤的內容，請考慮將輸入焦點提供給內容選取控制項。
- Ctrl + W 應會關閉選取的索引標籤。請記住，TabView 將會自動選取下一個索引標籤。
- Ctrl + Shift + T 應會開啟最近關閉的索引標籤 (或更精確地說，開啟具有與最近關閉的索引標籤相同內容的新索引標籤)。 從最近關閉的索引標籤開始，並在後續每次叫用快速鍵時向後移動。 請注意，這需要維護一份最近關閉的索引標籤清單。
- Ctrl + 1 應會選取索引標籤清單中的第一個索引標籤。 同樣地，Ctrl + 2 應選取第二個索引標籤，Ctrl + 3 應選取第三個，一直到 Ctrl + 8。
- 無論清單中有多少索引標籤，Ctrl + 9 都應該會選取索引標籤清單中的最後一個索引標籤。
- 如果索引標籤提供的不只是關閉命令 (例如複製或釘選索引標籤)，請使用內容功能表來顯示可在索引標籤上執行的所有可用動作。

### <a name="implement-browser-style-keyboarding-behavior"></a>實作瀏覽器樣式的鍵盤輸入行為

這個範例會在 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 上執行數個上述建議。 具體而言，此範例會執行 Ctrl + T、Ctrl + W、Ctrl + 1-8 和 Ctrl + 9。

```xaml
<muxc:TabView x:Name="TabRoot">
    <muxc:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </muxc:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</muxc:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Create new tab.
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    TabRoot.TabItems.Add(newTab);
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only remove the selected tab if it can be closed.
    if (((muxc.TabViewItem)TabRoot.SelectedItem).IsClosable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>相關文章

- [MasterDetails](./master-details.md)
- [NavigationView](./navigationview.md)
- [樞紐分析](./pivot.md)
