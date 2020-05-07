---
description: 您可以將 ItemsSource 繫結至階層式資料來源以建立可展開的樹狀檢視，也可以自行建立及管理 TreeViewNode 物件。
title: 樹狀檢視
label: Tree view
template: detail.hbs
ms.date: 07/24/2019
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: 68682d7b47e42995060601f5ae1c9b8d891aa3ff
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "81643705"
---
# <a name="treeview"></a>TreeView

XAML [TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview) 控制項啟用提供包含巢狀項目的展開及摺疊節點的階層式清單。 此控制項可以用來說明 UI 中的資料夾結構或巢狀關聯性。

**TreeView** API 支援下列功能：

- N 層巢狀結構
- 選取單一節點或多個節點
- 資料繫結至 **TreeView** 和 [TreeViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewitem) 的 **ItemsSource** 屬性
- **TreeViewItem** 作為 **TreeView** 項目範本的根
- **TreeViewItem** 中內容的任意類型
- 在樹狀檢視之間拖放

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | **TreeView** 控制項包含在 Windows UI 程式庫中；此程式庫是包含適用於 UWP 應用程式的新控制項和 UI 功能的 NuGet 封裝。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 程式庫 API：** [TreeView 類別](/uwp/api/microsoft.ui.xaml.controls.treeview)、[TreeViewNode 類別](/uwp/api/microsoft.ui.xaml.controls.treeviewnode)、[TreeView.ItemsSource 屬性](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource)
>
> **平台 API：** [TreeView 類別](/uwp/api/windows.ui.xaml.controls.treeview)、[TreeViewNode 類別](/uwp/api/windows.ui.xaml.controls.treeviewnode)、[TreeView.ItemsSource 屬性](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)

> [!TIP]
> 在這整份文件中，我們使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的 [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在後方的程式碼中，我們也使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

- 當項目有巢狀清單項目時，以及如果向對等項目與節點協助說明項目的階層式關係很重要時，請使用 **TreeView**。

- 如果將項目的巢狀關係以醒目提示方式顯示不是優先考量，請避免使用 **TreeView**。 對於大部分深入案例來說，適合使用一般清單檢視。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/TreeView">開啟應用程式並查看 TreeView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>TreeView UI

樹狀檢視使用縮排和圖示的組合來表示父節點和子節點之間的巢狀關係。 摺疊的節點使用＞形箭號指向右方，而展開的節點使用＞形箭號指向下方。

![TreeView 中的＞形箭號圖示](images/treeview-simple.png)

您可以在樹檢視項目資料範本中包含圖示來表示節點。 比方說，如果您顯示檔案系統階層，您可以將資料夾圖示用於父節點，以及將檔案圖示用於分葉節點。

![同在 TreeView 中的＞形箭號和資料夾圖示](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>建立樹狀檢視

您可以將 [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) 繫結至階層式資料來源以建立樹狀檢視，也可以自行建立及管理 **TreeViewNode** 物件。

若要建立樹狀檢視，請使用 [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 控制項和 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 物件階層。 您可以將一個或多個根節點新增至 **TreeView** 控制項的 [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) 集合，來建立節點階層。 然後，每個 **TreeViewNode** 都可以有更多新增至其 [Children](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewnode.children) 集合的節點。 您可以將樹狀檢視節點巢狀化到您需要的任何深度。

您可以將階層式資料來源繫結至 [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) 屬性，以提供樹狀檢視內容，就如同處理 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 的 **ItemsSource** 一樣。 同樣地，使用 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (及選擇性 [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate))，以提供可呈現項目的 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)。

> [!IMPORTANT]
> **ItemsSource** 及其相關 API 都需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk) 或更新版本)，或 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。
>
> **ItemsSource** 是 **TreeView.RootNodes** 用於將內容放入 **TreeView** 控制項的替代機制。 您無法同時設定 **ItemsSource** 與 **RootNodes**。 當您使用 **ItemsSource** 時，系統會為您建立節點，您可以從 **TreeView.RootNodes** 屬性存取它們。

以下是使用 XAML 宣告的簡單樹狀檢視範例。 您通常會在程式碼中新增節點，但我們在此顯示 XAML 階層，因為這對視覺化展示如何建立節點階層可能會有幫助。

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

在大部分情況下，樹狀檢視會顯示資料來源的資料，因此您通常在 XAML 中宣告根 **TreeView** 控制項，而在程式碼中新增 **TreeViewNode** 物件或使用資料繫結。

### <a name="bind-to-a-hierarchical-data-source"></a>繫結至階層式資料來源

