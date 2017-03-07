---
author: Jwmsft
Description: "搭配使用拖動重新整理模式和清單檢視。"
title: "拖動以重新整理"
label: Pull-to-refresh
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: e062ed2910e20ba187b8a0726a0061f0dd4b07f8
ms.lasthandoff: 02/08/2017

---
# <a name="pull-to-refresh"></a>拖動以重新整理

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

拖動以重新整理模式可讓使用者以觸控的方式將資料清單向下拖動以抓取更多資料。 拖動重新整理廣泛地用於行動裝置 App，且對任何配備觸控式螢幕的裝置都很實用。 您可以處理[操作事件](../input-and-devices/touch-interactions.md#manipulation-events)，以在 App 中實作拖動重新整理。

[拖動重新整理範例](http://go.microsoft.com/fwlink/p/?LinkId=620635) (英文) 示範如何延伸 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 控制項以支援此模式。 在本文中。我們使用這個範例說明實作拖動重新整理的要點。

![拖動重新整理範例](images/ptr-phone-1.png)

## <a name="is-this-the-right-pattern"></a>這是正確的模式嗎？

當您有使用者會想經常重新整理的資料清單或方格，且您的 App 應是在觸控為主的行動裝置上執行時，請使用拖動重新整理模式。

## <a name="implement-pull-to-refresh"></a>實作拖動重新整理

若要實作拖動重新整理，您需要處理操作事件，以偵測使用者向下拖動清單的時刻、提供視覺化回饋，並重新整理資料。 讓我們在[拖動重新整理範例](http://go.microsoft.com/fwlink/p/?LinkId=620635) (英文) 中看看如何實作。 我們沒有在這裡顯示所有的程式碼，所以您需要下載該範例，或在 GitHub 上檢視程式碼。

拖動重新整理範例會建立延伸 **ListView** 控制項的自訂控制項，其稱為 `RefreshableListView`。 這個控制項會加入重新整理指示器來提供視覺化回饋，並處理清單檢視的內部捲動檢視器上的操作事件。 它也會新增 2 個事件，以通知您拖動清單的時刻，以及應重新整理資料的時刻。 RefreshableListView 僅提供應重新整理資料的通知。 您需要在您的 App 中處理該事件以更新資料，且每個 App 的程式碼都不同。

RefreshableListView 提供「自動重新整理」模式，可判斷要求重新整理的時刻，及重新整理指示器如何離開檢視。 可開啟或關閉自動重新整理。
- 關閉：只有在超過 `PullThreshold` 的時候放開清單，才會要求重新整理。 當使用者放開捲動器時，指示器會以動畫方式離開檢視。 (在手機上) 如果狀態列指示器可供使用，則會顯示。
- 開啟：一超過 `PullThreshold` 就會重新整理，不論是否放開。 指示器會留在檢視中，直到已抓取新資料，然後才以動畫方式離開檢視。 當資料抓取完成時，會使用 **Deferral** 通知 App。

> **注意**&nbsp;&nbsp;範例中的程式碼也適用於 [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)。 若要修改 GridView，請自 GridView 衍生自訂類別，而不是 ListView，並修改預設的 GridView 範本。

## <a name="add-a-refresh-indicator"></a>新增重新整理指示器

請務必提供視覺化回饋，讓使用者知道 App 支援拖動重新整理。 RefreshableListView 的 `RefreshIndicatorContent` 屬性可讓您在 XAML 中設定指示器的視覺效果。 如果您未設定 `RefreshIndicatorContent`，則會改為使用其中包含的預設文字指示器。

以下是重新整理指示器的建議指導方針。

![重新整理指示器參考線](images/ptr-redlines-1.png)

**修改清單檢視範本**

在拖動重新整理範例中，`RefreshableListView` 控制項範本會透過新增重新整理指示器，來修改標準的 **ListView** 範本。 重新整理指示器是放在 [**Grid**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx) 中，且在 [**ItemsPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx) (顯示清單項目的部分) 上方。

> **注意**&nbsp;&nbsp;只有未設定 `RefreshIndicatorContent` 屬性時，`DefaultRefreshIndicatorContent` 文字方塊才會提供後援文字指示器。

以下是由預設 ListView 範本修改的控制項範本之部分。

**XAML**
```xaml
<!-- Styles/Styles.xaml -->
<Grid x:Name="ScrollerContent" VerticalAlignment="Top">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Border x:Name="RefreshIndicator" VerticalAlignment="Top" Grid.Row="1">
        <Grid>
            <TextBlock x:Name="DefaultRefreshIndicatorContent" HorizontalAlignment="Center" 
                       Foreground="White" FontSize="20" Margin="20, 35, 20, 20"/>
            <ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}"></ContentPresenter>
        </Grid>
    </Border>
    <ItemsPresenter FooterTransitions="{TemplateBinding FooterTransitions}" 
                    FooterTemplate="{TemplateBinding FooterTemplate}" 
                    Footer="{TemplateBinding Footer}" 
                    HeaderTemplate="{TemplateBinding HeaderTemplate}" 
                    Header="{TemplateBinding Header}" 
                    HeaderTransitions="{TemplateBinding HeaderTransitions}" 
                    Padding="{TemplateBinding Padding}"
                    Grid.Row="1"
                    x:Name="ItemsPresenter"/>
</Grid>
```

**在 XAML 中設定內容**

您會在 XAML 中設定清單檢視的重新整理指示器內容。 您設定的 XAML 內容是由重新整理指示器的 [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx) (`<ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}">`) 顯示。 如果您未設定此內容，則會顯示預設的文字指示器。

**XAML**
```xaml
<!-- MainPage.xaml -->
<c:RefreshableListView
    <!-- ... See sample for removed code. -->
    AutoRefresh="{x:Bind Path=UseAutoRefresh, Mode=OneWay}"
    ItemsSource="{x:Bind Items}"
    PullProgressChanged="listView_PullProgressChanged"
    RefreshRequested="listView_RefreshRequested">

    <c:RefreshableListView.RefreshIndicatorContent>
        <Grid Height="100" Background="Transparent">
            <FontIcon
                Margin="0,0,0,30"
                HorizontalAlignment="Center"
                VerticalAlignment="Bottom"
                FontFamily="Segoe MDL2 Assets"
                FontSize="20"
                Glyph="&#xE72C;"
                RenderTransformOrigin="0.5,0.5">
                <FontIcon.RenderTransform>
                    <RotateTransform x:Name="SpinnerTransform" Angle="0" />
                </FontIcon.RenderTransform>
            </FontIcon>
        </Grid>
    </c:RefreshableListView.RefreshIndicatorContent>
    
    <!-- ... See sample for removed code. -->

</c:RefreshableListView>
```

**動畫顯示旋轉指示器**

將清單向下拖動之後，RefreshableListView 的 `PullProgressChanged` 事件會發生。 您會在您的 App 中處理這個事件，以控制重新整理指示器。 在此範例中，這個腳本即開始以動畫顯示指示器的 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.aspx)，並旋轉重新整理指示器。 

**XAML**
```xaml
<!-- MainPage.xaml -->
<Storyboard x:Name="SpinnerStoryboard">
    <DoubleAnimation
        Duration="00:00:00.5"
        FillBehavior="HoldEnd"
        From="0"
        RepeatBehavior="Forever"
        Storyboard.TargetName="SpinnerTransform"
        Storyboard.TargetProperty="Angle"
        To="360" />
</Storyboard>
```

## <a name="handle-scroll-viewer-manipulation-events"></a>處理捲動檢視器操作事件

清單檢視控制項範本中包含內建的 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)，可讓使用者捲動瀏覽清單項目。 若要實作拖動重新整理，您必須處理內建捲動檢視器上的操作事件，和數個相關事件。 如需操作事件的詳細資訊，請參閱[觸控互動](../input-and-devices/touch-interactions.md)。

** OnApplyTemplate**

您必須覆寫 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.onapplytemplate.aspx) 方法，才能存取捲動檢視器及其他範本組件，以新增事件處理常式並稍後於程式碼中呼叫它們。 在 OnApplyTemplate 中，呼叫 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.gettemplatechild.aspx)，以取得控制項範本中已命名部分的參考，並可將它儲存到您的程式碼中以稍後使用。

