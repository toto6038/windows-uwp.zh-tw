---
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
title: 動畫概觀
description: 使用 Windows 執行階段動畫庫的動畫，可以將 Windows 的外觀及操作方式整合到您的應用程式中。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de2544bbd8c7abe9b1852268373cc88913a30227
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7833728"
---
# <a name="animations-in-xaml"></a>XAML 中的動畫

UWP 動畫可透過新增移動與互動性來增強您的 app。 透過使用 Windows 執行階段動畫庫的動畫，您可以將 Windows 的外觀和操作方式整合到您的 app 中。 本主題提供動畫摘要以及每個典型案例使用的範例。

> [!TIP]
> XAML 的 Windows 執行階段控制項包括特定類型的動畫，作為來自動畫庫的內建行為。 在 app 中使用這些控制項，不需要自行進行程式設計，就可以取得動畫的外觀及操作。

Windows 執行階段動畫庫中的動畫有下列優點：

-   與[動畫的指導方針](https://msdn.microsoft.com/library/windows/apps/Dn611854)保持一致的動作
-   在 UI 狀態之間進行快速且流暢的轉換，可以通知但不會打擾到使用者
-   可在應用程式內向使用者指示轉換的視覺行為

例如，當使用者將項目新增到清單時，新項目不會立即出現在清單中，新項目會以動畫方式就定位。 清單中的其他項目則會在短時間內，以動畫方式移至它們的新位置，以便將空間讓給新增的項目。 對使用者而言，這裡的轉換行為會使控制項互動更為明顯。

Windows 10 版本 1607 針對實作動畫引進新的 [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) API，元素會在瀏覽期間顯示在檢視之間的動畫。 這個 API 的使用模式與其他動畫程式庫 API 不同。 **ConnectedAnimationService** 的用法涵蓋在[參考頁面](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx)。

然而動畫庫無法為每種可能的狀況提供動畫。 在某些狀況下，您可能會希望以 XAML 建立自訂動畫。 如需詳細資訊，請參閱[腳本動畫](storyboarded-animations.md)。

此外，針對諸如根據 ScrollViewer 捲動位置產生項目動畫效果的特定進階案例，開發人員可能會想要使用視覺層交互操作來實作自訂動畫。 如需詳細資訊，請參閱[視覺層](https://msdn.microsoft.com/windows/uwp/composition/visual-layer)。

## <a name="types-of-animations"></a>動畫類型

Windows 執行階段動畫系統與動畫庫的目標更為遠大，也就是讓控制項與 UI 的其他部分的行為都能有動畫效果。 動畫有好幾種不同的類型。

-   當 UI 中的特定條件變更時 (與來自預先定義之 Windows 執行階段 XAML UI 類型的控制項或元素有關)，就會自動套用*佈景主題轉換*。 這些項目之所以名為*佈景主題轉換*，是因為動畫在從某個互動模式變更為另一個互動模式時，可支援 Windows 外觀及操作，而且可定義所有應用程式針對特定 UI 狀況所執行的動作。 這些佈景主題轉換是動畫庫的一部分。
-   *佈景主題動畫*是預先定義之 Windows 執行階段 XAML UI 類型的一或多個屬性的動畫。 佈景主題動畫與佈景主題轉換不同，因為佈景主題動畫會針對某個特定元素，並存在於某個控制項內的特定視覺狀態中，而佈景主題轉換則會指派給存在於視覺狀態外部控制項的屬性，而且會影響這些狀態之間的轉換。 許多 Windows 執行階段 XAML 控制項在屬於其控制項範本一部分的腳本內，都包含佈景主題動畫以及由視覺狀態觸發的動畫。 只要您沒有修改範本，您所擁有的這些內建佈景主題動畫就可供您 UI 中的控制項使用。 不過，如果您取代了範本，則也將移除內建的控制項佈景主題動畫。 若要回復這些佈景主題動畫，您必須定義一個腳本，在控制項的這組視覺狀態內包含佈景主題動畫。 您也可以從不在視覺狀態內的腳本執行佈景主題動畫，並使用 [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491) 方法啟動這些佈景主題動畫，但這比較少見。 佈景主題動畫是動畫庫的一部分。
-   當控制項從某個已定義的視覺狀態轉換成另一個狀態時，就會套用*視覺轉換*。 它們是您撰寫的自訂動畫，通常與您針對控制項撰寫的自訂範本以及該範本內的視覺狀態定義有關。 這個動畫只會在狀態與狀態之間執行；執行時間通常很短，頂多只有幾秒鐘。 如需詳細資訊，請參閱[視覺狀態的腳本動畫的 "VisualTransition" 區段](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808#VisualTransition)。
-   *腳本動畫*會隨著時間變更 Windows 執行階段相依性屬性的值。 腳本可定義為視覺轉換的一部分，或是在執行階段由應用程式所觸發。 如需詳細資訊，請參閱[腳本動畫](storyboarded-animations.md)。 如需相依性屬性及其存在位置的詳細資訊，請參閱[相依性屬性概觀](https://msdn.microsoft.com/library/windows/apps/Mt185583)。
-   全新 [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) API 提供的*連接動畫*，讓開發人員輕鬆就能建立元素在瀏覽期間顯示於檢視之間的動畫效果。 Windows 10 版本 1607 起可以取得此 API。 如需詳細資訊，請參閱 [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx)。

## <a name="animations-available-in-the-library"></a>動畫庫中可用的動畫

動畫庫提供下列動畫。 按一下動畫名稱，以深入了解動畫的主要使用狀況、定義動畫的方式，以及查看動畫的範例。

-   [頁面轉換](#page-transition)：在[**畫面**](https://msdn.microsoft.com/library/windows/apps/br242682)中以動畫方式進行頁面轉換。
-   [內容和入場轉換](#content-transition-and-entrance-transition)：讓一個或一組內容以動畫方式進入或離開檢視。
-   [淡入/淡出以及淡入與淡出](#fade-in-out-and-crossfade)：顯示暫時性元素或控制項，或重新整理內容區域。
-   [指標向上/向下](#pointer-up-down)：在點選或按一下磚時提供的視覺化回饋。
-   [重新定位](#reposition)：將元素移至新位置。
-   [顯示/隱藏快顯](#show-hide-popup)：在檢視頂端顯示與內容相關的 UI。
-   [顯示/隱藏邊緣 UI](#show-hide-edge-ui)：將以邊緣為基礎的 UI (包括大型 UI，如面板) 滑入或滑出檢視。
-   [清單項目變更](#list-item-changes)：從清單中新增或刪除某個項目，或為這些項目重新排序。
-   [拖放](#drag-drop)：在拖放操作期間提供視覺化回饋。

### <a name="page-transition"></a>頁面轉換

使用頁面轉換在 App 中產生動畫瀏覽效果。 由於絕大部分的 App 皆使用某種類型的瀏覽，因此頁面轉化動畫是 App 最常使用的佈景主題動畫類型。 如需關於頁面轉換 API 的詳細資訊，請參閱 [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition)。



### <a name="content-transition-and-entrance-transition"></a>內容轉換和入場轉換

使用內容轉換動畫 ([**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103))，將內容片段或一組內容移入或移出目前的檢視。 例如，內容轉換動畫會顯示最初載入頁面時或變更頁面區段內容時，尚未準備顯示的內容。

[**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 代表先載入頁面或大型 UI 區段時可套用至內容的動作。 因此在內容第一次出現時，可以提供與內容變更不同的回饋。 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 等同於具預設參數的 [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition)，但可在[**畫面**](https://msdn.microsoft.com/library/windows/apps/br242682)之外使用。
 
 
<span id="fade-in-out-and-crossfade"/>

### <a name="fade-inout-and-crossfade"></a>淡入/淡出以及淡入與淡出

使用淡入動畫和淡出動畫來顯示或隱藏暫時性 UI 或控制項。 在 XAML 中，這些以 [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) 表示。 其中一個範例就是新控制項因為使用者互動而出現在應用程式列中。 另一個範例則是經過一段時間未偵測到使用者輸入而淡出的暫時性捲軸或移動瀏覽指標。 應用程式也應該在預留位置項目隨著內容動態載入而轉換成最終項目時，使用淡入動畫。

使用淡入與淡出動畫，在項目狀態變更 (例如應用程式重新整理檢視的目前內容) 時順暢轉換。 XAML 動畫庫不提供專用的淡入與淡出動畫 (與 [**crossFade**](https://msdn.microsoft.com/library/windows/apps/BR212661) 不同)，但是您可以使用具有重疊時間的 [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) 達到相同的結果。

<span id="pointer-up-down"/>

### <a name="pointer-updown"></a>指標向上/向下

使用 [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) 和 [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) 動畫，在順利點選或按一下磚時提供使用者回饋。 例如，在使用者按一下或點選磚時，播放指標向下動畫。 只要放開按一下或點選的動作，則播放指標向上動畫。

### <a name="reposition"></a>調整位置

使用重新定位動畫 ([**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) 或 [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429)) 將元素移入新位置。 例如，在項目控制項中移動標頭即可使用重新定位動畫。

<span id="show-hide-popup"/>

### <a name="showhide-popup"></a>顯示/隱藏快顯

當您在目前檢視頂端顯示和隱藏 [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) 或類似與內容相關的 UI 時，請使用 [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) 和 [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391)。 如果您要消失關閉快顯，[**PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) 是一個屬於實用的回饋的佈景主題轉換。

<span id="show-hide-edge-ui"/>

### <a name="showhide-edge-ui"></a>顯示/隱藏邊緣 UI

使用 [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324) 動畫，將以邊緣為基礎的小型 UI 滑入和滑出檢視。 例如，當您在螢幕頂端或螢幕底端顯示自訂應用程式列，或在螢幕頂端顯示錯誤及警告的 UI 表面時，就可以使用這些動畫。

使用 [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) 動畫顯示和隱藏窗格或面板。 這是供以邊緣為基礎的大型 UI 使用，例如自訂鍵盤或工作窗格。

### <a name="list-item-changes"></a>清單項目變更

當您在現有的清單中新增或刪除某個項目時，使用 [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) 動畫新增動畫行為。 若要新增，轉換會先重新定位清單中的現有項目，為新項目挪出空間，然後再新增新項目。 若要刪除，轉換會從清單中移除項目，移除要刪除的項目之後，便會視需要重新定位剩餘的清單項目。

如果清單中的項目變更位置，您也可以套用個別的 [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409)。 此動畫效果產生的方式與刪除某個項目，然後再以相關的刪除/新增動畫，將其新增在新位置的方式不同。

請注意，這些動畫已包含於預設的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) 範本，因此您無須手動新增這些動畫 (若您已使用這些控制項)。

<span id="drag-drop"/>

### <a name="dragdrop"></a>拖放

使用拖曳動畫 ([**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173)、[**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177)) 和放下動畫 ([**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185))，在使用者拖曳或放下項目時提供視覺化回饋。

作用時，這些動畫會為使用者示範清單可以在放下的項目周圍重新排列。 這有助於使用者了解如果在目前的位置放下項目之後，該項目會放置在清單何處。 如果某個拖曳的項目可以放在清單中的兩個其他項目之間，且這兩個項目會讓開，這些動畫會提供視覺化回饋。

## <a name="using-animations-with-custom-controls"></a>使用動畫搭配自訂控制項

下表摘要說明當您建立這些 Windows 執行階段控制項的自訂版本時，應該使用的動畫建議：

| UI 類型 | 建議的動畫 |
|---------|-----------------------|
| 對話方塊 | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| 飛出視窗 | [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) 和 [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| 工具提示 | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| 操作功能表 | [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) 和 [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| 命令列 | [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| 工作窗格或以邊緣為基礎的面板 | [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| 任何 UI 容器的內容 | [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| 用於控制項，或者沒有其他適用的動畫 | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation.aspx) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |

 

## <a name="transition-animation-examples"></a>轉換動畫範例

在理想的情況下，app 會使用動畫來增強使用者介面，或使它變得更具吸引力且不會造成使用者的困擾。 若要達到這樣的效果，您可以將具動畫效果的轉換套用至 UI，如此一來，當項目進入或離開畫面，甚至是產生變更時，動畫都能將使用者的注意力轉移到變更上。 例如，您的按鈕可能會快速淡入或淡出檢視，而不只是出現或消失而已。 我們建立許多 API，可用來建立一致的建議或典型動畫轉換。 下列範例顯示如何將動畫套用至按鈕，使其能夠迅速滑入檢視中。

```xml
<Button Content="Transitioning Button">
     <Button.Transitions>
         <TransitionCollection> 
             <EntranceThemeTransition/>
         </TransitionCollection>
     </Button.Transitions>
 </Button>
 ```

在此程式碼中，我們將 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 物件新增至按鈕的轉換集合中。 現在，當按鈕首次呈現時，會迅速地滑入檢視中，而不只是出現而已。 您可以在動畫物件上設定一些屬性，以便調整物件滑入的距離及方向，但是我們真正要做的是針對特定狀態建立一個簡單的 API，也就是建立一個搶眼的進入方式。

您也可以在應用程式的樣式資源中定義轉換動畫佈景主題，以便套用一致的效果。 此範例與前一個範例相同，唯一的差異在於它是使用 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) 來套用：

```xml
<UserControl.Resources>
     <Style x:Key="DefaultButtonStyle" TargetType="Button">
         <Setter Property="Transitions">
             <Setter.Value>
                 <TransitionCollection>
                     <EntranceThemeTransition/>
                 </TransitionCollection>
             </Setter.Value>
        </Setter>
    </Style>
</UserControl.Resources>
      
<StackPanel x:Name="LayoutRoot">
    <Button Style="{StaticResource DefaultButtonStyle}" Content="Transitioning Button"/>
</StackPanel>
```

前一個範例是在個別控制項套用佈景主題轉換，但是如果您將佈景主題轉換套用至物件的容器，會更加有趣美觀。 這樣做的話，容器中的所有子物件都會參與轉換。 在下列範例中，會將 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 套用至矩形的 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)。

```xml
<!-- If you set an EntranceThemeTransition animation on a panel, the
     children of the panel will automatically offset when they animate
     into view to create a visually appealing entrance. -->        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
            <EntranceThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- The sequence children appear depends on their order in 
         the panel's children, not necessarily on where they render
         on the screen. Be sure to arrange your child elements in
         the order you want them to transition into view. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

[**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 的子項矩形會以賞心悅目的方式逐一轉換至檢視中，而不像在將這個動畫個別套用至各個矩形時一次顯示所有子項矩形。

以下是此動畫的示範：

![顯示子項矩形轉換到檢視的動畫](./images/animation-child-rectangles.gif)

當一或多個容器的子物件變更位置時，這些子物件也會重新流動到新的位置。 在下列範例中，我們會將 [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) 套用至矩形的格線。 當您移除其中一個矩形時，所有其他矩形都會重新流動到新的位置。

```xml
<Button Content="Remove Rectangle" Click="RemoveButton_Click"/>
        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
                    
            <!-- Without this, there would be no animation when items 
                 are removed. -->
            <RepositionThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- All these rectangles are just to demonstrate how the items
         in the grid re-flow into position when one of the child items
         are removed. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

```cs
private void RemoveButton_Click(object sender, RoutedEventArgs e)
{
    if (rectangleItems.Items.Count > 0)
    {    
        rectangleItems.Items.RemoveAt(0);
    }                         
}
```

```cpp
// .h
private:
void RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);

//.cpp
void BlankPage::RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    if (rectangleItems->Items->Size > 0)
    {    
        rectangleItems->Items->RemoveAt(0);
    }
}
```

您可以在單一物件或物件容器套用多個轉換動畫。 例如，如果您想要讓一組矩形以動畫方式進入檢視，同時還要在它們變更位置時產生動畫效果，您可以套用 [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) 和 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288)，如下所示：

```xml
...
<ItemsControl.ItemContainerTransitions>
    <TransitionCollection>
        <EntranceThemeTransition/>                    
        <RepositionThemeTransition/>
    </TransitionCollection>
</ItemsControl.ItemContainerTransitions>
...      
```

您可以使用數種轉場效果，透過新增、移除、重新排序等動作在 UI 元素上建立動畫。 這些 API 的名稱都包含 "ThemeTransition"：

| API | 說明 |
|-----|-------------|
| [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition) | 針對[**畫面**](https://msdn.microsoft.com/library/windows/apps/br242682)中的頁面瀏覽，提供 Windows 個人化動畫效果。 |
| [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) | 提供控制項新增或刪除子項或內容時的動畫轉換行為。 通常控制項就是一個項目容器。 |
| [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103) | 提供控制項內容變更時的動畫轉換行為。 除了 [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) 之外，您也可以套用此項目。 |
| [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324) | 為 (小) 邊緣 UI 轉換提供動畫轉換行為。 |
| [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) | 提供控制項首次出現時的動畫轉換行為。 |
| [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) | 為面板 (大邊緣 UI) UI 轉換提供動畫轉換行為。 |
| [**PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) | 提供控制項的彈入元件出現時套用的動畫轉換行為 (例如，物件類似工具提示的 UI)。 |
| [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409) | 提供清單檢視控制項項目變更順序時的動畫轉換行為。 通常會在拖放作業時產生這種情況。 不同的控制項和佈景主題會具備不同的動畫特性。 |
| [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) | 提供控制項變更位置時的動畫轉換行為。 |

 

## <a name="theme-animation-examples"></a>佈景主題動畫範例

轉換動畫的套用方法很簡單。 不過，您可能想要對動畫效果的時間和順序進行更多的控制。 您可以使用佈景主題動畫來取得更多控制，同時仍能針對動畫的行為方式使用一致的佈景主題。 佈景主題動畫所需的標記也比自訂動畫的標記來得少。 我們將在這裡使用 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302)，讓矩形淡出檢視。

```xml
<StackPanel>    
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <FadeOutThemeAnimation TargetName="myRectangle" />
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle PointerPressed="Rectangle_Tapped" x:Name="myRectangle"  
              Fill="Blue" Width="200" Height="300"/>
</StackPanel>
```

```cs
// When the user taps the rectangle, the animation begins.
private void Rectangle_Tapped(object sender, PointerRoutedEventArgs e)
{
    myStoryboard.Begin();
}
```

```vb
' When the user taps the rectangle, the animation begins.
Private Sub Rectangle_Tapped(sender As Object, e As PointerRoutedEventArgs)
    myStoryboard.Begin()
End Sub
```

```cpp
//.h
void Rectangle_Tapped(Platform::Object^ sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs^ e);

//.cpp
void BlankPage::Rectangle_Tapped(Object^ sender, PointerRoutedEventArgs^ e)
{
    myStoryboard->Begin();
}
```

與轉換動畫不同，佈景主題動畫沒有自動執行的內建觸發程序 (轉換)。 當您以 XAML 定義佈景主題動畫時，必須使用 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 以包含該佈景主題動畫。 您也可以變更動畫的預設行為。 例如，可藉由提高 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) 上的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 時間值，使淡出的速度變慢。

**注意：** 基於說明基本動畫技術的目的，我們使用應用程式程式碼來啟動動畫，藉由呼叫[**腳本**](https://msdn.microsoft.com/library/windows/apps/BR210490)的方法。 您可以使用 [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491)、[**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop)、[**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) 與 [**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) **Storyboard** 方法，控制如何執行 **Storyboard** 動畫。 不過，那通常不是您將動畫庫加入應用程式的方法。 但是，您經常要將動畫庫整合到套用至控制項或元素的 XAML 樣式和範本中。 了解範本和視覺狀態有一點複雜。 但是我們的確涵蓋了您在視覺狀態中使用動畫庫的方式，做為[視覺狀態的腳本動畫](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)主題的一部分。

 

您可以在 UI 元素套用數個其他佈景主題動畫，以建立動畫效果。 這些 API 的名稱都包含 "ThemeAnimation"：

| API | 說明 |
|-----|-------------|
| [**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173) | 表示可套用至要拖曳之項目元素的預先設定動畫。 |
| [**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177) | 表示可套用至要拖曳之元素下方元素的預先設定動畫。 |
| [**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185) | 可套用至可能之拖放目標元素的預先設定動畫。 |
| [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) | 在控制項首次出現時套用的預先設定不透明動畫。 |
| [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) | 從 UI 移除或隱藏控制項時套用的預先設定不透明動畫。 |
| [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) | 使用者點選或按一下項目或元素時的預先設定動畫。 |
| [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) | 使用者點選項目或元素然後動作釋放之後所執行的使用者動作預先設定動畫。 |
| [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) | 控制項的彈入元件出現時套用的預先設定動畫。 此動畫結合了不透明和轉譯。 |
| [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391) | 控制項的彈入元件關閉或移除時套用的預先設定動畫。 此動畫結合了不透明和轉譯。 |
| [**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) | 重新放置物件時套用的預先設定動畫。 |
| [**SplitCloseThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210454) | 隱藏目標 UI 的預先設定動畫，會使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) 開啟與關閉樣式中的動畫。 |
| [**SplitOpenThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210472) | 顯示目標 UI 的預先設定動畫，會使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) 開啟與關閉樣式中的動畫。 |
| [**DrillInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.drillinthemeanimation) | 顯示使用者在邏輯階層中正向瀏覽時執行的預先設定動畫，例如從主要頁面瀏覽至詳細資料頁面。 |
| [**DrillOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.drilloutthemeanimation.aspx) | 顯示使用者在邏輯階層中反向瀏覽時執行的預先設定動畫，例如從詳細資料頁面瀏覽至主要頁面。 |

 

## <a name="create-your-own-animations"></a>建立自己的動畫

當佈景主題動畫無法滿足您的需求時，您可以建立自己的動畫。 您是透過設定一或多個物件屬性值的動畫效果，讓物件產生動畫效果。 例如，您可以讓矩形的寬度、[**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 的角度或按鈕的色彩值產生動畫效果。 我們將這類型的自訂動畫術語定義為腳本動畫，用以區分 Windows 執行階段已經提供為預先設定之動畫類型的動畫庫。 對於腳本動畫，您要使用可以變更特定類型值 (例如可為 **Double** 建立動畫效果的 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)) 的動畫，並將該動畫放在 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 內以進行控制。

為了建立動畫效果，要產生動畫的屬性必須是*相依性屬性*。 如需相依性屬性的詳細資訊，請參閱[相依性屬性概觀](https://msdn.microsoft.com/library/windows/apps/Mt185583)。 如需建立自訂腳本動畫 (包括如何做為目標並加以控制) 的詳細資訊，請參閱[腳本動畫](storyboarded-animations.md)。

您以 XAML 定義控制項之視覺狀態的一個狀況是，您將定義自訂腳本動畫所在 XAML 中最大的應用程式 UI 定義區域。 這麼做的原因是，您要建立新的控制項類別，或是您要在其控制項範本中，為具有視覺狀態的現有控制項重新建立範本。 如需詳細資訊，請參閱[視覺狀態的腳本動畫](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。

 

 




