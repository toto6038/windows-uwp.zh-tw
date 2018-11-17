---
author: jwmsft
ms.assetid: 26DF15E8-2C05-4174-A714-7DF2E8273D32
title: ListView 與 GridView UI 最佳化
description: 透過 UI 虛擬化、減少元素以及漸進式更新項目，改善 ListView 和 GridView 的效能和啟動時間。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 25eeea58e1e03eedfca3aaafda1cee13cac1f3c4
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7151792"
---
# <a name="listview-and-gridview-ui-optimization"></a>ListView 與 GridView UI 最佳化


**注意：** 如需詳細資訊，請參閱 //build/ 工作階段[與大量資料 GridView 與 ListView 中的使用者互動時大幅提升效能](https://channel9.msdn.com/events/build/2013/3-158)。

透過 UI 虛擬化、減少元素以及漸進式更新項目，改善 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 的效能和啟動時間。 如需資料虛擬化技術的資訊，請參閱 [ListView 和 GridView 資料虛擬化](listview-and-gridview-data-optimization.md)。

## <a name="two-key-factors-in-collection-performance"></a>集合效能的兩個關鍵因素

操作集合是常見的案例。 相片檢視器有相片的集合、閱讀程式有文章/書籍/故事的集合，而購物 app 有產品的集合。 這個主題說明您可以執行的動作，讓您的 app 在操作集合時更有效率。

與集合有關的效能有兩個關鍵因素：一個是 UI 執行緒建立項目所花的時間，另一個則是原始資料集與用來轉譯該資料的 UI 元素所使用的記憶體。

如果想要順暢移動瀏覽/捲動，UI 執行緒必須能夠執行有效且智慧的具現化、資料繫結和配置項目工作。

## <a name="ui-virtualization"></a>UI 虛擬化

UI 虛擬化是您可以執行的最重要改善。 這表示代表項目的 UI 元素是依需求建立。 對於繫結至 1000 個項目集合的項目控制項，同時針對所有項目建立 UI 是浪費資源，因為項目不會同時全部顯示。 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) (及其他標準 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) 衍生的控制項) 會為您執行 UI 虛擬化。 當項目即將被捲動到檢視 (相差幾頁) 時，架構會產生項目的 UI 並且快取它們。 不會再次顯示項目時，架構會回收記憶體。

如果您提供自訂項目面板範本 (請參閱 [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx))，則確定您使用虛擬面板，例如 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) 或 [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795)。 如果您使用 [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227651)、[**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227717) 或 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)，則不會虛擬化。 此外，僅在使用 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) 或 [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795) 時會引發下列 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 事件：[**ChoosingGroupHeaderContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer)、[**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) 和 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging)。

檢視區這個概念對 UI 虛擬化很重要，因為架構必須建立可能要加以顯示的元素。 一般而言，[**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) 的檢視區是邏輯控制項的延伸。 例如，[**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 的檢視區是 **ListView** 元素的寬度和高度。 有些面板允許子元素有不限數量的空間，範例是 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) 和 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)，使用自動調整大小的列或欄。 當虛擬化的 **ItemsControl** 放在這類的面板中時，會採用足夠的空間以顯示其所有項目，虛擬化就無效。 在 **ItemsControl** 設定寬度和高度以還原虛擬化。

## <a name="element-reduction-per-item"></a>每個項目的元素減少

將用來轉譯您的項目的 UI 元素數保持在合理的最小值。

當第一次顯示項目控制項時，會建立用來轉譯充滿項目之檢視區所需的所有元素。 此外，當項目接近檢視區，架構會使用繫結資料物件更新快取項目範本中的 UI 元素。 最小化範本內標記的複雜度可以換得記憶體和 UI 執行緒花費的時間，特別是在移動瀏覽/捲動時能夠改善回應性。 問題中的範本是項目範本 (請參閱 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)) 與 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) 或 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) (項目控制項範本或 [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)) 的控制項範本。 即使是小型元素計數減少乘上顯示項目的好處。

如需元素減少的範例，請參閱[最佳化您的 XAML 標記](optimize-xaml-loading.md)。

