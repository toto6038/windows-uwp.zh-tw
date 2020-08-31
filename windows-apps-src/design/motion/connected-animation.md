---
description: 連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。
title: 連接動畫
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ff252faf4dd49929ec46c2ceaa02f94011e6b225
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169322"
---
# <a name="connected-animation-for-windows-apps"></a>適用于 Windows 應用程式的連線動畫

連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。 這可幫助使用者能夠在檢視之間保持其脈絡並連續性。

在連接的動畫中，專案在 UI 內容變更期間，會在兩個視圖之間顯示為 [繼續]，從畫面上的位置，將其從來源視圖中的位置飛出至新的視圖目的地。 這會強調視圖之間的一般內容，並在轉換過程中建立美觀的動態效果。

> **重要 api**：  [ConnectedAnimation 類別](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)、 [ConnectedAnimationService 類別](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項資源庫</strong> 應用程式，請按一下這裡 <a href="xamlcontrolsgallery:/item/ConnectedAnimation">開啟應用程式，並查看作用中的連線動畫</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

在這段短片中，應用程式會使用連接動畫來建立專案影像的動畫，因為它會「繼續」成為下一個頁面標頭的一部分。 此效果有助於維持整個轉換過程的使用者內容。

![連接動畫](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>連接動畫和 Fluent 設計系統

 Fluent Design 系統能協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 連接動畫是將動作加入應用程式中的 Fluent 設計系統元件。 若要深入了解，請參閱 [Fluent Design 概觀](/windows/apps/fluent-design-system)。

## <a name="why-connected-animation"></a>為何要使用連接動畫？

在頁面之間瀏覽時，請務必讓使用了解在瀏覽之後會呈現哪些新的內容，以及在瀏覽時與使用者的目的有何相關。 連接動畫會將使用者的注意力吸引到兩個檢視之間共用的內容，而以視覺上強大的暗示來強調檢視之間的關係。 此外，連接動畫讓畫面更賞心悅目，並能改進有助於區分您應用程式動作設計的頁面瀏覽方式。

## <a name="when-to-use-connected-animation"></a>連接動畫使用時機

雖然當您變更 UI 內容及想要使用者維持脈絡時可應用動畫，但連接動畫一般用於變更頁面時。 只要來源檢視與目的地檢視之間有共用影像或其他的 UI 部分，您應考慮使用連接動畫，而非[向下切入的瀏覽轉換](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)。

## <a name="configure-connected-animation"></a>設定連線的動畫

> [!IMPORTANT]
> 這項功能會要求您的應用程式目標版本 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本。 舊版 Sdk 無法使用設定屬性。 您可以使用調適型程式碼或條件式 XAML，以低於 SDK 17763 的最低版本為目標。 如需詳細資訊，請參閱 [版本適應性應用程式](../../debug-test-perf/version-adaptive-apps.md)。

從 Windows 10 版本1809開始，連線動畫可提供特別針對向前和向後頁面導覽量身打造的動畫設定，進一步體現流暢的設計。

您可以藉由設定 ConnectedAnimation 上的設定屬性來指定動畫設定。  (在下一節中，我們將示範這項功能的範例。 ) 

下表描述可用的設定。 如需這些動畫中所套用之動作原則的詳細資訊，請參閱 [方向和引力](index.md)。

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| 這是預設設定，建議用於向前導覽。 |
當使用者在應用程式中向前流覽 (A 到 B) 時，連接的元素會顯示為實體「拉出頁面」。 如此一來，專案就會顯示在 z 空間中向前移動，並使其成為引力的效果。 為了克服引力的影響，元素會獲得速度並加速至其最終位置。 結果會是「縮放和 dip」動畫。 |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| 當使用者在應用程式中向後導覽 (B 至) 時，動畫會更直接。 連接的元素會使用減速三次方貝塞爾函式，以線性方式從 B 轉譯成 A。 回溯視覺效果 affordance 會盡可能快速地將使用者傳回至先前的狀態，同時仍維持導覽流程的內容。 |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| 這是預設 (，且僅) Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 之前的版本中使用的動畫。 |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 設定

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)類別有兩個套用至個別動畫的屬性，而不是整個服務。

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

為了達成各種效果，某些設定會忽略 ConnectedAnimationService 上的這些屬性，並改為使用自己的值，如下表所述。

| 設定 | 尊重 DefaultDuration 嗎？ | 尊重 DefaultEasingFunction 嗎？ |
| - | - | - |
| 重力 | 是 | 是* <br/> **從 A 到 B 的基本轉譯會使用此簡化函式，但是「引力 dip」有它自己的緩時函數。*  |
| 直接 | 否 <br/> *在150ms 上繪製動畫。*| 否 <br/> *使用減速緩動函數。* |
| 基本 | 是 | 是 |

## <a name="how-to-implement-connected-animation"></a>如何執行連接的動畫

設定連接動畫有兩個步驟︰

1. *準備* [來源] 頁面上的動畫物件，它會向系統表示來源元素將參與連接的動畫。
1. 在 [目的地] 頁面上，將參考傳遞至目的地元素，以*啟動*動畫。

從來源頁面流覽時，呼叫 [ConnectedAnimationService. GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) 以取得 ConnectedAnimationService 的實例。 若要準備動畫，請在此實例上呼叫 [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) ，然後傳入您想要在轉換中使用的唯一索引鍵和 UI 元素。 唯一索引鍵可讓您稍後在目的地頁面上取出動畫。

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

進行流覽時，在 [目的地] 頁面中啟動動畫。 若要啟動動畫，請呼叫 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)。 您可以使用您在編輯動畫時提供的唯一索引鍵來呼叫 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)，藉此取得適當的動畫執行個體。

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>向前導覽

