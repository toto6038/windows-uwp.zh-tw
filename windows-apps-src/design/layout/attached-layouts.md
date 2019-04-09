---
Description: 您可以定義附加的版面配置容器，例如 ItemsRepeater 控制項搭配。
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6ff73b13acb5f5970bb79755b0bf5706fb12545a
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913988"
---
# <a name="attached-layouts"></a>附加的版面配置

委派 (delegate) 到另一個物件其版面配置邏輯的容器 （例如面板） 依賴為其子系的項目提供的版面配置行為附加的配置物件。  附加的版面配置模型提供變更的項目，在執行階段，版面配置的應用程式的彈性或更輕鬆地共用使用者介面 （例如項目會對齊資料行內顯示資料表的資料列中） 的不同部分之間的版面配置的層面。

在本主題中，我們將討論牽涉哪些步驟建立附加版面配置 （虛擬化和非虛擬化） 的概念和類別，您必須了解，並需要它們之間做決定時要考慮的取捨。

| **取得 Windows UI 程式庫** |
| - |
| 此控制項是包含 Windows UI 程式庫，包含新的控制項和 UWP 應用程式的 UI 功能的 NuGet 套件的過程。 如需詳細資訊，包括安裝指示，請參閱 < [Windows 的 UI 程式庫概觀](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **重要的 Api**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [配置](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) （預覽）

## <a name="key-concepts"></a>重要概念

執行配置時，需要兩個問題要回答每個項目：

1. 什麼***大小***將這個項目是？

2. 項目有哪些***位置***這個項目是？

XAML 的版面配置系統，回答上述問題，簡短說明的討論內容的一部分[自訂面板](/windows/uwp/design/layout/custom-panels-overview)。

### <a name="containers-and-context"></a>容器和內容

就概念而言，XAML 的[面板](/uwp/api/windows.ui.xaml.controls.panel)填滿 framework 中的兩個重要角色：

1. 它可以包含子項目，並導入的項目樹狀結構中的分支。
2. 它適用於這些子系的特定版面配置策略。

基於這個理由，XAML 中的面板時，通常是同義詞版面配置，但就技術上而言，不只是版面配置。

[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)行為也像窗格中，但，不同於面板，它不會公開允許以程式設計方式新增或移除的 UIElement 子系的子系屬性。  相反地，以對應至一組資料的項目架構會自動管理其子系的存留期。  雖然它不衍生自面板，它的行為，並處理架構，例如工作面板。

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)都是容器，衍生自面板中，會委派其邏輯，以附加[版面配置](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout)物件。  LayoutPanel 處於*Preview*和目前僅適用於*發行前版本*WinUI 封裝的卸除。

#### <a name="containers"></a>容器

就概念而言，[面板](/uwp/api/windows.ui.xaml.controls.panel)是容器的項目，也能夠呈現的像素[背景](/uwp/api/windows.ui.xaml.controls.panel.background)。  面板會提供用來封裝易於使用的封裝中常見的配置邏輯。

概念**附加配置**可更清楚的版面配置容器的兩個角色之間的差異。  如果容器委派 (delegate) 到另一個物件其版面配置邏輯我們會呼叫該物件附加的版面配置下列程式碼片段所示。 容器繼承自[FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)，例如 LayoutPanel，自動公開提供輸入 XAML 的版面配置程序 （例如高度和寬度） 的通用屬性。

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

在版面配置程序期間容器依賴附加*UniformGridLayout*測量和排列子系。

#### <a name="per-container-state"></a>每個容器狀態

具有附加的配置，可能會配置物件的單一執行個體相關聯*許多*容器是由下列程式碼片段所示; 因此，它必須不依賴或直接參考的主應用程式容器。  例如: 

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

這種情況下*ExampleLayout*必須仔細考慮它會在其版面配置計算和儲存該狀態使用，以免影響無與另一個面板中的項目配置的狀態。  它會是類似的 MeasureOverride 和 ArrangeOverride 邏輯取決於值的自訂面板及其*靜態*屬性。

#### <a name="layoutcontext"></a>LayoutContext

目的[LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)是來克服這些挑戰。  它提供附加的版面配置與主應用程式容器，例如擷取子項目，且不會造成直接的相依性，兩者之間互動的能力。 內容也可讓版面配置，以儲存它需要與容器的子系項目相關的任何狀態。

