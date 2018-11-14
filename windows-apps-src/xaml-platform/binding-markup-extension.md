---
author: jwmsft
description: Binding 標記延伸會在 XAML 載入時間轉換成 Binding 類別的執行個體。
title: Binding 標記延伸'
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 02c856fd697bef958eb45a0f0f133e06f63a7f51
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6457345"
---
# <a name="binding-markup-extension"></a>{Binding} 標記延伸


**注意：** 新的繫結機制是適用於 windows 10，可針對效能與開發人員生產力最佳化。 請參閱 [{x:Bind} 標記延伸](x-bind-markup-extension.md)。

**注意：** 一般了解使用資料繫結中您的應用程式搭配 **{Binding}** （並用於全啟動和之間的比較 **{X:bind}** **{Binding}**），請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)。

**{Binding}** 標記延伸用來來自資料來源，例如程式碼值的控制項上的資料繫結屬性。 **{Binding}** 標記延伸會在 XAML 載入時間轉換成 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 類別的執行個體。 這個繫結物件會取得資料來源上的屬性值，並推送至控制項上的屬性。 您可以選擇性地設定繫結物件，以便觀察資料來源屬性值的變更，並根據這些變更對它做出更新。 您也可以選擇性地設定繫結物件，以便將控制項值中的變更推回到來源屬性。 做為資料繫結目標的屬性必須是相依性屬性。 如需詳細資訊，請參閱[相依性屬性概觀](dependency-properties-overview.md)。