在範例中，用來儲存範本組件的變數是在 Private Variable (私用變數) 區域中宣告。 在 OnApplyTemplate 方法中抓取它們之後，會針對 [**DirectManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationstarted.aspx)、[**DirectManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationcompleted.aspx)、[**ViewChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.viewchanged.aspx) 和 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 事件新增事件處理常式。

**DirectManipulationStarted**

為了初始化拖動重新整理動作，當使用者向下拖動時，內容必須已捲動到捲動檢視器頂端。 否則，會假設使用者拖動是要在清單中向上移動瀏覽。 這個處理常式中的程式碼會判斷是否要從捲動檢視器頂端的內容開始操作，並且最後能使清單被重新整理。 控制項的「可重新整理」(refreshable) 狀態是據此來設定。 

如果可以重新整理該控制項，則也會新增動畫的事件處理常式。

**DirectManipulationCompleted**

當使用者停止將清單向下拖動時，這個處理常式中的程式碼會檢查操作期間是否有啟動重新整理。 如果有啟動重新整理，就會引發 `RefreshRequested` 事件並執行 `RefreshCommand` 命令。

也會移除動畫的事件處理常式。

根據 `AutoRefresh` 屬性的值而定，清單可立即以動畫方式回到上方，或等候重新整理完成再以動畫方式回到上方。 會使用 [**Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 物件來標示重新整理完成的狀態。 屆時會隱藏重新整理指示器 UI。

這部分的 DirectManipulationCompleted 事件處理常式會引發 `RefreshRequested` 事件，並在需要時取得 Deferral。

**C#**
```csharp
if (this.RefreshRequested != null)
{
    RefreshRequestedEventArgs refreshRequestedEventArgs = new RefreshRequestedEventArgs(
        this.AutoRefresh ? new DeferralCompletedHandler(RefreshCompleted) : null);
    this.RefreshRequested(this, refreshRequestedEventArgs);
    if (this.AutoRefresh)
    {
        m_scrollerContent.ManipulationMode = ManipulationModes.None;
        if (!refreshRequestedEventArgs.WasDeferralRetrieved)
        {
            // The Deferral object was not retrieved in the event handler.
            // Animate the content up right away.
            this.RefreshCompleted();
        }
    }
}
```

**ViewChanged**

有兩種案例是在 ViewChanged 事件處理常式中處理。

第一種，如果檢視因為捲動檢視器縮放而變更，則會取消控制器的「可重新整理」狀態。

第二種，如果在自動重新整理結束時，內容已由動畫方式回到上方，則會隱藏填補的矩形、重新啟用與捲動檢視器的互動，且將 [VerticalOffset](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticaloffset.aspx) 設為 0。

**PointerPressed**

只有當清單是以觸控操作向下拖動時，才會發生拖動重新整理。 在 PointerPressed 事件處理常式中，程式碼會檢查造成事件的是何種指標，並設定變數 (`m_pointerPressed`) 以指示是否為觸控指標。 這個變數用於 DirectManipulationStarted 處理常式。 如果指標不是觸控指標，則傳回的 DirectManipulationStarted 不會執行任何事。

## <a name="add-pull-and-refresh-events"></a>新增拖動和重新整理事件

'RefreshableListView' 會新增 2 個您可以在 App 中處理的事件，以重新整理資料及管理重新整理指示器。

如需事件的詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)。

