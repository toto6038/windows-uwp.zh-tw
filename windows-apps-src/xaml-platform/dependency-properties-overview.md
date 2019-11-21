---
description: 這個主題說明當您使用 C++、C# 或 Visual Basic 搭配 UI 的 XAML 定義來撰寫 Windows 執行階段應用程式時，可供使用的相依性屬性系統。
title: 相依性屬性概觀
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 279f0d007be927e29632986ce8178c4e0b9778b3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259860"
---
# <a name="dependency-properties-overview"></a>相依性屬性概觀

這個主題說明當您使用 C++、C# 或 Visual Basic 搭配 UI 的 XAML 定義來撰寫 Windows 執行階段應用程式時，可供使用的相依性屬性系統。

## <a name="what-is-a-dependency-property"></a>什麼是相依性屬性？

相依性屬性是特殊化的屬性類型。 更明確地說，這個屬性會追蹤屬性值，並受到專用屬性系統 (Windows 執行階段的一部分) 所影響。

為了支援相依性屬性，定義屬性的物件必須是 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) (也就是在其繼承中包含了 **DependencyObject** 基底類別的類別)。 許多您用於使用 XAML 之 UWP 應用程式的 UI 定義的類型，都是**system.windows.dependencyobject>** 子類別，而且會支援相依性屬性。 但是，任何來自其名稱內不含 "XAML" 的 Windows 執行階段命名空間的類型將不支援相依性屬性；這類型的屬性是一般屬性，將不會擁有屬性系統的相依性行為。

相依性屬性的目的是提供一個系統方法，以根據其他輸入 (當您的 app 執行時，於該應用程式內發生的其他屬性、事件及狀態) 來計算屬性值。 這些其他輸入可能包括：

- 外部輸入，如使用者喜好設定
- Just-In-Time 屬性判定機制，如資料繫結、動畫及分鏡腳本
- 多用途範本模式，如資源和樣式
- 透過與物件樹中其他元素的父系-子系關係取得的值

相依性屬性代表或支援程式設計模型的特定功能，可使用適用于 UI 的 XAML 和C#、Microsoft Visual Basic 或 Visual C++ component extensions （C++/cx）來定義用於程式碼的 Windows 執行階段應用程式。 這些功能包括：

- 資料繫結
- 樣式
- 腳本動畫
- "PropertyChanged" 行為；可以實作相依性屬性來提供可將變更傳播到其他相依性屬性的回呼
- 使用來自屬性中繼資料的預設值
- 一般屬性系統公用程式，像是 [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) 與中繼資料查閱

## <a name="dependency-properties-and-windows-runtime-properties"></a>相依性屬性與 Windows 執行階段屬性

相依性屬性會藉由提供全域內部屬性儲存區來擴充基本的 Windows 執行階段屬性功能，這個屬性儲存區會在執行階段支援應用程式中的所有相依性屬性。 這是以私用欄位支援屬性的標準模式替代方案，而私用欄位在屬性定義類別中為私用的。 您可以將這個內部屬性儲存區想像成是為任一特定物件而存在的一組屬性識別碼和值 (只要它是 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 即可)。 儲存區中的每個屬性是由 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 執行個體來識別，而不是由名稱來識別。 但是，屬性系統常會隱藏這個實作詳細資料：您通常可以使用簡單名稱 (程式碼語言中使用的程式設計屬性名稱，或是撰寫 XAML 時使用的屬性名稱) 來存取相依性屬性。

提供相依性屬性系統基礎的基底類型是 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)。 **DependencyObject** 會定義可以存取相依性屬性的方法，而 **DependencyObject** 衍生類別的執行個體內部支援我們先前提及的屬性儲存區概念。

下列是我們在討論相依性屬性時，於文件中所使用的詞彙摘要：

| 詞彙 | 描述 |
|------|-------------|
| 相依性屬性 | 存在於 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 識別碼的屬性 (如下所示)。 這個識別碼通常是以負責定義 **DependencyObject** 衍生類別的靜態成員方式來提供。 |
| 相依性屬性識別碼 | 用來識別屬性的常數值，通常是公用和唯讀的。 |
| 屬性包裝函式 | Windows 執行階段屬性的可呼叫 **get** 與 **set** 實作。 或者是原始定義的語言特定投影。 **get** 屬性包裝函式實作會呼叫 [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue)，傳遞相關的相依性屬性識別碼。 |

屬性包裝函式不只對呼叫者方便，也可以向使用 Windows 執行階段屬性定義的任何程序、工具或投影公開相依性屬性。