**{Binding}** 具有與本機值相同的相依性屬性優先順序，並且在命令式程式碼中設定本機值將會移除標記中任何 **{Binding}** 設定的效果。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| 詞彙 | 說明 |
|------|-------------|
| *propertyPath* | 指定繫結屬性路徑的字串。 如需詳細資訊，請參閱下面[屬性路徑](#property-path)一節。 |
| *bindingProperties* | *propName*=*value*\[, *propName*=*value*\]*<br/>使用名稱/值對語法指定的一或多個繫結屬性。 |
| *propName* | 要在 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 物件上設定的屬性字串名稱。 例如，"Converter"。 |
| *value* | 設定屬性使用的值。 引數的語法取決於下面[可以使用 {Binding} 設定的繫結類別屬性](#properties-of-the-binding-class-that-can-be-set-with-binding)一節的屬性。 |

## <a name="property-path"></a>屬性路徑

[**路徑**](https://msdn.microsoft.com/library/windows/apps/br209830)說明您繫結的屬性 (來源屬性)。 路徑是一個位置參數，表示您可以明確使用參數名稱 (`{Binding Path=EmployeeID}`)，或者您可以將它指定為第一個未命名參數 (`{Binding EmployeeID}`)。

[**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 的類型是屬性路徑，它是自訂類型或架構類型之屬性或子屬性的評估字串。 類型可以是 (但不一定要是) [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)。 屬性路徑中的步驟會使用句點 (.) 隔開，您可以納入多個分隔符號來周遊連續的子屬性。 使用句點分隔符號，無論用來實作繫結目標物件的程式設計語言為何。

例如，若要將 UI 繫結到員工物件的名字屬性，則您的屬性路徑可能會是 "Employee.FirstName"。 如果您是要將項目控制項繫結到包含員工相依項的屬性，則您的屬性路徑可能會是 "Employee.Dependents"，而項目控制項的項目範本會負責顯示 "Dependents" 中的項目。

如果資料來源是一個集合，則屬性路徑可以依據項目的位置或索引來指定集合中的項目。 例如，"Teams\[0\].Players"，其中常值 "\[\]" 括住 "0"，來指定集合中的第一個項目。

使用繫結到現有 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的 [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) 時，可以使用附加屬性做為屬性路徑的一部分。 為使附加屬性的意義清楚，讓附加屬性名稱中間的點不會被視為屬性路徑的步驟，請使用括號括住擁有者限定的附加屬性名稱；例如，`(AutomationProperties.Name)`。

屬性路徑中繼物件會儲存為執行階段表示法中的 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 物件，但是大部分案例都不需要與程式碼中的 **PropertyPath** 物件互動。 您通常可以使用 XAML 來指定所需的繫結資訊。

如需屬性路徑的字串語法、動畫功能區域中的屬性路徑以及建構 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 物件的詳細資訊，請參閱 [Property-path 語法](property-path-syntax.md)。

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>可以使用 {Binding} 設定的繫結類別屬性


**{Binding}** 以 *bindingProperties* 預留位置語法進行說明，因為在標記延伸中可以設定 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 的多個讀取/寫入屬性。 這些屬性能以任意順序設定 (以逗號分隔的 *propName*=*value* 組)。 某些屬性需要不具類型轉換的類型，因此這些屬性需要將自己的標記延伸巢狀在 **{Binding}** 內。

| 屬性 | 說明 |
|----------|-------------|
| [**路徑**](https://msdn.microsoft.com/library/windows/apps/br209830) | 請參閱先前的[屬性路徑](#property-path)一節。 |
| [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) | 指定繫結引擎呼叫的轉換器物件。 轉換器可以使用 [{StaticResource} 標記延伸](staticresource-markup-extension.md)在標記中設定，以參考至資源字典中的該物件。 |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | 指定轉換器要使用的文化特性 (若您要設定 [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826))。文化特性可以設定為標準式識別碼。 如需詳細資訊，請參閱 [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) |
| [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | 指定可用於轉換器邏輯的轉換器參數。 (如果您要設定 [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826))。大多數轉換器都可以使用簡單邏輯，從傳遞的值中取得所需的所有資訊進行轉換，而且不需要 **ConverterParameter** 值。 **ConverterParameter** 參數適用於更為複雜的轉換器實作，這些實作具備可切斷 **ConverterParameter** 中傳遞之內容的條件式邏輯。 您可以撰寫一個使用字串以外的值的轉換器，但這並不常見，請參閱 **ConverterParameter** 中的＜備註＞，以了解詳細資訊。 |
| [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | 透過參考相同 XAML 建構中具有 **Name** 屬性或 [x:Name](x-name-attribute.md) 屬性的另一個元素來指定資料來源。 這通常是用來共用相關的值，或使用一個 UI 元素的子屬性提供特定值給另一個元素 (例如，在 XAML 控制項範本中)。 |
| [**FallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | 指定當無法解析來源或路徑時，所要顯示的值。 |
| [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) | 指定繫結模式，如下列其中一值："OneTime"、"OneWay" 或 "TwoWay"。 這些會對應 [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822) 列舉的常數名稱。 預設值是 "OneWay"。 請注意，這與 **{x:Bind}** 的預設值 ("OneTime") 不同。 | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | 透過描述相對於繫結目標位置的繫結來源位置，以指定資料來源。 這最常用於 XAML 控制項範本內的繫結中。 設定 [{RelativeSource} 標記延伸](relativesource-markup-extension.md)。 |
| [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) | 指定物件資料來源。 在 **Binding** 標記延伸內，[**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) 屬性需要物件參考，例如 [{StaticResource} 標記延伸](staticresource-markup-extension.md)參考。 如果未指定這個屬性，動作資料內容會指定來源。 通常不會在個別的繫結中指定 Source 值，而是倚賴共用的 **DataContext** 進行多重繫結。 如需詳細資訊，請參閱[**DataContext**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.datacontext.aspx)或[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)。 |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | 指定當來源值解析結果明確為 **null** 時，所要顯示的值。 |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | 指定繫結來源更新的時機。 如果沒有指定，則預設為 **Default**。 |

**注意：** 如果您正在將轉換標記從 **{X:bind}** **{Binding}**，請留意**模式**屬性的預設值差異。

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)、[**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) 與 **ConverterLanguage** 皆與來自繫結來源的值或類型轉換成和繫結目標屬性相容的類型或值的案例相關。 如需詳細資訊和範例，請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)中的＜資料轉換＞一節。

> [!NOTE]
> 從 Windows10 版本 1607 開始，XAML 架構針對可見度轉換器提供了內建布林值。 轉換器會將 **true** 對應至 **Visible** 列舉值，並將 **false** 對應至 **Collapsed**，這樣您就可以將 Visibility 屬性繫結至布林值而不用建立轉換器。 若要使用內建轉換器，您 App 的最低目標 SDK 版本必須為 14393 或更新版本。 當您的 App 是以舊版 Windows10 為目標時，您就無法使用它。 如需目標版本的相關詳細資訊，請參閱[版本調適型程式碼](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

[**Source**](https://msdn.microsoft.com/library/windows/apps/br209832)、[**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) 與 [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) 都可以指定繫結來源，因此彼此互斥。

**提示：** 如果您需要指定值的單一大括號，例如在[**路徑**](https://msdn.microsoft.com/library/windows/apps/br209830)或[**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827)，然後前面加上一個反斜線： `\{`。 或者，將整個字串括起來，以包含需要在設定的第二個引號中逸出的括號，例如 `ConverterParameter='{Mix}'`。

## <a name="examples"></a>範例

```XML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/>
</Page>
```

第二個範例設定了四個不同的 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 屬性：[**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828)、[**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)、[**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) 與 [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)。 **Path** 在此例中顯示明確命名為 **Binding** 屬性。 **Path** 被評估為資料繫結來源，這個來源是相同執行階段物件樹中的另一個物件 (一個名稱為 `sliderValueConverter` 的 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614))。

請注意 [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) 屬性值如何使用另一個標記延伸 ([{StaticResource} 標記延伸](staticresource-markup-extension.md))，因此這裡使用了兩個巢狀標記延伸。 內部的標記延伸會先進行評估，因此一旦取得資源之後，就會有一個可供繫結使用的實用 [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) (由資源中的 `local:S2Formatter` 元素所具現化的自訂類別)。

## <a name="tools-support"></a>工具支援

在 XAML 標記編輯器中撰寫 **{Binding}** 時，Microsoft Visual Studio 中的 Microsoft IntelliSense 會顯示資料內容的屬性。 在您輸入 "{Binding" 後，適用於 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 的資料內容屬性即會顯示在下拉式清單中。 IntelliSense 也可協助 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 的其他屬性。 若要讓此功能運作，您必須在標記頁面中設定資料內容或設計階段資料內容。 **移至定義** (F12) 也可以用於 **{Binding}**。 另一種方法是使用資料繫結對話方塊。

 
