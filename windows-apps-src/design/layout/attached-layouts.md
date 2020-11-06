---
description: 您可以定義附加的版面配置，來與 ItemsRepeater 控制項之類的容器搭配使用。
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 62ecc21d3ed9835ae7360d0c0dfdfa0b09cbdced
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034861"
---
# <a name="attached-layouts"></a>附加的版面配置

將其版面配置邏輯委派給另一個物件的容器 (例如 Panel)，會依賴附加的版面配置物件來為其子元素提供版面配置行為。  附加的版面配置模型可讓應用程式有彈性地在執行階段改變項目的版面配置，或更輕鬆地在 UI 的不同部分之間共用版面配置的各個層面 (例如，資料表之資料列中的項目會在資料行內顯示為對齊)。

在此主題中，我們將討論如何建立附加的版面配置 (虛擬化和非虛擬化)、您將需了解的概念和類別，以及在這兩者間進行決策時需考慮的取捨項目。

| **取得 Windows UI 程式庫** |
| - |
| 此控制項包含在 Windows UI 程式庫中；該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫概觀](/uwp/toolkits/winui/)。 |

> **重要 API** ：

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](../controls-and-patterns/items-repeater.md)
> * [配置](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (預覽)

## <a name="key-concepts"></a>重要概念

執行版面配置需要針對每個元素回答兩個問題：

1. 此元素的「大小」為何？

2. 此元素的「位置」為何？

對於[自訂面板](./custom-panels-overview.md)的討論將會簡要說明 XAML 的版面配置系統 (以回答這些問題)。

### <a name="containers-and-context"></a>容器和內容

概念上，XAML 的 [Panel](/uwp/api/windows.ui.xaml.controls.panel) 會在架構中填入兩個重要角色：

1. 其可包含子元素，並在元素的樹狀結構中引進分支。
2. 其會將特定的版面配置策略套用到那些子系。

基於這個理由，XAML 中的 Panel 通常會與版面配置同義，但技術上來說，其不只是版面配置。

[ItemsRepeater](../controls-and-patterns/items-repeater.md) 的行為也與 Panel 類似，但與 Panel 不同的是，其不會公開 Children 屬性，此屬性允許以程式設計方式新增或移除 UIElement 子系。  相反地，其子系的存留期會由架構自動管理，以對應到資料項目的集合。  儘管其不是衍生自 Panel，但會運作且由 Panel 之類的架構來處理。

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) 是衍生自 Panel 的容器，會將其邏輯委派給附加的 [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) 物件。  LayoutPanel 處於「預覽」狀態，目前僅適用於 WinUI 套件的「發行前版本」。

#### <a name="containers"></a>容器

概念上，[Panel](/uwp/api/windows.ui.xaml.controls.panel) 是元素的容器，也能夠轉譯適用於 [Background](/uwp/api/windows.ui.xaml.controls.panel.background) 的像素。  Panel 提供一種方式，可將通用版面配置邏輯封裝於易於使用的套件中。

**附加的版面配置** 的概念讓容器和版面配置這兩個角色之間的區別更清楚。  如果容器將其版面配置邏輯委派給另一個物件，我們會將該物件稱為附加的版面配置，如下列程式碼片段所示。 繼承自 [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement) 的容器 (例如 LayoutPanel) 會自動公開通用屬性，以便為 XAML 的版面配置程序 (例如，高度和寬度) 提供輸入。

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

在版面配置程序期間，容器會依賴附加的 *UniformGridLayout* 來測量和排列其子系。

#### <a name="per-container-state"></a>每個容器狀態

使用附加的版面配置，版面配置物件的單一執行個體可能會與「許多」  容器相關聯 (如下列程式碼片段所示)；因此，其不得相依於或直接參考主控件容器。  例如：

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

在這種情況下， *ExampleLayout* 必須仔細考慮其在版面配置計算中所使用的狀態及該狀態的儲存位置，以避免一個面板中元素的版面配置會對另一個產生影響。  這類似於自訂 Panel，其 MeasureOverride 和 ArrangeOverride 邏輯取決於其「靜態」  屬性的值。

#### <a name="layoutcontext"></a>LayoutContext

[LayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) 的目的是處理這些挑戰。  其會為附加的版面配置提供與主控件容器互動的能力，例如，擷取子元素，而不會在這兩者之間引進直接相依性。 內容也可讓版面配置儲存其所需的任何狀態，這可能與容器的子元素相關。

簡單且非虛擬化的版面配置通常不需要維護任何狀態，因而不會發生問題。 不過，更複雜的版面配置 (例如 Grid) 可能會選擇在測量和排列呼叫之間維護狀態，以避免重新計算值。

