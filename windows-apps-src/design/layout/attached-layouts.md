---
Description: 您可以定義附加配置以用於容器，例如 ItemsRepeater 控制項。
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dc23e86f85c5db3dd10c5cec152047be387d4513
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282287"
---
# <a name="attached-layouts"></a>附加的版面配置

將其配置邏輯委派給另一個物件的容器（例如面板），會依賴附加的設定物件來提供其子專案的版面配置行為。  附加的版面配置模型可讓應用程式在執行時間改變專案配置的彈性，或更輕鬆地在 UI 的不同部分之間共用版面配置的層面（例如，資料表的資料列中顯示的專案會在資料行中對齊）。

在本主題中，我們將討論如何建立附加的配置（虛擬化和非虛擬化）、您需要瞭解的概念和類別，以及在兩者之間進行決定時所需考慮的取捨。

| **取得 Windows UI 程式庫** |
| - |
| 此控制項包含在 Windows UI 程式庫中；此程式庫是包含適用於 UWP 應用程式的新控制項和 UI 功能的 NuGet 封裝。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫概觀](https://docs.microsoft.com/uwp/toolkits/winui/) \(英文\)。 |

> **重要 API**：

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [配置](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) （預覽）

## <a name="key-concepts"></a>重要概念

執行版面配置需要針對每個元素回答兩個問題：

1. 此元素的***大小***為何？

2. 此元素的***位置***為何？

XAML 的版面配置系統（回答這些問題）會在[自訂面板](/windows/uwp/design/layout/custom-panels-overview)討論中簡短涵蓋。

### <a name="containers-and-context"></a>容器和內容

在概念上，XAML 的[面板](/uwp/api/windows.ui.xaml.controls.panel)會在架構中填入兩個重要角色：

1. 它可以包含子專案，並在專案的樹狀結構中引進分支。
2. 它會將特定的版面配置策略套用至這些子系。

基於這個理由，XAML 中的面板通常與版面配置同義，但是在技術上來說，不只是配置。

[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)也有類似的面板，但是與 panel 不同的是，它不會公開允許以程式設計方式新增或移除 UIElement 子系的子屬性。  相反地，其子系的存留期會由架構自動管理，以對應至資料項目的集合。  雖然它不是衍生自 Panel，但它會運作，並由像是面板的架構來處理。

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)是衍生自 Panel 的容器，它會將其邏輯委派給附加的[版面](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout)設定物件。  LayoutPanel 處於*預覽*狀態，目前僅適用于 WinUI 套件的*發行*前版本。

#### <a name="containers"></a>容器

就概念而言， [Panel](/uwp/api/windows.ui.xaml.controls.panel)是專案的容器，也能夠呈現[背景](/uwp/api/windows.ui.xaml.controls.panel.background)的圖元。  面板提供一種方法，可將通用版面配置邏輯封裝在便於使用的封裝中。

**附加**配置的概念讓容器和版面配置的兩個角色之間的區別更為清楚。  如果容器將其版面配置邏輯委派給另一個物件，我們會呼叫該物件附加的配置，如下列程式碼片段所示。 繼承自[FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)的容器（例如 LayoutPanel）會自動公開可提供輸入給 XAML 版面配置進程的通用屬性（例如，高度和寬度）。

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

在版面配置處理期間，容器會依賴附加的*UniformGridLayout*來測量和排列其子系。

#### <a name="per-container-state"></a>每個容器狀態

使用附加的版面配置，版面設定物件的單一實例可能會與*許多*容器相關聯，如下列程式碼片段所示。因此，它不能相依于或直接參考主機容器。  例如：

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

在這種情況下， *ExampleLayout*必須仔細考慮它在版面配置計算中所使用的狀態，以及該狀態的儲存位置，以避免影響某個面板中專案的配置。  它類似于自訂面板，其 MeasureOverride 和 ArrangeOverride 邏輯會依賴其*靜態*屬性的值。

#### <a name="layoutcontext"></a>LayoutCoNtext

[LayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)的目的是要處理這些挑戰。  它提供連接的配置與主機容器互動的能力，例如，抓取子專案，而不會在兩者之間引入直接相依性。 內容也可以讓配置儲存其所需的任何狀態，這可能與容器的子項目相關。

簡單、非虛擬化的版面配置通常不需要維護任何狀態，因此不會發生問題。 不過，更複雜的版面配置（例如 Grid）可能會選擇在量值和排列呼叫之間維護狀態，以避免重新計算值。

