---
Description: Pivot 控制項可讓使用者在一小組內容區段之間進行觸控撥動。
title: 樞紐分析
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 12b6662eb7bbfc08563dd9f7313aa0ea7d0a0e18
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749704"
---
# <a name="pivot"></a>樞紐分析

[Pivot](/uwp/api/Windows.UI.Xaml.Controls.Pivot) \(英文\) 控制項可讓使用者在一小組內容區段之間進行觸控撥動。

![預設焦點底線選取的標頭](images/pivot_focus_selectedHeader.png)

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](../style/rounded-corner.md)。 WinUI 是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **平台 API**：[Pivot 類別](/uwp/api/Windows.UI.Xaml.Controls.Pivot) \(英文\)、[NavigationView 類別](/uwp/api/Windows.UI.Xaml.Controls.NavigationView) \(英文\)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡以<a href="xamlcontrolsgallery:/item/Pivot">開啟應用程式並查看 Pivot 控制項的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Pivot 控制項和 [NavigationView](navigationview.md) 一樣，都會為所選取的項目加上底線。

![預設焦點底線選取的標頭](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

為了達成常見的頂端瀏覽及索引標籤模式，我們建議使用 [NavigationView](navigationview.md)，期能自動適應不同的畫面大小，並允許進行更大幅度的自訂。

不過，如果您的瀏覽需要觸控撥動，我們建議使用 Pivot。

NavigationView 和 Pivot 控制項的另一個差異，在於其預設溢位行為和瀏覽 API：

- Pivot 會浮動切換溢位項目，而 NavigationView 會使用功能表下拉式清單溢位，來讓使用者能夠看見所有項目。
- Pivot 會處理內容區段之間的瀏覽，而 NavigationView 能讓您對瀏覽行為取得進一步的控制。

## <a name="use-navigationview-instead-of-pivot"></a>使用 NavigationView 而非 Pivot

如果您應用程式的 UI 使用 Pivot 控制項，您可以使用下列程式碼將 Pivot 轉換為 NavigationView。

此 XAML 會建立具有 3 個內容區段的 NavigationView，如同[建立 Pivot 控制項](#create-a-pivot-control)中所提供的範例 Pivot。

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

NavigationView 能針對瀏覽自訂提供更大幅度的控制，並需要相對應的程式碼後置。 若要搭配上述 XAML，請使用下列程式碼後置：

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

此程式碼能模擬 Pivot 控制項的內建瀏覽體驗，但無法提供內容區段之間的觸控撥動體驗。 不過正如您所見，您也可以自訂數個點，包括動畫轉換、瀏覽參數，以及堆疊功能。

## <a name="create-a-pivot-control"></a>建立 Pivot 控制項

此程式碼會建立具有 3 個內容區段的基本 Pivot 控制項。

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

Pivot 是一種 [ItemsControl](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)，因此可以包含任何型別的項目集合。 所有新增到 Pivot 且不是明確為 [PivotItem](/uwp/api/Windows.UI.Xaml.Controls.PivotItem) 的項目，都會隱含地包裝在 PivotItem 中。 因為 Pivot 通常是用來在頁面內容之間瀏覽，所以通常會直接使用 XAML UI 元素填入 [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合。 或者，您可以將 [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設為資料來源。 ItemsSource 中繫結的項目可以是任何型別，但如果它們不是明確為 PivotItems，則您必須定義 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 和 [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.pivot.headertemplate) 來指定如何顯示這些項目。

您可以使用 [SelectedItem](/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) 屬性擷取或設定選取的項目。 使用 [SelectedIndex](/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) 屬性擷取或設定選取的項目。

### <a name="pivot-headers"></a>樞紐標頭

您可以使用 [LeftHeader](/uwp/api/windows.ui.xaml.controls.pivot.leftheader) 和 [RightHeader](/uwp/api/windows.ui.xaml.controls.pivot.rightheader) 屬性來將其他控制項新增到樞紐標頭。

例如，您可以在 Pivot 的 RightHeader 中加入 [CommandBar](./app-bars.md)。

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

> **注意** Pivot 標頭不應在 [10 英呎環境](../devices/designing-for-tv.md)中進行浮動切換。 如果您的應用程式會在 Xbox 上執行，請將 [IsHeaderItemsCarouselEnabled](/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) \(英文\) 屬性設為 **false**。

## <a name="recommendations"></a>建議

- 當使用浮動切換 (反覆) 模式時請避免使用超過 5 個標頭，因為循環超過 5 個標頭可能會混淆使用者。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題

- [Pivot 類別](/uwp/api/Windows.UI.Xaml.Controls.Pivot) \(英文\)
- [瀏覽設計基本知識](../basics/navigation-basics.md)
