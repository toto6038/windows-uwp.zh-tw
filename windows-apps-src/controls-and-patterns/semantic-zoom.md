---
author: Jwmsft
Description: "語意式縮放控制項可以讓使用者在相同資料集的兩個不同語意式檢視間縮放。"
title: "語意式縮放"
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 125cb6e45defe3213af3f5cd20f524a5311241af

---
# 語意式縮放

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

語意式縮放可讓使用者在相同內容的兩個不同檢視之間切換，這樣就能快速瀏覽大型分組資料集。
 
- 放大檢視是內容的主要檢視。 這是您顯示個別資料項目的主要檢視。 
- 縮小檢視是相同內容的高階檢視。 您通常會在此檢視中顯示分組資料集的群組標頭。 

例如，使用者檢視通訊錄時，可縮小以快速跳到字母 "W"，然後放大該字母以查看與其相關的名稱。 

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/hh702601"><strong>SemanticZoom 類別</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx"><strong>ListView 類別</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx"><strong>GridView 類別</strong></a></li>
</ul>

</div>
</div>






**功能**：

-   縮小檢視的大小受限於語意式縮放控制項的範圍。
-   點選群組標題會切換檢視。 可以啟用捏合在檢視之間切換。
-   使用中的標題可在檢視之間切換。

## 這是正確的控制項嗎？

當您需要顯示的分組資料集大到無法全部顯示在一或兩個頁面上時，請使用 **SemanticZoom** 控制項。

請勿混淆語意式縮放與視覺化縮放。 雖然它們共用相同的互動和基本行為 (根據縮放係數顯示較多或較少的詳細資料)，但視覺化縮放是指內容區域或物件 (例如相片) 的倍率調整。 如需執行視覺化縮放的控制項相關資訊，請參閱 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) 控制項。

## 範例

**相片 App**

以下是相片 app 中使用的語意式縮放。 相片是依月份分組。 選取預設方格檢視中的月份標頭時，會縮小月份清單檢視以供快速瀏覽。

![相片 app 中使用的語意式縮放](images/control-examples/semantic-zoom-photos.png)

**通訊錄**

通訊錄為另一個使用語意式縮放來更輕鬆地瀏覽資料集的範例。 您可以使用縮小檢視以快速跳到您需要的字母 (左側影像)，而放大檢視則會顯示個別的資料項目 (右側影像)。

![用於連絡人清單的語意式縮放範例](images/semanticzoom-win10.png)

## 建立語意式縮放

**SemanticZoom** 控制項不具有屬於自己的任何視覺化呈現方式。 它是可管理另外 2 個提供內容檢視的控制項 (通常是 **ListView** 或 **GridView** 控制項) 之間轉換的主控制項。  您會將檢視控制項設定為 SemanticZoom 的 [**ZoomedInView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedinview.aspx) 與 [**ZoomedOutView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedoutview.aspx) 屬性。

語意式縮放需要的 3 個元素為︰
- 分組的資料來源
- 可顯示項目層級資料的放大檢視。
- 可顯示群組層級資料的縮小檢視。

使用語意式縮放之前，您應該了解如何使用具有分組資料的清單檢視。 如需詳細資訊，請參閱[清單檢視與方格檢視](listview-and-gridview.md)和[在清單中將項目分組]()。 

> **注意**&nbsp;&nbsp;若要定義 SemanticZoom 控制項的放大檢視和縮小檢視，您可以使用實作 [**ISemanticZoomInformation**]() 介面的任兩個控制項。 XAML 架構提供 3 個實作此介面的控制項︰ListView、GridView 及 Hub。
 
 這個 XAML 會顯示 SemanticZoom 控制項的結構。 您會將其他控制項指派給 ZoomedInView 與 ZoomedOutView 屬性。
 
 **XAML**
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed out view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed in view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
這裡的範例取自 [XAML UI 基本知識範例](http://go.microsoft.com/fwlink/p/?LinkId=619992)的 SemanticZoom 頁面。 您可以下載範例以查看包括資料來源的完整程式碼。 此語意式縮放使用 GridView 來提供放大檢視，並使用 ListView 來提供縮小檢視。
  
**定義放大檢視**

以下是用於放大檢視的 GridView 控制項。 放大檢視應會顯示群組中的個別資料項目。 這個範例顯示如何在格線內以影像與文字顯示項目。 

**XAML**
```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
群組標頭的外觀是在 `ZoomedInGroupHeaderTemplate` 資源中定義。 項目的外觀是在 `ZoomedInTemplate` 資源中定義。 

**XAML**   
```xaml
<DataTemplate x:Key="" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**定義縮小檢視**

此 XAML 定義用於縮小檢視的 ListView 控制項。 這個範例說明如何在清單中將群組標頭顯示為文字。

**XAML**
```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 外觀是在 `ZoomedOutTemplate` 資源中定義。
 
 **XAML**   
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**同步檢視**

放大檢視與縮小檢視應該同步，讓使用者在縮小檢視中選取群組時，該群組的詳細資料能夠顯示在放大檢視中。 您可以使用 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) 或新增程式碼來同步檢視。

繫結到相同 CollectionViewSource 的任何控制項將永遠具有相同的目前項目。 如果兩種檢視都使用相同的 CollectionViewSource 做為資料來源，CollectionViewSource 就會自動同步檢視。 如需詳細資訊，請參閱 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)。

如果您不使用 CollectionViewSource 來同步檢視，則應該處理 [**ViewChangeStarted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.viewchangestarted.aspx) 事件，並在事件處理常式中同步項目，如下所示。

**XAML**
```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

**C#**
```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## 建議事項

-   在您的應用程式中使用語意式縮放時，請不要隨縮放層級變更項目配置和移動瀏覽方向。 配置和移動瀏覽互動在縮放比例之間應該保持一致且可預測。
-   語意式縮放可以讓使用者快速跳到內容，所以請將縮小檢視模式中頁面/畫面的數量限制為三個。 過多的移動瀏覽會減少語意式縮放的實用性。
-   請避免使用語意式縮放來變更內容的範圍。 例如，相簿不應該切換到 [檔案總管] 的資料夾檢視。
-   使用檢視所必要的結構和語意檢視。
-   使用群組集合中項目的群組名稱。
-   針對未分組但已排序的集合使用排序 (例如依時間順序排序日期，或依字母順序排序名稱清單)。

## 相關文章

- [瀏覽設計基本知識](../layout/navigation-basics.md)
- [清單檢視和方格檢視](listview-and-gridview.md)
- [清單檢視項目範本](listview-item-templates.md)

**範例**

- [XAML UI 基本知識範例](http://go.microsoft.com/fwlink/p/?LinkId=619992)

 







<!--HONumber=Aug16_HO3-->