下列範例定義為 C# 定義的自訂 "IsSpinning" 相依性屬性，並顯示相依性屬性識別碼與屬性包裝函式的關係。

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty =
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

> [!NOTE]
> 上述範例不是用來建立自訂相依性屬性的完整範例。 而是為了顯示相依性屬性概念，讓使用者藉由程式碼具體了解概念。 如需更完整的範例，請參閱[自訂相依性屬性](custom-dependency-properties.md)。

## <a name="dependency-property-value-precedence"></a>相依性屬性值優先順序

當您取得相依性屬性的值時，您所取得的值是透過任一種參與 Windows 執行階段屬性系統的輸入，針對該屬性進行判斷的值。 因為有相依性屬性值的優先順序，使得 Windows 執行階段屬性系統可以透過可預期的方式來計算值，而且您也應該熟悉基本優先順序。 否則，您可能會發現自己處於下列情況中：您正嘗試在某一個優先順序層級上設定屬性，但其他作業 (系統、協力廠商呼叫者、您自己的部分程式碼) 正在其他層級上設定該屬性，而當您嘗試找出使用的是哪個屬性值以及該值是來自何處時感到沮喪。

例如，樣式與範本是建立屬性值與控制項外觀的共用起點。 但在特定的控制項執行個體中，您可能想變更常見的範本值，例如，為該控制項提供不同的背景色彩或不同的文字字串做為內容。 Windows 執行階段屬性系統會考量優先順序高於樣式與範本所提供值的本機值。 這樣便能啟用讓應用程式特定的值覆寫範本的案例，因此，控制項對於您在應用程式 UI 中自行使用它們而言非常有用。

### <a name="dependency-property-precedence-list"></a>相依性屬性優先順序清單

下列是指派相依性屬性的執行階段值時，屬性系統使用的決定性順序。 優先順序最高的會優先列出。 您只需瀏覽這個清單，就能找到更詳細的說明。