[**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) 和 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) 的預設控制項範本包含 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/Dn298500) 元素。 這個展示器是單一的最佳化元素，顯示焦點、選擇和其他視覺狀態的複雜視覺效果。 如果您已經有自訂項目控制項範本 ([**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle))，或如果您在未來會編輯項目控制項範本的複本，我們建議您使用 **ListViewItemPresenter**，因為在大部分情況下，這些元素會提供您效能和自訂之間的最佳平衡。 您可以在展示器上設定屬性以自訂。 例如，以下標記移除選取項目時預設顯示的核取記號，並將所選項目的背景色彩變更為橘色。

```xml
...
<ListView>
 ...
 <ListView.ItemContainerStyle>
 <Style TargetType="ListViewItem">
 <Setter Property="Template">
 <Setter.Value>
 <ControlTemplate TargetType="ListViewItem">
 <ListViewItemPresenter SelectionCheckMarkVisualEnabled="False" SelectedBackground="Orange"/>
 </ControlTemplate>
 </Setter.Value>
 </Setter>
 </Style>
 </ListView.ItemContainerStyle>
</ListView>
<!-- ... -->
```

有大約 25 個屬性含有類似於 [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled.aspx) 和 [**SelectedBackground**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedbackground.aspx) 的自我描述名稱。 如果展示器類型證明不足以針對您的使用案例進行自訂，您可以改為編輯 `ListViewItemExpanded` 或 `GridViewItemExpanded` 控制項範本的複本。 您可以在 `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml` 中找到這些項目。 請注意，使用這些範本表示，增加自訂就要犧牲一些效能。

<span id="update-items-incrementally"/>

## <a name="update-listview-and-gridview-items-progressively"></a>漸進式更新 ListView 與 GridView 項目

如果您使用資料虛擬化，則可以保留 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 和[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 的高回應性，方法是設定控制項針對仍在 (下載) 載入的項目轉譯暫時 UI 元素。 隨著資料載入，暫時元素隨後會逐漸被實際 UI 取代。

而且—不論您是從哪裡載入資料 (本機磁碟機、網路或雲端)—使用者可以快速移動瀏覽/捲動 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 或 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)，以致於無法在保留順暢移動瀏覽/捲動的同時，完全不失真地轉譯每個項目。 若要保留順暢的移動瀏覽/捲動，您可以選擇在使用預留位置之外，在多個階段中轉譯項目。

這些技術的範例常見於相片檢視 app：即使尚未下載和顯示所有影像，使用者仍然可以移動瀏覽/捲動並與集合互動。 或者，對於「影片」項目，您可以在第一個階段中顯示標題，在第二個階段顯示分級，以及在第三個階段顯示海報影像。 使用者能夠越早看到每個項目的最重要資料，就表示他們能夠越快採取動作。 然後在時間允許時會填入較不重要的資訊。 以下是您可以用來實作這些技術的平台功能。

### <a name="placeholders"></a>預留位置

暫時預留位置視覺效果功能預設為開啟，它是使用 [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) 屬性進行控制。 在快速移動瀏覽/捲動期間，這項功能可為使用者提供視覺提示，以了解還有更多項目尚未完整顯示，同時保留順暢度。 如果您使用下列其中一個技術，若您不想讓系統轉譯預留位置，則可將 **ShowsScrollingPlaceholders** 設為 False。

**使用 x:Phase 的漸進式資料範本更新**

