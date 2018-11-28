---
description: 您可以使用 PropertyPath 類別和字串語法，具現化 XAML 或程式碼中的 PropertyPath 值。
title: Property-path 語法
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f0f49792a92010f97c8388540fd63c38eed5f75e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7838761"
---
# <a name="property-path-syntax"></a>Property-path 語法


您可以使用 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 類別和字串語法，具現化 XAML 或程式碼中的 **PropertyPath** 值。 資料繫結會使用 **PropertyPath** 值。 而目標腳本動畫也會使用類似的語法。 在這兩個案例中，屬性路徑描述了一或多個物件-屬性關係的周遊，這些關係最後會解析為單一屬性。

您可以直接將屬性路徑字串設定為 XAML 中的屬性。 您可以使用相同的字串語法來建構 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)，以便在程式碼中設定 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)，或是使用 [**SetTargetProperty**](https://msdn.microsoft.com/library/windows/apps/br210503) 在程式碼中設定動畫目標。 Windows 執行階段中有兩個獨特的功能領域會使用屬性路徑：資料繫結和動畫目標。 在 Windows 執行階段實作中，動畫目標不會建立基礎的 Property-path 語法值，它會將資訊保持為字串，但是這兩者的物件-屬性周遊的概念非常類似。 資料繫結與動畫目標在評估屬性路徑上有些微的差異，所以我們會分別描述它們的屬性路徑語法。

## <a name="property-path-for-objects-in-data-binding"></a>資料繫結中物件的屬性路徑

在 Windows 執行階段中，您可以繫結到任何相依性屬性的目標值。 資料繫結的來源屬性值不一定要是相依性屬性；它可以是商務物件上的屬性 (例如使用 Microsoft .NET 語言或 C++ 編寫的類別)。 或者，繫結值的來源物件可以是應用程式已經定義的現有相依性物件。 若要參考來源，您可以使用簡單的屬性名稱，或使用商務物件的物件圖形中物件-屬性關係的周遊。

您可以繫結到個別屬性值或繫結到保存清單或集合的目標屬性。 如果您的來源是集合，或者如果路徑指定了集合屬性，則資料繫結引擎會將來源的集合項目與繫結目標進行比對，進而產生將資料來源集合的項目清單填入 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) 而不需要預期該集合中特定項目的這類行為。

### <a name="traversing-an-object-graph"></a>周遊物件圖形

在物件圖形中標示物件-屬性關係周遊的語法元素是點 (**.**) 字元。 屬性路徑字串中的每個點表示物件 (點的左邊) 與該物件之屬性 (點的右邊) 間的分隔。 字串的會由左到右逐步評估多個物件-屬性的關係。 讓我們看一下範例：

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

以下是這個路徑的評估方式：

1.  針對資料內容物件 (或同一個 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 所指定的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832)) 搜尋一個名為 "Customer" 的屬性。
2.  針對物件 ("Customer" 屬性的值) 搜尋一個名為 "Address" 的屬性。
3.  針對物件 ("Address" 屬性的值) 搜尋一個名為 "StreetAddress1" 的屬性。

在每一個步驟中，每個值均被視為一個物件。 只有當繫結套用到特定屬性時，才會檢查結果類型。 如果 "Address" 只是字串值，而且沒有公開字串中哪個部分是街道地址，則這個範例就會失敗。 一般而言，繫結是指商務物件 (具有已知和明確資訊結構) 的特定巢狀屬性值。

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>資料繫結屬性路徑中屬性的規則

-   屬性路徑參考的所有屬性在來源商務物件中必須是公開的。
-   結尾屬性 (即路徑中最後一個具名屬性) 必須是公開且可變的 (您不能繫結到靜態值)。
-   如果這個路徑在雙向繫結中做為 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 資訊，則結尾屬性必須可以被讀取/寫入。

### <a name="indexers"></a>索引子