虛擬化版面配置*通常*需要在量值和排列之間，以及反復的版面設定階段之間維護一些狀態。

#### <a name="initializing-and-uninitializing-per-container-state"></a>針對每個容器狀態初始化和取消初始化

當版面配置附加至容器時，會呼叫其[InitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)方法，並提供將物件初始化以儲存狀態的機會。

同樣地，從容器中移除版面配置時，將會呼叫[UninitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)方法。  這可讓配置有機會清除與該容器相關聯的任何狀態。

版面配置的狀態物件可以與內容中的[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)屬性一起儲存並從容器中取出。

### <a name="ui-virtualization"></a>UI 虛擬化

UI 虛擬化表示在_需要時，會_延遲建立 UI 物件。  這是效能優化。  針對非滾動案例，判斷_需要_的時機可能是以應用程式特定的任何數目為依據。  在這些情況下，應用程式應該考慮使用[x:Load](../../xaml-platform/x-load-attribute.md)。 它不需要在您的版面配置中進行任何特殊處理。

在以滾動為基礎的案例（例如清單）中，決定_需要的時間_通常是以「使用者看得見」，而這主要取決於在版面配置過程中放置的位置，而且需要特殊的考慮。  此案例是本檔的焦點。

> [!NOTE]
> 雖然本檔未涵蓋，但可在非滾動案例中套用在滾動案例中啟用 UI 虛擬化的相同功能。  例如，資料驅動的工具列控制項，它會管理它所呈現之命令的存留期，並藉由在可見區域和溢位功能表之間回收/移動元素，來回應可用空間中的變更。

## <a name="getting-started"></a>開始使用

首先，決定您需要建立的版面配置是否應支援 UI 虛擬化。

**請記住一些事項 。**

1. 非虛擬化的版面配置比較容易撰寫。 如果專案數一律會很小，則建議您撰寫非虛擬化版面配置。
2. 平臺提供一組附加的配置，可與[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items)和[LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)搭配使用，以涵蓋常見的需求。  在決定需要定義自訂版面配置之前，先熟悉這些需求。
3. 相較于非虛擬化的配置，虛擬化配置一律會有一些額外的 CPU 和記憶體成本/複雜性/負擔。  以一般經驗法則而言，如果配置需要管理的子系可能會放在圖格中大小3倍的區域中，則虛擬化配置可能不會有太大的增益。 本檔稍後將更詳細地討論過3倍的大小，但這是因為在 Windows 上滾動的非同步本質及其對虛擬化的影響。

> [!TIP]
> 就參考的觀點而言， [ListView](/uwp/api/windows.ui.xaml.controls.listview) （和[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)）的預設設定是，直到專案數足以填滿目前的視口大小的3倍為止，回收才會開始。

**選擇您的基底類型**

![附加的版面配置階層](images/xaml-attached-layout-hierarchy.png)

基底[版面](/uwp/api/microsoft.ui.xaml.controls.layout)配置類型具有兩種衍生類型，做為撰寫附加配置的起點：

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>非虛擬化版面配置

建立[自訂面板](/windows/uwp/design/layout/custom-panels-overview)的任何人都應該熟悉建立非虛擬化配置的方法。  適用相同的概念。  主要的差異在於[NonVirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)是用來存取[子](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children)集合，而 layout 則可以選擇儲存狀態。

1. 衍生自基底類型[NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) （而不是 Panel）。
2. *（選擇性）* 定義在變更時將會使配置失效的相依性屬性。
3. _（**新**的/Optional）_ 將版面配置所需的任何狀態物件初始化，做為[InitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)的一部分。 使用與內容一起提供的[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) ，將它與主機容器一起隱藏。
4. 覆寫[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) ，並在所有子系上呼叫[Measure](/uwp/api/windows.ui.xaml.uielement.measure)方法。
5. 覆寫[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) ，並在所有子系上呼叫[排列](/uwp/api/windows.ui.xaml.uielement.arrange)方法。
6. *（**新**的/Optional）* 清除任何已儲存的狀態，做為[UninitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)的一部分。

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>範例：簡單的堆疊版面配置（大小不同的專案）

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

以下是一個非常基本的非虛擬化堆疊版面配置，其大小不同的專案。 它缺少任何屬性來調整版面配置的行為。 以下的執行說明配置如何依賴容器所提供的內容物件來執行下列動作：

