---
author: jwmsft
description: "透過評估對某個已定義資源的參考，以提供任一 XAML 屬性的值。 資源是在 ResourceDictionary 中定義，而 StaticResource 用法會參考該資源在 ResourceDictionary 中的索引鍵。"
title: "StaticResource 標記延伸"
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 539bc6b0a43491c9ef75701bc574c7e31d2c02e7
ms.lasthandoff: 02/07/2017

---

# <a name="staticresource-markup-extension"></a>{StaticResource} 標記延伸

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

透過評估對某個已定義資源的參考，以提供任一 XAML 屬性的值。 資源是在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中定義，而 **StaticResource** 用法會參考該資源在 **ResourceDictionary** 中的索引鍵。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 說明 |
|------|-------------|
| 索引鍵 | 要求的資源的索引鍵。 這個索引鍵最初是由 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 所指派。 資源索引鍵可以是定義在 XamlName 文法中的任何字串。 |

## <a name="remarks"></a>備註

**StaticResource** 是一項技術，可以為 XAML 屬性取得定義在 XAML 資源字典中其他位置的值。 由於這些值是要供多個屬性值共用，或由於 XAML 資源字典被當作一項 XAML 封裝或分解技術來使用，因此這些值可能會放在資源字典中。 其中一個 XAML 封裝技術範例就是控制項的佈景主題字典。 另一個範例是用於資源後援的合併資源字典。

**StaticResource** 可採用一個引數，這個引數會指定所要求的資源的索引鍵。 資源索引鍵一律是 Windows 執行階段 XAML 中的一個字串。 如需如何在一開始指定資源索引鍵的詳細資訊，請參閱 [x:Key 屬性](x-key-attribute.md)。

將 **StaticResource** 解析成資源字典中的項目時所依據的規則，並不屬於本主題說明的範圍。 那些規則取決於參考和資源是否同時存在於範本中，以及是否使用合併的資源字典等等。 如需有關如何定義資源及正確使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) (包括範例程式碼) 的詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)。

**重要**  
**StaticResource** 不得嘗試對在 XAML 檔案中進一步定義詞彙的資源做向前參考。 不支援嘗試這樣的做法。 即使向前參考並未失敗，但是嘗試這樣做會導致效能降低。 為獲得最佳結果，請調整您資源字典的組合以避免向前參考。

嘗試將 **StaticResource** 指定給無法解析的索引鍵，會導致在執行階段擲回 XAML 剖析例外狀況。 設計工具也可能發出警告或錯誤。

在 Windows 執行階段 XAML 處理器實作中，沒有 **StaticResource** 功能的支援類別表示法。 **StaticResource** 僅限在 XAML 中使用。 在程式碼中最接近的相等做法是使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的集合 API，例如呼叫 [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) 或 [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139)。

[{ThemeResource} 標記延伸](themeresource-markup-extension.md)是類似的標記延伸，會參考其他位置中的具名資源。 差異在於 {ThemeResource} 標記延伸能夠根據作用中的系統佈景主題傳回不同的資源。 如需詳細資訊，請參閱 [{ThemeResource} 標記延伸](themeresource-markup-extension.md)。

**StaticResource** 是一個標記延伸。 當有需要將屬性值逸出文字值或處理常式名稱時，通常就會實作標記延伸，而且這需求是全域性的，而不只是在特定類型或屬性放置類型轉換器。 XAML 的所有標記延伸會在屬性語法中使用 "\{" 和 "\}" 字元，這是慣例，XAML 處理器藉此來辨識必須處理屬性的標記延伸。

### <a name="an-example-staticresource-usage"></a>{StaticResource} 用法範例

這個 XAML 範例來自 [XAML 資料繫結範例](http://go.microsoft.com/fwlink/p/?linkid=226854)。

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

這個特定的範例會建立一個由自訂類別支援的物件，並且在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中將它建立成資源。 若要成為有效的資源，這個 `local:S2Formatter` 元素必須也具有一個 **x:Key** 屬性值。 這個屬性的值會設為 "GradeConverter"。

然後，在 XAML 中稍微再進一步的地方會呼叫該資源，其中您會看到 `{StaticResource GradeConverter}`。

請注意 {StaticResource} 標記延伸用法如何設定另一個標記延伸 [{Binding} 標記延伸](binding-markup-extension.md)的屬性，因此這裡使用了兩個巢狀標記延伸。 內部的項目會先進行評估，因此可以先取得資源並做為值。 {Binding} 標記延伸中也顯示了這個相同的範例。

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>**{StaticResource}** 標記延伸的設計階段工具支援

當您在 XAML 頁面中使用 **{StaticResource}** 標記延伸時，Microsoft Visual Studio 2013 可以在 Microsoft IntelliSense 下拉式清單中包含可能的索引鍵值。 例如，一旦輸入 "{StaticResource" 之後，任何來自目前查閱範圍的資源索引鍵就會立即顯示於 IntelliSense 下拉式清單中。 除了您在頁面層級 ([**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)) 和 app 層級 ([**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338)) 上擁有的典型資源之外，您也會看到 [XAML 佈景主題資源](https://msdn.microsoft.com/library/windows/apps/mt187274)，以及專案正在使用之任何延伸的資源。

一旦資源索引鍵存在於任何 **{StaticResource}** 用法中，**移至定義** \(F12\) 功能就可以立即解析該資源，並為您顯示其定義所在的目錄。 針對佈景主題資源，這會在設計階段移至 generic.xaml。

## <a name="related-topics"></a>相關主題

* [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [x:Key 屬性](x-key-attribute.md)
* [{ThemeResource} 標記延伸](themeresource-markup-extension.md)


