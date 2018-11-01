---
author: jwmsft
description: 我們將說明 XAML 語法規則，以及說明描述 XAML 語法可用之限制或選項的詞彙。
title: XAML 語法指南
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1fe2460dfc5ab11a9168f1d1d87207d2b9490026
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5923288"
---
# <a name="xaml-syntax-guide"></a>XAML 語法指南


我們將說明 XAML 語法規則，以及說明描述 XAML 語法可用之限制或選項的詞彙。 如果您是剛開始使用 XAML 語言的使用者，或是想複習語法的詞彙或特定部分的使用者，或者對於 XAML 語言運作方式感到好奇且需要更多背景與內容的使用者，您會發現本主題非常有用。

## <a name="xaml-is-xml"></a>XAML 是一種 XML 檔案

Extensible Application Markup Language (XAML) 具有以 XML 為基礎的語法，而且根據定義，任何有效的 XAML 必須是有效的 XML。 但是 XAML 也有它自己的延伸 XML 的語法概念。 指定的 XML 實體在純 XML 中可能有效，但該語法如果是 XAML 形式，則可能有不同或更完整的意義。 本主題說明這些 XAML 語法的概念。

## <a name="xaml-vocabularies"></a>XAML 詞彙

XAML 與大部分 XML 用法不同的一點是，通常不會使用結構描述 (例如 XSD 檔案) 強制執行 XAML。 這是因為 XAML 預期是可延伸的，這就是 XAML 縮寫中的 "X" 所代表的意義。 剖析 XAML 之後，您在 XAML 中參考的元素和屬性預期會存在於部分負責支援的程式碼表示法中，可能是在 Windows 執行階段所定義的核心類型中，或者是在延伸或採用 Windows 執行階段的類型中。 SDK 文件有時會參考已經內建於 Windows 執行階段的類型，並且可以在 XAML 中用來做為 Windows 執行階段的「XAML 詞彙」**。 Microsoft Visual Studio 可以協助您在此 XAML 詞彙中產生有效的標記。 Visual Studio 也可以包含您針對 XAML 用法自訂的類型，只要您在專案中正確參考這些類型的來源即可。 如需 XAML 和自訂類型的詳細資訊，請參閱 [XAML 命名空間與命名空間對應](xaml-namespaces-and-namespace-mapping.md)。

##  <a name="declaring-objects"></a>宣告物件

程式設計師通常要考量物件與成員，而標記語言則被概念化為元素與屬性。 從最基本的意義上來說，您在 XAML 標記中宣告的元素，會成為負責支援的執行階段物件表示法中的物件。 若要為您的應用程式建立一個執行階段物件，您需要在 XAML 標記中宣告 XAML 元素。 該物件會在 Windows 執行階段載入您的 XAML 時建立。

XAML 檔案的根目錄永遠只有一個元素，其中宣告了將會做為某些程式設計結構之概念根目錄的物件 (例如一個頁面)，或應用程式整個執行階段定義的物件圖形。

以 XAML 語法而言，有三種方式可以在 XAML 中宣告物件：

-   **直接使用物件元素語法：** 這會使用開頭和結尾標記將物件具現化為 XML 格式元素。 您可以使用這個語法來宣告根物件，或建立設定屬性值的巢狀物件。
-   **間接使用屬性語法：** 這會使用內嵌字串值，此字串值包含如何建立物件的指示。 XAML 剖析器會使用這個字串將屬性值設成新建立的參考值。 對它的支援僅限於特定的通用物件和屬性。
-   使用標記延伸。

這不表示在 XAML 詞彙中您總是可以選擇任何語法來建立物件。 部分物件只能使用物件元素語法來建立。 部分物件則只能透過一開始就在屬性中設定來建立。 事實上，可以使用物件元素或屬性語法來建立的物件，在 XAML 詞彙中是相對少見的。 儘管這兩種語法格式都是可能使用的格式，但就樣式而言，其中一種語法會是較為通用的格式。
在 XAML 中也有一些技術可供您用來參考現有的物件，而不必建立新的值。 現有的物件可能是定義在 XAML 的其他區域中，或是透過平台及其應用程式或程式設計模型的一些行為以隱含的方式存在。

