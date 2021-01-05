---
description: 使用資料範本選擇器，根據項目屬性自訂項目的樣式。
title: 選取資料範本
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: 0d9a35c3e66a4d4189016ca87d3da51da5bf5be4
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860396"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>選取資料範本：根據項目屬性設定項目的樣式

集合控制項的自訂設計是由 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 管理。 資料範本會定義應該如何配置每個項目和設定其樣式，而且該標記會套用至集合中的每個項目。 本文說明如何使用 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)，在集合上套用不同的資料範本，並根據特定項目屬性或您選擇的值，選取要使用的資料範本。

> **重要 API**：[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)、[DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 是啟用自訂範本選擇邏輯的類別。 它可讓您定義一些規則，指定要用於集合中特定項目的資料範本。 若要實作此邏輯，您可以在程式碼後置中建立 DataTemplateSelector 的子類別，並定義邏輯，以決定哪個資料範本用於哪個類別的項目 (例如，特定類型的項目或具有特定屬性值的項目等)。 您可在 XAML 檔案的 Resources 區段中宣告這個類別的執行個體，以及您將使用的資料範本定義。 您可以使用 `x:Key` 值來識別這些資源，讓您在 XAML 中參考它們。

## <a name="prerequisites"></a>必要條件

- 如何[使用和建立集合控制項，例如 ListView 或 GridView。](listview-and-gridview.md)
- 如何[使用 DataTemplate 自訂項目的外觀。](item-containers-templates.md#data-template)

## <a name="when-not-to-use-a-datatemplateselector"></a>不使用 DataTemplateSelector 的時機

一般來說，您不應該為 ListView 或 GridView 中的每個項目提供完全不同的配置/樣式，這會造成無法好好使用 DataTemplateSelector，並對效能產生負面影響。

您可以只使用一個資料範本，透過繫結特定屬性，來控制清單項目的特定元素視覺顯示方式。 例如，每個項目都可以具有不同的圖示，方法為繫結至資料範本中的圖示來源屬性，並針對該圖示來源屬性為每個項目提供不同值。 如此一來將比使用 DataTemplateSelector 提供更好的效能。

## <a name="when-to-use-a-datatemplateselector"></a>使用 DataTemplateSelector 的時機

當您想要在集合控制項中使用多個資料範本時，應該建立 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)。 `DataTemplateSelector` 可讓您彈性地突顯特定項目，同時讓這些項目仍保留類似的配置。 有許多使用案例，DataTemplateSelector 可在其中提供助益，但也有少數案例，更適合讓您重新思考正在使用的控制項和策略。

集合控制項通常會繫結至全都屬於某個類型的項目集合。 不過，即使項目可能屬於相同的類型，某些屬性的值也有可能不同，或代表不同的意義。 某些項目也可能比其他項目更為重要，或者某個項目可能特別重要或不同，而且需要以視覺化方式突顯它。在這些情況下，DataTemplateSelector 將會很有幫助。

您也可以繫結至包含不同項目類型的集合 - 繫結集合可以混合使用字串、整數、自訂類別物件等等。 這讓 DataTemplateSelector 特別有用，因為它可以根據項目的物件類型來指派不同的資料範本。

以下是一些您可能使用資料範本選擇器的範例：

- 代表 ListView 內員工的不同層級 - 每個員工類型/層級都可能需要不同的顏彩背景，才能輕鬆地區分。
- 代表產品資源庫 (使用 GridView) 中的銷售項目 - 銷售項目可能需要紅色背景或不同顏色的字型，使其有別於一般訂價項目。
- 使用 FlipView 代表相片圖庫中的優勝者/熱門相片。
- 需要以不同的方式代表 ListView 中的負數/正數，或是短字串/長字串。

## <a name="create-a-datatemplateselector"></a>建立 DataTemplateSelector

建立資料範本選擇器時，您會在程式碼中定義範本選擇邏輯，並在 XAML 中定義資料範本。

### <a name="code-behind-component"></a>程式碼後置元件

若要使用資料範本選擇器，請先在程式碼後置中建立 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 的子類別 (衍生自它的類別)。 在您的類別中，您會將每個範本宣告為類別的屬性。 然後，覆寫 [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) 方法，以包含您自己的範本選擇邏輯。

以下是簡單 `DataTemplateSelector` 子類別 (稱為 `MyDataTemplateSelector`) 的範例。

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

`MyDataTemplateSelector` 類別衍生自 `DataTemplateSelector` 類別，且首先定義兩個 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 物件：`Normal` 和 `Accent`。 這些是目前的空宣告，但會以 XAML 檔案中的標記「填入」。

`SelectTemplateCore` 方法會接受項目物件 (亦即每個集合項目)，並使用在哪些情況下傳回哪個 `DataTemplate` 的規則來覆寫。 在此情況下，如果項目是奇數，則它會接收 `Accent` 資料範本，若是偶數，則會接收 `Normal` 資料範本。

### <a name="xaml-component"></a>XAML 元件

第二，您必須在 XAML 檔案的 Resources 區段中，建立這個新 `MyDataTemplateSelector` 類別的執行個體。 所有資源都需要一個 `x:Key`，您可以用來將它繫結至集合控制項的 `ItemTemplateSelector` 屬性 (在稍後的步驟中)。 您也會建立兩個 `DataTemplate` 物件執行個體，並在 Resources 區段中定義其配置。 您可以將這些資料範本指派給您在 `MyDataTemplateSelector` 類別中宣告的 `Accent` 和 `Normal` 屬性。

以下是 XAML 資源和所需標記的範例：

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

如您所見，已定義兩個資料範本 `Normal` 和 `Accent` - 它們都會將項目顯示為按鈕，不過，`Accent` 資料範本會對背景使用輔色筆刷，而 `Normal` 資料範本則使用灰色筆刷 (`SystemChromeLowColor`)。 然後，這兩個資料範本會指派給 `Normal` 和 `Accent` DataTemplate 物件，這是在 C# 程式碼後置中建立的 MyDataTemplateSelector 類別屬性。

最後一個步驟是將您的 `DataTemplateSelector` 繫結至集合控制項的 `ItemTemplateSelector` 屬性 (在此情況下，指的是 ListView)。 這會取代將值指派給 `ItemTemplate` 屬性的需求。 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

一旦您的程式碼進行編譯，每個集合項目就會透過 `MyDataTemplateSelector` 中覆寫的 `SelectTemplateCore` 方法執行，而且將會以適當的 DataTemplate 呈現。

> [!IMPORTANT]
> 搭配 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2&preserve-view=true) 使用 `DataTemplateSelector` 時，您可以將 `DataTemplateSelector` 繫結至 `ItemTemplate` 屬性。 `ItemsRepeater` 沒有 `ItemTemplateSelector` 屬性。

## <a name="datatemplateselector-performance-considerations"></a>DataTemplateSelector 效能考量

當您使用 ListView 或 GridView 搭配大型資料集合時，捲動和移動瀏覽效能可能會是一大顧慮。 為了讓大型集合可以妥善執行，有一些步驟，您可以採取以改善資料範本的效能。 在 [ListView 和 GridView UI 最佳化](../../debug-test-perf/optimize-gridview-and-listview.md)中有更詳細的說明。

- _每個項目的元素減少_ - 將資料範本中的 UI 元素數目保持在合理的下限。
- 含異質集合的容器回收
  - 使用 _ChoosingItemContainer 事件_ - 此事件是一種將不同資料範本用於不同項目的高效能方式。 為了實現最佳效能，您應該最佳化快取，並為特定資料選取最適合的資料範本。
  - 使用 _項目範本選擇器_ - 在某些情況中應該避免使用項目範本選擇器 (`DataTemplateSelector`)，因為它會對效能產生影響。