此範例示範如何使用 ConnectedAnimationService 來建立在兩個頁面之間進行向前導覽的轉換 (Page_A Page_B) 。

建議的向前流覽動畫設定是 [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)。 這是預設值，因此除非您想要指定不同 [的設定](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) ，否則不需要設定設定屬性。

在 [來源] 頁面中設定動畫。

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

### <a name="back-navigation"></a>返回流覽

若要向後流覽 (Page_B Page_A) ，您可以遵循相同的步驟，但來源和目的地頁面會反轉。

當使用者流覽回來時，他們預期應用程式會儘快回到先前的狀態。 因此，建議的設定為 [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)。 這種動畫的速度更快、更直接，並且使用減速的簡化。

在 [來源] 頁面中設定動畫。

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

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

在設定動畫和啟動動畫的時間之間，來源元素會顯示在應用程式中的其他 UI 上方。 這可讓您同時執行任何其他轉換動畫。 基於這個理由，您不應該在兩個步驟之間等待超過 ~ 250 毫秒，因為來源元素的存在可能會造成干擾。 如果您準備動畫，且在三秒內未啟動動畫，則系統將會處理動畫，任何後續對 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 的呼叫都將會失敗。

## <a name="connected-animation-in-list-and-grid-experiences"></a>清單中的連接動畫與格線體驗

通常，您會想要從清單或格線控制項建立連接動畫，或連接動畫建立至清單或格線控制項。 您可以在 [ListView](/uwp/api/windows.ui.xaml.controls.listview) 和 [GridView](/uwp/api/windows.ui.xaml.controls.gridview)（ [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) 和 [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)）上使用這兩個方法，以簡化此程式。

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

若要使用對應至指定清單專案的省略號來準備連接的動畫，請使用唯一索引鍵、專案和名稱 "PortraitEllipse" 來呼叫 [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) 方法。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

若要使用這個元素做為目的地來啟動動畫，例如從詳細資料檢視中流覽回來時，請使用 [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)。 如果您剛剛才為 ListView 載入資料來源，TryStartConnectedAnimationAsync 將等待啟動動畫，直到對應的項目容器已建立為止。

```csharp
private async void ContactsListView_Loaded(object sender, RoutedEventArgs e)
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

*協調的動畫*是一種特殊的進入動畫，其中元素會與連接的動畫目標一起出現，並隨著連接的動畫專案在螢幕上移動時建立動畫。 協調動畫可以為轉換增加更多視覺上的趣味，進一步吸引使用者對在來源檢視與目的地檢視之間共用內容的注意力。 在這些影像中。項目的標題 UI 正使用協調動畫呈現動畫效果。

當協調的動畫使用引力設定時，重力會套用至連接的動畫元素和協調的元素。 協調的元素會與連接的元素一起「全部吸收」，讓元素保持真正的協調。

使用雙參數多載的 **TryStart** 將協調元素新增至連接動畫。 這個範例會示範名為 "DescriptionRoot" 之方格配置的協調動畫，其會以名為 "CoverImage" 的連接動畫元素一起輸入。

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
- 使用 [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) 進行向前導覽。
- 使用 [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) 進行後置導覽。
- 請勿等待網路要求或其他長時間執行的非同步作業在準備和啟動連接的動畫之間進行。 您可能需要預先載入必要的資訊，才能事先執行轉換，或在目的地檢視中載入高解析度影像時，使用低解析度預留位置影像。
- 如果您使用**ConnectedAnimationService**，請使用[SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)來防止**框架**中的轉換動畫，因為連接的動畫不打算同時與預設導覽轉換一起使用。 請參閱 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 以取得如何使用瀏覽轉換的詳細資訊。

## <a name="related-articles"></a>相關文章

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)