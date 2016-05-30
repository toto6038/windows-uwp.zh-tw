---
author: jwmsft
description: xBind 標記延伸是 Binding 的替代項目。 xBind 沒有 Binding 的部分功能，但在執行時所需的時間和記憶體都比 Binding 少，並且支援較強的偵錯功能。
title: xBind 標記延伸
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
---

# {x&#58;Bind} 標記延伸

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**注意** 如需有關如何在 app 中使用資料繫結與 **{x:Bind}** (以及完整比較 **{x:Bind}** 和 **{Binding}**) 的一般資訊，請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)。

**{x:Bind}** 標記延伸 (Windows 10 的新功能) 是 **{Binding}** 的替代項目。 **{x:Bind}** 沒有 **{Binding}** 的部分功能，但在執行時所需的時間和記憶體都比 **{Binding}** 少，並且支援較強的偵錯功能。

在 XAML 載入時，**{x:Bind}** 會轉換成可視為繫結物件的項目，而此物件會從資料來源的屬性取得值。 您可以選擇性地設定繫結物件，以便觀察資料來源屬性值的變更，並根據這些變更自我重新整理。 您也可以選擇性地設定繫結物件，以便將自己的值中的變更推回到來源屬性。 由 **{x:Bind}** 和 **{Binding}** 建立的繫結物件在功能上大致相同。 但 **{x:Bind}** 會執行它在編譯階段產生的特殊用途程式碼，而 **{Binding}** 會使用一般用途的執行階段物件檢查。 因此，**{x:Bind}** 繫結 (通常指已編譯的繫結) 具有高度效能，可在編譯時驗證您的繫結運算式，並讓您在產生做為頁面部分類別的原始程式碼中設定中斷點進行偵錯，而支援偵錯功能。 您可以在 `obj` 資料夾中找到這些檔案，其名稱類似於 (適用於 C#) `<view name>.g.cs`。

**示範 {x:Bind} 的範例 app**

-   [{x:Bind} 範例](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [XAML UI 基本知識範例](http://go.microsoft.com/fwlink/p/?linkid=619992)

## XAML 屬性用法

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
```

| 詞彙 | 說明 |
|------|-------------|
| _propertyPath_ | 指定繫結屬性路徑的字串。 如需詳細資訊，請參閱下面[屬性路徑](#property-path)一節。 |
| _bindingProperties_ |
| _propName_ = _value_\[, _propName_=_value_\]* | 使用名稱/值對語法指定的一或多個繫結屬性。 |
| _propName_ | 要在繫結物件上設定之屬性的字串名稱。 例如，"Converter"。 | 
| _value_ | 設定屬性使用的值。 引數的語法取決於目前設定的屬性。 以下為 _propName_=_value_ 用法的範例，其中的 value 本身是標記延伸：`Converter={StaticResource myConverterClass}`。 如需詳細資訊，請參閱以下的[您可以使用 {x:Bind} 設定的屬性](#properties-you-can-set)一節。 | 

## 屬性路徑

*PropertyPath* 會為 **{x:Bind}** 運算式設定 **Path**。 **Path** 是一個屬性路徑，會指定您要繫結至 (來源) 的屬性、子屬性、欄位或方法的值。 您可以明確地提及 **Path** 屬性的名稱：`{Binding Path=...}`。 或者，您可以將它省略：`{Binding ...}`。

**{x:Bind}** 不會使用 **DataContext** 做為預設來源，而是使用頁面或使用者控制項本身。 因此，它會在您的頁面或使用者控制項的程式碼後置中尋找屬性、欄位及方法。 若要向 **{x:Bind}** 公開您的檢視模型，一般會採用的方式是將新的欄位或屬性新增到頁面或使用者控制項的程式碼後置。 屬性路徑中的步驟會使用句點 (.) 隔開，您可以納入多個分隔符號來周遊連續的子屬性。 使用句點分隔符號，無論用來實作繫結目標物件的程式設計語言為何。

例如：在頁面中，**Text="{x:Bind Employee.FirstName}"** 會先在頁面上尋找 **Employee** 成員，然後在 **Employee** 所傳回的物件上尋找 **FirstName** 成員。 如果您是要將項目控制項繫結到包含員工相依項的屬性，則您的屬性路徑可能會是 "Employee.Dependents"，而項目控制項的項目範本會負責顯示 "Dependents" 中的項目。

針對 C++/CX，**{x:Bind}** 無法繫結至頁面或資料模型中的私用欄位和屬性；您必須使用公用屬性才可加以繫結。 繫結的表面區域必須公開為 CX 類別/介面，以便我們取得相關的中繼資料。 此時應該不需要 **\[Bindable\]** 屬性。

如果資料來源是一個集合，則屬性路徑可以依據項目的位置或索引來指定集合中的項目。 例如，"Teams\[0\].Players"，其中常值 "\[\]" 括住 0，代表要求的是從 0 開始建立索引之集合中的第一個項目。

若要使用索引子，模型必須對要編製索引的屬性類型實作 **IList&lt;T&gt;** 或 **IVector&lt;T&gt;**。 如果已編製索引的屬性類型支援 **INotifyCollectionChanged** 或 **IObservableVector**，且繫結為 OneWay 或 TwoWay，則會登錄並接聽這些介面的變更通知。 變更偵測邏輯會根據所有的集合變更進行更新，即使不會影響特定的索引值亦然。 這是因為所有集合執行個體的接聽邏輯是通用的。

若要繫結至附加屬性，您必須將類別和屬性名稱放到點後面的括號中。 例如 **Text="{x:Bind Button22.(Grid.Row)}"**。 如果屬性未宣告在 Xaml 命名空間中，您將需要以 xml 命名空間為其加上首碼，而此命名空間應對應於文件最前面的程式碼命名空間。

編譯的繫結屬於強式類型，會解析路徑中每個步驟的類型。 如果傳回的類型沒有成員，將會在編譯時失敗。 您可以指定轉換，以指出要繫結物件的真實類型。 在下列案例中，**obj** 是類型物件的屬性，但包含文字方塊，因此我們可以使用 **Text="{x:Bind obj.(TextBox.Text)}"**。

**Text="{x:Bind groups3\[0\].(data:SampleDataGroup.Title)}"** 中的 **groups3** 欄位是物件的字典，因此您必須將它轉換為 **data:SampleDataGroup**。 請注意將物件類型對應至不屬於預設 XAML 命名空間的命名空間時所使用的 **data:** 命名空間首碼。

使用 **x:Bind** 時，您無須以 **ElementName=xxx** 做為繫結運算式的一部分。 透過 **x:Bind**，您可以使用元素的名稱做為繫結路徑的第一個部分，因為命名的元素會成為頁面或使用者控制項中代表根繫結來源的欄位。

事件繫結是編譯繫結的新功能。 它可讓您使用繫結指定事件的處理常式，而無須將其做為程式碼後置上的方法。 例如：**Click="{x:Bind rootFrame.GoForward}"**。

事件的目標方法不可以多載，且必須：

-   符合事件的簽章。
-   或不具參數。
-   或屬於下列類型的參數數量是相同的：可從事件參數的類型指派的參數。

在產生的程式碼後置中，編譯的繫結會處理事件並將其傳送到模型上的方法，以在事件發生時評估繫結運算式的路徑。 這表示它不會追蹤模型的變更，這一點與屬性繫結不同。

如需屬性路徑之字串語法的詳細資訊，請參閱 [Property-path 語法](property-path-syntax.md)，並留意此處針對 **{x:Bind}** 所說明的差異。

##  您可以使用 {x:Bind} 設定的屬性


**{x:Bind}** 以 *bindingProperties* 預留位置語法進行說明，因為在標記延伸中可以設定多個讀取/寫入屬性。 這些屬性能以任意順序設定 (以逗號分隔的 *propName*=*value* 組)。 請注意，不可在繫結運算式中加入分行符號。 某些屬性需要不具類型轉換的類型，因此這些屬性需要將自己的標記延伸巢狀在 **{x:Bind}** 內。

這些屬性的運作方式與 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 類別的屬性十分類似。

| 屬性 | 說明 |
|----------|-------------|
| **路徑** | 請參閱先前的[屬性路徑](#property-path)一節。 |
| **轉換器** | 指定繫結引擎呼叫的轉換器物件。 轉換器可以在 XAML 中設定，但若您參考已在 [{StaticResource} 標記延伸](staticresource-markup-extension.md)中指派的物件執行個體，請在資源字典中參考該物件。 |
| **ConverterLanguage** | 指定轉換器要使用的文化特性 (如果您要設定 **ConverterLanguage**，則也應該設定 **Converter**)。文化特性可以設定為標準式識別碼。 如需詳細資訊，請參閱 [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880)。 |
| **ConverterParameter** | 指定可用於轉換器邏輯的轉換器參數 (如果您要設定 **ConverterParameter**，則也應該設定 **Converter**)。大多數轉換器都可以使用簡單邏輯，從傳遞的值中取得所需的所有資訊進行轉換，而且不需要 **ConverterParameter** 值。 **ConverterParameter** 參數適用於中度進階轉換器實作，其中具備一個以上使用 **ConverterParameter** 傳遞內容的邏輯。 您可以撰寫一個使用字串以外的值的轉換器，但這並不常見，請參閱 [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) 中的＜備註＞，以了解詳細資訊。 |
| **FallbackValue** | 指定當無法解析來源或路徑時，所要顯示的值。 |
| **模式** | 指定繫結模式，如下列其中一個字串："OneTime"、"OneWay" 或 "TwoWay"。 預設值是 "OneTime"。 請注意，此與 **{Binding}** 的預設值 (通常為 "OneWay") 不同。 |
| **TargetNullValue** | 指定當來源值解析結果明確為 **null** 時，所要顯示的值。 | 

**注意** 如果要將標記從 **{Binding}** 轉換成 **{x:Bind}**，請留意 **Mode** 屬性的預設值不同。
 
## 備註

由於 **{x:Bind}** 會使用產生的程式碼來達成其效益，因此在編譯時需要使用類型資訊。 這表示您無法繫結至您未事先得知類型的屬性。 因此，您無法搭配使用 **{x:Bind}** 與 **DataContext** 屬性，因為後者屬於 **Object** 類型，也可能在執行階段有所變更。

在使用 **{x:Bind}** 與資料範本時，您必須設定 **x:DataType** 值以指出要繫結到的類型，如下列範例所示。 您也可以將類型設為介面或基底類別類型，然後在必要時使用轉換來編寫完整的運算式。

已編譯的繫結取決於程式碼產生。 因此，如果您在資源字典中使用 **{x:Bind}**，則資源字典需要具備程式碼後製類別。 如需程式碼範例，請參閱[資源字典搭配 {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind)。

**重要** 如果您為先前含有 **{x:Bind}** 標記延伸的屬性設定本機值，繫結會徹底移除。

**提示** 如果您需要為值 (例如，[**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 或 [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827)) 指定單一大括號，請在它的前面加上一個反斜線：`\{`。 或者，將整個字串括起來，以包含需要在設定的第二個引號中逸出的括號，例如 `ConverterParameter='{Mix}'`。

[
            **Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)、[**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) 與 **ConverterLanguage** 皆與來自繫結來源的值或類型轉換成和繫結目標屬性相容的類型或值的案例相關。 如需詳細資訊和範例，請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)中的＜資料轉換＞一節。

**{x:Bind}** 只是標記延伸，無法以程式設計方式建立或操作這類繫結。 如需標記延伸的詳細資訊，請參閱 [XAML 概觀](xaml-overview.md)。

## 範例

```XML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

此範例 XAML 會搭配使用 **{x:Bind}** 與 **ListView.ItemTemplate** 屬性。 請注意 **x:DataType** 值的宣告。

```XML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```



<!--HONumber=May16_HO2-->


