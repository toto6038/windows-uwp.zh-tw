---
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
description: 了解如何使用撥動命令作為操作功能表的觸控快速操作，讓使用者可以快速連續地對多個項目執行相同的作業。
title: Swipe
label: Swipe
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7cc03fd8b2acafd7f3e85b0a6438365d74d5657c
ms.sourcegitcommit: c5fdcc0779d4b657669948a4eda32ca3ccc7889b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102784819"
---
# <a name="swipe"></a>Swipe

撥動命令是操作功能表的快速操作，讓使用者不需要在應用程式中變更狀態，即可透過觸控輕鬆存取常用的功能表動作。

> **Windows UI 程式庫 API**：[SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol)、[SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem)
>
> **平台 API**：[SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol)、[SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem)、[ListView class](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![執行並顯示淺色佈景主題](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

撥動命令功能節省空間。 在使用者可能快速連續地對多個項目執行相同作業的情況中，這會很有用。 它可在不需於頁面中產生完整快顯或狀態變更的項目上提供「快速控制項目」。

當您的項目群組可能會很龐大，而每個項目有 1-3 個或許需要使用者定期執行的動作時，應該使用撥動命令功能。 這些動作包括但不限於：

- 刪除
- 標示或封存
- 儲存或下載
- 回覆

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/SwipeControl">開啟應用程式並查看 SwipeControl 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>撥動如何運作？

UWP 撥動命令有兩種模式：[顯示](/uwp/api/windows.ui.xaml.controls.swipemode)及[執行](/uwp/api/windows.ui.xaml.controls.swipemode)。 同時還支援四個不同的撥動方向：向上、向下、向左和向右。

### <a name="reveal-mode"></a>顯示模式

在顯示模式下，使用者撥動項目以開啟包含一個或多個命令的功能表，而且必須明確點選要執行的命令。 當使用者撥動並放開某個項目，功能表會保持開啟，直到選取了一個命令，或透過往回撥動、點選關閉，或將開啟的撥動項目捲動到螢幕外面而再次關閉功能表。

![撥動以顯示](images/SwipeCommand-Reveal_v2.gif)

顯示模式是一種更安全、更多功能的撥動模式，可用於大多數功能表動作類型，甚至包括潛在破壞性動作，例如刪除。

當使用者選取其中一個在顯示之開啟及靜止狀態中出現的功能選項時，會叫用該項目的命令並關閉撥動控制項。

### <a name="execute-mode"></a>執行模式

在執行模式下，使用者撥動開啟的項目，以透過這一個撥動來顯示和執行單一命令。 如果使用者在撥動超過閾值之前放開所撥動的項目，則會關閉功能表，並且不執行命令。 如果使用者撥動超過閾值然後放開項目，則立即執行命令。

![撥動以執行](images/SwipeCommand_Delete_v2.gif)

如果到達閾值後，使用者未放開手指，並且拖動再次關閉的撥動項目時，則不執行命令，而且項目上不會執行任何動作。

正在撥動項目時，執行模式透過色彩和標籤方向提供更多視覺化回饋。

執行最適合用於使用者正在執行最常見的動作。

還可能用於更具破壞性的動作，例如刪除項目。 不過，請記住，「執行」只需要一個依方向撥動的動作，而「顯示」則不同，它需要使用者明確地按一下按鈕。

### <a name="swipe-directions"></a>撥動方向

撥動適用於所有基本方向：向上、向下、向左和向右。 每個撥動方向都可以擁有其本身的撥動項目或內容，但在單一可撥動元素上，一次只能設定一個方向執行個體。

例如，在同一個 SwipeControl 上，不能有兩個 [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems) 定義。

## <a name="how-to-create-a-swipe-command"></a>如何建立撥動命令

撥動命令有兩個需要定義的元件：

- [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol)，這會包裝您的內容。 在集合 (例如 ListView) 中，這會置於 DataTemplate 內。
- 撥動功能表項目 (這是放在撥動控制項方向容器中的一個或多個 [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem) 物件)：[LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems)、[RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems)、[TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems)或 [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)

撥動內容可以用內嵌方式放置，或定義於頁面或應用程式的 [資源] 區段。

以下是包裝一些文字的 SwipeControl 簡單範例。 其中顯示建立撥動命令所需之 XAML 元素的階層。

```xaml
<SwipeControl HorizontalAlignment="Center" VerticalAlignment="Center">
    <SwipeControl.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Pin">
                <SwipeItem.IconSource>
                    <SymbolIconSource Symbol="Pin"/>
                </SwipeItem.IconSource>
            </SwipeItem>
        </SwipeItems>
    </SwipeControl.LeftItems>

     <!-- Swipeable content -->
    <Border Width="180" Height="44" BorderBrush="Black" BorderThickness="2">
        <TextBlock Text="Swipe to Pin" Margin="4,8,0,0"/>
    </Border>
</SwipeControl>
```

我們現在要看一下更完整的範例，以了解通常如何在清單中使用撥動命令。 在此範例中，您將會設定使用執行模式的刪除命令，以及其他使用顯示模式之命令的功能表。 這兩組命令都是在頁面的 [資源] 區段中定義。 您會將撥動命令套用到 ListView 中的項目。

首先，建立撥動項目 (表示命令) 做為頁面層級資源。 SwipeItem 使用 [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) 做為其圖示。 另外再建立圖示做為資源。

```xaml
<Page.Resources>
    <SymbolIconSource x:Key="ReplyIcon" Symbol="MailReply"/>
    <SymbolIconSource x:Key="DeleteIcon" Symbol="Delete"/>
    <SymbolIconSource x:Key="PinIcon" Symbol="Pin"/>

    <SwipeItems x:Key="RevealOptions" Mode="Reveal">
        <SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"/>
        <SwipeItem Text="Pin" IconSource="{StaticResource PinIcon}"/>
    </SwipeItems>

    <SwipeItems x:Key="ExecuteDelete" Mode="Execute">
        <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
                   Background="Red"/>
    </SwipeItems>
</Page.Resources>
```

記得讓撥動內容中的功能表項目保持為簡明扼要的文字標籤。 這些動作應該是使用者可能會在短時間內執行多次的主要動作。

設定要在集合或 ListView 中使用的撥動命令，除了是在 DataTemplate 中定義 SwipeControl 以便套用至集合中每個項目之外，與定義單一撥動命令 (先前示範過) 並無不同。

以下是已在其項目 DataTemplate 中套用 SwipeControl 的 ListView。 LeftItems 和 RightItems 屬性會參考您已建立做為資源的撥動項目。

```xaml
<ListView x:Name="sampleList" Width="300">
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
        </Style>
    </ListView.ItemContainerStyle>
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <SwipeControl x:Name="ListViewSwipeContainer"
                          LeftItems="{StaticResource RevealOptions}"
                          RightItems="{StaticResource ExecuteDelete}"
                          Height="60">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="{x:Bind}" FontSize="18"/>
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit..." FontSize="12"/>
                    </StackPanel>
                </StackPanel>
            </SwipeControl>
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

## <a name="handle-an-invoked-swipe-command"></a>處理叫用的撥動命令

若要因應撥動命令執行動作，請處理其 [Invoked](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked) 事件  (如需使用者如何叫用命令的詳細資訊，請檢閱稍早在本文中的 _撥動如何運作？_ 一節。)一般而言，撥動命令是在 ListView 或類似清單的案例中。 在該情況下，當叫用命令時，您會想要對這個撥動項目執行動作。

以下說明如何在您先前建立的 _刪除_ 撥動項目上處理叫用事件。

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

資料項目是 SwipeControl 的 DataContext。 在程式碼中，您可以藉由從事件引數取得 SwipeControl.DataContext 屬性，存取所撥動的項目，如下所示。

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> 為了簡單起見，這裡已直接將項目新增至 ListView.Items 集合，因此項目也是以同樣方式來刪除。 如果您改以較常見的方式將 ListView.ItemsSource 設定為集合，就必須從來源集合中刪除項目。

在這個特定範例中，您已從清單移除項目，因此撥動項目的最終視覺狀態不是很重要。 不過，如果您只是要執行動作，然後讓撥動再次摺疊時，則可以將 [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) 屬性設定為其中一個 [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked) 列舉值。

- **Auto**
  - 在執行模式中，開啟的撥動項目仍然會在進行叫用時保持開啟。
  - 在顯示模式中，開啟的撥動項目則會在進行叫用時摺疊。
- **關閉**
  - 叫用項目時，不論模式為何，撥動控制項永遠都會摺疊並回復正常狀態。
- **RemainOpen**
  - 叫用項目時，不論模式為何，撥動控制項永遠保持開啟。

這裡已將 _回覆_ 撥動項目設定為要在叫用後關閉。

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>可行與禁止事項

- 請勿在 FlipViews 或集線器中使用 [滑動]。 這種組合可能會因為撥動方向衝突而造成使用者的困惑。
- 不要同時進行水平撥動和水平瀏覽，或同時進行垂直撥動和垂直瀏覽。
- 務必確定使用者撥動的是相同動作，且在所有可撥動的相關項目中保持一致。
- 務必使用撥動進行使用者想要執行的主要動作。
- 務必在會重複相同動作多次的項目上使用撥動。
- 務必在較寬的項目上使用水平撥動，在較高的項目上使用垂直撥動。
- 務必使用簡明扼要的文字標籤。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [清單檢視和方格檢視](listview-and-gridview.md)
- [項目容器與範本](item-containers-templates.md)
- [拖動以重新整理](pull-to-refresh.md)
