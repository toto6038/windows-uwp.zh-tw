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
ms.openlocfilehash: 31e940c87626a05ee6911d3ffda36ab8dfd3fad0
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2018
ms.locfileid: "4564025"
---
# <a name="connected-animation-for-uwp-apps"></a>UWP 應用程式適用的連接動畫

連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。 這可幫助使用者能夠在檢視之間保持其脈絡並連續性。

連接動畫，在項目會顯示 「 繼續 」 飛過畫面從其來源檢視中的位置到其新檢視中的目的地的 UI 內容變更期間的兩個檢視之間。 這可強調檢視之間的通用內容並創造美麗且動態的效果轉換的一部分。

> **重要 Api**: [ConnectedAnimation 類別](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)、 [ConnectedAnimationService 類別](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

## <a name="see-it-in-action"></a>以動作呈現

在這個簡短的影片中，應用程式會使用連接的動畫，以動畫顯示項目，因為它會 「 持續 」 成為下一頁的標頭的一部分。 此效果可幫助使用者在轉換之間維持脈絡。

![連接動畫](images/connected-animations/example.gif)

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

## <a name="configure-connected-animation"></a>設定連接的動畫

> [!IMPORTANT]
> 這項功能需要，您的應用程式目標版本是 RS5 (Windows SDK 版本 10.0.NNNNN.0 (Windows 10，版本 YYMM) 或更高版本。 設定屬性不適用於較舊版本的 Sdk。 您可以以 RS5 較低的最小版本為目標 (使用 10.0.NNNNN.0 (Windows 10，版本 YYMM) 的 Windows SDK 版本調適型程式碼或條件式 XAML。 如需詳細資訊，請參閱[版本調適型應用程式](/debug-test-perf/version-adaptive-apps)。

從開始 RS5，進一步連接的動畫只限 Fluent design 藉由提供動畫設定量身訂做的專為向前和向後頁面瀏覽。

您可以指定動畫設定藉由設定 ConnectedAnimation 上設定屬性。 （我們將在下一節中說明的範例）。

下表說明可用的組態。 如需有關在這些動畫中套用的動作原則的詳細資訊，請參閱[方向性和重力](index.md)。

| [GravityConnectedAnimationConfiguration]() |
| - |
| 這是預設設定，並建議向前瀏覽。 |
使用者瀏覽正向應用程式 (A 到 B) 中的，已連接的項目會顯示實際 」 提取關閉頁面 」。 在此情況下，元素會出現在 z 空間向前移動，並卸除稍做為進行保留的重力的效果。 有需要克服的重力效果，元素取得速度，並加速到最終的位置。 結果會是 「 縮放比例及 dip 」 的動畫。 |

| [DirectConnectedAnimationConfiguration]() |
| - |
| 當使用者往回應用程式 (B 到 A) 中，動畫會是更直接存取。 連接的項目以線性方式從 B 轉譯，以使用開始減速三次方貝茲 easing 函式。 向後視覺能供性讓使用者回到先前的狀態速仍維持瀏覽流程的內容。 |

| [BasicConnectedAnimationConfiguration]() |
| - |
| 這是預設值 （與只） RS5 之前的 SDK 版本中使用的動畫 (Windows SDK 版本 10.0.NNNNN.0 (Windows 10，版本 YYMM)。 |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 設定

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)類別有兩個屬性套用至個別的動畫，而不是整體的服務。

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

若要達到各種不同的效果，有些設定忽略這些屬性上 ConnectedAnimationService 並改用他們自己的值，此表格中所述。

| 組態 | 方面 DefaultDuration 嗎？ | 方面 DefaultEasingFunction 嗎？ |
| - | - | - |
| 重力 | 是 | 是* <br/> **基本轉譯從 A 到 B 會使用這個 easing 函式，但是 「 重力 dip 」 有它自己的 easing 函式。*  |
| 直接存取 | 否 <br/> *動畫超過 150ms年。*| 否 <br/> *使用 easing 函式開始減速。* |
| 基本 | 是 | 是 |

## <a name="how-to-implement-connected-animation"></a>如何實作連接的動畫

設定連接動畫有兩個步驟︰

1. *準備*在來源頁面上，這表示來源元素將參與連接動畫系統的動畫物件。
1. *開始*動畫在目的地頁面上，傳遞目的地元素的參考。

當從來源頁面瀏覽，呼叫[ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview)取得的 ConnectedAnimationService 執行個體。 若要準備動畫，呼叫[PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate)在此執行個體，並傳遞唯一索引鍵和您想要在轉換中使用的 UI 元素中。 唯一索引鍵可讓您擷取動畫稍後的目的地頁面。

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

瀏覽時，請在目的地頁面上開始動畫。 若要啟動動畫，請呼叫 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)。 您可以使用您在編輯動畫時提供的唯一索引鍵來呼叫 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)，藉此取得適當的動畫執行個體。

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>正向瀏覽

這個範例示範如何使用 ConnectedAnimationService 建立可向前瀏覽 (以 Page_B Page_A) 的兩個頁面之間的轉換。

正向瀏覽的建議的動畫設定是[GravityConnectedAnimationConfiguration]()。 這是預設值，因此您不需要設定[設定](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration)屬性，除非您想要指定不同的設定。

設定來源頁面中的動畫。

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

在目的地頁面上開始動畫。

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>返回瀏覽

返回瀏覽 (以 Page_A Page_B)，請依照相同的步驟，但在來源和目的地頁面會反轉。

當使用者瀏覽回時，他們會預期儘速返回到先前的狀態應用程式。 因此，建議的設定是[DirectConnectedAnimationConfiguration]()。 這個動畫是快速、 更直接存取時，並開始減速加/減速。

設定來源頁面中的動畫。

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimationService.GetForCurrentView()
            .PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

在目的地頁面上開始動畫。

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

之間的時間，動畫就會設定完成，並啟動時，來源元素會顯示成凍結應用程式中其他 UI 上。 這可讓您同時執行任何其他轉換動畫。 基於這個原因，您不應等待超過 ~ 250 毫秒，兩個步驟之間，因為來源元素的存在可能會造成分心。 如果您準備動畫，且在三秒內未啟動動畫，則系統將會處理動畫，任何後續對 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 的呼叫都將會失敗。

## <a name="connected-animation-in-list-and-grid-experiences"></a>清單中的連接動畫與格線體驗

通常，您會想要從清單或格線控制項建立連接動畫，或連接動畫建立至清單或格線控制項。 您可以在[ListView](/uwp/api/windows.ui.xaml.controls.listview)和[GridView](/uwp/api/windows.ui.xaml.controls.gridview)、 [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)及[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)，使用兩種方法，來簡化此程序。

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

若要準備一個連接的動畫使用對應至所指定的清單項目的橢圓形，呼叫[PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)方法，使用唯一索引鍵、 項目及"PortraitEllipse"名稱。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

若要啟動以此元素為目的地，例如從詳細資料檢視，當瀏覽回動畫使用[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)。 如果您剛剛才為 ListView 載入資料來源，TryStartConnectedAnimationAsync 將等待啟動動畫，直到對應的項目容器已建立為止。

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

*協調的動畫*是一種特殊類型的進入動畫，以及連接的動畫目標，當橫跨螢幕移動，連同連接的動畫元素建立動畫元素的顯示位置。 協調動畫可以為轉換增加更多視覺上的趣味，進一步吸引使用者對在來源檢視與目的地檢視之間共用內容的注意力。 在這些影像中。項目的標題 UI 正使用協調動畫呈現動畫效果。

當協調的動畫便使用重力組態時，重力會套用至連接的動畫元素與協調的元素。 協調的元素將 」 swoop 「 已連線的項目旁邊這樣元素才能維持真正地協調。

使用雙參數多載的 **TryStart** 將協調元素新增至連接動畫。 此範例示範名為"DescriptionRoot"輸入不斷更新使用名為"CoverImage"的連接的動畫元素的格線配置的協調的動畫。

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
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
- 使用[GravityConnectedAnimationConfiguration]()向前瀏覽。
- 使用[DirectConnectedAnimationConfiguration]()返回瀏覽。
- 不要等候網路要求或其他長時間執行非同步作業，準備與啟動連接的動畫之間。 您可能需要預先載入必要的資訊，才能事先執行轉換，或在目的地檢視中載入高解析度影像時，使用低解析度預留位置影像。
- 若要防止**畫面**中的轉換動畫，如果您使用**ConnectedAnimationService**，因為連接的動畫不是要用於與預設的瀏覽同時使用[SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)轉換。 請參閱 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 以取得如何使用瀏覽轉換的詳細資訊。

## <a name="download-the-code-samples"></a>下載程式碼範例

請參閱 [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) 範例庫中的[連接動畫範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample)。

## <a name="related-articles"></a>相關文章

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