### <a name="declaring-an-object-by-using-object-element-syntax"></a>使用物件元素語法宣告物件

如果要使用物件元素語法來宣告物件，您可以依照下列方式撰寫標記：`<objectName>  </objectName>`，其中 *objectName* 是您要具現化之物件的類型名稱。 以下是宣告 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 物件的物件元素用法：

```xml
<Canvas>
</Canvas>
```

如果物件未包含其他物件，您可以使用一個自我結尾標記 (而不使用開頭/結尾標記組) 來宣告物件元素： `<Canvas />`

### <a name="containers"></a>容器

許多做為 UI 元素使用的物件 (像是 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)) 可以包含其他物件。 這些有時稱為容器。 下列範例顯示只包含一個元素 ([**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)) 的 **Canvas** 容器。

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>使用屬性語法來宣告物件

由於這個行為與屬性設定密切相關，我們將在接下來的小節中進一步討論。

### <a name="initialization-text"></a>初始化文字

對於某些物件，您可以使用被當作建構初始化值的內部文字來宣告新值。 在 XAML 中，這個方法與語法稱為「初始化文字」**。 在概念上，初始化文字類似於呼叫含有參數的建構函式。 為某些結構設定初始值時，初始化文字就很有用。

當您希望結構值含有 **x:Key** 以便讓它存在於 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中時，通常就會使用物件元素語法搭配初始化文字。 如果您將該結構值在多個目標屬性之間共用，就可以這麼做。 對某些結構來說，您無法使用屬性語法來設定結構值：初始化文字是可以產生有用且可共用的 [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/br242343)、[**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)、[**GridLength**](https://msdn.microsoft.com/library/windows/apps/br208754) 或 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 資源的唯一方法。

下列的簡短範例使用初始化文字指定 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 的值，這個案例指定的值將 **Left** 與 **Right** 設為 20，而 **Top** 與 **Bottom** 設為 10。 這個範例顯示建立為索引鍵來源的 **Thickness**，和該資源的參考。 如需 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 初始化文字的詳細資訊，請參閱 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)。

```xml
<UserControl ...>
  <UserControl.Resources>
    <Thickness x:Key="TwentyTenThickness">20,10</Thickness>
    ....
  </UserControl.Resources>
  ...
  <Grid Margin="{StaticResource TwentyTenThickness}">
  ...
  </Grid>
</UserControl ...>
```