1. 取得子系的計數，以及
2. 依索引存取每個子項目。

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

## <a name="virtualizing-layouts"></a>虛擬化版面配置

類似于非虛擬化配置，虛擬化配置的高階步驟也一樣。  複雜度主要在於判斷哪些元素會落在視口中，而且應該實現。

1. 衍生自基底類型[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)。
2. 選擇性定義在變更時將會使配置失效的相依性屬性。
3. 將版面配置所需的任何狀態物件初始化，做為[InitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)的一部分。 使用與內容一起提供的[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) ，將它與主機容器一起隱藏。
4. 覆寫[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) ，並針對應該實現的每個子系呼叫[Measure](/uwp/api/windows.ui.xaml.uielement.measure)方法。
   1. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法是用來抓取已由架構（例如，套用的資料系結）所準備的 UIElement。
5. 覆寫[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) ，並針對每個已實現的子系呼叫[排列](/uwp/api/windows.ui.xaml.uielement.arrange)方法。
6. 選擇性清除任何已儲存的狀態，做為[UninitializeForCoNtextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)的一部分。

> [!TIP]
> [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)傳回的值會用來做為虛擬化內容的大小。

撰寫虛擬化版面配置時，有兩個一般的考慮方法。  是否選擇其中一種主要取決於「您要如何判斷元素的大小」。  如果它足以知道資料集中專案的索引，或資料本身會決定其最終大小，則我們會將它視為**資料相依**。  這些是更直接建立的。  不過，如果要判斷專案大小的唯一方式是建立並測量 UI，則我們會假設它是**內容相關**的。  這些比較複雜。

### <a name="the-layout-process"></a>版面配置流程

不論您是建立資料或內容相依的配置，請務必瞭解版面配置程式以及 Windows 非同步滾動的影響。

從啟動到螢幕上顯示 UI 所執行的步驟（透過）簡化的觀點，如下所示：

1. 它會剖析標記。

2. 產生元素的樹狀結構。

3. 執行版面設定階段。

4. 執行轉譯階段。

使用 UI 虛擬化，建立通常在步驟2中完成的專案，會在判斷是否已建立足夠的內容以填滿視口之後，提早延遲或結束。 虛擬化容器（例如，ItemsRepeater）會延遲其附加的配置，以驅動此進程。 它提供附加的版面配置，其中包含[VirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) ，可呈現虛擬化配置所需的其他資訊。

**RealizationRect （亦即視口）**

Windows 上的滾動會在 UI 執行緒上非同步執行。 它不是由架構的版面配置所控制。  相反地，互動和移動會發生在系統的組合器中。 這種方法的優點是，移動流覽內容一律可以在60fps 上完成。  不過，這項挑戰是，如配置所看到的「視口」，相對於螢幕上實際顯示的內容，可能會略有不同。 如果使用者快速地滾動，他們可能會超過 UI 執行緒的速度，以產生新的內容和「平移至黑色」。 基於這個理由，虛擬化配置通常需要產生額外的備妥元素緩衝區，足以填滿一個大於此區的區域。 在滾動期間的負載較大時，使用者仍會看到內容。

![實現矩形](images/xaml-attached-layout-realizationrect.png)

因為建立專案的成本很高，所以虛擬化容器（例如， [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)）一開始會以符合此區的[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)提供附加的配置。 在閒置時間，容器可能會使用越來越大的實現矩形重複呼叫配置，來擴大已備妥內容的緩衝區。 這種行為是一種效能優化，會嘗試在快速啟動時間和良好的移動體驗之間取得平衡。 ItemsRepeater 將產生的緩衝區大小上限是由其[VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)和[HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)屬性所控制。

**重複使用元素（回收）**

版面配置應該會在每次執行時，調整專案的大小和位置，以填滿[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) 。 根據預設， [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)會在每個版面設定階段結束時回收任何未使用的元素。

當做[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)和[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)之一部分傳遞至配置的[VirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)會提供虛擬化配置所需的其他資訊。 它所提供的部分最常使用的專案，就是能夠執行下列動作：

