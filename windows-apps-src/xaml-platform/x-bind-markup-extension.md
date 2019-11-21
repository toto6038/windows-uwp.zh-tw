---
description: The xBind markup extension is a high performance alternative to Binding. xBind - new for Windows 10 - runs in less time and less memory than Binding and supports better debugging.
title: xBind 標記延伸
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 10f601b29ff441fe8cec9261d7751ba525c7f52b
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258745"
---
# <a name="xbind-markup-extension"></a>{x:Bind} 標記延伸

**Note**  For general info about using data binding in your app with **{x:Bind}** (and for an all-up comparison between **{x:Bind}** and **{Binding}** ), see [Data binding in depth](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

The **{x:Bind}** markup extension—new for Windows 10—is an alternative to **{Binding}** . **{x:Bind}** runs in less time and less memory than **{Binding}** and supports better debugging.

在 XAML 編譯時間， **{x:Bind}** 會轉換為可取得來自資料來源上之屬性值的程式碼，並將它設定在標記中指定的屬性上。 您可以選擇性地設定繫結物件，以便觀察資料來源屬性值的變更，並根據這些變更自我重新整理 (`Mode="OneWay"`)。 您也可以選擇性地設定繫結物件，以便將自己的值中的變更推回到來源屬性 (`Mode="TwoWay"`)。

由 **{x:Bind}** 和 **{Binding}** 建立的繫結物件在功能上大致相同。 但 **{x:Bind}** 會執行它在編譯階段產生的特殊用途程式碼，而 **{Binding}** 會使用一般用途的執行階段物件檢查。 因此， **{x:Bind}** 繫結 (通常指已編譯的繫結) 具有高度效能，可在編譯時驗證您的繫結運算式，並讓您在產生做為頁面部分類別的原始程式碼中設定中斷點進行偵錯，而支援偵錯功能。 您可以在 `obj` 資料夾中找到這些檔案，其名稱類似於 (適用於 C#) `<view name>.g.cs`。

> [!TIP]
> **{x:Bind}** 的預設模式為 **OneTime**，不同於 **{Binding}** ，其預設模式為 **OneWay**。 這是基於效能考量所選擇，因為使用 **OneWay** 會導致產生更多程式碼來連結和處理變更偵測。 您可以明確地指定模式使用 OneWay 或 TwoWay 繫結。 您也可以使用 [x:DefaultBindMode](x-defaultbindmode-attribute.md) 變更標記樹狀結構特定片段的 **{x:Bind}** 預設模式。 指定的模式會在該元素及其子系上，套用任何未明確指定模式做為繫結之一部分的 **{x:Bind}** 運算式。

**Sample apps that demonstrate {x:Bind}**

-   [{x:Bind} sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) \(英文\)
-   [XAML UI Basics sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| 詞彙 | 說明 |
|------|-------------|
| _propertyPath_ | 指定繫結屬性路徑的字串。 如需詳細資訊，請參閱下面[屬性路徑](#property-path)一節。 |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | 使用名稱/值對語法指定的一或多個繫結屬性。 |
| _propName_ | 要在繫結物件上設定之屬性的字串名稱。 例如，"Converter"。 |
| _值_ | 設定屬性使用的值。 引數的語法取決於目前設定的屬性。 以下為 _propName_=_value_ 用法的範例，其中的 value 本身是標記延伸：`Converter={StaticResource myConverterClass}`。 如需詳細資訊，請參閱以下的[您可以使用 {x:Bind} 設定的屬性](#properties-that-you-can-set-with-xbind)一節。 |

## <a name="examples"></a>範例

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

此範例 XAML 會搭配使用 **{x:Bind}** 與 **ListView.ItemTemplate** 屬性。 請注意 **x:DataType** 值的宣告。

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>屬性路徑

*PropertyPath* 會為 **{x:Bind}** 運算式設定 **Path**。 **Path** 是一個屬性路徑，會指定您要繫結至 (來源) 的屬性、子屬性、欄位或方法的值。 您可以明確地提及 **Path** 屬性的名稱：`{x:Bind Path=...}`。 或者，您可以將它省略：`{x:Bind ...}`。

### <a name="property-path-resolution"></a>屬性路徑解析

**{x:Bind}** 不會使用 **DataContext** 做為預設來源，而是使用頁面或使用者控制項本身。 因此，它會在您的頁面或使用者控制項的程式碼後置中尋找屬性、欄位及方法。 若要向 **{x:Bind}** 公開您的檢視模型，一般會採用的方式是將新的欄位或屬性新增到頁面或使用者控制項的程式碼後置。 屬性路徑中的步驟會使用句點 (.) 隔開，您可以納入多個分隔符號來周遊連續的子屬性。 使用句點分隔符號，無論用來實作繫結目標物件的程式設計語言為何。

例如：在頁面中，**Text="{x:Bind Employee.FirstName}"** 會先在頁面上尋找 **Employee** 成員，然後在 **Employee** 所傳回的物件上尋找 **FirstName** 成員。 如果您是要將項目控制項繫結到包含員工相依項的屬性，則您的屬性路徑可能會是 "Employee.Dependents"，而項目控制項的項目範本會負責顯示 "Dependents" 中的項目。

針對 C++/CX， **{x:Bind}** 無法繫結至頁面或資料模型中的私用欄位和屬性；您必須使用公用屬性才可加以繫結。 繫結的表面區域必須公開為 CX 類別/介面，以便我們取得相關的中繼資料。 The **\[Bindable\]** attribute should not be needed.

使用 **x:Bind** 時，您無須以 **ElementName=xxx** 做為繫結運算式的一部分。 Instead, you can use the name of the element as the first part of the path for the binding because named elements become fields within the page or user control that represents the root binding source. 


### <a name="collections"></a>集合

如果資料來源是一個集合，則屬性路徑可以依據項目的位置或索引來指定集合中的項目。 For example, "Teams\[0\].Players", where the literal "\[\]" encloses the "0" that requests the first item in a zero-indexed collection.

若要使用索引子，模型必須對要編製索引的屬性類型實作 **IList&lt;T&gt;** 或 **IVector&lt;T&gt;** 。 (Note that IReadOnlyList&lt;T&gt; and IVectorView&lt;T&gt; do not support the indexer syntax.) If the type of the indexed property supports **INotifyCollectionChanged** or **IObservableVector** and the binding is OneWay or TwoWay, then it will register and listen for change notifications on those interfaces. 變更偵測邏輯會根據所有的集合變更進行更新，即使不會影響特定的索引值亦然。 這是因為所有集合執行個體的接聽邏輯是通用的。

如果資料目錄為字典或地圖，則屬性路徑可以依它們的字串名稱指定集合中的項目。 For example **&lt;TextBlock Text="{x:Bind Players\['John Smith'\]" /&gt;** will look for an item in the dictionary named "John Smith". 名稱必須加上引號，而且可以使用單引號或雙引號。 上標三角 (^) 可以用來逸出字串中的引號。 最簡單的方式是使用針對 XAML 屬性所使用的引號之外的替代引號。 (Note that IReadOnlyDictionary&lt;T&gt; and IMapView&lt;T&gt; do not support the indexer syntax.)

若要使用字串索引子，模型必須在要編製索引的屬性類型上實作 **IDictionary&lt;string, T&gt;** 或 **IMap&lt;string, T&gt;** 。 如果已編製索引的屬性類型支援 **IObservableMap**，且繫結為 OneWay 或 TwoWay，則它會登錄並接聽那些介面的變更通知。 變更偵測邏輯會根據所有的集合變更進行更新，即使不會影響特定的索引值亦然。 這是因為所有集合執行個體的接聽邏輯是通用的。

### <a name="attached-properties"></a>附加屬性

To bind to [attached properties](./attached-properties-overview.md), you need to put the class and property name into parentheses after the dot. 例如 **Text="{x:Bind Button22.(Grid.Row)}"** 。 如果屬性未在 Xaml 命名空間中宣告，您將需要以 xml 命名空間為它加上首碼，而此命名空間應對應於文件最前面的程式碼命名空間。

### <a name="casting"></a>轉型

編譯的繫結屬於強式類型，會解析路徑中每個步驟的類型。 如果傳回的類型沒有成員，將會在編譯時失敗。 您可以指定轉換，以向繫結指出物件的真實類型。 在下列案例中，**obj** 是類型物件的屬性，但是包含文字方塊，因此我們可以使用 **Text="{x:Bind ((TextBox)obj).Text}"** 或 **Text="{x:Bind obj.(TextBox.Text)}"** 。
The **groups3** field in **Text="{x:Bind ((data:SampleDataGroup)groups3\[0\]).Title}"** is a dictionary of objects, so you must cast it to **data:SampleDataGroup**. 請注意將物件類型對應至不屬於預設 XAML 命名空間的命名空間時，所使用的 xml **data:** 命名空間首碼。

_Note: The C#-style cast syntax is more flexible than the attached property syntax, and is the recommended syntax going forward._

## <a name="functions-in-binding-paths"></a>繫結路徑中的函式

從 Windows 10 版本 1607 開始， **{x:Bind}** 支援使用函式作為繫結路徑的分葉步驟。 This is a powerful feature for databinding that enables several scenarios in markup. See [function bindings](../data-binding/function-bindings.md) for details.

## <a name="event-binding"></a>事件繫結

事件繫結是已編譯繫結的獨特功能。 它可讓您使用繫結指定事件的處理常式，而無須將它做為程式碼後置上的方法。 例如：**Click="{x:Bind rootFrame.GoForward}"** 。

事件的目標方法不可以多載，且必須：

- 符合事件的簽章。
- 或不具參數。
- 或屬於下列類型的參數數量是相同的：可從事件參數的類型指派的參數。

在產生的程式碼後置中，編譯的繫結會處理事件並將其傳送到模型上的方法，以在事件發生時評估繫結運算式的路徑。 這表示它不會追蹤模型的變更，這一點與屬性繫結不同。

如需屬性路徑之字串語法的詳細資訊，請參閱 [Property-path 語法](property-path-syntax.md)，並留意此處針對 **{x:Bind}** 所說明的差異。

## <a name="properties-that-you-can-set-with-xbind"></a>您可以使用 {x:Bind} 設定的屬性

**{x:Bind}** 以 *bindingProperties* 預留位置語法進行說明，因為在標記延伸中可以設定多個讀取/寫入屬性。 這些屬性能以任意順序設定 (以逗號分隔的 *propName*=*value* 組)。 請注意，不可在繫結運算式中加入分行符號。 某些屬性需要不具類型轉換的類型，因此這些屬性需要將自己的標記延伸巢狀在 **{x:Bind}** 內。

這些屬性的運作方式與 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 類別的屬性十分類似。

| 屬性 | 說明 |
|----------|-------------|
| **路徑** | 請參閱先前的[屬性路徑](#property-path)一節。 |
| **Converter** | 指定繫結引擎呼叫的轉換器物件。 轉換器可以在 XAML 中設定，但若您參考已在 [{StaticResource} 標記延伸](staticresource-markup-extension.md)中指派的物件執行個體，請在資源字典中參考該物件。 |
| **ConverterLanguage** | 指定轉換器要使用的文化特性 (如果您要設定 **ConverterLanguage**，則也應該設定 **Converter**)。文化特性可以設定為標準式識別碼。 如需詳細資訊，請參閱 [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)。 |
| **ConverterParameter** | 指定可用於轉換器邏輯的轉換器參數 (如果您要設定 **ConverterParameter**，則也應該設定 **Converter**)。大多數轉換器都可以使用簡單邏輯，從傳遞的值中取得所需的所有資訊進行轉換，而且不需要 **ConverterParameter** 值。 **ConverterParameter** 參數適用於中度進階轉換器實作，其中具備一個以上使用 **ConverterParameter** 傳遞內容的邏輯。 您可以撰寫一個使用字串以外的值的轉換器，但這並不常見，請參閱 [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) 中的＜備註＞，以了解詳細資訊。 |
| **FallbackValue** | 指定當無法解析來源或路徑時，所要顯示的值。 |
| **Mode** | 指定繫結模式，如下列其中一個字串："OneTime"、"OneWay" 或 "TwoWay"。 預設值是 "OneTime"。 請注意，此與 **{Binding}** 的預設值 (通常為 "OneWay") 不同。 |
| **TargetNullValue** | 指定當來源值解析結果明確為 **null** 時，所要顯示的值。 |
| **BindBack** | 指定要針對雙向繫結的相反方向使用的函式。 |
| **UpdateSourceTrigger** | 指定何時將變更從控制項推送回 TwoWay 繫結中的模型。 The default for all properties except TextBox.Text is PropertyChanged; TextBox.Text is LostFocus.|

> [!NOTE]
> 如果要將標記從 **{Binding}** 轉換成 **{x:Bind}** ，請留意 **Mode** 屬性的預設值不同。
> [**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) can be used to change the default mode for x:Bind for a specific segment of the markup tree. 選取的模式將會在該元素及其子系上，套用任何未明確指定模式做為繫結之一部分的 x:Bind 運算式。 OneTime 效能優於 OneWay，因為使用 OneWay 將會導致產生更多程式碼來連結和處理變更偵測。

## <a name="remarks"></a>備註

由於 **{x:Bind}** 會使用產生的程式碼來達成其效益，因此在編譯時需要使用類型資訊。 這表示您無法繫結至您未事先得知類型的屬性。 因此，您無法搭配使用 **{x:Bind}** 與 **DataContext** 屬性，因為後者屬於 **Object** 類型，也可能在執行階段有所變更。

When using **{x:Bind}** with data templates, you must indicate the type being bound to by setting an **x:DataType** value, as shown in the [Examples](#examples) section. 您也可以將類型設為介面或基底類別類型，然後在必要時使用轉換來編寫完整的運算式。

已編譯的繫結取決於程式碼產生。 因此，如果您在資源字典中使用 **{x:Bind}** ，則資源字典需要具備程式碼後製類別。 如需程式碼範例，請參閱[資源字典搭配 {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind)。

包含已編譯繫結的頁面與使用者控制項，在產生的程式碼中將會有 "Bindings" 屬性。 這包括下列方法︰

- **Update()** - 這會更新所有已編譯繫結的值。 任何單向/雙向繫結都會連結接聽程式以偵測變更。
- **Initialize()** - 如果繫結尚未初始化，則它會呼叫 Update() 來初始化繫結
- **StopTracking()** - 這會解除連結針對單向與雙向繫結建立的所有接聽程式。 它們可以使用 Update() 方法重新初始化。

> [!NOTE]
> 從 Windows 10 版本 1607 開始，XAML 架構針對可見度轉換器提供了內建布林值。 轉換器會將 **true** 對應至 **Visible** 列舉值，並將 **false** 對應至 **Collapsed**，這樣您就可以將 Visibility 屬性繫結至布林值而不用建立轉換器。 請注意，這不是函式繫結的功能，而是屬性繫結。 若要使用內建轉換器，您 App 的最低目標 SDK 版本必須為 14393 或更新版本。 當您的 App 是以舊版 Windows 10 為目標時，您就無法使用它。 如需目標版本的相關詳細資訊，請參閱[版本調適型程式碼](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

**Tip**   If you need to specify a single curly brace for a value, such as in [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) or [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), precede it with a backslash: `\{`. 或者，將整個字串括起來，以包含需要在設定的第二個引號中逸出的括號，例如 `ConverterParameter='{Mix}'`。

[**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) and **ConverterLanguage** are all related to the scenario of converting a value or type from the binding source into a type or value that is compatible with the binding target property. 如需詳細資訊和範例，請參閱[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)中的＜資料轉換＞一節。

**{x:Bind}** 只是標記延伸，無法以程式設計方式建立或操作這類繫結。 如需標記延伸的詳細資訊，請參閱 [XAML 概觀](xaml-overview.md)。

