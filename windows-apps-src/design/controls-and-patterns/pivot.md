---
author: serenaz
Description: The Pivot control enables touch-swiping between a small set of content sections.
title: Pivot
template: detail.hbs
ms.author: sezhen
ms.date: 06/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e8f0fbbfacc3fa4edb602f7505ea1e88f211a81a
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "2814850"
---
# <a name="pivot"></a>Pivot

[[樞紐分析表](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)控制項可讓觸控 swiping 內容小節一小群之間。

> **重要 Api**： [[樞紐分析表類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)、 [NavigationView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有安裝的<strong style="font-weight: semi-bold">XAML 控制項圖庫</strong>應用程式，請按一下這裡以<a href="xamlcontrolsgallery:/item/Pivot">開啟 [應用程式並查看巨集指令中的 [樞紐分析表控制項</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

[樞紐分析表控制項，就像是[NavigationView](navigationview.md)，加上底線選取的項目。

![預設焦點底線選取的標頭](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

若要達到常見的上方導覽列和索引標籤模式，建議使用[NavigationView](navigationview.md)、 自動適應不同螢幕大小，並允許更大的自訂。

不過，如果您導覽需要觸控 swiping，我們建議使用 [樞紐分析表。

其他之間的主要差異 NavigationView 和樞紐分析表控制項是預設溢位行為和導覽 API：

- 樞紐分析項目，而 NavigationView 使用功能表下拉式清單溢出，讓使用者可以查看所有項目的各種溢位。
- [樞紐分析表會處理內容] 區段中，而 NavigationView 允許導覽行為的更多控制權之間導覽。

## <a name="use-navigationview-instead-of-pivot"></a>使用 NavigationView 而不是 [樞紐分析表

如果您的應用程式使用者介面使用 [樞紐分析表控制，然後您可以將轉換樞紐分析表 NavigationView 使用以下的程式碼。

此 XAML 建立 NavigationView 3 區段的 [樞紐分析表中[建立樞紐分析表控制項](#create-a-pivot-control)範例類似的內容。

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView 提供更多控制權導覽自訂並要求相對應的程式碼後置。 若要隨附上述 XAML，請使用下列程式碼後置：

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

這段程式碼模擬樞紐分析表控制項的內建的瀏覽體驗，減去內容區段間的觸控 swiping 經驗。 但如您所見，您也可以自訂包括動畫的轉換、 導覽參數及堆疊功能的數個點。

## <a name="create-a-pivot-control"></a>建立 Pivot 控制項

這段程式碼會建立內容的 3 幾個基本 [樞紐分析表控制項。

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Pivot 項目

Pivot 是一種 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何型別的項目集合。 所有新增到 Pivot 且不是明確為 [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) 的項目，都會隱含地包裝在 PivotItem 中。 因為 Pivot 通常是用來在頁面內容之間瀏覽，所以通常會直接使用 XAML UI 元素填入 [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 或者，您可以將 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設為資料來源。 ItemsSource 中繫結的項目可以是任何型別，但如果它們不是明確為 PivotItems，則您必須定義 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 和 [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) 來指定如何顯示這些項目。

您可以使用 [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 屬性擷取或設定選取的項目。 使用 [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 屬性擷取或設定選取的項目。

### <a name="pivot-headers"></a>樞紐標頭

您可以使用 [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 和 [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 屬性來將其他控制項新增到樞紐標頭。

例如，您可以在樞紐 RightHeader 中新增 [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars)。

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>樞紐互動

以下是該控制項主要的觸控手勢互動：

- 點選樞紐項目標頭可瀏覽到該標頭的區段內容。
- 在樞紐項目標頭上向左或向右撥動可瀏覽到相鄰的區段。
- 在區段內容上向左或向右撥動可瀏覽到相鄰的區段。

此控制項有兩種模式：

**靜止**

- 當所有的樞紐標頭大小符合允許的空間時，樞紐會靜止。
- 雖然樞紐本身不會移動，但點選樞紐標籤會瀏覽到對應的頁面。 使用中的樞紐會反白顯示。

**浮動切換**

- 當所有的樞紐標頭大小都不符合允許的空間時，樞紐會浮動切換。
- 點選樞紐標籤會瀏覽到對應的頁面，且使用中的樞紐標籤會浮動切換到第一個位置。
- 各樞紐項目會浮動循環切換，從最後一個接到第一個樞紐區段。

> **注意** 樞紐標頭不可在 [10 英呎環境](../devices/designing-for-tv.md)中浮動切換。 如果 App 會在 Xbox 上執行，請將 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 屬性設為 **false**。

## <a name="recommendations"></a>建議事項

- 當使用浮動切換 (反覆) 模式時請避免使用超過 5 個標頭，因為循環超過 5 個標頭可能會混淆使用者。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題

- [Pivot 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [瀏覽設計基本知識](../basics/navigation-basics.md)