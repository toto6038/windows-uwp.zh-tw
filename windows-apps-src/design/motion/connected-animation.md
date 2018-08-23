---
author: mijacobs
description: 連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。
title: 連接動畫
template: detail.hbs
ms.author: jimwalk
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a789a8f082192b79b3e96990827f9a4f6a0eacbc
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "2815300"
---
# <a name="connected-animation-for-uwp-apps"></a>UWP 應用程式適用的連接動畫

## <a name="what-is-connected-animation"></a>什麼是連接動畫？

連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。 這可幫助使用者能夠在檢視之間保持其脈絡並連續性。
在連接動畫中，當 UI 內容變更期間元素在兩個檢視之間看起來是「連續的」，從其來源檢視中的位置飛過畫面到其新檢視中的目的地。 這可強調檢視之間的通用內容，並在轉換時創造美麗且動態的效果。

## <a name="see-it-in-action"></a>以動作呈現

在這個簡短的影片中，應用程式使用連接動畫讓項目有動畫效果，因為它會「持續」成為下一頁標題的一部分。 此效果有助於維持整個轉換過程的使用者內容。

![連續動作的 UI 範例](images/continuous3.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>連接動畫和 Fluent 設計系統

 Fluent 設計系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 連接動畫是將動作加入應用程式中的 Fluent 設計系統元件。 若要深入瞭解，請參閱[適用於 UWP 的 Fluent 設計概觀](../fluent-design-system/index.md)。

## <a name="why-connected-animation"></a>為何要使用連接動畫？

在頁面之間瀏覽時，請務必讓使用了解在瀏覽之後會呈現哪些新的內容，以及在瀏覽時與使用者的目的有何相關。 連接動畫會將使用者的注意力吸引到兩個檢視之間共用的內容，而以視覺上強大的暗示來強調檢視之間的關係。 此外，連接動畫讓畫面更賞心悅目，並能改進有助於區分您應用程式動作設計的頁面瀏覽方式。

## <a name="when-to-use-connected-animation"></a>連接動畫使用時機

雖然當您變更 UI 內容及想要使用者維持脈絡時可應用動畫，但連接動畫一般用於變更頁面時。 只要來源檢視與目的地檢視之間有共用影像或其他的 UI 部分，您應考慮使用連接動畫，而非[向下切入的瀏覽轉換](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)。

## <a name="how-to-implement"></a>實作方式

設定連接動畫有兩個步驟︰

1.  *準備*來源頁面上的動畫物件，這表示來源元素將參與連接動畫的系統 
2.  *啟動*目的地頁面上的動畫，以傳遞目的地元素的參考動畫

在這兩個步驟之間，來源元素會在應用程式中其他 UI 上顯示成凍結，讓您能夠同時執行任何其他轉換動畫。 有基於此，您在兩個步驟之間不應等待超過 ~250 毫秒，因為來源元素的存在可能會造成分心。 如果您準備動畫，且在三秒內未啟動動畫，則系統將會處理動畫，任何後續對 **TryStart** 的呼叫都將會失敗。

您可以透過呼叫 **ConnectedAnimationService.GetForCurrentView** 來取得目前 ConnectedAnimationService 執行個體的存取權， 若要準備動畫，請在執行個體上呼叫 **PrepareToAnimate**，傳遞唯一索引鍵的參考及您想要在轉換中使用的 UI 元素。 唯一索引鍵可讓您日後擷取動畫，例如擷取不同頁面上的動畫。

```csharp
ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
```

若要啟動動畫，請呼叫 **ConnectedAnimation.TryStart**。 您可以使用您在編輯動畫時提供的唯一索引鍵來呼叫 **ConnectedAnimationService.GetAnimation**，藉此取得適當的動畫執行個體。

```csharp
ConnectedAnimation imageAnimation = 
    ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
if (imageAnimation != null)
{
    imageAnimation.TryStart(DestinationImage);
}
```

以下是使用 ConnectedAnimationService 在兩個頁面之間建立轉換的完整範例。

*SourcePage.xaml*

```xaml
<Image x:Name="SourceImage"
       Width="200"
       Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*SourcePage.xaml.cs*

```csharp
private void NavigateToDestinationPage()
{
    ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
    Frame.Navigate(typeof(DestinationPage));
}
```

*DestinationPage.xaml*

```xaml
<Image x:Name="DestinationImage"
       Width="400"
       Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*DestinationPage.xaml.cs*

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation imageAnimation = 
        ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
    if (imageAnimation != null)
    {
        imageAnimation.TryStart(DestinationImage);
    }
}
```

## <a name="connected-animation-in-list-and-grid-experiences"></a>清單中的連接動畫與格線體驗

通常，您會想要從清單或格線控制項建立連接動畫，或連接動畫建立至清單或格線控制項。 您可以使用兩種新的方法，分別是 **ListView** 與 **GridView**、**PrepareConnectedAnimation** 與 **TryStartConnectedAnimationAsync**，來簡化此程序。
例如，假設您有一個 **ListView** 在其資料範本中包含名為 "PortraitEllipse" 的元素。

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate> 
    </ListView.ItemTemplate>
</ListView>
```

若要準備一個連接動畫，其中包含對應至所指定清單項目的橢圓形，請透過唯一索引鍵呼叫 **PrepareConnectedAnimation** 方法、呼叫項目及 “PortraitEllipse” 名稱。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

或者，若要啟動以此元素為目的地的動畫，例如當從詳細檢視瀏覽時，請使用 **TryStartConnectedAnimationAsync**。 如果您剛剛才為 **ListView** 載入資料來源，**TryStartConnectedAnimationAsync** 將等待啟動動畫，直到對應的項目容器已建立為止。

```csharp
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>協調動畫

![協調動畫](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

*協調動畫*是特殊類型的進入動畫，在此動畫中元素將出現在連接動畫目標的旁邊，當橫跨螢幕移動時與連接動畫元素一前一後地顯示動畫效果。 協調動畫可以為轉換增加更多視覺上的趣味，進一步吸引使用者對在來源檢視與目的地檢視之間共用內容的注意力。 在這些影像中。項目的標題 UI 正使用協調動畫呈現動畫效果。

使用雙參數多載的 **TryStart** 將協調元素新增至連接動畫。 此範例示範名為 “DescriptionRoot” 之格線配置的協調動畫，此動畫會與名為 “CoverImage” 的連接動畫元素一前一後進入。

*DestinationPage.xaml*

```xaml
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

*DestinationPage.xaml.cs*

```csharp
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");
    
    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>應做與不應做事項

- 在來源頁面與目的地頁面之間共用元素的頁面轉換中使用連接動畫。
- 在準備與啟動連接動畫之間，不要等候網路要求或其他長時間執行的非同步作業。 您可能需要預先載入必要的資訊，才能事先執行轉換，或在目的地檢視中載入高解析度影像時，使用低解析度預留位置影像。
- 如果您正在使用 **ConnectedAnimationService**，請使用 [SuppressNavigationTransitionInfo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 防止 **Frame** 中的轉換動畫，因為連接動畫並不用於與預設的瀏覽轉換同時使用。 請參閱 [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) 以取得如何使用瀏覽轉換的詳細資訊。


## <a name="download-the-code-samples"></a>下載程式碼範例

請參閱 [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) 範例庫中的[連接動畫範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample)。

## <a name="related-articles"></a>相關文章

- [ConnectedAnimation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation)
- [ConnectedAnimationService](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation.aspx)
- [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)