**注意：** 某些結構不能被宣告為物件元素。 初始化文字不受支援並且不能當作資源使用。 您必須使用屬性語法，以便在 XAML 中將屬性設成這些值。 這些類型包含：[**Duration**](https://msdn.microsoft.com/library/windows/apps/br242377)、[**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/br210411)、[**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)、[**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 以及 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)。

## <a name="setting-properties"></a>設定屬性

您可以使用物件元素語法，在您宣告的物件中設定屬性。 有多種方法可以在 XAML 中設定屬性：

-   使用屬性語法。
-   使用屬性元素語法。
-   使用元素語法，其中內容 (內部文字或子元素) 設定了物件的 XAML 內容屬性。
-   使用集合語法 (通常是隱含集合語法)。

和在物件宣告一樣，這份清單並不表示每一項方法都可用來設定任何屬性。 某些屬性僅支援其中一種方法。
某些屬性支援一種以上的格式；例如，有的屬性可以使用屬性元素語法或屬性語法。 使用哪一種語法，取決於屬性及該屬性所使用的物件類型。 在 Windows 執行階段 API 參考中，您將會在 **\[語法\]** 區段中看見可使用的 XAML 用法。 有時會提供可運作但更詳細的替代用法。 這些詳細的用法不一定會顯示，因為我們正嘗試為您顯示在 XAML 中使用該屬性的最佳做法或真實案例。 參考頁面的 **\[XAML 用法\]** 區段中提供了可在 XAML 中設定之屬性的 XAML 語法指導方針。

有一些物件屬性無法以任何方式在 XAML 中設定，只能使用程式碼來設定。 這些通常是較適合在程式碼後置而不是在 XAML 中使用的屬性。

唯讀屬性不能在 XAML 中設定。 甚至在程式碼中，擁有者類型也必須支援某些其他方式才能設定它，例如建構函式多載、Helper 方法或計算屬性支援。 計算屬性有賴於其他可設定屬性的值，有時再加上含有內建處理的事件；在相依性屬性系統中都有提供這些功能。 如需相依性屬性對計算屬性支援如何有用的詳細資訊，請參閱[相依性屬性概觀](dependency-properties-overview.md)。

XAML 中的集合語法看起來像是您正在設定唯讀屬性，但實際上並非如此。 請參閱本主題中稍後的＜使用集合語法設定屬性＞一節。

### <a name="setting-a-property-by-using-attribute-syntax"></a>使用屬性語法設定屬性

設定屬性值是在標記語言 (如 XML 或 HTML) 中設定屬性值的典型方法。 XAML 屬性的設定方式與在 XML 中設定屬性值的方式類似。 屬性名稱是在元素名稱之後標記內的任何一個點指定，與元素名稱之間至少間隔一個空格。 屬性名稱後面跟著等號。 屬性值是包含在一組引號內。 引號可以是雙引號或單引號，只要引號是成對的並且括住值即可。 屬性值本身需以字串表示。 這個字串通常包含數字，但對 XAML 來說，在 XAML 剖析器參與並且執行一些基本值轉換之前，所有屬性值都是字串值。

下列範例使用四個屬性的屬性語法設定 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 物件的 [**Name**](https://msdn.microsoft.com/library/windows/apps/br208735)、[**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 以及 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)。

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>使用屬性元素語法設定屬性

物件的許多屬性可以使用屬性元素語法來設定。 屬性元素看起來如下：`<`*object*`.`*property*`>`.

如果要使用屬性元素語法，您可以為要設定的屬性建立 XAML 屬性元素。 在標準 XML 中，這個元素只會被當做名稱中含有點的元素。 不過，在 XAML 中，元素名稱中的點會將元素識別為屬性元素，而 *property* 則會是支援的物件模型實作中的 *object* 成員。 如果要使用屬性元素語法，就必須要能夠指定物件元素，才能「填滿」屬性元素標記。 屬性元素一律會有一些內容 (單一元素、多個元素或內部文字)；自我結尾屬性元素的存在並沒有任何意義。

在下列文法中，*property* 是您要設定之屬性的名稱，*propertyValueAsObjectElement* 是一個要用來滿足該屬性的值類型需求的單一物件元素。

`<`*object*`>`

`<`*object*`.`*property*`>`

*propertyValueAsObjectElement*

`</`*object*`.`*property*`>`

`</`*object*`>`

下列範例使用屬性元素語法來設定含有 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 物件元素之 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) (在 **SolidColorBrush** 內，[**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 設定為屬性)。這個 XAML 的剖析結果，與使用屬性語法設定 **Fill** 的上一個 XAML 範例相同。

```xml
<Rectangle
  Name="rectangle1"
  Width="100" 
  Height="100"
> 
  <Rectangle.Fill> 
    <SolidColorBrush Color="Blue"/> 
  </Rectangle.Fill>
</Rectangle>
```

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>XAML 詞彙和物件導向程式設計

當屬性和事件顯示為 Windows 執行階段 XAML 類型的 XAML 成員時，通常是繼承自基底類型。 請思考這個範例：`<Button Background="Blue" .../>`。 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 屬性不是一個在 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 類別上立即宣告的屬性。 反而，**Background** 是繼承自基底 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 類別。 實際上，如果您查看 **Button** 的參考主題，您將會看到成員清單對於連續基底類別鏈結中的每個類別至少都各包含一個繼承成員：[**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736)、[**Control**](https://msdn.microsoft.com/library/windows/apps/br209390)、[**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)、[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)、[**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)。 在 **\[屬性\]** 清單中，以 XAML 詞彙來說，所有讀寫屬性和集合屬性都是繼承而來的。 事件 (例如各種 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 事件) 也是繼承而來的。

如果您使用 Windows 執行階段參考做為 XAML 指導，語法中或甚至是程式碼範例中顯示的元素名稱有時會用於原先定義屬性的類型，因為從基底類別繼承該參考主題的所有可能類型都會共用該參考主題。 如果您在 XML 編輯器中針對 XAML 使用 Visual Studio 的 IntelliSense，IntelliSense 及其下拉式清單在聯合繼承項目和提供精確的屬性清單上都有傑出的表現，這些屬性是一旦您已經開始著手於某個類別執行個體的物件元素時即可供設定的屬性。

### <a name="xaml-content-properties"></a>XAML 內容屬性

某些類型會定義它們的其中一個屬性，讓該屬性啟用 XAML 內容語法。 對於類型的 XAML 內容屬性，您可以在 XAML 中指定該屬性時，省略該屬性的屬性元素。 或者，您可以將該屬性設定到內部文字值，方法是直接在擁有者類型的物件元素標記內提供該內部文字。 XAML 內容屬性針對該屬性支援直接標記語法，透過減少巢狀結構的方式，讓一般人更容易看懂 XAML。

如果有可用的 XAML 內容語法，該語法就會顯示在 Windows 執行階段參考文件裡該屬性之 **\[語法\]** 的 [XAML] 區段中。 例如，[**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 的 [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) 屬性頁會顯示 XAML 內容語法，而非設定 **Border** 的單一物件 **Border.Child** 值的屬性元素語法，如下：

```xml
<Border>
  <Button .../>
</Border>
```

如果宣告為 XAML 內容屬性的屬性是 **Object** 類型或 **String** 類型，則 XAML 內容語法就能支援 XML 文件模型中基本上視為內部文字的字串：開頭與結尾物件標記之間的字串。 例如，[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 的 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 屬性頁會顯示含有要設定 **Text** 的內部文字值的 XAML 內容語法，但是標記中從未出現 "Text" 這個字串。 這裡提供一個範例用法：

```xml
<TextBlock>Hello!</TextBlock>
```

如果某個類別有 XAML 內容屬性，在＜屬性＞小節裡該類別的參考主題中就會指出該內容屬性。 請尋找 [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) 的值。 這個屬性使用具名欄位 "Name"。 "Name" 的值是身為 XAML 內容屬性的該類別屬性的名稱。 例如，在 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 參考頁上，您會看到：ContentProperty("Name=Child")。

其中一個我們應該提到的重要 XAML 語法規則就是，您不能將 XAML 內容屬性與您在元素上設定的其他屬性元素混合使用。 XAML 內容屬性必須在任何屬性元素之前或之後以整體方式設定。 例如，下列是無效的 XAML：

``` syntax
<StackPanel>
  <Button>This example</Button>
  <StackPanel.Resources>
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>
  </StackPanel.Resources>
  <Button>... is illegal XAML</Button>
</StackPanel>
```

## <a name="collection-syntax"></a>集合語法

目前為止顯示的所有語法，都是將屬性設定為單一物件。 但是，許多 UI 案例卻需要指定的父元素可以包含多個子元素。 例如，某個輸入表單的 UI 需要數個文字方塊元素、一些標籤，或者是一個 "Submit" 按鈕。 然而，如果您要使用程式撰寫物件模型來存取這些多重元素，它們通常是單一集合屬性中的項目，而非每個項目各為不同屬性的值。 XAML 可以透過將使用集合類型的屬性視為隱含屬性，並為一個集合類型的任何子元素執行特殊處理，支援多重子元素以及支援一般的支援集合模型。

許多集合屬性也會被識別為類別的 XAML 內容屬性。 隱含集合處理與 XAML 內容語法的組合在用於控制項組合的類型中很常見，例如面板、檢視或項目控制項。 例如，下列範例顯示在 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) 內組合兩個對等 UI 元素可能使用的最簡單 XAML。

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>XAML 集合語法的機制

一開始看起來可能像是 XAML 正在啟用「一組」唯讀的集合屬性。 事實上，XAML 在這裡是在新增項目到現有的集合。 實作 XAML 支援的 XAML 語言與 XAML 處理器需要支援的集合類型中的一個慣例，才能啟用這個語法。 通常會有一個參考集合的特定項目的支援屬性，例如索引子或 **Items** 屬性。 該屬性在 XAML 語法中一般並不明確。 對集合來說，XAML 剖析的基礎機制不是屬性，而是方法：在大多數案例中，尤其是 **Add** 方法。 當 XAML 處理器在 XAML 集合語法內遇到一或多個物件元素時，會先從某個元素建立每個這類物件，然後再呼叫集合的 **Add** 方法，依序將每個新物件新增到包含的集合。

當 XAML 剖析器將項目新增到集合後，是由 **Add** 方法的邏輯判斷指定的 XAML 元素是否為集合物件的允許項目子系。 許多集合類型是支援的實作的強類型，表示 **Add** 的輸入參數預期無論傳遞什麼項目，都必須是符合 **Add** 參數類型的類型。

對於集合屬性，當您嘗試將集合明確地指定為物件元素時，請小心。 XAML 剖析器會在每次遇到物件元素時就建立一個新物件。 如果您正嘗試使用的集合屬性是唯讀的，這可能會導致擲回 XAML 剖析例外狀況。 只使用隱含集合語法，您就不會看到該例外狀況。

## <a name="when-to-use-attribute-or-property-element-syntax"></a>使用屬性語法或屬性元素語法的時機

所有支援在 XAML 中設定的屬性，都會支援用於直接值設定的屬性或屬性元素語法，但可能不支援交替使用上述任一種語法。 某些屬性確實支援上述任一種語法，而某些屬性則支援其他的語法選項，例如 XAML 內容屬性。 屬性所支援的 XAML 語法類型取決於該屬性當作其屬性類型使用的物件類型。 如果屬性類型是基本類型 (例如雙精準數 (浮點數或十進位數)、整數、布林值或字串)，則該屬性一律支援屬性語法。

如果可以透過處理字串建立您用來設定屬性的物件類型，您也可以使用屬性語法來設定該屬性。 以基本類型來說，一律是這樣的情況，類型轉換是內建在剖析器中。 不過，某些其他物件類型也可以透過使用指定為屬性值的字串 (而不是屬性元素內的物件元素) 來建立。 如果要讓這個方法能夠運作，必須使用基礎類型轉換，這項轉換是由該特定屬性支援，或普遍對使用該屬性類型的所有值支援。 屬性的字串值會用來設定對新物件值初始化來說重要的屬性。 依據轉換器如何唯一處理字串中的資訊而定，特定的類型轉換器可能也可以建立常見屬性類型的不同子類別。 在這個參考文件的語法章節中，將列出支援這個行為的物件類型的特殊文法。 舉例來說，[**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 的 XAML 語法顯示如何使用屬性語法，為任何類型為 **Brush** 的屬性建立新的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 值 (Windows 執行階段 XAML 中有許多 **Brush** 屬性)。

## <a name="xaml-parsing-logic-and-rules"></a>XAML 剖析邏輯與規則

有時，以類似 XAML 剖析器必須判讀 XAML 的方式來判讀 XAML 會相當具參考價值：當成一組字串語彙基元依直線順序判讀。 XAML 剖析器必須在一組規則下解譯這些語彙基元，而該組規則是 XAML 運作方式定義的一部分。

設定屬性值是在標記語言 (如 XML 或 HTML) 中設定屬性值的典型方法。 在下列語法中，*objectName* 是您要具現化的物件，*propertyName* 是您要在該物件設定的屬性的名稱，*propertyValue* 是要設定的值。

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

任一種語法都可以宣告物件，並在該物件設定屬性。 雖然第一個範例是標記中的單一元素，但這裡有實際的不同步驟與 XAML 處理器如何剖析這個標記有關。

首先，物件元素的顯示狀態指示必須具現化新的 *objectName* 物件。 只有在這個執行個體存在後，才能在該執行個體中設定執行個體屬性 *propertyName*。

另一個 XAML 規則是元素的屬性必須能以任何順序設定。 例如，`<Rectangle Height="50" Width="100" />` 和 `<Rectangle Width="100"  Height="50" />` 並無差別。 使用哪個順序只是樣式上的偏好。

**注意：** XAML 設計工具常常會將排序慣例升級，如果您使用設計介面而非 XML 編輯器中，但您可以自由地編輯該 XAML 更新版本，將屬性重新排序或引進新的。

## <a name="attached-properties"></a>附加屬性

XAML 透過新增名為「附加屬性」** 的語法元素延伸了 XML 的功能。 附加屬性語法與屬性元素語法類似，它也包含點，而且這個點對於 XAML 剖析來說有特殊的意義。 具體地說，點分隔了附加屬性的擁有者提供者以及屬性名稱。

在 XAML 中，您使用語法 *AttachedPropertyProvider*.*PropertyName* 來設定附加屬性。 這裡是如何在 XAML 中設定附加屬性 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 的範例：

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

當元素的支援類型中沒有該名稱的屬性時，您可以在那些元素上設定附加屬性，藉由這個方式，元素的作用就會像是全域屬性，或是由不同的 XML 命名空間所定義的屬性，例如 **xml:space** 屬性。

在 Windows 執行階段 XAML 中，您會看到支援下列案例的附加屬性：

-   子元素可以通知父容器面板它們在配置中應該有的行為：[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)、[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)、[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227651)。
-   控制項用法可以影響來自控制項範本的重要控制項部分的行為：[**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)、[**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/br227689)。
-   使用可以在相關類別中取得的服務，而使用它的服務和類別不會共用繼承：[**Typography**](https://msdn.microsoft.com/library/windows/apps/hh702143)、[**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021)、[**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081)、[**ToolTipService**](https://msdn.microsoft.com/library/windows/apps/br227609)。
-   動畫目標設定：[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)。

如需詳細資訊，請參閱[附加屬性概觀](attached-properties-overview.md)。

## <a name="literal--values"></a>常值 "{" 的值

由於左大括號符號 { 是標記延伸序列的開頭，因此您可以使用逸出序列來指定開頭是 "\{" 的常值字串值。 逸出序列為 "\{\}"。 例如，如果要指定單一左大括號的字串值，請將屬性值指定為 "\{\}\{"。 您也可以使用替代引號 (例如，在以 **""** 分隔的屬性值內使用 **'**) 來提供 "\{" 值做為字串。

**注意：** 在引號的屬性內時，也適用於 「 \}"。
 
## <a name="enumeration-values"></a>列舉值

Windows 執行階段 API 中的許多屬性都使用列舉做為值。 如果成員是讀寫屬性，您就可以提供屬性值來設定這樣的屬性。 您可以透過使用常數名稱的不完整名稱，識別要使用哪個列舉值做為屬性的值。 例如，以下是如何在 XAML 中設定 [**UIElement.Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)：`<Button Visibility="Visible"/>`。 這裡的 "Visible" 是當作字串使用，可直接對應到 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006) 列舉的具名常數 **Visible**。

-   請勿使用完整格式，這會沒有作用。 例如，下列是無效的 XAML：`<Button Visibility="Visibility.Visible"/>`。
-   不要使用常數的值。 換句話說，無論列舉定義為明確或隱含，都不要依賴列舉整數值。 儘管看起來可行，但是這在 XAML 或程式碼中都是一個不好的做法，因為您會依賴可能是暫時性實作的詳細資料。 例如，不要這樣做：`<Button Visibility="1"/>`。

**注意：** 中使用 XAML 和使用列舉的 Api 參考主題，按一下 [**屬性值**] 區段中的**語法**列舉類型連結。 這會連結到列舉頁面，您可以在這裡探索該列舉的具名常數。

列舉可以是旗標的形式，這表示列舉具備 **FlagsAttribute** 屬性。 如果您需要為旗標形式的列舉指定一個值組合來做為 XAML 屬性值，請使用每個列舉常數的名稱，在每個名稱之間加上逗號 (,)，中間不要有空格字元。 旗標形式的屬性在 Windows 執行階段的 XAML 詞彙中並不常用，但是，[**ManipulationModes**](https://msdn.microsoft.com/library/windows/apps/br227934) 是在 XAML 中設定旗標形式列舉值的支援範例。

## <a name="interfaces-in-xaml"></a>XAML 中的介面

在少數情況下，您會看到屬性類型為介面的 XAML 語法。 在 XAML 類型系統中，分析時可以接受實作該介面的類型做為值。 必須要有這種類型已建立的例項可用，才能做為值。 您會在 [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736) 的 [**Command**](https://msdn.microsoft.com/library/windows/apps/br227740) 和 [**CommandParameter**](https://msdn.microsoft.com/library/windows/apps/br227741) 屬性的 XAML 語法中看到用來做為類型的介面。 這些屬性支援 Model-View-ViewModel (MVVM) 設計模式，其中 **ICommand** 介面是檢視和模型互動方式的協定。

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>Windows 執行階段參考中的 XAML 預留位置慣例

如果您仔細看過可使用 XAML 的 Windows 執行階段 API 參考主題的任何 **\[語法\]** 區段，可能會看到語法包含非常多的預留位置。 XAML 語法是不同於 C#、 Microsoft Visual Basic 或 VisualC + + 元件延伸 (C + + /CX) 語法因為 XAML 語法是一種使用語法。 這是在您自己的 XAML 檔案中給予提示的最終用法，但是不要過度限制您可以使用的值。 所以通常用法會描述一種混合常值和預留位置的文法，並在 **\[XAML 值\]** 區段中定義部分預留位置。

當您在屬性的 XAML 語法中看到類型名稱/元素名稱時，顯示的名稱是原來定義屬性的類型名稱。 但是 Windows 執行階段 XAML 支援以 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 為基礎之類別的類別繼承模型。 因此，您通常可以在類別上使用屬性，該類別並非實際定義類別，而是改為從最初定義屬性 (Property)/屬性 (Attribute) 的類別衍生。 例如，您可以在任何使用深度繼承的 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 衍生類別上，將 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 設定為屬性。 例如：`<Button Visibility="Visible" />`。 因此，不要照字面解釋任何 XAML 用法語法中顯示的元素名稱；此語法或許能供代表該類別的元素使用，同時也可供代表衍生類別的元素使用。 如果類型在實際用法中極少或不可能顯示為定義元素，該類型名稱在語法中會特別以小寫顯示。 例如，您看到的 **UIElement.Visibility** 語法為：

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

許多 XAML 語法區段都會在「用法」中包含預留位置，然後在 **\[語法\]** 區段之下的 **\[XAML 值\]** 區段定義用法。

XAML 用法區段也使用各種一般化的預留位置。 這些預留位置不會每次在 **\[XAML 值\]** 中重新定義，因為您可以猜想到或是最後都能了解它們所代表的意義。 我們認為大部分的讀者應該都不想在 **\[XAML 值\]** 中重複看到它們出現，所以定義中予以省略。 如果需要參考資料，以下是部分預留位置及它們以廣義來說所代表的意義：

-   *object*：理論上是任何物件值，但實際上通常限制為特定的物件類型，例如，string-or-object 選項，詳細資訊請參閱參考頁面的＜備註＞。
-   *object* *property*：當顯示的語法是類型的語法，而該類型可用來做為許多屬性的屬性值時，會將 *object* 和 *property* 搭配使用。 例如，針對 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 顯示的 [**XAML 屬性用法**] 包含：&lt;*object* *property*="*predefinedColorName*"/&gt;
-   *eventhandler*：這會顯示為事件屬性所顯示的每個 XAML 語法的屬性值。 您在這裡所提供的資訊，就是事件處理常式函式的函式名稱。 該函式必須定義在 XAML 頁面的程式碼後置中。 在程式設計層級，該函式必須符合您所處理事件的委派簽章，否則無法編譯應用程式程式碼。 不過這實際上是程式設計方面的考量，而非 XAML 的考量，所以我們不會嘗試提示任何關於 XAML 語法中的委派類型。 如果您想要知道應該為事件實作的委派，請參閱事件參考主題的 **\[事件資訊\]** 區段中標示為**委派**的表格列。
-   *enumMemberName*：顯示在所有列舉的屬性語法中。 使用列舉值的屬性也有類似的預留位置，但通常會在預留位置加上列舉名稱提示的首碼。 例如，針對 [**FrameworkElement.FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 顯示的語法為 <*frameworkElement***FlowDirection**="* flowDirectionMemberName*"/>。 如果您正位於其中一個屬性參考頁面，按一下顯示在 **\[屬性值\]** 區段中 **\[類型:\]** 旁的列舉類型連結。 對於使用該列舉之屬性的屬性值，您可以使用列於 **\[成員\]** 清單的 **\[成員\]** 欄中的任何字串。
-   *double*、*int*、*string*、*bool*：這些是 XAML 語言已知的基本類型。 如果您使用 C# 或 Visual Basic 進行程式設計，這些類型可對應 Microsoft .NET 的等同類型，例如 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)、[**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx)、[**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) 和 [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx)，當您在 .NET 程式碼後置中使用 XAML 定義的值時，可以使用這些 .NET 類型的任何成員。 如果您使用 C++/CX 進行程式設計，可以使用 C++ 基本類型，也可以考慮使用等同於 [**Platform**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx) 命名空間所定義類型的這些項目，例如 [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx)。 針對特定的屬性值有時會有額外的限制。 但您通常會在 **\[屬性值\]** 區段或 [備註] 區段而不會在 XAML 區段中看到這些註解，因為這些限制同時適用於程式碼用法和 XAML 用法。

## <a name="tips-and-tricks-notes-on-style"></a>祕訣與技巧，樣式附註

-   標記延伸一般是在主要 [XAML 概觀](xaml-overview.md)中加以描述。 但是，對本主題中提供的指導方針影響最大的標記延伸則是 [StaticResource](staticresource-markup-extension.md) 標記延伸 (和相關的 [ThemeResource](themeresource-markup-extension.md))。 StaticResource 標記延伸的功能是將您的 XAML 分解成來自 XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的可重複使用資源。 您幾乎都是在 **ResourceDictionary** 中定義控制項範本和相關的樣式。 通常也會在 **ResourceDictionary** 中定義較小部分的控制項範本定義或 app 特定樣式，例如，app 針對不同 UI 部分使用一次以上的色彩 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)。 透過使用 StaticResource，任何需要使用屬性元素才能設定的屬性，現在可以在屬性語法中設定。 但是，分解 XAML 以供重複使用的好處並不僅僅是簡化頁面層級語法而已。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)。
-   您會在 XAML 範例中，看到數種有關如何套用空格和換行字元的不同慣例。 特別是針對如何拆開設定了許多不同屬性的物件元素，也有各種不同的慣例可供套用。 那些都只是樣式上的偏好。 當您編輯 XAML 時，Visual Studio XML 編輯器會套用一些預設樣式規則，但是您可以在設定中變更這些規則。 在少數情況下，XAML 檔案中的空格會被視為有意義的；如需詳細資訊，請參閱 [XAML 與空格](xaml-and-whitespace.md)。

## <a name="related-topics"></a>相關主題

* [XAML 概觀](xaml-overview.md)
* [XAML 命名空間與命名空間對應](xaml-namespaces-and-namespace-mapping.md)
* [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)
 

