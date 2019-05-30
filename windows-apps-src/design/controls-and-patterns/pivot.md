---
Description: 樞紐分析控制項啟用 touch 撥動之間較少的內容區段。
title: 樞紐分析
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 232da8afeccf5d82f65b51ae0a40905b3433d412
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364537"
---
# <a name="pivot"></a>樞紐分析

[Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)控制項啟用 touch 撥動之間較少的內容區段。

> **重要的 Api**:[樞紐類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)， [NavigationView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可<a href="xamlcontrolsgallery:/item/Pivot">開啟 應用程式，並查看作用中的樞紐分析控制項</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

樞紐分析控制項，如同[NavigationView](navigationview.md)，選取的項目會加上底線。

![預設焦點底線選取的標頭](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

若要達到常見的上方導覽和索引標籤模式，我們建議您使用[NavigationView](navigationview.md)，其自動適應不同螢幕大小，並可讓您更高的自訂。

不過，如果您瀏覽需要觸控撥動，我們建議使用 Pivot。

NavigationView 及樞紐分析表控制項之間的其他主要差異是預設的溢位行為和瀏覽 API:

- 樞紐分析項目，而 NavigationView 使用功能表的下拉式清單中的溢位，讓使用者能夠看到所有項目可提領轉盤溢位。
- 樞紐處理而 NavigationView 允許更充分掌控導覽行為的內容區段之間的巡覽。

## <a name="use-navigationview-instead-of-pivot"></a>使用 NavigationView，而不是樞紐分析

如果您的應用程式的 UI 使用樞紐分析控制項，然後您可以將轉換 Pivot NavigationView 下列程式碼。

此 XAML 會建立 3 個區段的內容，如範例樞紐 NavigationView 中[建立樞紐分析控制項](#create-a-pivot-control)。

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

NavigationView 提供更充分掌控瀏覽自訂，而且需要對應的程式碼後置。 若要搭配上述 XAML，使用下列程式碼後置：

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

此程式碼會模擬樞紐分析控制項的內建的瀏覽體驗，減去觸控撥動體驗之間的內容區段。 不過，如您所見，您也可以自訂數個點，包括動畫的轉換、 瀏覽參數和堆疊功能。

## <a name="create-a-pivot-control"></a>建立 Pivot 控制項

此程式碼會建立基本的樞紐分析控制項具有 3 個區段的內容。

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

Pivot 是一種 [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)，因此可以包含任何型別的項目集合。 所有新增到 Pivot 且不是明確為 [PivotItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PivotItem) 的項目，都會隱含地包裝在 PivotItem 中。 因為 Pivot 通常是用來在頁面內容之間瀏覽，所以通常會直接使用 XAML UI 元素填入 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合。 或者，您可以將 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設為資料來源。 ItemsSource 中繫結的項目可以是任何型別，但如果它們不是明確為 PivotItems，則您必須定義 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 和 [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.headertemplate) 來指定如何顯示這些項目。

您可以使用 [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) 屬性擷取或設定選取的項目。 使用 [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) 屬性擷取或設定選取的項目。

### <a name="pivot-headers"></a>樞紐標頭

您可以使用 [LeftHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.leftheader) 和 [RightHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.rightheader) 屬性來將其他控制項新增到樞紐標頭。

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

**「 定態**

- 當所有的樞紐標頭大小符合允許的空間時，樞紐會靜止。
- 雖然樞紐本身不會移動，但點選樞紐標籤會瀏覽到對應的頁面。 使用中的樞紐會反白顯示。

**浮動切換**

- 當所有的樞紐標頭大小都不符合允許的空間時，樞紐會浮動切換。
- 點選樞紐標籤會瀏覽到對應的頁面，且使用中的樞紐標籤會浮動切換到第一個位置。
- 各樞紐項目會浮動循環切換，從最後一個接到第一個樞紐區段。

> **注意** 樞紐標頭不可在 [10 英呎環境](../devices/designing-for-tv.md)中浮動切換。 如果 App 會在 Xbox 上執行，請將 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 屬性設為 **false**。

## <a name="recommendations"></a>建議

- 當使用浮動切換 (反覆) 模式時請避免使用超過 5 個標頭，因為循環超過 5 個標頭可能會混淆使用者。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題

- [Pivot 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [瀏覽設計基本概念](../basics/navigation-basics.md)