1. 查詢資料中的專案數（[ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)）。
2. 使用[GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat)方法來取出特定專案。
3. 取出[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) ，其代表配置應該填入已實現專案的視口和緩衝區。
4. 使用[GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法來要求特定專案的 UIElement。

針對指定的索引要求元素，會導致該元素在該配置的階段中標示為「使用中」。 如果專案不存在，則會實現並自動備妥以供使用（例如，因而誇大 DataTemplate 中定義的 UI 樹狀結構、處理任何資料系結等）。  否則，它將會從現有實例的集區中取出。

在每個測量階段結束時，任何未標示為「使用中」的現有、已實現的專案，都會自動視為可供重複使用，除非透過下列專案抓取專案時，使用 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) 的選項[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法。 架構會自動將它移至回收集區，並讓它可供使用。 之後可能會提取以供不同的容器使用。 架構會嘗試避免這種情況，因為可能會有一些成本與重新父元素的相關聯。

如果虛擬化配置知道每個量值的開頭，哪些元素不會再落在實現矩形內，則可以將其重複使用優化。 而不是依賴架構的預設行為。 版面配置可以使用[RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement)方法事先將專案移至回收集區。  在要求新專案之前呼叫這個方法，會在配置稍後針對尚未與專案相關聯的索引發出[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)要求時，讓這些現有的元素可供使用。

VirtualizingLayoutCoNtext 提供兩個額外的屬性，專為版面配置作者建立與內容相依的配置所設計。 稍後將更詳細地討論它們。

1. 提供版面配置選擇性_輸入_的[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 。
2. [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) ，這是版面配置的選擇性_輸出_。

## <a name="data-dependent-virtualizing-layouts"></a>資料相依的虛擬化版面配置

如果您知道每個專案的大小為何，而不需要測量要顯示的內容，則虛擬化配置會變得更容易。  在本檔中，我們只是將這類虛擬化版面配置稱為**資料**配置，因為它們通常會包含檢查資料。  根據資料，應用程式可能會挑選已知大小的視覺標記法，可能是因為它的資料部分或先前已由設計決定。

一般的方法是將版面配置用於：

1. 計算每個專案的大小和位置。
2. 做為[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)的一部分：
   1. 使用[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)來判斷哪些專案應該出現在視口中。
   2. 使用[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法，抓取應代表該專案的 UIElement。
   3. 使用預先計算的大小來[測量](/uwp/api/windows.ui.xaml.uielement.measure)UIElement。
3. 做為[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)的一部分，請使用預先計算的位置來[排列](/uwp/api/windows.ui.xaml.uielement.arrange)每個已實現的 UIElement。

> [!NOTE]
> 資料配置方法通常與_資料虛擬化_不相容。  具體而言，唯一載入記憶體中的資料就是填入使用者可以看到的內容所需的資料。  資料虛擬化不會在使用者于資料持續的位置向下滾動時，參考延遲或累加式資料載入。  相反地，它會在從記憶體中的專案被顯示為無法查看時，參考它們。  擁有資料配置來檢查每個資料項目做為資料配置的一部分，會使資料虛擬化無法如預期般運作。  例外狀況是 UniformGridLayout 這類配置，其假設所有專案的大小都相同。

> [!TIP]
> 如果您要建立控制項程式庫的自訂控制項，讓其他人在各種情況下使用，則資料配置可能不適合您的選擇。

### <a name="example-xbox-activity-feed-layout"></a>範例： Xbox 活動摘要版面配置

Xbox 活動摘要的 UI 會使用重複模式，其中每一行都有寬磚，後面接著兩個在後續行上反轉的窄磚。 在此配置中，每個專案的大小是資料集中專案位置的函式，以及磚的已知大小（寬和窄）。

![Xbox 活動摘要](images/xaml-attached-layout-activityfeedscreenshot.png)

下列程式碼會逐步解說活動摘要的自訂虛擬化 UI 如何說明您可能會對**資料**配置採取的一般方法。

<table>
<td>
    <p>如果您已安裝<strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡以開啟應用程式，並使用此範例版面配置查看<a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>的作用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>實作

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>選擇性管理專案與 UIElement 的對應

根據預設， [VirtualizingLayoutCoNtext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)會維護已實現的專案與其所代表之資料來源中的索引之間的對應。  版面配置可以選擇透過[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法（可防止預設的自動回收行為），在抓取專案時，一律要求[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)選項來管理此對應。  版面配置可以選擇執行這項操作，例如，只有在將滾動限制為一個方向，而且它所考慮的專案一律是連續的（亦即，知道第一個和最後一個元素的索引足以知道所有應該反應的元素時，才會使用它）。lized).

#### <a name="example-xbox-activity-feed-measure"></a>範例： Xbox 活動摘要量值

下列程式碼片段顯示可新增至先前範例中的 MeasureOverride 以管理對應的其他邏輯。

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

## <a name="content-dependent-virtualizing-layouts"></a>內容相依的虛擬化版面配置

如果您必須先測量專案的 UI 內容，以找出它的確切大小，則它是**內容相依的版面**配置。  您也可以將它視為一個版面配置，其中每個專案都必須自行調整大小，而不是告訴專案大小的版面配置。 將屬於此類別的配置虛擬化更牽涉。

> [!NOTE]
> 內容相依的版面配置不會中斷資料虛擬化。

### <a name="estimations"></a>估計

內容相依的版面配置會依賴估計，以猜測未實現內容的大小和所實現內容的位置。 當這些估計值變更時，會導致內容在可捲動區域內定期轉移位置。 如果不緩和，這可能會導致非常令人沮喪的 jarring 使用者體驗。 這裡會討論潛在的問題和緩和措施。

> [!NOTE]
> 資料配置會考慮每個專案，並知道所有專案的確切大小、已實現或不存在，而且其位置可以完全避免這些問題。

**滾動錨定**

XAML 提供了一種機制，可讓您藉由執行[IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)介面來支援[滾動](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)控制項，藉此減輕突然的視口移位 當使用者操作內容時，滾動控制項會持續從一組選擇要追蹤的候選項目中選取元素。 如果錨點專案的位置在版面配置期間轉移，則捲軸控制項會自動移動其視口，以維護此區。

提供給配置的[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)值可能會反映出滾動控制項所選擇的目前選取的錨定元素。 或者，如果開發人員明確要求使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)上的[GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement)方法來實現某個索引的元素，則會在下一個設定階段將該索引指定為[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 。 這可讓您在可能的情況下準備配置，以供開發人員瞭解專案，並隨後要求透過[StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview)方法將其納入觀看。

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)是資料來源中專案的索引，內容相依的配置在估計其專案的位置時，應該先定位。 它應該作為定位其他已實現專案的起點。

**捲軸的影響**

即使使用 scroll 錨定，如果配置的估計值變化很大，也許是因為內容大小有明顯的變化，則捲軸的捲動方塊位置可能會出現跳躍。  如果捲動方塊不會在拖曳滑鼠指標時追蹤其位置，則可以 jarring 使用者。

配置可以在其估計中更精確，因此使用者不太可能看到捲軸捲動方塊的跳躍。

### <a name="layout-corrections"></a>版面配置更正

內容相依的配置應該做好準備，以合理化其預估的實際情況。  例如，當使用者滾動到內容的頂端，而且配置發現第一個專案時，它可能會發現專案相對於其啟動專案的預期位置，會使其出現在來源以外的地方（x:0, y:0). 發生這種情況時，配置可以使用[LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)屬性，將它所計算的位置設定為新的版面配置來源。  最終結果類似于滾動錨定，其中滾動控制項的視口會自動調整，以考慮配置所報告的內容位置。

![更正 LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>中斷連線的區

從配置的[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)方法傳回的大小，代表可能會隨著每個連續版面配置而變更的內容大小最佳猜測。  當使用者滾動時，將會持續以更新的[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)重新評估版面配置。

如果使用者非常快速地拖曳捲動方塊，則從配置的角度來看，它可能會變得很大跳躍，而先前的位置並不會重迭目前的位置。  這是因為滾動的非同步本質所致。 使用配置的應用程式也可能會要求將元素帶入目前尚未實現的專案，並估計在配置所追蹤的目前範圍之外進行配置。

當版面配置發現其猜測不正確，且/或看到未預期的區域移位時，它必須重新置放其開始位置。  隨附在 XAML 控制項中的虛擬化配置會作為內容相依的配置而開發，因為它們會對將要顯示的內容特性提供較少的限制。


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>範例：適用于可變大小專案的簡單虛擬化堆疊版面配置

下列範例示範簡單的堆疊配置，適用于變數大小的專案：

* 支援 UI 虛擬化，
* 會使用估計來猜測未使用專案的大小，
* 瞭解潛在的非連續的視口移位，以及
* 套用版面配置更正以考慮這些轉移。

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

**後置： Main.cs**

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

**程式碼： VirtualizingStackLayout.cs**

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