若要使用資料繫結建立樹狀檢視，請設定 **TreeView.ItemsSource** 屬性的階層式集合。 然後在 **ItemTemplate** 中，設定 **TreeViewItem.ItemsSource** 屬性的子系項目集合。

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

請參閱[使用資料繫結的樹狀檢視](#tree-view-using-data-binding)以取得完整程式碼。

#### <a name="items-and-item-containers"></a>項目和項目容器

如果您使用 **TreeView.ItemsSource**，這些 API 可用於從容器中取得節點或資料項目，反之亦然。

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | 取得所指定 **TreeViewItem** 容器的資料項目。 |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | 取得所指定資料項目的 **TreeViewItem** 容器。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | 取得所指定 **TreeViewItem** 容器的 **TreeViewNode**。 |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | 取得所指定 **TreeViewNode** 的 **TreeViewItem** 容器。 |

### <a name="manage-tree-view-nodes"></a>管理樹狀檢視節點

此樹狀檢視與先前使用 XAML 建立的樹狀檢視相同，但節點是使用程式碼所建立。

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

這些 API 可用來管理樹狀檢視的資料階層。

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | 樹狀檢視可以有一或多個根節點。 將 **TreeViewNode** 物件新增至 **RootNodes** 集合以建立根節點。 根節點的 **Parent** 永遠為 **null**。 根節點的 **Depth** 為 0。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [子系](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | 將 **TreeViewNode** 物件新增至父節點的 **Children** 集合以建立您的節點階層。 節點是其 **Children** 集合中所有節點的 **Parent**。 |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | 如果節點有具現化的子系，則為 **true**。 **false** 表示空的資料夾或一個項目。 |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | 如果您正在節點展開時填滿節點，請使用此屬性。 請參閱本文稍後的＜[當節點正在展開時填滿節點](#fill-a-node-when-its-expanding)＞。 |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | 表示子節點距離根節點有多遠。 |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | 取得擁有此節點所屬 **Children** 集合的 **TreeViewNode**。 |

樹狀檢視使用 **HasChildren** 和 **HasUnrealizedChildren** 屬性來判斷展開/摺疊圖示是否已顯示。 如果任一屬性為 **true**，則圖示已顯示，否則未顯示。

## <a name="tree-view-node-content"></a>樹狀檢視節點內容

您可以將樹狀檢視所表示的資料項目儲在其 [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 屬性中。

在前一個範例中，內容是簡單的字串值。 在這裡，樹狀檢視節點表示使用者的 [圖片]  資料夾，因此圖片媒體櫃 [StorageFolder](/uwp/api/windows.storage.storagefolder) 已指派給節點的 **Content** 屬性。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> 若要取得 [圖片]  資料夾的存取權，您必須在應用程式資訊清單中指定**圖片媒體櫃**功能。 如需詳細資訊，請參閱[應用程式功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) \(部分機器翻譯\)。

您可以提供 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 來指定資料項目在樹狀檢視中的顯示方式。

> [!NOTE]
> 在 Windows 10 版本 1803 中，如果您的內容不是字串，就必須重新建立 **TreeView** 控制項的範本並指定自訂 **ItemTemplate**。 在更新版本中，設定 **ItemTemplate**屬性。 如需詳細資訊，請參閱 [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)。

### <a name="item-container-style"></a>項目容器樣式

不論您使用 **ItemsSource** 或 **RootNodes**，用來顯示每個節點的實際元素 (稱為「容器」) 是 [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) 物件。 您可以使用 **TreeView** 的 [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle) 或 [ItemContainerStyleSelector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector) 屬性，修改 **TreeViewItem** 屬性以設定容器的樣式。

這個範例示範如何將展開/摺疊的字符變更為橘色 +/- 符號。 在預設 **TreeViewItem**範本中，字符會設定為使用 `Segoe MDL2 Assets` 字型。 您可以設定 **Setter.Value** 屬性，作法是以 XAML 使用的格式提供 Unicode 字元值，如下所示：`Value="&#xE948;"`。

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>項目範本選取器

根據預設，[TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 會顯示每個節點的資料項目字串表示法。 您可以設定 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) 屬性來變更要針對所有節點顯示的內容。 或者，您可以使用 [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector)，根據項目類型或您指定的其他準則，為樹狀檢視項目選擇不同的 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)。

例如，在檔案總管應用程式中，您可以將一個資料範本用於資料夾，將另一個資料範本用於檔案。

![使用不同資料範本的資料夾和檔案](images/treeview-icons.png)

以下是如何建立和使用項目範本選取器的範例。  如需詳細資訊，請參閱 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 類別。

> [!NOTE]
> 這段程式碼是大型範例的一部分，且本身不會自行運作。 若要查看完整範例 (包括定義 `ExplorerItem` 的程式碼)，請參閱 GitHub 上的 [Xaml-Controls-Gallery 存放庫](https://github.com/microsoft/Xaml-Controls-Gallery)。 [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) 和 [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs) 包含相關的程式碼。

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
        ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

傳遞至 [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) 方法的物件類型取決於您是藉由設定 **ItemsSource**屬性，還是藉由建立和管理 **TreeViewNode** 物件來建立樹狀檢視。

- 如果已設定 **ItemsSource**，則物件會採用資料項目的類型。 在上述範例中，物件為 `ExplorerItem`，所以可以在簡單轉換成 `ExplorerItem` 之後使用：`var explorerItem = (ExplorerItem)item;`。
- 如果未設定 **ItemsSource**，且您自行管理樹狀檢視節點，則傳遞至 **SelectTemplateCore** 的物件為 **TreeViewNode**。 在此情況下，您可以從 [TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 屬性取得資料項目。

以下是[圖片及音樂媒體櫃樹狀檢視](#pictures-and-music-library-tree-view)範例中的資料範本選取器。 **SelectTemplateCore** 方法會收到 **TreeViewNode**，其可能以 [StorageFolder](/uwp/api/windows.storage.storagefolder) 或 [StorageFile](/uwp/api/windows.storage.storagefile) 作為其內容。 根據內容，您可以傳回預設範本，或音樂資料夾、圖片資料夾、音樂檔案或圖片檔案的特定範本。

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
```

## <a name="interacting-with-a-tree-view"></a>與樹狀檢視互動

您可以設定樹狀檢視，讓使用者以多種不同方式與之互動：

- 展開或摺疊節點
- 單一或多重選取項目
- 按一下以叫用項目

### <a name="expandcollapse"></a>展開/摺疊

任何有子系的樹狀檢視節點永遠都可以藉由按一下展開/摺疊字符來展開或摺疊。 您也可以透過程式設計方式展開或摺疊節點，並在節點狀態變更時做出回應。

#### <a name="expandcollapse-a-node-programmatically"></a>以程式設計方式展開/摺疊節點

有 2 種方式可以在您的程式碼中展開或摺疊樹狀檢視節點。

- [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 類別具有 [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) 和 [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand) 方法。 呼叫這些方法時，您會傳入您要展開或摺疊的 **TreeViewNode**。

- 每個 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 都有 [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded) 屬性。 您可以使用此屬性來檢查節點的狀態，或加以設定來變更狀態。 您也可以在 XAML 中設定此屬性，以設定節點的初始狀態。

### <a name="fill-a-node-when-its-expanding"></a>當節點正在展開時填滿節點

您可能需要在樹狀檢視中顯示大量節點，或者您無法事先知道其中會有多少節點。 **TreeView** 控制項並未虛擬化，因此您可以透過在每個節點展開時填滿該節點，或在其摺疊時移除子節點這樣的方式來管理資源。

處理 [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) 事件，並在節點正在展開時，使用 [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) 屬性將子系新增至該節點。 **HasUnrealizedChildren** 屬性會指出節點是否需要加以填滿，或是否已填入其 **Children** 集合。 請務必記住，**TreeViewNode** 不會設定這個值，您必須在應用程式的程式碼中管理該值。

以下是這些使用中 API 的範例。 請參閱本文結尾的完整範例程式碼，以了解相關內容，包括 **FillTreeNode** 的實作。

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

這不是必要動作，但您可能會想要另外再處理 [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) 事件，並在關閉父節點時移除子節點。 如果樹狀檢視有許多節點，或是節點資料使用大量資源，這點就會相當重要。 相對於在關閉的節點上保留子節點，您應該考量每次在開啟節點時填滿節點的效能影響。 最佳選項取決於您的應用程式。

以下是 **Collapsed** 事件的處理常式範例。

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>叫用項目

使用者可以叫用動作 (將項目當做按鈕一樣處理)，而不選取項目。 您可以處理 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) 事件來回應此使用者互動。

> [!NOTE]
> 與具有 [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 屬性的 **ListView** 不同，樹狀檢視永遠會啟用對項目的叫用。 您仍然可以選擇是否要處理事件。

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs) 類別**

**ItemInvoked** 事件引數可讓您存取叫用的項目。 [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) 屬性含有已叫用的節點。 您可以將其轉換為 **TreeViewNode**，並從 **TreeViewNode.Content** 屬性取得資料項目。

以下是 **ItemInvoked** 事件處理常式的範例。 資料項目是 [IStorageItem](/uwp/api/windows.storage.istorageitem)，而此範例只是顯示一些有關檔案和樹狀的資訊。 此外，如果節點是資料夾節點，還會同時展開或摺疊節點。 如果不是，節點只會在按一下＞形箭號時展開或摺疊。

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>項目選取

**TreeView** 控制項同時支援單一選取和多重選取。 預設會關閉節點選取，但您可以設定 [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) 屬性來允許選取節點。 [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) 值會是 **None**、**Single** 和 **Multiple**。

#### <a name="multiple-selection"></a>多重選取

啟用多個選項時，每個樹狀檢視節點旁邊會顯示核取方塊，並反白顯示選取的項目。 使用者可以使用核取方塊來選取或取消選取項目。按一下項目仍會導致叫用該項目。

選取或取消選取父節點，將會選取或取消選取該節點之下的所有子系。 如果已選取父節點之下的其中一些 (但不是所有) 子系，則父節點的核取方塊會顯示為不確定的狀態。

![樹狀檢視中的多個選取項目](images/treeview-selection.png)

選取的節點會新增至樹狀檢視的 [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) 集合。 您可以呼叫 [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) 方法來選取樹狀檢視中的所有節點。

> [!NOTE]
> 如果您呼叫 **SelectAll**，則不論 **SelectionMode** 為何，都會選取所有已具現化的節點。 為了提供一致的使用者體驗，如果 **SelectionMode** 為 **Multiple**，您應該只呼叫 **SelectAll**。

#### <a name="selection-and-realizedunrealized-nodes"></a>選取項目以及具現化/未具現化節點

如果樹狀檢視有未具現化的節點，這些節點不會納入選取項目。 以下是一些需要在選取項目與未具現化節點方面注意的事項。

- 如果使用者選取父節點，也會選取該父節點下所有的具現化子節點。 同樣的，如果選取所有的子節點，父節點也會變成已選取狀態。
- **SelectAll** 方法只會將具現化節點新增至 **SelectedNodes** 集合。
- 如果選取包含未具現化子節點的父節點，則會在子節點具現化時選取這些子節點。

## <a name="code-examples"></a>程式碼範例

下列程式碼範例會示範樹狀檢視控制項的各種功能。

### <a name="tree-view-using-xaml"></a>使用 XAML 的樹狀檢視

此範例顯示如何使用 XAML 建立簡單樹狀檢視結構。 樹狀檢視依類型排列，顯示可供使用者選擇的冰淇淋口味及配料。 已啟用多重選取，當使用者按一下按鈕時，主應用程式 UI 會顯示選取的項目。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all" Click="SelectAllButton_Click"/>
                <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
                <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>使用資料繫結的樹狀檢視

此範例示範如何建立與前一個範例相同的樹狀檢視。 不過，資料會建立於程式碼中並繫結至樹狀檢視的 **ItemsSource** 屬性，而不是在 XAML 中建立資料階層。 (前一個範例所示的按鈕事件處理常式也適用於此範例。)

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

        public MainPage()
        {
            this.InitializeComponent();
            DataSource = GetDessertData();
        }

        private ObservableCollection<Item> GetDessertData()
        {
            var list = new ObservableCollection<Item>();

            Item flavorsCategory = new Item()
            {
                Name = "Flavors",
                Children =
                {
                    new Item() { Name = "Vanilla" },
                    new Item() { Name = "Strawberry" },
                    new Item() { Name = "Chocolate" }
                }
            };

            Item toppingsCategory = new Item()
            {
                Name = "Toppings",
                Children =
                {
                    new Item()
                    {
                        Name = "Candy",
                        Children =
                        {
                            new Item() { Name = "Chocolate" },
                            new Item() { Name = "Mint" },
                            new Item() { Name = "Sprinkles" }
                        }
                    },
                    new Item()
                    {
                        Name = "Fruits",
                        Children =
                        {
                            new Item() { Name = "Mango" },
                            new Item() { Name = "Peach" },
                            new Item() { Name = "Kiwi" }
                        }
                    },
                    new Item()
                    {
                        Name = "Berries",
                        Children =
                        {
                            new Item() { Name = "Strawberry" },
                            new Item() { Name = "Blueberry" },
                            new Item() { Name = "Blackberry" }
                        }
                    }
                }
            };

            list.Add(flavorsCategory);
            list.Add(toppingsCategory);
            return list;
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }

    public class Item
    {
        public string Name { get; set; }
        public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

        public override string ToString()
        {
            return Name;
        }
    }
}

```

### <a name="pictures-and-music-library-tree-view"></a>圖片及音樂媒體櫃樹狀檢視

此範例示範如何建立顯示使用者 [圖片]  及 [音樂]  媒體櫃內容和結構的樹狀檢視。 無法事先知道項目的數量，因此會在每個節點展開時填滿該節點，並在其摺疊時加以清空。

自訂項目範本會用來顯示資料項目，而這些項目的類型為 [IStorageItem](/uwp/api/windows.storage.istorageitem)。

> [!IMPORTANT]
> 此範例中的程式碼需要 **picturesLibrary** 和 **musicLibrary** 功能。 如需檔案存取的詳細資訊，請參閱[檔案存取權限](../../files/file-access-permissions.md)、[列舉和查詢檔案及資料夾](../../files/quickstart-listing-files-and-folders.md)以及[音樂、圖片及影片媒體櫃中的檔案和資料夾](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            // A TreeView can have more than 1 root node. The Pictures library
            // and the Music library will each be a root node in the tree.
            // Get Pictures library.
            StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
        {
            // Get the contents of the folder represented by the current tree node.
            // Add each item as a new child node of the node that's being expanded.

            // Only process the node if it's a folder and has unrealized children.
            StorageFolder folder = null;

            if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
            {
                folder = node.Content as StorageFolder;
            }
            else
            {
                // The node isn't a folder, or it's already been filled.
                return;
            }

            IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

            if (itemsList.Count == 0)
            {
                // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
                // that the chevron appears, but don't try to process children that aren't there.
                return;
            }

            foreach (var item in itemsList)
            {
                var newNode = new muxc.TreeViewNode();
                newNode.Content = item;

                if (item is StorageFolder)
                {
                    // If the item is a folder, set HasUnrealizedChildren to true.
                    // This makes the collapsed chevron show up.
                    newNode.HasUnrealizedChildren = true;
                }
                else
                {
                    // Item is StorageFile. No processing needed for this scenario.
                }

                node.Children.Add(newNode);
            }

            // Children were just added to this node, so set HasUnrealizedChildren to false.
            node.HasUnrealizedChildren = false;
        }

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

            if (node.Content is IStorageItem item)
            {
                FileNameTextBlock.Text = item.Name;
                FilePathTextBlock.Text = item.Path;
                TreeDepthTextBlock.Text = node.Depth.ToString();

                if (node.Content is StorageFolder)
                {
                    node.IsExpanded = !node.IsExpanded;
                }
            }
        }

        private void RefreshButton_Click(object sender, RoutedEventArgs e)
        {
            sampleTreeView.RootNodes.Clear();
            InitializeTreeView();
        }
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
        ' Get the contents of the folder represented by the current tree node.
        ' Add each item as a new child node of the node that's being expanded.

        ' Only process the node if it's a folder and has unrealized children.
        Dim folder As StorageFolder = Nothing
        If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
            folder = TryCast(node.Content, StorageFolder)
        Else
            ' The node isn't a folder, or it's already been filled.
            Return
        End If

        Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
        If itemsList.Count = 0 Then
            ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
            ' that the chevron appears, but don't try to process children that aren't there.
            Return
        End If

        For Each item In itemsList
            Dim newNode As New muxc.TreeViewNode With {
            .Content = item
        }
            If TypeOf item Is StorageFolder Then
                ' If the item is a folder, set HasUnrealizedChildren to True.
                ' This makes the collapsed chevron show up.
                newNode.HasUnrealizedChildren = True
            Else
                ' Item is StorageFile. No processing needed for this scenario.
            End If
            node.Children.Add(newNode)
        Next

        ' Children were just added to this node, so set HasUnrealizedChildren to False.
        node.HasUnrealizedChildren = False
    End Sub

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
        Dim item = TryCast(node.Content, IStorageItem)
        If item IsNot Nothing Then
            FileNameTextBlock.Text = item.Name
            FilePathTextBlock.Text = item.Path
            TreeDepthTextBlock.Text = node.Depth.ToString()
            If TypeOf node.Content Is StorageFolder Then
                node.IsExpanded = Not node.IsExpanded
            End If
        End If
    End Sub

    Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
        sampleTreeView.RootNodes.Clear()
        InitializeTreeView()
    End Sub

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>在樹狀檢視之間拖放項目

下列範例會示範如何建立兩個樹狀檢視，而其中的項目可以在彼此之間拖放。 當項目拖曳至其他樹狀檢視時，系統會將其新增至清單的結尾。 不過，您可以在樹狀檢視中重新排序項目。 此範例中也只考量到包含一個根節點的樹狀檢視。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>相關文章

- [TreeView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [ListView 和 GridView](listview-and-gridview.md)