以下說明如何使用 [x:Phase 屬性](https://msdn.microsoft.com/library/windows/apps/Mt204790)與 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 繫結，實作漸進式資料範本更新。

1.  以下是繫結來源的外觀 (這是我們將繫結至的資料來源)。

    ```csharp
    namespace LotsOfItems
    {
        public class ExampleItem
        {
            public string Title { get; set; }
            public string Subtitle { get; set; }
            public string Description { get; set; }
        }

        public class ExampleItemViewModel
        {
            private ObservableCollection<ExampleItem> exampleItems = new ObservableCollection<ExampleItem>();
            public ObservableCollection<ExampleItem> ExampleItems { get { return this.exampleItems; } }

            public ExampleItemViewModel()
            {
                for (int i = 1; i < 150000; i++)
                {
                    this.exampleItems.Add(new ExampleItem(){
                        Title = "Title: " + i.ToString(),
                        Subtitle = "Sub: " + i.ToString(),
                        Description = "Desc: " + i.ToString()
                    });
                }
            }
        }
    }
    ```
2.  以下顯示 `DeferMainPage.xaml` 包含的標記。 格線檢視包含項目範本，具有繫結至 **MyItem** 類別的 **Title**、**Subtitle** 和 **Description** 屬性的元素。 請注意，**x:Phase** 預設值為 0。 這裡項目僅以可見的標題進行初始轉譯。 然後副標題元素會資料繫結並且對所有項目顯示，直到所有階段都已處理。
    ```xml
    <Page
        x:Class="LotsOfItems.DeferMainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Text="{x:Bind Subtitle}" x:Phase="1"/>
                            <TextBlock Text="{x:Bind Description}" x:Phase="2"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```

3.  如果您現在執行 app 並且快速移動瀏覽/捲動格線檢視，則您會發現畫面上出現的每個新項目，一開始轉譯成暗灰色矩形 (由於 [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) 屬性預設為 **true**)，然後標題會出現，後面跟著副標題，再來是描述。

**使用 ContainerContentChanging 的漸進式資料範本更新**

[**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) 事件的一般策略是使用 **Opacity** 來隱藏不需要立即看到的元素。 回收元素時，它們會保留舊值，所以我們想要隱藏這些元素直到我們已經從新的資料項目更新這些值。 我們在事件引數上使用 **Phase** 屬性，以判斷要更新和顯示的項目。 如果需要額外的階段，我們會註冊回呼。

1.  我們會針對 **x:Phase** 使用相同的繫結來源。

2.  以下顯示 `MainPage.xaml` 包含的標記。 格線檢視宣告處理常式為其 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) 事件，所包含的項目範本具有用來顯示 **MyItem** 類別之 **Title**、**Subtitle** 和 **Description** 屬性的元素。 為了獲得使用 **ContainerContentChanging** 的最大效能優點，我們不在標記中使用繫結，而是改為以程式設計的方式指派值。 以下的例外狀況是我們在階段 0 中考量，顯示標題的元素。
    ```xml
    <Page
        x:Class="LotsOfItems.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}" ContainerContentChanging="GridView_ContainerContentChanging">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Opacity="0"/>
                            <TextBlock Opacity="0"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```
3.  最後，這是 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) 事件處理常式的實作。 這個程式碼也說明我們如何將類型 **RecordingViewModel** 的屬性新增至 **MainPage**，以公開類別的繫結來源類別，該類別代表我們的標記頁面。 只要您在資料範本中沒有任何 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 繫結，則請在處理常式的第一個階段將事件引數物件標示為已處理，提示該項目不需要設定資料內容。
    ```csharp
    namespace LotsOfItems
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                this.ViewModel = new ExampleItemViewModel();
            }

            public ExampleItemViewModel ViewModel { get; set; }

            // Display each item incrementally to improve performance.
            private void GridView_ContainerContentChanging(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 0)
                {
                    throw new System.Exception("We should be in phase 0, but we are not.");
                }

                // It's phase 0, so this item's title will already be bound and displayed.

                args.RegisterUpdateCallback(this.ShowSubtitle);

                args.Handled = true;
            }

            private void ShowSubtitle(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 1)
                {
                    throw new System.Exception("We should be in phase 1, but we are not.");
                }

                // It's phase 1, so show this item's subtitle.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[1] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Subtitle;
                textBlock.Opacity = 1;

                args.RegisterUpdateCallback(this.ShowDescription);
            }

            private void ShowDescription(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 2)
                {
                    throw new System.Exception("We should be in phase 2, but we are not.");
                }

                // It's phase 2, so show this item's description.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[2] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Description;
                textBlock.Opacity = 1;
            }
        }
    }
    ```

4.  如果您現在執行 app 並且快速移動瀏覽/捲動格線檢視，則您會看到與使用 **x:Phase** 時相同的行為。

## <a name="container-recycling-with-heterogeneous-collections"></a>含異質集合的容器回收

在某些應用程式中，在集合內需要針對不同類型的項目有不同的 UI。 這會產生一種虛擬化面板不可能發生的情況，也就是重複使用/回收使用過的元素來顯示項目。 在移動瀏覽期間重新建立項目的視覺元素，會消除許多虛擬化提供的效能優點。 不過，稍加規劃就可允許虛擬化面板重複使用元素。 開發人員視其案例會有數個選項：[**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) 事件或項目範本選取器。 **ChoosingItemContainer** 方法有較佳的效能。

**ChoosingItemContainer 事件**