虛擬化的版面配置「通常」  必須在測量與排列之間，以及反覆執行的版面配置階段之間維護某個狀態。

#### <a name="initializing-and-uninitializing-per-container-state"></a>針對每個容器狀態進行初始化和取消初始化

將版面配置附加至容器時，會呼叫其 [InitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) 方法，並提供將物件初始化以儲存狀態的機會。

同樣地，從容器中移除版面配置時，將會呼叫 [UninitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) 方法。  這讓版面配置有機會清除其與該容器相關聯的任何狀態。

版面配置的狀態物件可以與內容上的 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) 屬性一起儲存，並從容器中加以擷取。

### <a name="ui-virtualization"></a>UI 虛擬化

UI 虛擬化表示會將 UI 物件建立延遲到「需要時」  才進行。  此為效能最佳化。  如果是非捲動案例，則判斷「何時需要」  可能會以任意數量的應用程式特定事物為依據。  在那些情況下，應用程式應該考慮使用 [x:Load](../../xaml-platform/x-load-attribute.md)。 其不需要在您的版面配置中進行任何特殊處理。

在捲動型案例 (例如清單) 中，判斷「何時需要」  通常會以「使用者將可看見該項目」為依據，這主要取決於在版面配置程序期間要將其放置於何處，而且需要特殊考量。  此案例為此文件的焦點。

> [!NOTE]
> 儘管此文件中並未說明，但可在非捲動案例中套用在捲動案例中啟用 UI 虛擬化的相同功能。  例如，資料導向的 ToolBar 控制項會管理其所呈現之命令的存留期，並藉由在可見區域和溢位功能表之間回收/移動元素，來回應可用空間中的變更。

## <a name="getting-started"></a>開始使用

首先，決定您需要建立的版面配置是否應支援 UI 虛擬化。

**謹記下列事項...**

1. 非虛擬化的版面配置比較容易撰寫。 如果項目數量一向很少，則建議您撰寫非虛擬化的版面配置。
2. 此平台提供一組附加的版面配置，可與 [ItemsRepeater](../controls-and-patterns/items-repeater.md#change-the-layout-of-items) 和 [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) 搭配使用以涵蓋常見需求。  請先熟悉那些需求，然後再決定您需要定義自訂版面配置。
3. 相較於非虛擬化的版面配置，虛擬化的版面配置總是會有一些額外的 CPU 和記憶體成本/複雜性/額外負荷。  以一般經驗法則而言，如果版面配置將需管理的子系很可能要符合檢視區大小 3 倍的區域，則虛擬化的版面配置可能不會帶來太多好處。 此文件稍後將更詳細地討論 3 倍大小，但這是由於 Windows 上捲動的非同步性質及其對虛擬化的影響所致。

> [!TIP]
> 參考的重點是，[ListView](/uwp/api/windows.ui.xaml.controls.listview) (和 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) 的預設設定是回收要到項目數量足以填滿目前檢視區大小的 3 倍為止才會開始。

**選擇您的基底類型**

![附加的版面配置階層](images/xaml-attached-layout-hierarchy.png)

基底 [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) 類型具有兩種衍生類型，可用來作為撰寫附加版面配置的起點：

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>非虛擬化的版面配置

建立非虛擬化版面配置的方法，對於已建立[自訂面板](./custom-panels-overview.md)的任何人而言都應該感到熟悉。  適用相同概念。  主要差異在於 [NonVirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) 可用來存取 [Children](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) 集合，而 Layout 可以選擇儲存狀態。

1. 衍生自基底類型 [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (而不是 Panel)。
2. (選擇性)  定義相依性屬性，在變更時將會使版面配置失效。
3. ( **新增** /選擇性)  將版面配置所需的任何狀態物件初始化，以作為 [InitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) 的一部分。 使用與內容一起提供的 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)，將其與主控件容器一起隱藏。
4. 覆寫 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride)，並在所有子系上呼叫 [Measure](/uwp/api/windows.ui.xaml.uielement.measure) 方法。
5. 覆寫 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride)，並在所有子系上呼叫 [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) 方法。
6. ( **新增** /選擇性)  清除任何已儲存的狀態，以作為 [UninitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) 的一部分。

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>範例：簡單的堆疊版面配置 (大小不一的項目)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

以下是一個非常基本的非虛擬化堆疊版面配置，其中含有大小不一的項目。 其缺少任何可調整版面配置行為的屬性。 以下實作說明版面配置如何依賴容器所提供的內容物件來執行下列動作：

