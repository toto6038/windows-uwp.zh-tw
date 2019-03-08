---
description: 連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。
title: 連接動畫
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a205fb151d1c9e6614dc97ccde639e43720aa8a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618193"
---
# <a name="connected-animation-for-uwp-apps"></a>UWP 應用程式適用的連接動畫

連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。 這可幫助使用者能夠在檢視之間保持其脈絡並連續性。

在 已連線的動畫，項目會出現 繼續 期間變更 UI 內容，在畫面上飛從其原始碼檢視中的位置到其目的地，在新的檢視中的兩個檢視之間。 這強調在檢視之間常見的內容，並建立美觀和動態效果之轉換的一部分。

> **重要的 Api**:[ConnectedAnimation 類別](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)， [ConnectedAnimationService 類別](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

## <a name="see-it-in-action"></a>以動作呈現

在這個簡短的影片中，應用程式會使用已連線的動畫以動畫顯示的項目映像，「 持續 」 會成為下一個頁面的標頭的一部分。 此效果有助於維持整個轉換過程的使用者內容。

![連接動畫](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>連接動畫和 Fluent 設計系統

 Fluent Design 系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 連接動畫是將動作加入應用程式中的 Fluent 設計系統元件。 若要深入了解，請參閱[適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)。

## <a name="why-connected-animation"></a>為何要使用連接動畫？

在頁面之間瀏覽時，請務必讓使用了解在瀏覽之後會呈現哪些新的內容，以及在瀏覽時與使用者的目的有何相關。 連接動畫會將使用者的注意力吸引到兩個檢視之間共用的內容，而以視覺上強大的暗示來強調檢視之間的關係。 此外，連接動畫讓畫面更賞心悅目，並能改進有助於區分您應用程式動作設計的頁面瀏覽方式。

## <a name="when-to-use-connected-animation"></a>連接動畫使用時機

雖然當您變更 UI 內容及想要使用者維持脈絡時可應用動畫，但連接動畫一般用於變更頁面時。 只要來源檢視與目的地檢視之間有共用影像或其他的 UI 部分，您應考慮使用連接動畫，而非[向下切入的瀏覽轉換](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)。

## <a name="configure-connected-animation"></a>設定已連線的動畫

> [!IMPORTANT]
> 這項功能需要您的應用程式的目標版本是 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本。 無法在舊版的 Sdk 中取得組態屬性。 您可以為目標的最小版本低於 SDK 17763 使用調適性的程式碼或條件式 XAML。 如需詳細資訊，請參閱 <<c0> [ 版本的自適性應用程式](/windows/uwp/debug-test-perf/version-adaptive-apps)。

從 Windows 10 版本 1809，進一步相連的動畫結合 Fluent 設計藉由提供動畫設定量身訂做特別針對向前及向後頁面導覽。

您可以指定動畫設定藉由設定 ConnectedAnimation 中的 組態屬性。 （我們將在下一節中示範的範例。）

下表描述可用的組態。 如需有關套用這些動畫的影片原則的詳細資訊，請參閱[方向和重力](index.md)。

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| 這是預設的設定，並建議向前巡覽。 |
當使用者瀏覽順向 (A 到 B) 的應用程式中，已連線的項目會出現實際 「 提取出頁面 」。 在此情況下，項目會出現在 z 空間中向前移動，而且會有點採取保留受到重力影響卸除。 若要克服受到重力影響，項目提升速度，並加速其最終位置。 結果會是 「 小數位數和 dip 」 的動畫。 |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| 當使用者向後巡覽 (A 到 B) 的應用程式中，動畫會是更直接。 已連線的項目以線性方式會將轉譯 B 使用開始減速三次方貝茲 easing 函式。 回溯視覺化功能的可見性會傳回使用者為先前的狀態以最快速度同時仍可保有瀏覽流程的內容。 |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| 這是預設值 （僅與） 在 Windows 10 版本 1809年之前的版本中使用的動畫 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk))。 |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 組態

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)類別有兩個屬性套用至個別的動畫，而不是整體的服務。

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

若要達到的各種效果，某些組態會忽略 ConnectedAnimationService 這些屬性，並使用自己的值，這個資料表中所述。

| 設定 | 尊重 DefaultDuration 嗎？ | 尊重 DefaultEasingFunction 嗎？ |
| - | - | - |
| 重力 | 是 | 是* <br/> **從 A 到 B 的基本轉譯會使用這個 easing 函式，但是"重力 dip 」 有它自己的 easing 函式。*  |
| 直接存取 | 否 <br/> *以動畫顯示超過 150ms年。*| 否 <br/> *會使用加/減速函數開始減速。* |
| 基本 | 是 | 是 |