[**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) 事件可讓您在每當啟動或回收期間需要新項目時就提供項目 (**ListViewItem**/**GridViewItem**) 給 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)/[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)。 您可以根據容器將會顯示的資料項目類型建立容器 (如下列範例所示)。 **ChoosingItemContainer** 是針對不同項目使用不同資料範本，一個高效能的方式。 容器快取可以使用 **ChoosingItemContainer** 來達成。 例如，如果您有五個不同的範本，其中某個範本比其他範本更常發生，則 ChoosingItemContainer 不僅可讓您以需要的比例建立項目，還可保留適當的快取元素數目以供回收。 [**ChoosingGroupHeaderContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer) 針對群組標頭提供相同的功能。

```csharp
// Example shows how to use ChoosingItemContainer to return the correct
// DataTemplate when one is available. This example shows how to return different 
// data templates based on the type of FileItem. Available ListViewItems are kept
// in two separate lists based on the type of DataTemplate needed.
private void ListView_ChoosingItemContainer
    (ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    // Determines type of FileItem from the item passed in.
    bool special = args.Item is DifferentFileItem;

    // Uses the Tag property to keep track of whether a particular ListViewItem's 
    // datatemplate should be a simple or a special one.
    string tag = special ? "specialFiles" : "simpleFiles";

    // Based on the type of datatemplate needed return the correct list of 
    // ListViewItems, this could have also been handled with a hash table. These 
    // two lists are being used to keep track of ItemContainers that can be reused.
    List<UIElement> relevantStorage = special ? specialFileItemTrees : simpleFileItemTrees;

    // args.ItemContainer is used to indicate whether the ListView is proposing an 
    // ItemContainer (ListViewItem) to use. If args.Itemcontainer, then there was a 
    // recycled ItemContainer available to be reused.
    if (args.ItemContainer != null)
    {
        // The Tag is being used to determine whether this is a special file or 
        // a simple file.
        if (args.ItemContainer.Tag.Equals(tag))
        {
            // Great: the system suggested a container that is actually going to 
            // work well.
        }
        else
        {
            // the ItemContainer's datatemplate does not match the needed 
            // datatemplate.
            args.ItemContainer = null;
        }
    }

    if (args.ItemContainer == null)
    {
        // see if we can fetch from the correct list.
        if (relevantStorage.Count > 0)
        {
            args.ItemContainer = relevantStorage[0] as SelectorItem;
        }
        else
        {
            // there aren't any (recycled) ItemContainers available. So a new one 
            // needs to be created.
            ListViewItem item = new ListViewItem();
            item.ContentTemplate = this.Resources[tag] as DataTemplate;
            item.Tag = tag;
            args.ItemContainer = item;
        }
    }
}
```

**項目範本選取器**

項目範本選取器 ([**DataTemplateSelector**](https://msdn.microsoft.com/library/windows/apps/BR209469)) 可讓 app 根據要顯示的資料項目類型，在執行階段傳回不同的項目範本。 這可以讓開發更具生產力，但因為不是每個項目範本都可以針對每個資料項目重複使用，這也會讓 UI 虛擬化更困難。

回收項目 (**ListViewItem**/**GridViewItem**) 時，架構必須決定回收佇列 (回收佇列會快取目前沒有用來顯示資料的項目) 中可供使用的項目是否有項目範本符合目前資料項目所需的項目範本。 如果回收佇列中沒有任何項目有適當的項目範本，則會建立新的項目，並且會針對這個項目具現化適當的項目範本。 但是如果回收佇列中的項目有適當的項目範本，則該項目會從回收佇列中移除，並用於目前的資料項目。 項目範本選取器只能在使用少量項目範本的情形中運作，在使用不同項目範本的項目集合中具有平坦的分佈。

如果使用不同項目範本的項目有不平坦的分佈，很可能需要在移動瀏覽時建立新的項目範本，這會讓虛擬化提供的許多好處無效。 此外，項目範本選取器在評估特定容器是否可供目前的資料項目重複使用時，只考量五個可能的候選項目。 因此您在 app 中使用項目範本選取器之前，應該仔細考量您的資料是否適合。 如果您的集合大部分是同質的，則選取器大部分 (可能幾乎所有) 時間都會傳回相同的類型。 需要注意的是您對該同質化的極少數例外狀況要付出的代價，並考量使用 [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) (或兩個項目控制項) 是否會更好。

 