1. 取得子系的計數，以及
2. 依索引存取每個子元素。

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>虛擬化的版面配置

與非虛擬化的版面配置類似，虛擬化版面配置的高階步驟也一樣。  複雜度主要用於判斷哪些元素將位於檢視區內且應該具現化。

1. 衍生自基底類型 [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)。
2. (選擇性) 定義您的相依性屬性，在變更時將會使版面配置失效。
3. 將版面配置所需的任何狀態物件初始化，以作為 [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) 的一部分。 使用與內容一起提供的 [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)，將其與主控件容器一起隱藏。
4. 覆寫 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)，並針對應該具現化的每個子系呼叫 [Measure](/uwp/api/windows.ui.xaml.uielement.measure) 方法。
   1. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 方法可用來擷取架構所備妥的 UIElement (例如，已套用的資料繫結)。
5. 覆寫 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)，並在每個具現化的子系上呼叫 [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) 方法。
6. (選擇性) 清除任何已儲存的狀態，以作為 [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) 的一部分。

> [!TIP]
> [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) 所傳回的值可用來作為虛擬化內容的大小。

撰寫虛擬化的版面配置時，有兩種一般方法需要考量。  要選擇哪一種主要取決於「您將如何判斷元素的大小」。  如果其足以知道資料集中某個項目的索引，或資料本身就能決定其最終大小，則我們會將其視為 **資料相依** 。  這些更容易建立。  不過，如果判斷項目大小的唯一方式是建立並測量 UI，則我們會假設它是 **內容相依** 。  這些比較複雜。

### <a name="the-layout-process"></a>版面配置程序

不論您建立的是資料或內容相依的版面配置，都請務必了解版面配置程序及 Windows 非同步捲動的影響。

以下為從啟動到在螢幕上顯示 UI 的架構所執行步驟的 (過度) 簡化觀點：

1. 其會剖析標記。

2. 產生元素的樹狀結構。

3. 執行版面配置階段。

4. 執行轉譯階段。

使用 UI 虛擬化，在判斷已建立足以填滿檢視區的內容之後，就會延遲或提早結束通常要在步驟 2 中完成的元素建立。 虛擬化的容器 (例如 ItemsRepeater) 會延遲其附加的版面配置來驅動此程序。 其會搭配 [VirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 提供附加的版面配置，以呈現虛擬化版面配置所需的其他資訊。

**RealizationRect (例如檢視區)**

在 Windows 上捲動會以非同步方式發生於 UI 執行緒上。 其不是由架構的版面配置所控制。  而互動和移動會發生於系統的撰寫器中。 這種方法的優點是移動瀏覽內容一律可以 60fps 完成。  不過，挑戰在於，相對於螢幕上實際顯示的內容，版面配置所看到的「檢視區」可能略有不同。 如果使用者快速捲動，可能會超過 UI 執行緒產生新內容的速度，因而呈現「泛黑」。 基於這個理由，虛擬化的版面配置通常需要產生一個額外的備妥元素緩衝區，這樣就足以填滿比檢視區還大的區域。 在捲動期間承受較大負載時，使用者仍可看到內容。

![具現化的矩形](images/xaml-attached-layout-realizationrect.png)

由於建立元素的成本很高，因此，虛擬化的容器 (例如 [ItemsRepeater](../controls-and-patterns/items-repeater.md)) 一開始會搭配 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 來提供附加的版面配置，以符合檢視區。 在閒置期間，容器可能會使用越來越大的具現化矩形重複呼叫版面配置，以擴大已備妥內容的緩衝區。 這種行為是一種效能最佳化，會嘗試在快速啟動時間和良好移動瀏覽體驗之間取得平衡。 ItemsRepeater 將產生的緩衝區大小上限會由其 [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 和 [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) 屬性所控制。

**重複使用元素 (回收)**

版面配置預期應該會在每次執行時，調整元素的大小和位置以填入 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)。 根據預設，[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) 將在每個版面配置階段結束時，回收任何未使用的元素。

傳遞到版面配置以作為 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 和 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) 一部分的 [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)，會提供虛擬化版面配置所需的其他資訊。 其提供的一些最常用功能如下：