資料繫結的屬性路徑可以包含索引屬性的參考。 這樣便能繫結到已排序的清單/向量，或繫結到字典/對應。 使用方括號 "\[\]" 字元來表示索引屬性。 這種括號中的內容可以是整數 (已排序的清單) 或未加引號的字串 (字典)。 您也可以繫結到索引鍵為整數的字典。 您可以在相同的路徑中使用不同的索引屬性，並使用點來分隔物件-屬性。

例如，假設有一個含 "Teams" 清單 (已排序清單) 的商務物件，每一個清單都包含一個 "Players" 字典，其中每個玩家都以姓氏做為索引鍵。 則第二隊特定玩家的屬性路徑範例是："Teams\[1\].Players\[Smith\]" (您使用 1 來指出 "Teams" 中的第二個項目，因為這個清單是從 0 開始建立索引。)

**注意：** 對 c + + 資料來源的索引功能支援是有限的;請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)。

### <a name="attached-properties"></a>附加屬性

屬性路徑可以包含對附加屬性的參考。 因為附加屬性的識別名稱已經包含點，所以您必須將任何附加屬性名稱放入括號內，這樣點就不會被視為物件-屬性步驟。 例如，用來指定使用 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) 做為繫結路徑的字串是 "(Canvas.ZIndex)"。 如需附加屬性的詳細資訊，請參閱[附加屬性概觀](attached-properties-overview.md)。

### <a name="combining-property-path-syntax"></a>合併屬性路徑的語法

您可以將屬性路徑語法的各種元素合併為單一字串。 例如，如果您的資料來源包含參考索引附加屬性的屬性路徑，您可以定義該屬性路徑。

### <a name="debugging-a-binding-property-path"></a>偵錯繫結屬性路徑

因為屬性路徑由繫結引擎加以解譯而且必須使用僅在執行階段出現的資訊，所以您必須經常偵錯繫結的屬性路徑，而不能依賴開發工具中對設計階段或編譯階段的傳統支援。 在許多情況下，執行階段無法解析屬性路徑所產生的結果是沒有錯誤的空白值，因為這是程式設計原本設定的繫結解析後援行為。 所幸，Microsoft Visual Studio 提供的偵錯輸出模式可以在指定繫結來源的屬性路徑中獨立指出哪個部分無法解析。 如需使用這項開發工具功能的詳細資訊，請參閱[深入了解資料繫結的＜偵錯＞一節](../data-binding/data-binding-in-depth.md#debugging)。

## <a name="property-path-for-animation-targeting"></a>動畫目標的屬性路徑

動畫的目標是相依性屬性，動畫執行時會在相依性屬性套用腳本值。 為了識別含有動畫屬性的物件，動畫會使用名稱 ([x:Name 屬性](x-name-attribute.md)) 來設定目標元素。 通常必須定義以 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) 識別的物件開頭，並以應該套用動畫的特定相依性屬性值結束的屬性路徑。 這個屬性路徑可用來做為 [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/hh759824) 的值。

如需如何在 XAML 中定義動畫的詳細資訊，請參閱[腳本動畫](https://msdn.microsoft.com/library/windows/apps/mt187354)。

## <a name="simple-targeting"></a>簡單目標

如果您要在位於目標物件本身的屬性產生動畫效果，而且該屬性的類型具備可直接套用其本身的動畫 (而不是套用到屬性值的子屬性)，則您只需命名要產生動畫效果的屬性，而不用進一步限定。 例如，如果您要以 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 子類別 (如 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)) 為目標，並要將動畫的 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 套用到 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 屬性，則屬性路徑可以是 "Fill"。

## <a name="indirect-property-targeting"></a>間接屬性目標