## <a name="how-to-implement-connected-animation"></a>如何實作連線的動畫

設定連接動畫有兩個步驟︰

1. *準備*動畫物件在來源頁面上，這表示系統的來源項目將參與連接的動畫。
1. *啟動*在 [目的地] 頁面中，參考傳遞給目的地項目的動畫。

瀏覽時從 [來源] 頁面，呼叫[ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview)取得 ConnectedAnimationService 的執行個體。 若要準備動畫，請呼叫[PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate)這個執行個體，並傳遞的唯一索引鍵和您想要在轉換中使用的 UI 項目中。 唯一索引鍵可讓您擷取動畫稍後在 [目的地] 頁面。

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

瀏覽時，請在 [目的地] 頁面中啟動動畫。 若要啟動動畫，請呼叫 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)。 您可以使用您在編輯動畫時提供的唯一索引鍵來呼叫 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)，藉此取得適當的動畫執行個體。

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>向前巡覽

此範例示範如何使用 ConnectedAnimationService 建立向前巡覽兩個頁面 (以 Page_B Page_A) 之間的轉換。

向前巡覽的建議的動畫設定尚未[GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)。 這是預設值，因此您不需要設定[組態](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration)屬性除非您想要指定不同的組態。

設定來源 頁面中的動畫。

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

在 [目的地] 頁面中啟動動畫。

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

### <a name="back-navigation"></a>向後巡覽

為向後巡覽 (Page_B Page_A 到)，您可以遵循相同的步驟，但會反轉的來源和目的地的頁面。

當使用者瀏覽上一步時，他們會期望應用程式，以儘速返回先前的狀態。 因此，建議的設定是[DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)。 這個動畫更快、 更直接的並使用 開始減速加/減速。

設定來源 頁面中的動畫。

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

在 [目的地] 頁面中啟動動畫。

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

之間的時間，並將動畫設定和啟動時，來源項目會出現上述應用程式中的其他 UI 凍結。 這可讓您同時執行任何其他轉換動畫。 基於這個理由，您不應該等到超過 ~ 250 毫秒之間的兩個步驟，因為來源項目可能會變得令人分心。 如果您準備動畫，且在三秒內未啟動動畫，則系統將會處理動畫，任何後續對 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 的呼叫都將會失敗。

## <a name="connected-animation-in-list-and-grid-experiences"></a>清單中的連接動畫與格線體驗

通常，您會想要從清單或格線控制項建立連接動畫，或連接動畫建立至清單或格線控制項。 您可以使用兩個方法上[ListView](/uwp/api/windows.ui.xaml.controls.listview)並[GridView](/uwp/api/windows.ui.xaml.controls.gridview)， [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)並[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)，若要簡化這個程序。

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

若要準備連線的動畫與指定的清單項目對應的省略符號，請呼叫[PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)唯一索引鍵、 項目，與名稱"PortraitEllipse 」 的方法。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

若要啟動動畫與這個項目，做為目的地，例如當瀏覽回從詳細資料檢視中，使用[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)。 如果您只是已載入的資料來源的 ListView，TryStartConnectedAnimationAsync 會等候直到已建立對應的項目容器啟動動畫。

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

A*協調動畫*是一種特殊的開場動畫項目以及已連線的動畫目標，以動畫顯示與已連線的動畫元素一起移動在畫面上出現的位置。 協調動畫可以為轉換增加更多視覺上的趣味，進一步吸引使用者對在來源檢視與目的地檢視之間共用內容的注意力。 在這些影像中。項目的標題 UI 正使用協調動畫呈現動畫效果。

當協調的動畫使用重力組態時，重力會套用到已連線的動畫項目協調的項目。 協調的項目會 「 swoop"與連接的項目一起讓項目保持真正協調。

使用雙參數多載的 **TryStart** 將協調元素新增至連接動畫。 此範例會示範名為"DescriptionRoot"，使用名為"CoverImage 「 已連線的動畫元素會一起進入格線版面配置協調的動畫。

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
- 使用[GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)向前巡覽。
- 使用[DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)針對回 瀏覽。
- 不要再等其他長時間執行的非同步作業準備和啟動已連線的動畫之間網路要求上。 您可能需要預先載入必要的資訊，才能事先執行轉換，或在目的地檢視中載入高解析度影像時，使用低解析度預留位置影像。
- 使用[SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)若要避免在過場動畫**框架**如果您使用**ConnectedAnimationService**，自相連的動畫不適用於同時預設瀏覽轉換。 請參閱 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 以取得如何使用瀏覽轉換的詳細資訊。

## <a name="download-the-code-samples"></a>下載程式碼範例

請參閱 [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) 範例庫中的[連接動畫範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample)。

## <a name="related-articles"></a>相關文章

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