1. **動畫值：** 作用中動畫、視覺狀態動畫，或具有 [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) 行為的動畫。 為取得任何實際效果，套用到屬性的動畫優先順序必須高於基礎 (未動畫化的) 值，即使該值是在本機設定也一樣。
1. **本機值：** 本機值可以輕鬆透過屬性包裝函式設定，這也等同於在 XAML 中設定為屬性 (Attribute) 或屬性 (Property) 元素，或使用特定執行個體的屬性 (Property) 呼叫 [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 方法。 如果您使用繫結或靜態資源設定本機值，在優先順序上就像分別設定了本機值，如果設定了新的本機值，就會清除繫結或資源參考。
1. **範本化屬性：** 如果將元素建立為範本 (來自 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 或 [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)) 的一部分，元素就會含有這些屬性。
1. **Style Setter：** 頁面或應用程式資源樣式中的 [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) 值。
1. **預設值：** 相依性屬性可以有預設值做為其中繼資料的一部分。

### <a name="templated-properties"></a>範本化屬性

做為優先順序項目的範本化屬性不會套用到您直接在 XAML 頁面標記中宣告的任何元素屬性。 範本化屬性概念只存在 Windows 執行階段將 XAML 範本套用到 UI 元素，從而定義其視覺效果時建立的物件中。

從控制項範本設定的所有屬性都含有某些種類的值。 這些值一般都與一組控制項延伸的預設值類似，而且通常與您稍後可藉由直接設定屬性值的方式重設的值產生關聯。 因此，由範本設定的值必須能夠和實際的本機值區分，如此一來，任何新的本機值便能覆寫它。

> [!NOTE]
> 在某些情況下，如果範本無法為應可在執行個體上設定的屬性公開 [{TemplateBinding} 標記延伸](templatebinding-markup-extension.md)參考，範本甚至可能覆寫本機值。 這通常只有在屬性確實不是要在執行個體上設定時才會完成，例如，如果它只與視覺效果和範本行為相關，但與使用範本之控制項所需的函式或執行階段邏輯無關。

### <a name="bindings-and-precedence"></a>繫結與優先順序

繫結操作針對將它們用於任何範圍的情況都具有適當的優先順序。 例如，套用到本機值的 [{Binding}](binding-markup-extension.md) 會以本機值的方式運作，而屬性 setter 的 [{TemplateBinding} 標記延伸](templatebinding-markup-extension.md)的套用方式與樣式 setter 一樣。 由於繫結必須等到執行階段從資料來源取得值，因此，判斷任何屬性的屬性值優先順序的程序也會延伸到執行階段。

繫結不只和本機值具有相同優先順序，它們實際上也是本機值，其中繫結是已延遲之值的預留位置。 如果您的屬性值含有繫結，且您在執行階段於其上設定了本機值，則它會完全取代繫結。 同理，如果您呼叫 [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) 來定義只存在執行階段的繫結，則您可以取代任何已在 XAML 中套用的本機值，或者使用先前執行的程式碼來取代。

### <a name="storyboarded-animations-and-base-value"></a>腳本動畫和基礎值

腳本動畫是以「基礎值」的概念來操作。 基礎值是屬性系統使用它的優先順序來判斷的值，但會省略尋找動畫的最後一個步驟。 例如，基礎值可能來自控制項的範本，也可能來自在控制項執行個體上設定本機值。 無論使用何種方法，只要您的動畫持續執行，套用動畫將覆寫這個基礎值並套用動畫值。

對於動畫屬性，如果該動畫未明確指定 **From** 與 **To**，或者，如果動畫會在完成時將屬性還原為其基礎值，基礎值就仍能對動畫值產生影響。 在這些情況下，當動畫不再執行之後，就會再次使用剩餘的優先順序。

但是，以HoldEnd[**行為指定**To](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) 的動畫在移除之前可以覆寫本機值，即使它看起來像是已停止也一樣。 概念上來說，這就像是一個會永久執行的動畫，即使 UI 中不含視覺動畫也一樣。

多重動畫可套用到單一屬性。 這其中每一個動畫可能都已定義來取代來自值優先順序中不同時點的基礎值。 但是，這些動畫都將在執行階段同時執行，這通常表示它們必須結合其值，因為每一個動畫對於值的影響都一樣。 這完全取決於動畫如何定義，和正在動畫化的值類型。

如需詳細資訊，請參閱[腳本動畫](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)。

### <a name="default-values"></a>預設值

如需更多使用 [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata) 值建立相依性屬性預設值的詳細資料，請參閱[自訂相依性屬性](custom-dependency-properties.md)主題。

即使未在相依性屬性的中繼資料中明確定義這些預設值，該屬性仍會有預設值。 除非已使用中繼資料變更它們，否則 Windows 執行階段相依性屬性的預設值通常是下列其中一項：

- 使用執行階段物件或基本 **Object** 類型 (「參考類型」) 的屬性具有 **null** 的預設值。 例如，在刻意設定或繼承 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 之前，它會是 **null**。
- 使用像是數值或布林值 (「值類型」) 等基礎值的屬性，會使用該值的預期預設值。 例如，若為整數和浮點數則為 0，若為布林值則為 **false**。
- 使用 Windows 執行階段結構的屬性預設值，可透過呼叫該結構的隱含預設建構函式來取得。 這個建構函式會針對結構的每一個基礎值欄位使用預設值。 例如，[**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) 值的預設值會使用它的 **X** 和 **Y** 值初始化為 0。
- 使用列舉的屬性預設值為該列舉中第一個定義的成員。 檢查特定列舉的參考，以查看預設值為何。
- 使用字串 (適用於 .NET 的 [**System.String**](https://docs.microsoft.com/dotnet/api/system.string)、適用於 C++/CX 的 [**Platform::String**](https://docs.microsoft.com/cpp/cppcx/platform-string-class)) 的屬性預設值為空字串 ( **""** )。
- 根據本主題中深入探討的因素，集合屬性通常不會實作為相依性屬性。 但是，如果您實作自訂集合屬性且想要讓它成為相依性屬性，請確定會避免「不想要的單一執行個體」，如接近[自訂相依性屬性](custom-dependency-properties.md)結尾處所述。

## <a name="property-functionality-provided-by-a-dependency-property"></a>相依性屬性提供的屬性功能

### <a name="data-binding"></a>資料繫結

相依性屬性可以透過套用資料繫結來設定它的值。 資料繫結在 XAML 中使用 [{Binding} 標記延伸](binding-markup-extension.md)語法，在程式碼中使用 [{x:Bind} 標記延伸](x-bind-markup-extension.md)或 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 類別。 針對資料繫結屬性，最終的屬性值判斷會延遲到執行階段。 那時就會從資料來源中取得該值。 相依性屬性系統在此處扮演的角色是，讓預留位置行為能夠運作，例如，在尚未知道值時載入 XAML，然後透過與 Windows 執行階段資料繫結引擎互動，在執行階段提供值。

下列範例會在 XAML 中使用繫結，以設定 [**TextBlock**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) 元素的 [**Text**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 值。 繫結會使用繼承的資料內容與物件資料來源 (這個簡短範例中並未顯示這兩者，如需顯示內容與來源的更完整範例，請參閱[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth))。

```xaml
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

您也可以使用程式碼建立繫結，而不要使用 XAML。 請參閱 [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding)。

> [!NOTE]
> 像這樣的系結會被視為用於相依性屬性值優先順序的區域值。 如果您將另一個本機值設成原先擁有 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 值的屬性，將會完全覆寫該繫結，而不只是繫結的執行階段值。 {x:Bind} 繫結會使用產生的程式碼 (將會為屬性設定本機值) 來實作。 如果您針對使用 {x:Bind} 的屬性設定了本機值，那麼下一次評估繫結時 (例如當繫結在其來源物件上觀察到屬性變更時) 將會取代該值。

### <a name="binding-sources-binding-targets-the-role-of-frameworkelement"></a>繫結來源、繫結目標、FrameworkElement 的角色

若要做為繫結來源，屬性不需是相依性屬性；儘管這會根據您的程式設計語言而定且每個都擁有特定的邊緣案例，但是您通常可以使用任一屬性做為繫結來源。 不過，要成為 [{Binding} 標記延伸](binding-markup-extension.md)或 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 的目標，該屬性必須是相依性屬性。 {x:Bind} 不需要這項需求，因為它是使用產生的程式碼來套用它的繫結值。

如果您正在程式碼中建立繫結，請注意，[**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) API 只針對 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 進行定義。 不過，您可以改用 [**BindingOperations**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingOperations) 來建立繫結定義，以參考任何 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 屬性。

無論是使用程式碼或 XAML，請記住 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 是 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 屬性。 透過使用父系-子系屬性繼承格式 (通常建立在 XAML 標記中)，繫結系統可以解析存在父元素中的 **DataContext**。 即使子物件 (擁有目標屬性) 不是 **FrameworkElement**，這個繼承仍然可以評估，因此不會包含它自己的 **DataContext** 值。 不過，要繼承的父元素必須是 **FrameworkElement**，才能設定與包含 **DataContext**。 或者，您必須定義繫結，繫結才能以 **null** 值的 **DataContext** 作用。

建立繫結不是大多數資料繫結案例唯一需要做的事。 如果要讓單向或雙向繫結生效，來源屬性必須支援傳播到繫結系統 (因此就是目標) 的來源屬性。 針對自訂的繫結來源，這表示屬性必須是相依性屬性，或物件必須支援 [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged)。 集合應支援 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged)。 某些類別在它們的實作中支援這些介面，因此使用它們做為基底類別對資料繫結案例來說很有用；[**ObservableCollection&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1) 是這種類別的範例之一。 如需有關資料繫結以及資料繫結如何與屬性系統建立關聯的詳細資訊，請參閱[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)。

> [!NOTE]
> 此處所列的類型支援 Microsoft .NET 資料來源。 C++/CX 資料來源會針對變更通知或可觀察的行為使用不同的介面，請參閱[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)。

### <a name="styles-and-templates"></a>樣式與範本

樣式與範本是將屬性定義為相依性屬性的其中兩個案例。 樣式對於定義應用程式 UI 的屬性設定相當有用。 樣式在 XAML 中會定義為資源，是定義為 [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) 集合中的項目，或是定義在個別的 XAML 檔案 (例如佈景主題資源字典) 中。 樣式會與屬性系統互動，因為它們包含屬性的 setter。 這裡最重要的屬性就是 [**Control**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template) 的 [**Control.Template**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) 屬性：它定義了 **Control** 的大部分視覺化外觀和視覺狀態。 如需樣式的詳細資訊，以及定義 [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 和使用 setter 的一些 XAML 範例，請參閱[設定控制項的樣式](https://docs.microsoft.com/windows/uwp/controls-and-patterns/styling-controls)。

來自樣式或範本的值是延遲的值，與繫結類似。 這是為了讓控制項使用者可以重新範本化控制項或重新定義樣式。 此外，這就是為什麼樣式中的屬性只能以相依性屬性來操作，而不能以一般屬性來操作。

### <a name="storyboarded-animations"></a>腳本動畫

您可以使用腳本動畫將相依性屬性的值製作成動畫。 Windows 執行階段中的腳本動畫不僅僅是視覺裝飾。 這在考慮將動畫做為狀態機器技術時更有用，這類技術可設定個別屬性的值或所有屬性的值以及控制項的視覺效果，並且在一段時間後變更這些值。

若要動畫化，動畫的目標屬性必須是相依性屬性。 此外，若要動畫化，現有的 [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) 衍生動畫類型之一必須支援目標屬性的值類型。 [  **Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color)、[**Double**](https://docs.microsoft.com/dotnet/api/system.double) 及 [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) 的值可以使用內插補點或主要畫面格技術來製作動畫效果。 大部分的其他值可以使用分離的 **Object** 主要畫面格來製作動畫效果。

套用與執行動畫時，動畫化的值的優先順序高於屬性另外包含的任何值 (像是本機值)。 動畫也包含一個選擇性的 [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) 行為，即使動畫看起來像是已停止，仍會造成動畫套用到屬性值。

狀態電腦原則的具體表現方式是使用腳本動畫做為控制項的 [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) 狀態模型的一部分。 如需腳本動畫的詳細資訊，請參閱[腳本動畫](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)。 如需 **VisualStateManager** 和定義控制項視覺狀態的詳細資訊，請參閱[視覺狀態的腳本動畫](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10))或[控制項範本](../design/controls-and-patterns/control-templates.md)。

### <a name="property-changed-behavior"></a>屬性變更的行為

屬性變更的行為是相依性屬性詞彙「相依性」部分的根源。 在許多架構中，當另一個屬性會影響第一個屬性的值時，要維持某個屬性的有效值是很棘手的開發問題。 在 Windows 執行階段屬性系統中，每個相依性屬性都可以指定一個回呼，該回呼會在它的屬性值變更時叫用。 使用這個回呼通常可以同時通知或變更相關屬性值。 許多現有的相依性屬性都包含一個屬性變更的行為。 您也可以新增類似的回呼行為到自訂相依性屬性，並實作您自己的屬性變更行為。 請參閱[自訂相依性屬性](custom-dependency-properties.md)中的範例。

Windows 10 引進了 [**RegisterPropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback) 方法。 這可讓應用程式程式碼在 [**DependencyObject**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject) 執行個體上的指定相依性屬性變更時登錄變更通知。

### <a name="default-value-and-clearvalue"></a>預設值與 **ClearValue**

相依性屬性可以在其屬性中繼資料中定義一個預設值。 針對相依性屬性，它的預設值不會在第一次設定屬性預設值之後變成無關的。 每當值優先順序中有一些其他行列式消失時，預設值可能會在執行階段再次套用。 (相依性屬性值的優先順序會在下一節中討論)。例如，您可能會刻意移除套用到屬性的樣式值或動畫，但卻希望這樣做之後，將該值設定為合理的預設值。 相依性屬性的預設值可以提供這個值，而不需要執行額外步驟來特別設定每個屬性的值。

即使在您已經使用本機值來設定屬性之後，您還是能夠刻意將該屬性設為預設值。 若要再次將值重設為預設值，並且還要一併啟用可能覆寫預設值但不會覆寫本機值的優先順序中的其他參與者，可以呼叫 [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) 方法 (參考要清除的屬性以做為方法參數)。 您不一定想讓屬性照字面使用預設值，但是清除本機值並還原為預設值，可能會讓優先順序中您所需的另一個項目立即運作，例如，使用來自控制項範本中樣式 setter 的值。

## <a name="dependencyobject-and-threading"></a>**DependencyObject** 和執行緒處理

所有的 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 執行個體都必須在 UI 執行緒上建立，而這個執行緒與 Windows 執行階段 app 所顯示的目前 [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 關聯。 雖然每個 **DependencyObject** 都必須在主 UI 執行緒上建立，但是只要存取 [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) 屬性，即可使用其他緒行緒的發送器參考來存取物件。 接著，您可以在 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 物件上呼叫像是 [**RunAsync**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 的方法，並在 UI 執行緒上的執行緒限制規則內執行您的程式碼。

[  **DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 的執行緒層面都是相關的，因為它通常表示只有在 UI 執行緒上執行的程式碼才可以變更或甚至是讀取相依性屬性的值。 在一般的 UI 程式碼中通常可以避免緒行緒處理的問題，因為它能夠正確使用 **async** 模式及背景工作者執行緒。 通常您只會在定義自己的 **DependencyObject** 類型並且嘗試在 **DependencyObject** 不適用的資料來源或其他案例中使用這些類型時，才會遇到 **DependencyObject** 相關的執行緒處理問題。

## <a name="related-topics"></a>相關主題

### <a name="conceptual-material"></a>概念資料

- [自訂相依性屬性](custom-dependency-properties.md)
- [附加屬性概觀](attached-properties-overview.md)
- [深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
- [Storyboarded 動畫](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
- [建立 Windows 執行階段元件](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
- [XAML 使用者和自訂控制項範例](https://code.msdn.microsoft.com/windowsapps/XAML-user-and-custom-a8a9505e)

## <a name="apis-related-to-dependency-properties"></a>與相依性屬性相關的 Api

- [**System.windows.dependencyobject>** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)
- [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)