**RefreshRequested**

若使用者已經拖動清單來重新整理，'RefreshRequested' 事件會通知您的 App。 您需處理這個事件以擷取新資料並更新清單。

以下是範例中的事件處理常式。 請注意重要的一點，它會檢查清單檢視的 `AutoRefresh` 屬性，如果為 **true**，就會取得 Deferral。 因為有 Deferral，直到重新整理完成時，重新整理指示器才會停止並隱藏。

**C#**
```csharp
private async void listView_RefreshRequested(object sender, RefreshableListView.RefreshRequestedEventArgs e)
{
    using (Deferral deferral = listView.AutoRefresh ? e.GetDeferral() : null)
    {
        await FetchAndInsertItemsAsync(_rand.Next(1, 5));

        if (SpinnerStoryboard.GetCurrentState() != Windows.UI.Xaml.Media.Animation.ClockState.Stopped)
        {
            SpinnerStoryboard.Stop();
        }
    }
}
```

**PullProgressChanged**

在範例中，重新整理指示器的內容是由 App 提供及控制。 當使用者拖動清單時，'PullProgressChanged' 事件會通知您的 App，讓您可以啟動、停止及重設重新整理指示器。 

## <a name="composition-animations"></a>組合動畫

根據預設，捲動檢視器中的內容會在捲軸到達頂端時停止。 為了讓使用者繼續向下拖動清單，您需要存取視覺層，並以動畫方式顯示清單內容。 此範例是使用[組合動畫](https://msdn.microsoft.com/windows/uwp/graphics/composition-animation)這麼做，更明確地說，是 [Expression 動畫](https://msdn.microsoft.com/windows/uwp/graphics/composition-animation#expression-animations)。

在範例中，這項工作主要是在 `CompositionTarget_Rendering` 事件和 `UpdateCompositionAnimations` 方法中完成。

## <a name="related-articles"></a>相關文章

- [設定控制項的樣式](styling-controls.md)
- [觸控互動](../input-and-devices/touch-interactions.md)
- [清單檢視和方格檢視](listview-and-gridview.md)
- [清單檢視項目範本](listview-item-templates.md)
- [Expression 動畫](https://msdn.microsoft.com/windows/uwp/graphics/composition-animation#expression-animations)