1. 查詢資料中的項目數量 ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount))。
2. 使用 [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) 方法來擷取特定項目。
3. 擷取 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)，其代表版面配置應該使用具現化元素填入的檢視區和緩衝區。
4. 使用 [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 方法來要求特定項目的 UIElement。

針對指定的索引要求元素，將導致該元素在版面配置的該階段中標記為「使用中」。 如果元素尚未存在，則會將其具現化並自動備妥以供使用 (例如，擴大 DataTemplate 中定義的 UI 樹狀結構、處理任何資料繫結等)。  否則，將會從現有執行個體的集區中加以擷取。

在每個測量階段結束時，除非在透過 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 方法擷取元素時使用了 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 的選項，否則，任何未標記「使用中」之現有已具現化的元素都會被自動視為可重複使用。 此架構會自動將其移至回收集區，並使其可供使用。 隨後可能會加以提取，以供不同容器使用。 此架構會嘗試避免這種情況，因為可能會有一些與重新作為元素父代相關聯的成本。

如果虛擬化的版面配置在每次測量開始時就知道哪些元素將不再位於具現化的矩形內，則可將其重複使用最佳化， 而不需依賴架構的預設行為。 版面配置可以使用 [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) 方法，事先將元素移至回收集區。  在要求新元素之前呼叫此方法，會導致那些現有元素在版面配置稍後發出 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 要求，以找出尚未與元素相關聯的索引時可供使用。

VirtualizingLayoutCoNtext 提供兩個額外屬性，其專為建立內容相依之版面配置的版面配置作者所設計。 稍後將更詳細討論這兩個屬性。

1. [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)，可為版面配置提供選擇性「輸入」  。
2. [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)，這是版面配置的選擇性「輸出」  。

## <a name="data-dependent-virtualizing-layouts"></a>資料相依的虛擬化版面配置

如果您知道每個項目的大小且不需測量要顯示的內容，虛擬化的版面配置就會更容易。  在此文件中，我們會將這個類別的虛擬化版面配置稱為 **資料版面配置** ，因為其通常涉及檢查資料。  根據資料，應用程式可能會挑選已知大小的視覺表示，可能是因為其資料部分或先前已透過設計來決定。

一般的方法是將版面配置用於：

1. 計算每個項目的大小和位置。
2. 作為 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 的一部分：
   1. 使用 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 來判斷哪些項目應該出現在檢視區內。
   2. 使用 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 方法來擷取應代表項目的 UIElement。
   3. 使用預先計算的大小來[測量](/uwp/api/windows.ui.xaml.uielement.measure) UIElement。
3. 作為 [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) 的一部分，使用預先計算的位置來[排列](/uwp/api/windows.ui.xaml.uielement.arrange)每個具現化的 UIElement。

> [!NOTE]
> 資料版面配置方法通常與「資料虛擬化」  不相容。  具體而言，唯一載入至記憶體的資料就是填入使用者可看到內容所需的資料。  當使用者向下捲動到資料持續存在的位置時，資料虛擬化並不是指延遲或增量載入資料，  而是指將項目捲動到檢視範圍之外時，從記憶體中釋放那些項目的時機。  擁有資料版面配置來檢查每個資料項目以作為資料版面配置的一部分，會使資料虛擬化無法如預期般運作。  例外狀況是 UniformGridLayout 之類的版面配置，其假設所有項目的大小都相同。

> [!TIP]
> 如果您要針對控制項程式庫建立自訂控制項，讓其他人可在各種狀況中使用，則您可能不適合選擇資料版面配置。

### <a name="example-xbox-activity-feed-layout"></a>範例：Xbox 活動摘要版面配置

適用於 Xbox 活動摘要的 UI 會使用重複模式，其中每一行都有一個寬形磚，後面接著兩個要在下一行反轉的窄形磚。 在此版面配置中，每個項目的大小均取決於資料集中的項目位置及磚的已知大小 (寬和窄)。

![Xbox 活動摘要](images/xaml-attached-layout-activityfeedscreenshot.png)

下列程式碼將逐步解說適用於活動摘要的自訂虛擬化 UI 可能是什麼，以說明您可能會針對 **資料版面配置** 採取的一般方法。

<table>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項程式庫</strong>應用程式，請按一下這裡以開啟應用程式，並查看與此範例版面配置搭配使用的 <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>實作

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(選擇性) 管理項目到 UIElement 的對應

根據預設，[VirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) 會維護具現化的元素與其所代表之資料來源中索引間的對應。  版面配置可以選擇透過 [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 方法 (可避免預設的自動回收行為) 擷取元素時，藉由一律要求 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 的選項，自行管理此對應。  版面配置可能會選擇執行此動作，例如，只有在將捲動限制為一個方向，而且其所考慮的項目一律為連續 (也就是，知道第一個和最後一個元素的索引就足以知道所有應具現化的元素) 時，才會使用此項。

#### <a name="example-xbox-activity-feed-measure"></a>範例：Xbox 活動摘要測量

下列程式碼片段顯示可新增到先前範例中的 MeasureOverride 以管理對應的其他邏輯。

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>資料相依的虛擬化版面配置

如果您必須先測量項目的 UI 內容以找出其確切大小，則其為 **內容相依的版面配置** 。  您也可以將其視為版面配置，其中每個項目都必須自行調整大小，而不是由版面配置告訴該項目其大小。 屬於此類別的虛擬化版面配置更為相關。

> [!NOTE]
> 內容相依的版面配置不會 (不應) 中斷資料虛擬化。

### <a name="estimations"></a>估計

內容相依的版面配置會依賴估計，來猜測未具現化內容的大小和具現化內容的位置。 當那些估計變更時，其將導致內容在可捲動區域內規律地移動位置。 如果未緩和，則這可能導致非常令人沮喪且不悅的使用者體驗。 這裡會討論可能發生的問題及緩和措施。

> [!NOTE]
> 資料版面配置會考慮每個項目，並知道所有項目的確切大小、具現化與否及其位置，如此就能完全避免這些問題。

**捲動錨定**

XAML 提供一種機制，可透過實作 [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 介面，來讓捲動控制項支援[捲動錨定](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)，以緩和突然發生的檢視區移位。 當使用者操作內容時，捲動控制項會持續從一組選擇要追蹤的候選項目中選取元素。 如果錨點元素的位置會在版面配置期間移位，則捲動控制項會自動移動其檢視區來維護檢視區。

提供給版面配置的 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 值，可能會反映捲動控制項所選擇之目前選取的錨點元素。 或者，如果開發人員明確要求在 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 上使用 [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) 方法來將某個索引的元素具現化，則會在下一個版面配置階段，將該索引指定為 [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)。 這讓您能夠針對可能的案例準備版面配置，讓開發人員能夠將元素具現化，隨後要求透過 [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) 方法顯示該元素。

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 是資料來源中適用於項目的索引，在估計其項目的位置時，應該先放置內容相依的版面配置。 其應該用來作為放置其他具現化項目的起點。

**對捲軸的影響**

即使利用捲動錨定，如果版面配置的估計變化很大 (也許是因為內容大小有明顯變化)，則捲軸的捲動方塊位置可能會顯示為跳來跳去。  如果捲動方塊似乎不會在使用者拖曳該方塊時追蹤其滑鼠指標的位置，可能就會讓使用者感到不悅。

版面配置的估計越精準，使用者看到捲軸的捲動方塊跳來跳去的機會就越低。

### <a name="layout-corrections"></a>版面配置更正

內容相依的版面配置應做好準備，以使用實際情況來將其估計合理化。  例如，當使用者捲動到內容頂端，且版面配置將第一個元素具現化時，可能發現元素相對於其啟動所在之元素的預期位置，會導致該元素出現在原點 (x:0, y:0) 以外的地方。 發生這種情況時，版面配置可以使用 [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 屬性，將其計算出的位置設定為新的版面配置原點。  最終結果類似於捲動錨定，其中捲動控制項的檢視區會自動調整，以考慮儲存版面配置所報告的內容位置。

![更正 LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>中斷連線的檢視區

從版面配置的 [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) 方法傳回的大小代表對內容大小的最佳猜測，這可能會隨著每個連續的版面配置而變更。  當使用者捲動時，將持續以更新的 [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 重新評估版面配置。

如果使用者非常快速地拖曳捲動方塊，則從版面配置的觀點來看，其可能會進行大跳躍，而先前的位置並不會與目前的位置重疊。  這是因為捲動的非同步性質所致。 取用版面配置的應用程式也可能要求針對目前尚未具現化的項目顯示元素，並估計會在版面配置所追蹤的目前範圍外部進行配置。

當版面配置發現其猜測不正確，且/或看到未預期的檢視區移位時，版面配置就必須適應其開始位置。  XAML 控制項隨附的虛擬化版面配置會以內容相依的版面配置進行開發，因為其對將顯示之內容性質所設定的限制較少。


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>範例：適用於大小不一之項目的簡單虛擬化堆疊版面配置

下列範例所示範的簡單堆疊版面配置適用於大小不一的項目：

* 支援 UI 虛擬化，
* 使用估計來猜測未具現化之項目的大小，
* 注意到可能存在且非連續的檢視區移位，以及
* 套用版面配置更正以將那些移位納入考量。

**使用方式：標記**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**Codebehind：Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**程式碼：VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>相關文章

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