簡單、 非虛擬化的版面配置通常不需要維護任何狀態，因此問題。 不過，更複雜的配置，例如方格中，可以選擇維護此量值之間的狀態，以及排列呼叫，以避免重新計算值。

虛擬化版面配置*通常*需要維護兩個量值之間的某些狀態，並反覆進行的配置階段之間以及排列。

#### <a name="initializing-and-uninitializing-per-container-state"></a>初始化並取消初始化每個容器狀態

當版面配置附加至容器，其[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)方法呼叫，並提供機會來初始化物件以儲存狀態。

同樣地，當配置正在移除容器，從[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)會呼叫方法。  這會讓版面配置來清除它有與該容器相關聯的任何狀態有機會。

版面配置的狀態物件可以與一起儲存，而且從容器擷取[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)內容上的屬性。

### <a name="ui-virtualization"></a>UI 虛擬化

UI 虛擬化表示延遲的 UI 物件，直到建立_它會在需要時_。  它是一種效能最佳化。  非捲動的案例，判斷_視_可能根據任意數目的應用程式特定的項目。  在這些情況下，應用程式應該考慮使用[X:load](../../xaml-platform/x-load-attribute.md)。 它不需要任何特殊處理，在您的配置。

在捲動為基礎的案例，例如清單中，判斷_視_通常取決於"會是向使用者顯示 「 主要取決於它放置在版面配置程序期間，並且需要進行特殊考量。  此案例是本文的焦點。

> [!NOTE]
> 雖然未涵蓋在本文中，在非捲動的情況下套用相同的功能，可讓 UI 虛擬化捲動案例。  例如，資料驅動工具列控制項所管理的命令，它呈現，並以回應變更可用空間回收 / 可見的區域和溢位功能表之間移動項目存留期。

## <a name="getting-started"></a>開始使用

首先，決定您要建立版面配置是否應支援 UI 虛擬化。

**要牢記在心的幾件事...**

1. 非虛擬化的配置是容易撰寫。 如果項目數目一律為小型然後撰寫非虛擬化的版面配置建議。
2. 此平台提供一組的連接使用的版面配置[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items)並[LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)涵蓋常見的需求。  熟悉這些之前決定您需要定義自訂版面配置。
3. 虛擬化的版面配置一定會有一些額外的 CPU 和記憶體成本/複雜性/額外負荷相較於非虛擬化的版面配置。  依照一般經驗法則如果子系的版面配置將會需要管理將可能的區域中，是 3 倍的檢視區的大小，則不能有太多倍的虛擬化的版面配置。 3 倍大小會在此文件中，稍後更詳細地討論，但由於 Windows 和虛擬化其影響捲動的非同步本質。

