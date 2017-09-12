---
author: Jwmsft
Description: "使用樹狀檢視範例程式碼建立可展開的樹狀結構。"
title: "樹狀檢視"
label: Tree view
template: detail.hbs
ms.openlocfilehash: c7ad99d20fe30ea4b94ad62de45b3832aae3805e
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2017
---
# <a name="hierarchical-layout-with-treeview"></a>含有樹狀檢視的階層式版面配置
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

樹狀檢視是階層式清單樣式，具有包含巢狀項目的展開和折疊節點。 巢狀項目可以是額外的節點或一般清單項目。 您可以使用 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 建置樹狀檢視，以協助說明您 UI 中的資料夾結構或巢狀關係。

[樹狀檢視範例](http://go.microsoft.com/fwlink/?LinkId=785018)是使用 **ListView** 建置的參考實作。 它並不是獨立的控制項。 Microsoft Edge 瀏覽器 [我的最愛] 窗格中可看到的 [樹狀檢視] 就是使用此參考實作。

範例支援：
- N 層巢狀結構
- 展開/折疊節點
- 在樹狀檢視內拖放節點
- 內建協助工具

![參考範例中的樹狀檢視](images/tree-view-sample.png) | ![Edge 瀏覽器中的樹狀檢視](images/tree-view-edge.png)
-- | --
樹狀檢視參考範例 | Edge 瀏覽器中的樹狀檢視

## <a name="is-this-the-right-pattern"></a>這是正確的模式嗎？

- 當項目有巢狀清單項目時，以及如果向對等項目與節點協助說明項目的階層式關係很重要時，請使用樹狀檢視。

- 如果將項目的巢狀關係以醒目提示方式顯示不是優先考量，請避免使用樹狀檢視。 對於大部分深入案例來說，適合使用一般清單檢視

## <a name="treeview-ui-structure"></a>樹狀檢視 UI 結構

您可以使用圖示來代表樹狀檢視中的節點。 縮排和圖示的組合可以用來代表資料夾/父節點和非資料夾/子節點之間的巢狀關係。 以下是如何執行這項操作的指導方針。

### <a name="icons"></a>圖示

請使用圖示來指示項目是一個節點，以及指示節點的狀態 (展開或折疊)。

#### <a name="chevron"></a>&gt; 形箭號

為了保持一致性，摺疊的節點應該使用 &gt; 形箭號指向右方，展開的節點應該使用 &gt; 形箭號指向下方。

![&gt; 形箭號圖示在樹狀檢視中的用法](images/treeview_chevron.png)

#### <a name="folder"></a>資料夾

請只將資料夾圖示用於代表資料夾。

![資料夾圖示在樹狀檢視中的用法](images/treeview_folder.png)

#### <a name="chevron-and-folder"></a>&gt; 形箭號和資料夾

&gt; 形箭號和資料夾的組合應該只在樹狀檢視中的非節點清單項目也有圖示時才使用。

![在樹狀檢視中一起使用 &gt; 形箭號和資料夾圖示的用法](images/treeview_chevron_folder.png)

#### <a name="redlines-for-indentation-of-folders-and-non-folder-nodes"></a>資料夾和非資料夾節點的縮排紅線

請將下面螢幕擷取畫面中的紅線用於資料夾和非資料夾節點的縮排。

![資料夾和非資料夾節點的縮排紅線](images/treeview_chevron_folder_indent_rl.png)

## <a name="building-a-treeview"></a>建置樹狀檢視

樹狀檢視有下列主要類別。 這些都是在參考實作中已經定義且包含的類別。

> **注意**&nbsp;&nbsp;樹狀檢視是實作為 [Windows 執行階段元件](https://msdn.microsoft.com/windows/uwp/winrt-components/index) (以 C++ 撰寫)，所以可以被任何語言的 UWP 應用程式參考。 在範例中，樹狀檢視程式碼是位於 *cpp/Control* 資料夾中。 C# 中並沒有相對應的 *cs/Control* 資料夾。

- `TreeNode` 類別會實作樹狀檢視的階層式版面配置。 它也會保留項目範本中將會與它繫結的資料。
- `TreeView` 類別會實作　ItemClick、展開/折疊資料夾，及拖曳初始化的事件。
- `TreeViewItem` 類別會實作放下作業的事件。
- `ViewModel` 類別會簡維 TreeViewItems 的清單，讓像是鍵盤瀏覽和拖放這樣的作業可以從 ListView　繼承。

## <a name="create-a-data-template-for-your-treeviewitem"></a>為您的 TreeViewItem 建立資料範本

以下是可以為資料夾和非資料夾類型項目設定資料範本的 XAML 區段。
- 若要將 ListViewItem 指定為資料夾，您需要在該 ListViewItem 上將 [AllowDrop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.allowdrop.aspx) 屬性明確設定為 **true**。 此 XAML 示範您可以這樣做的一種方式。
- 若要將 ListViewItem 指定為非資料夾，您不需要在 ListViewItem 本身指定任何屬性。 只要在 ListView 上將 AllowDrop 屬性設為 True 即可。
- 您可以使用已展開/已折疊資料夾圖示或 &gt; 形箭號，以視覺化方式指示資料夾已經展開或折疊。
- 您可以如這個範例中所示，使用轉換器來為展開和折疊狀態選擇所需的不同圖示。

```xaml
<!-- MainPage.xaml -->
<DataTemplate x:Key="TreeViewItemDataTemplate">
    <StackPanel Orientation="Horizontal" Height="40" Margin="{Binding Depth, Converter={StaticResource IntToIndConverter}}" AllowDrop="{Binding Data.IsFolder}">
        <FontIcon x:Name="expandCollapseChevron"
                  Glyph="{Binding IsExpanded, Converter={StaticResource expandCollapseGlyphConverter}}"
                  Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"                           
                  FontSize="12"
                  Margin="12,8,12,8"
                  FontFamily="Segoe MDL2 Assets"                          
                  />
        <Grid>
            <FontIcon x:Name ="expandCollapseFolder"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderGlyphConverter}}"
                      Foreground="#FFFFE793"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="nonFolderIcon"
                      Glyph="&#xE160;"
                      Foreground="{ThemeResource SystemControlForegroundBaseLowBrush}"
                      FontSize="12"
                      Margin="20,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource inverseBooleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="expandCollapseFolderOutline"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderOutlineGlyphConverter}}"
                      Foreground="#FFECC849"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"/>
        </Grid>

        <TextBlock Text="{Binding Data.Name}"
                   HorizontalAlignment="Stretch"
                   VerticalAlignment="Center"  
                   FontWeight="Medium"
                   FontFamily="Segoe MDL2 Assests"                           
                   Style="{ThemeResource BodyTextBlockStyle}"/>
    </StackPanel>
</DataTemplate>
```

## <a name="set-up-the-data-in-your-treeview"></a>設定您樹狀檢視中的資料

以下是設定樹狀檢視範例中資料的程式碼。

```csharp
 public MainPage()
 {
     this.InitializeComponent();

     TreeNode workFolder = CreateFolderNode("Work Documents");
     workFolder.Add(CreateFileNode("Feature Functional Spec"));
     workFolder.Add(CreateFileNode("Feature Schedule"));
     workFolder.Add(CreateFileNode("Overall Project Plan"));
     workFolder.Add(CreateFileNode("Feature Resource allocation"));
     sampleTreeView.RootNode.Add(workFolder);

     TreeNode remodelFolder = CreateFolderNode("Home Remodel");
     remodelFolder.IsExpanded = true;
     remodelFolder.Add(CreateFileNode("Contactor Contact Information"));
     remodelFolder.Add(CreateFileNode("Paint Color Scheme"));
     remodelFolder.Add(CreateFileNode("Flooring woodgrain types"));
     remodelFolder.Add(CreateFileNode("Kitchen cabinet styles"));

     TreeNode personalFolder = CreateFolderNode("Personal Documents");
     personalFolder.IsExpanded = true;
     personalFolder.Add(remodelFolder);

     sampleTreeView.RootNode.Add(personalFolder);
 }

 private static TreeNode CreateFileNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) };
 }

 private static TreeNode CreateFolderNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) { IsFolder = true } };
 }
```

在您完成上述步驟之後，您將擁有已完整填入的樹狀檢視/階層式版面配置，且具備 N 層巢狀，並支援展開/折疊資料夾、在資料夾之間拖放，以及內建協助工具。

若要提供使用者從樹狀檢視新增/移除項目的功能，我們建議您新增操作功能表來向使用者公開那些選項。


## <a name="related-articles"></a>相關文章

- [樹狀檢視範例](http://go.microsoft.com/fwlink/?LinkId=785018)
- [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView 與 GridView](listview-and-gridview.md)
