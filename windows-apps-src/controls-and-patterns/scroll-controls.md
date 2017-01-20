---
author: Jwmsft
Description: "移動瀏覽和捲動可讓使用者到達超出螢幕界限的內容。"
title: "捲軸的指導方針"
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scroll bars
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 8ead56e84e21aaf5005530ed0509efa9440bce59

---
# <a name="scroll-bars"></a>捲軸

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

移動瀏覽和捲動可讓使用者到達超出螢幕界限的內容。

捲動檢視器控制項能夠盡可能在檢視區內容納許多內容，以及一或兩個捲軸。 觸控手勢可用來移動瀏覽和縮放 (捲軸只在操作期間淡入)，而指標可用來捲動。 撥動手勢會依慣性移動瀏覽。

**注意** Windows 有兩種捲動器視覺效果，並會根據使用者的輸入模式而定︰捲動指標適合在使用觸控板或控制器時使用，而使用滑鼠、鍵盤及手寫筆等其他輸入裝置時，則適合使用互動式捲軸。

![標準捲軸和移動瀏覽指標控制項外觀的範例](images/SCROLLBAR.png)

<div class="microsoft-internal-note">
請參閱 [Design Depot](http://designdepot/DesignDepot.FrontEnd/#/ML/Dashboard/1805) 中的完整紅線
</div>

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**ScrollViewer 類別**](https://msdn.microsoft.com/library/windows/apps/br209527)</li>
<li>[**ScrollBar 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.scrollbar.aspx)</li>
</ul>
</div>


## <a name="examples"></a>範例

[**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) 可讓內容顯示在小於其實際大小的區域中。 無法完整看到捲動檢視器的內容時，捲動檢視器就會顯示使用者可以用來移動可見內容區域的捲軸。 包含所有捲動檢視器內容的區域是 *extent*。 內容的可見區域是 *viewport*。

![說明標準捲軸控制項的螢幕擷取畫面](images/ScrollBar_Standard.jpg)

## <a name="create-a-scroll-viewer"></a>建立捲動檢視器
若要將垂直捲動功能新增至頁面，請將頁面內容包裝在捲動檢視器中。

```xaml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1">

    <ScrollViewer>
        <StackPanel>
            <TextBlock Text="My Page Title" Style="{StaticResource TitleTextBlockStyle}"/>
            <!-- more page content -->
        </StackPanel>
    </ScrollViewer>
</Page>
```
此 XAML 說明如何將影像放在捲動檢視器中，並啟用縮放功能。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## <a name="scrollviewer-in-a-control-template"></a>控制項範本中的 ScrollViewer

對於 ScrollViewer 控制項而言，當做其他控制項的複合部分存在很獨特。 只有在主控制項的配置空間限制小於展開的內容大小時，ScrollViewer 組件以及用於支援的 [**ScrollContentPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollcontentpresenter.aspx) 類別才會將檢視區連同捲軸一起顯示。 這種情況通常發生在清單中，因此 [**ListView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 範本一律包含 ScrollViewer。 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 和 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 也在其範本中包含一個 ScrollViewer。

當 **ScrollViewer** 部分存在於控制項中時，主控制項對於讓內容捲動的某些輸入事件以及操作，通常都有內建的事件處理方式。 例如，GridView 會解譯撥動手勢，這會導致水平捲動內容。 主控制項所收到的輸入事件與原始操作都會被視為由控制項處理，因此將不會引發較低層級的事件 (例如 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx))，也不會產生任何父容器。 您可以透過覆寫事件的控制項類別和 **On*** 虛擬方法，或透過重製控制項的範本，來變更部分內建的控制項處理。 但在任一種情況下，重現通常就在那裡的原始預設行為，讓控制項以預期的方式回應事件和使用者的輸入動作與手勢並不容易。 因此，您應該考慮您是否真的需要引發該輸入事件。 您可能會想要調查是否有不是由控制項處理的其他輸入事件或手勢，並在您的 app 中使用那些輸入事件或手勢，或控制互動設計。

為了能夠讓包含 ScrollViewer 的控制項影響來自 ScrollViewer 組件內的部分行為和屬性，ScrollViewer 會定義一些可以在樣式中設定，並在範本繫結中使用的 XAML 附加屬性。 如需有關附加屬性的詳細資訊，請參閱[附加屬性概觀](../xaml-platform/attached-properties-overview.md)。

**ScrollViewer XAML 附加屬性**

ScrollViewer 會定義下列 XAML 附加屬性︰
- [ScrollViewer.BringIntoViewOnFocusChange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange.aspx)
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx)
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx)
- [ScrollViewer.IsDeferredScrollingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled.aspx)
- [ScrollViewer.IsHorizontalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled.aspx)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled.aspx)
- [ScrollViewer.IsScrollInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled.aspx)
- [ScrollViewer.IsVerticalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled.aspx)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled.aspx)
- [ScrollViewer.IsZoomChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.IsZoomInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty.aspx)
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx)
- [ScrollViewer.ZoomMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

這些 XAML 附加屬性適用於 ScrollViewer 是隱含的情況，例如當 ScrollViewer 存在於 ListView 或 GridView 的預設範本中，而且您希望能夠在不存取範本組件的情況下，影響控制項的捲動行為時。

例如，以下是如何永遠為 ListView 的內建捲動檢視器顯示垂直捲軸。
```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

對於 ScrollViewer 在 XAML 中是明確的情況下 (如範例程式碼中所示)，您不需要使用附加屬性語法。 只要使用屬性語法，例如 `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`。


## <a name="dos-and-donts"></a>可行與禁止事項

-   可以的話，請採用垂直捲動的設計，而不是水平捲動。
-   對延長超過一個檢視區界限 (垂直或水平) 的內容區域，使用單軸移動瀏覽。 對延長超過兩個檢視區界限 (垂直和水平) 的內容區域，使用雙軸移動瀏覽。
-   在清單檢視、方格檢視、下拉式方塊、清單方塊、文字輸入方塊及中樞控制項中，使用內建的捲動功能。 如果要一次全部顯示的項目數量太多，請使用上述控制項，讓使用者能夠在項目清單水平或垂直捲動。
-   如果您希望使用者能夠在大型區域雙向移動瀏覽，還能予以縮放，例如，您想讓使用者在完整大小的影像 (而不是根據畫面來調整影像大小) 移動瀏覽和縮放，請將影像放在捲動檢視器內。
-   如果使用者將捲動很冗長的文字訊息，請將捲動檢視器設定為只能垂直捲動。
-   使用捲動檢視器以只限包含一個物件。 請注意，這一個物件可以是配置面板，其本身可以包含數目不拘的物件。
-   請不要將 [Pivot](tabs-pivot.md) 控制項放入捲動檢視器內，以避免與樞紐的捲動邏輯發生衝突。

## <a name="related-topics"></a>相關主題

**適用於開發人員 (XAML)**
* [**ScrollViewer 類別**](https://msdn.microsoft.com/library/windows/apps/br209527)



<!--HONumber=Dec16_HO2-->