> [!TIP]
> 參考的預設設定成[ListView](/uwp/api/windows.ui.xaml.controls.listview) (並[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) 都不會開始回收，直到項目數目便足以填滿 3 倍的目前檢視區大小。

**選擇您的基底類型**

![附加的版面配置階層](images/xaml-attached-layout-hierarchy.png)

基底[版面配置](/uwp/api/microsoft.ui.xaml.controls.layout)類型有兩個衍生的型別做為起點來撰寫附加的版面配置：

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>非虛擬化的版面配置

建立非虛擬化的版面配置的方法應該不陌生，已建立的任何人[自訂面板](/windows/uwp/design/layout/custom-panels-overview)。  適用相同概念。  主要差異在於[NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)用來存取[子系](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children)集合和版面配置，可以選擇儲存狀態。

1. 衍生自基底型別[NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) （而不是面板）。
2. *（選擇性）* 定義相依性屬性，當變更會使配置失效。
3. _(**的新**/選用)_ 初始化所需的版面配置，做為一部分的任何狀態物件[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)。 它使用隱藏與主應用程式容器[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)提供內容。
4. 覆寫[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) ，並呼叫[量值](/uwp/api/windows.ui.xaml.uielement.measure)方法上的所有子系。
5. 覆寫[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) ，並呼叫[排列](/uwp/api/windows.ui.xaml.uielement.arrange)方法上的所有子系。
6. *(**的新**/選用)* 清除任何已儲存狀態的一部分[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)。

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>範例：簡單的堆疊配置 （不同大小的項目）

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

以下是非常基本非虛擬化堆疊配置的不同大小的項目。 它沒有任何屬性，以調整的版面配置行為。 下列實作會說明如何配置仰賴提供容器的內容物件：

1. 取得子系的計數和
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

## <a name="virtualizing-layouts"></a>虛擬化的版面配置

類似於非虛擬化的版面配置，虛擬化的版面配置的高階步驟都相同。  複雜度是主要在決定什麼項目將會落在檢視區和應該實現。

1. 衍生自基底型別[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)。
2. （選擇性）定義相依性屬性，當變更會使配置失效。
3. 初始化會做為一部分的 版面配置所需的任何狀態物件[InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)。 它使用隱藏與主應用程式容器[LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)提供內容。
4. 覆寫[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) ，並呼叫[量值](/uwp/api/windows.ui.xaml.uielement.measure)應該實現每個子系的方法。
   1. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法用來擷取已備妥由架構 （例如資料繫結套用） 的 UIElement。
5. 覆寫[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) ，並呼叫[排列](/uwp/api/windows.ui.xaml.uielement.arrange)方法之每個具現化的子系。
6. （選擇性）清除任何儲存狀態的一部分[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)。

> [!TIP]
> 所傳回的值[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)做為虛擬化的內容的大小。

有兩個一般的方法，可以考慮撰寫虛擬化的版面配置時。  是否要選擇其中一種主要取決於 「 如何將決定項目大小 」。  如果其足以知道資料集或資料本身中項目的索引會指定其最終大小，則我們會考慮它**視資料而定**。  這些是比較容易建立。  如果，不過，決定大小的項目來建立量值的 UI，則我們會假設它是唯一的辦法**內容相依**。  這些是更複雜。

### <a name="the-layout-process"></a>版面配置程序

無論您建立資料或內容相關的版面配置，務必了解版面配置程序和 Windows 的非同步捲動的影響。

（使用量） 是簡化的檢視，由啟動顯示畫面上的 UI 架構執行的步驟：

1. 它會剖析標記。

2. 產生項目樹狀的結構。

3. 執行配置傳遞。

4. 執行呈現階段。

UI 虛擬化，在步驟 2 中建立，通常會來完成的項目會延遲或結束提早一次其已決定該足夠已建立內容以填滿檢視區。 虛擬化的容器 (例如 ItemsRepeater) 會將它附加的版面配置，來驅動此程序來延後。 它會提供具有附加的版面配置[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)所呈現的虛擬化的版面配置所需的其他資訊。

**RealizationRect （也就是檢視區）**

Windows 上捲動會發生非同步至 UI 執行緒。 它不會受到架構的版面配置。  相反地，互動和移動，就會發生在系統的複合項中。 這種方法的優點是，移動瀏覽內容永遠可為 60 fps。  不過，挑戰是 「 檢視區 」，所看到的版面配置可能稍微過期的時間相對於實際在螢幕上看見的功能。 如果使用者快速捲動時，它們可能會落在 UI 執行緒，來產生新的內容和 「 設為黑色取景位置調整 」 的速度。 基於這個理由，它通常是所需的虛擬化的版面配置，來產生額外的緩衝區，已備妥的項目，足以填滿大於檢視區的區域。 當您捲動使用者期間較重負載下仍看到的內容。

![實現 rect](images/xaml-attached-layout-realizationrect.png)

項目建立為高成本，因為虛擬化容器 (例如[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) 一開始會提供具有附加的版面配置[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)符合檢視區。 在閒置時容器可以藉由使用日漸變大實現就是 rect 版面配置的重複的呼叫增長之緩衝區的已備妥的內容 此行為是嘗試快速的啟動時間和良好的移動體驗之間取得平衡的效能最佳化。 ItemsRepeater 會產生最大緩衝區大小由控制其[VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)並[HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)屬性。

**（回收） 重複使用的項目**

版面配置預期的大小和位置的項目，以填滿[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)每次執行時。 依預設[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)就會回收結尾的每個配置傳遞任何未使用項目。

[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)一部分傳遞至版面配置[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)並[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)提供其他資訊需要虛擬化的版面配置。 它提供最常使用的項目包括能夠：

1. 查詢資料中的項目數 ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount))。
2. 擷取項目使用特定[GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat)方法。
3. 擷取[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)表示檢視區和版面配置中應填入的緩衝區實現項目。
4. 要求特定的項目與 UIElement [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法。

要求的指定索引的項目可能會造成該元素標示為 「 使用中 」 的版面配置該階段。 如果項目不存在，然後將可實現並會自動備妥供使用 （例如擴張 UI 樹狀目錄中定義 datatemplate 的範圍，處理任何資料繫結等。）。  否則，它會擷取從現有的執行個體的集區。

在每個量值階段結束時，任何現有的、 發現未標示 「 使用中 」 的項目會自動被視為可供重複使用除非選擇[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)透過已擷取的項目時所使用[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法。 架構會自動將它移至資源回收集區，並使其可。 它可能會隨後提取使用由不同的容器。 此架構會嘗試時避免這可能因為一些與重設父代的項目相關聯的成本。

如果虛擬化的版面配置會在每個量值的項目將不會再落在實現 rect 開頭知道它可以最佳化其重複使用。 而不是依賴架構的預設行為。 配置可以事先項目使用移至資源回收集區[RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement)方法。  要求新的項目之前呼叫這個方法會導致這些配置稍後問題時才能使用現有的項目[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)索引尚未與項目相關聯的要求。

VirtualizingLayoutContext 提供專為建立的內容相依版面配置的版面配置作者而設計的兩個額外屬性。 它們是稍後討論更多詳細資料。

1. A [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)提供選擇性_輸入_版面配置。
2. A [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)也就是一個選擇性_輸出_版面配置。

## <a name="data-dependent-virtualizing-layouts"></a>視資料而定的虛擬化版面配置

虛擬化的版面配置是如果您知道每個項目的大小應該是不需要以量值要顯示的內容更容易。  在本文件中只是我們會以這個類別的虛擬化做為版面配置**的資料配置**因為通常涉及檢查資料。  根據應用程式可能會挑選與已知的大小-的視覺表示法可能是因為資料其資料的一部分或先前已判斷所設計。

一般方法是針對至版面配置：

1. 計算的大小和位置的每個項目。
2. 做為一部分[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. 使用[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)來判斷哪些項目應該出現在檢視區。
   2. 擷取應該代表的項目的 UIElement [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法。
   3. [量值](/uwp/api/windows.ui.xaml.uielement.measure)的 UIElement 預先計算的大小。
3. 做為一部分[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)，[排列](/uwp/api/windows.ui.xaml.uielement.arrange)每個發現 UIElement 與預先計算的位置。

> [!NOTE]
> 資料的版面配置方法通常是與不相容_資料虛擬化_。  具體來說，唯一的資料載入到記憶體是什麼使用者可以看見，填入該資料。  資料虛擬化不指延遲或累加式載入的資料，當使用者捲動時關閉該資料保持常駐的位置。  相反地，它所參考時從記憶體釋放項目，是因為它們捲動到檢視。  遇到資料版面配置，其會檢查每個資料項目，因為資料配置的一部分會防止無法運作的資料虛擬化，如預期般運作。  例外狀況是 UniformGridLayout 假設所有項目具有相同的大小類似的版面配置。

> [!TIP]
> 如果您要建立自訂控制項程式庫將可供其他人在各種情況下，控制項的資料配置可能不會是您選項。

### <a name="example-xbox-activity-feed-layout"></a>範例：Xbox 活動摘要的版面配置

Xbox 活動摘要的 UI 使用重複的模式，其中每一行都有寬形磚，在後續的行後面兩個已反轉的窄磚。 在此配置中，每個項目大小會是項目的位置在資料集和已知的圖格大小 （寬 vs 縮小） 的函式。

![Xbox 活動摘要](images/xaml-attached-layout-activityfeedscreenshot.png)

以下會逐步哪些自訂虛擬化的活動摘要的 UI 可能要說明的一般接近您的程式碼可能需要**的資料配置**。

<table>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可開啟應用程式，並查看<a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>中使用此範例版面配置的動作。</p>
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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>（選擇性）管理要 UIElement 對應的項目

根據預設， [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)維護實現的項目與它們所代表的資料來源中的索引之間的對應。  配置可以選擇來管理此對應本身一律要求的選項，藉以[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)擷取透過項目時[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)方法可避免預設值自動回收行為。  如果它將只用於捲動僅限於單一方向，並將它視為項目一律會連續 （也就知道第一個和最後一個項目的索引，就足以知道應該反應的所有項目時，若要這樣做，比方說，可以選擇配置lized)。

#### <a name="example-xbox-activity-feed-measure"></a>範例：Xbox 活動摘要中的量值

下列程式碼片段會顯示可以加入在先前的範例 MeasureOverride，來管理對應的額外邏輯。

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

## <a name="content-dependent-virtualizing-layouts"></a>內容相關的虛擬化版面配置

如果您必須先測量項目來找出確切的大小的 UI 內容，則很**內容相依配置**。  您可以也將它視為配置，其中每個項目必須本身自行調整大小，而不是告訴項目大小的版面配置。 屬於此分類的虛擬化版面配置是更為複雜。

> [!NOTE]
> 內容相關的版面配置不 （不得） 中斷資料虛擬化。

### <a name="estimations"></a>估計

內容相關的版面配置依賴估計猜測實現內容的大小和具現化內容的位置。 隨著這些估計值的變更會導致要定期轉移可捲動區域內的位置的具現化的內容。 這可能會導致非常令人沮喪且突兀的使用者體驗如果不降低。 潛在的問題與緩解方式進行討論。

> [!NOTE]
> 請考慮每個項目，並知道確切的大小，實現的所有項目和其位置的資料配置可以完全避免這些問題。

**捲軸錨定**

XAML 會提供一個機制，讓捲動控制項支援緩和突然的檢視區排班[捲錨定](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)藉由實作[IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)介面。 使用者管理內容，捲動控制項就會持續從候選項目中選擇加入追蹤的一組選取的項目。 如果錨點元素的位置會在配置期間轉移再捲動控制項會自動移其維護檢視區的檢視區。

值[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)提供給版面配置可能會反映該選擇透過捲動控制項的目前選取的錨定項目。 或者，如果開發人員明確要求的項目來實現與索引[GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement)方法[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)，則為指定該索引[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)在下一步 的配置傳遞。 這可讓版面配置，以做好開發人員發現項目，以及後續要求，其帶入檢視透過可能的案例[StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview)方法。

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)是內容相關的版面配置在估計其項目的位置時，應該先定位的資料來源中項目的索引。 它應該做為起點來定位實現的其他項目。

**在 捲軸上的影響**

即使使用捲軸錨定，如果版面配置的估計值會因許多，可能是重大的變化，內容的大小則捲軸的捲動方塊的位置可能會跳至其他位置。  如果捲動方塊看起來並追蹤其滑鼠指標的位置，當它們將它拖曳，這可以是使用者突兀。

更精確配置可以是在其估計，然後就比較不可能的使用者會看到包含在跳躍的捲軸的捲動方塊。

### <a name="layout-corrections"></a>版面配置的修正

內容相關的版面配置應該準備好將其預估合理化配合實務範例。  例如，當使用者捲動至頂端的內容和版面配置中發現的第一個項目，它可能會發現的項目預期的位置，相對於從中啟動項目會使它出現的原點 (x: 0 以外的某處y:0)。 當發生這種情況時，可以使用版面配置[LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)設定做為新的版面配置原點的位置而計算的屬性。  最後結果就是類似於捲動錨定在其中捲動控制項的檢視區會自動調整的內容位置的帳戶所報告的版面配置。

![更正 LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>已中斷連線的檢視區

傳回與配置的大小[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)方法代表最佳猜測，這可能會隨每個後續的版面配置變更內容的大小。  當使用者捲動的版面配置將會持續重新評估，以更新[RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)。

如果使用者將非常快速地拖曳捲動方塊是可能的檢視區的版面配置，讓大型出現觀點跳會以先前的位置不重疊的目前位置的地方。  這是因為捲動的非同步本質。 您也可針對應用程式正在使用的配置要求的項目帶入檢視目前不會構成，並預估用以追蹤配置的範圍目前配置的項目。

當版面配置會發現其猜測不正確和/或看到非預期的檢視區 shift 鍵時，它需要重新調整其起始位置。  因為它們會顯示內容的本質上放置較少的限制會隨附於 XAML 控制項的虛擬化配置被開發成內容相關的版面配置。


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>範例：簡單的虛擬化堆疊配置的可變大小的項目

可變大小的項目，如下列範例會示範簡單的堆疊配置：

* 支援 UI 虛擬化，
* 用以估計猜測的未實現的項目大小
* 了解潛在的不連續的檢視區便會轉移，以及
* 適用於負責這些排班的版面配置更正。

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

**程式碼後置：Main.cs**

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
