---
description: 連結控制項範本中的屬性值和範本化控制項中的其他公開屬性值。 TemplateBinding 只能在 XAML 的 ControlTemplate 定義中使用。
title: TemplateBinding 標記延伸
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3b368ca2f429d52674ba1cb3493012d54dc0848a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154922"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} 標記延伸

連結控制項範本中的屬性值和範本化控制項中的其他公開屬性值。 **TemplateBinding** 只能在 XAML 的 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定義內使用。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 屬性用法 (適用於範本或樣式中的 Setter 屬性)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 描述 |
|------|-------------|
| propertyName | 正在 setter 語法中設定的屬性的名稱。 必須是相依性屬性。 |
| sourceProperty | 存在於範本化類型中其他相依性屬性的名稱。 |

## <a name="remarks"></a>備註

不論您是一個自訂控制項作者，還是要取代現有控制項的控制項範本，定義控制項範本的方法時，使用 **TemplateBinding** 是相當基礎的一個部分。 如需詳細資訊，請參閱[快速入門：控制項範本](/previous-versions/windows/apps/hh465374(v=win.10))。

讓 *propertyName* 與 *targetProperty* 使用相同的屬性名稱是相當常見的做法。 在這個情況下，控制項可以在本身定義一個屬性，然後將該屬性轉送到它的其中一個元件部分的現有直覺式具名屬性。 例如，在其撰寫中併入 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 的控制項（用來顯示控制項本身的 **Text** 屬性），可能會在控制項範本中包含此 XAML 做為元件： `<TextBlock Text="{TemplateBinding Text}" .... />`

用來做為來源屬性值和目標屬性值的類型必須相符。 當您正在使用 **TemplateBinding** 時，沒有機會可以引入轉換器。 這樣在剖析 XAML 時會因為無法比對值而產生錯誤。 如果您需要轉換器，您可以針對範本系結使用詳細語法，例如： `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

嘗試在 XAML 中的 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定義之外使用 **TemplateBinding** 會導致剖析器錯誤。

您可以針對範本化的父系值也會延遲為其他繫結的情況，使用 **TemplateBinding**。 **TemplateBinding** 的評估會等到所有必要的執行階段繫結都有值之後才進行。

**TemplateBinding** 一律是單向繫結。 相關的兩個屬性必須是相依性屬性。

**TemplateBinding** 是一個標記延伸。 如果必須將屬性 (Attribute) 值加上逸出符號，以免成為常值或處理常式名稱，而且這個動作必須更全面地實施 (而不是只對特定類型或屬性 (Property) 設定類型轉換子 (Type Converter))，則通常會實作標記延伸。 XAML 的所有標記延伸會在屬性語法中使用「{」和「}」字元，這是慣例，XAML 處理器藉此來辨識必須處理屬性的標記延伸。

**注意**   在 Windows 執行階段 XAML 處理器的執行中，沒有適用于**TemplateBinding**的支援類別標記法。 **TemplateBinding** 僅限在 XAML 標記中使用。 並沒有一個直接的方式可以在程式碼中重現行為。

### <a name="xbind-in-controltemplate"></a>ControlTemplate 中的 x:Bind

> [!NOTE]
> 在 ControlTemplate 中使用 x:Bind 需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本。 如需目標版本的相關詳細資訊，請參閱[版本調適型程式碼](../debug-test-perf/version-adaptive-code.md)。

從 Windows 10 版本1809開始，您可以在[**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)中使用**TemplateBinding**的任何地方使用**x:Bind**標記延伸。 

使用**x:Bind**時，不需要在[ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)上 () 選擇性的[TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype)屬性。

透過**x:Bind**支援，您可以在[ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)中同時使用[函數](../data-binding/function-bindings.md)系結和雙向系結。

在此範例中， **TextBlock** 屬性會評估為 **ToString**。 ControlTemplate 上的 TargetType 可作為資料來源，並完成與 TemplateBinding 對父系的相同結果。

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>相關主題

* [快速入門：控制項範本](/previous-versions/windows/apps/hh465374(v=win.10))
* [深入了解資料繫結](../data-binding/data-binding-in-depth.md)
* [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [XAML 概觀](xaml-overview.md)
* [相依性屬性概觀](dependency-properties-overview.md)
 