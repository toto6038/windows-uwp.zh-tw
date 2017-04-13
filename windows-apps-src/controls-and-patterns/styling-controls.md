---
author: Jwmsft
Description: "樣式可讓您設定控制項屬性，並在多個控制項重複使用這些設定來擁有一致的外觀。"
MS-HAID: dev\_ctrl\_layout\_txt.styling\_controls
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "設定控制項的樣式"
ms.assetid: AB469A46-FAF5-42D0-9340-948D0EDF4150
label: Styling controls
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: b9bc1e68e26830a283ed49b753f63a8f7ae63637
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="styling-controls"></a>設定控制項的樣式



您可以使用 XAML 架構，以許多方式自訂 app 的外觀。 樣式可讓您設定控制項屬性，並在多個控制項重複使用這些設定來擁有一致的外觀。

## <a name="style-basics"></a>樣式基本知識


使用樣式擷取視覺屬性設定，再放入可重複使用的資源中。 以下是一個顯示 3 個按鈕的範例，其中使用一個設定 [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397)、[**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399) 及 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) 屬性的樣式。 透過套用樣式，您可以讓控制項擁有相同的外觀，而不需在每個控制項上個別設定這些屬性。

![已設定樣式的按鈕](images/styles-rainbow-buttons.png)

您可以在 XAML 中定義控制項的樣式內崁，或當作可重複使用的資源。 在個別頁面的 XAML 檔案、在 App.xaml 檔案或在另一個資源字典 XAML 檔案中定義資源。 資源字典 XAML 檔案可以跨應用程式共用，而且一個以上的資源字典可以合併成一個應用程式。 定義資源的位置會決定資源可使用的範圍。 只有定義這些資源的頁面才可以使用頁面層級資源。 如果將包含相同索引鍵的資源定義在 App.xaml 和頁面中，則頁面中的資源會覆寫 App.xaml 中的資源。 如果資源是在不同的資源字典檔案中定義，則其範圍由資源字典的參照位置決定。

在 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 定義中，您需要一個 [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) 屬性以及包含一或多個 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 元素的集合。 
            **TargetType** 屬性是一個字串，會指定要套用樣式的 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 類型。 
            **TargetType** 值必須指定 Windows 執行階段定義的 **FrameworkElement** 衍生類型，或是可在參考組件中取得的自訂類型。 如果您嘗試將樣式套用到控制項，但控制項的類型不符合您嘗試套用之樣式的 **TargetType** 屬性，就會發生例外狀況。

每個 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 元素都需要 [**Property**](https://msdn.microsoft.com/library/windows/apps/br208836) 與 [**Value**](https://msdn.microsoft.com/library/windows/apps/br208838)。 這些屬性設定會指出該設定套用了什麼控制項屬性，以及為該屬性設定的值。 您可以利用屬性 (Attribute) 或屬性 (Property) 元素語法來設定 **Setter.Value**。 這裡的 XAML 說明套用至先前所示的按鈕的樣式。 這個 XAML 的前兩個 **Setter** 元素使用屬性 (Attribute) 語法，而最後一個用於 [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397) 屬性的 **Setter** 則是使用屬性 (Property) 元素語法。 此範例並未使用 [x:Key](../xaml-platform/x-key-attribute.md) 屬性，因此樣式會以隱含方式套用至按鈕。 下一節將說明如何以隱含或明確方式套用樣式。

```XAML
<Page.Resources>
    <Style TargetType="Button">
        <Setter Property="BorderThickness" Value="5" />
        <Setter Property="Foreground" Value="Blue" />
        <Setter Property="BorderBrush" >
            <Setter.Value>
                <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
                    <GradientStop Color="Yellow" Offset="0.0" />
                    <GradientStop Color="Red" Offset="0.25" />
                    <GradientStop Color="Blue" Offset="0.75" />
                    <GradientStop Color="LimeGreen" Offset="1.0" />
                </LinearGradientBrush>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<StackPanel Orientation="Horizontal">
    <Button Content="Button"/>
    <Button Content="Button"/>
    <Button Content="Button"/>
</StackPanel>
```

## <a name="apply-an-implicit-or-explicit-style"></a>套用隱含或明確的樣式

如果您將樣式定義成資源，則將它套用至控制項的方式有兩種：

-   隱含樣式，只為 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 指定 [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857)。
-   明確樣式，為 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 指定 [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) 與 [x:Key 屬性](../xaml-platform/x-key-attribute.md)，然後利用使用此明確索引鍵的 [{StaticResource} 標記延伸](https://msdn.microsoft.com/library/windows/apps/mt185588)參考來設定目標控制項的 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) 屬性。

如果樣式內含 [x:Key 屬性](../xaml-platform/x-key-attribute.md)，則您必須對這種索引鍵樣式設定控制項的 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) 屬性，才能將該樣式套用至控制項。 反之，無 x:Key 屬性的樣式就會自動套用至其目標類型的每個控制項，否則不會有明確的樣式設定。

以下兩個按鈕示範隱含與明確的樣式。

![隱含樣式與明確樣式設定的按鈕。](images/styles-buttons-implicit-explicit.png)

在這個範例中，第一個樣式具有 [x:Key](../xaml-platform/x-key-attribute.md) 屬性，其目標類型為 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)。 第一個按鈕的 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) 屬性設為此索引鍵，所以是明確套用此樣式。 第二種樣式是以隱含方式套用至第二個按鈕，因為其目標類型是 **Button**，而該樣式並沒有 x:Key 屬性。