您可以為目標物件的子屬性產生動畫效果。 換句話說，如果有一個目標物件的屬性是物件本身，且該物件具有屬性，您必須定義屬性路徑，說明如何逐步設定該物件-屬性關係。 如需指定要讓其子屬性產生動畫效果的物件，您必須將屬性名稱放入括號內，並以 *typename*.*propertyname* 格式指定屬性。 例如，若要指定目標物件 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980) 屬性的物件值，第一個步驟是在屬性路徑中指定 "(UIElement.RenderTransform)"。 這還不是完整的路徑，因為沒有動畫可以直接套用到 [**Transform**](https://msdn.microsoft.com/library/windows/apps/br243006) 值。 所以，在此例中，您現在要完成整個屬性路徑，讓結尾屬性成為可以使用 **Double** 值來產生動畫效果的 **Transform** 子類別屬性："(UIElement.RenderTransform).(CompositeTransform.TranslateX)"

## <a name="specifying-a-particular-child-in-a-collection"></a>指定集合中的特定子項

若要指定集合屬性中的子項目，可以使用數值索引子。 請將整數索引值括在方括號 "\[\]" 字元內。 您只可以參考已排序的清單，不可以參考字典。 因為集合不是可以產生動畫效果的值，所以屬性路徑的結尾屬性永遠不可以使用索引子。

例如，若要在套用到控制項[**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 屬性的 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/br210108) 中指定第一個色彩停止點的顏色動畫，則屬性路徑如下："(Control.Background).(GradientBrush.GradientStops)\[0\].(GradientStop.Color)"。 您應該注意到索引子不是路徑的最後一段，而最後一段必須明確地參考集合中項目 0 的 [**GradientStop.Color**](https://msdn.microsoft.com/library/windows/apps/br210094) 屬性，以便套用 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 動畫值。

## <a name="animating-an-attached-property"></a>讓附加屬性產生動畫效果

這不是常見情況；不過，只要附加屬性具有符合動畫類型的屬性值，就可以讓它產生動畫效果。 因為附加屬性的識別名稱已經包含點，所以您必須將任何附加屬性名稱放入括號內，這樣點就不會被視為物件-屬性步驟。 例如，用來指定讓物件上 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) 附加屬性產生動畫效果的字串會使用屬性路徑 "(Grid.Row)"。

**注意：** 針對此範例， [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795)的值是**Int32**屬性類型。 所以您不可以使用 **Double** 動畫讓它產生動畫效果。 您必須改為定義具有 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) 元件的 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)，其中 [**ObjectKeyFrame.Value**](https://msdn.microsoft.com/library/windows/apps/br210344) 會設定為 "0" 或 "1" 的整數。

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>動畫目標屬性路徑中的屬性規則

-   屬性路徑的假設起點是 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) 識別的物件。
-   屬性路徑中參考的所有物件及屬性必須是公開的。
-   結尾屬性 (即路徑中最後一個具名屬性) 必須是公開、唯讀且必須是相依性屬性。
-   結尾屬性必須具有能夠使用其中一個動畫類型廣泛類別產生動畫效果的屬性類型，這些動畫類型包括：[**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 動畫、**Double** 動畫、[**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 動畫、[**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)。

## <a name="the-propertypath-class"></a>PropertyPath 類別

[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 類別是用於繫結案例的 [**Binding.Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 相關屬性類型。

您通常可以在 XAML 中直接套用 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)，完全不需要使用任何程式碼。 但在某些情況下，您可能要使用程式碼來定義 **PropertyPath** 物件，並在執行階段將它指派給屬性。

[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 具有一個 [**PropertyPath(String)**](https://msdn.microsoft.com/library/windows/apps/br244261) 建構函式，而且不具有預設建構函式。 傳遞到這個建構函式的字串是使用屬性路徑語法定義的字串，如本文稍早的說明。 這也是用來將 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 指派為 XAML 屬性的同一個字串。 **PropertyPath** 類別唯一的另一個 API 是唯讀的 [**Path**](https://msdn.microsoft.com/library/windows/apps/br244260) 屬性。 您可以使用這個屬性做為另一個 **PropertyPath** 執行個體的建構字串。

## <a name="related-topics"></a>相關主題

* [深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [腳本動畫](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [{Binding} 標記延伸](binding-markup-extension.md)
* [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)
* [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**Binding 建構函式**](https://msdn.microsoft.com/library/windows/apps/br209825)
* [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)