```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="RenderTransform">
            <Setter.Value>
                <RotateTransform Angle="25"/>
            </Setter.Value>
        </Setter>
        <Setter Property="BorderBrush" Value="Orange"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

## <a name="use-based-on-styles"></a>使用根據樣式

為了更容易維持樣式，並將樣式重複使用率提升到最高，您可以建立會從其他樣式繼承的樣式。 您可以使用 [**BasedOn**](https://msdn.microsoft.com/library/windows/apps/br208852) 屬性建立繼承樣式。 繼承自其他樣式的樣式必須用在相同類型的控制項，或是基礎樣式之目標類型所衍生的控制項。 例如，如果基礎樣式的目標是 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365)，則以此樣式為基礎樣式的目標可以是 **ContentControl**，或是 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 與 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 這類衍生自 **ContentControl** 的類型。 如果根據樣式中沒有設定某個值，則該值就會繼承自基礎樣式。 若是變更基礎樣式的值，則根據樣式會覆寫該值。 在下一個範例中，**Button** 與 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 的樣式是繼承自同一個基礎樣式。

![使用根據樣式來設定樣式的按鈕。](images/styles-buttons-based-on.png)

基礎樣式的目標是 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365)，並設定 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 與 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 屬性。 以此樣式為基礎的樣式目標是 **ContentControl** 所衍生的 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 與 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)。 根據樣式可為 [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397) 與 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) 屬性設定不同色彩。 (您通常不會在 **CheckBox** 周圍設定邊界。 我們在這裡這樣做是為了顯示樣式的效果。)

```XAML
<Page.Resources>
    <Style x:Key="BasicStyle" TargetType="ContentControl">
        <Setter Property="Width" Value="130" />
        <Setter Property="Height" Value="30" />
    </Style>

    <Style x:Key="ButtonStyle" TargetType="Button" 
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Orange" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Red" />
    </Style>

    <Style x:Key="CheckBoxStyle" TargetType="CheckBox" 
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Blue" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Green" />
    </Style>
</Page.Resources>

<StackPanel>
    <Button Content="Button" Style="{StaticResource ButtonStyle}" Margin="0,10"/>
    <CheckBox Content="CheckBox" Style="{StaticResource CheckBoxStyle}"/>
</StackPanel>
```

## <a name="use-tools-to-work-with-styles-easily"></a>使用工具輕鬆處理樣式

快速將樣式套用到控制項的方法，就是在 Microsoft Visual Studio XAML 設計介面的控制項上按一下滑鼠右鍵，然後選取 **\[編輯樣式\]** 或 **\[編輯範本\]** \(依按右鍵的控制項而定\)。 接著，您可以選取 **\[套用資源\]** 來套用現有的樣式，或選取 **\[建立空白\]** 來定義新的樣式。 如果您建立空白樣式，則可以選擇在頁面中、在 App.xaml 檔案中，或者在個別資源字典中定義該樣式。

## <a name="modify-the-default-system-styles"></a>修改預設系統樣式

您應該儘可能使用來自 Windows 執行階段預設 XAML 資源的樣式。 當您必須定義自己的樣式時，如果可能，請嘗試讓您的樣式以預設樣式為基礎 (使用先前所述的基準樣式，或者從編輯原始預設樣式的複本開始)。

## <a name="the-template-property"></a>Template 屬性

Style Setter 可以用於 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 的 [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) 屬性，事實上，大多數的典型 XAML 樣式和應用程式 XAML 資源都是由此組成。 這部分內容將在[控制項範本](control-templates.md)主題中深入討論。

