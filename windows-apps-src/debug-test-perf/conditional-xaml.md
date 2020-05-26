---
title: 條件式 XAML
description: 在 XAML 標記中使用新的 API，並同時維持與先前版本的相容性
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef518c9974fb4c8bc0f09f442f4b78be1c9c85d2
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775838"
---
# <a name="conditional-xaml"></a>條件式 XAML

條件式 XAML 提供一種在 XAML 標記中使用 [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) 方法的方式。 這可讓您在標記中根據 API 是否存在來設定屬性和具現化物件，而不必使用程式碼後置。 它會選擇性剖析元素或屬性，判斷是否可在執行階段使用它們。 條件陳述式是在執行階段進行評估，如果評估為 **true**，則剖析條件式 XAML 標記所限定的元素，否則便將其忽略。

條件式 XAML 是從 Creators Update (版本 1703 組建 15063) 開始提供。 若要使用條件式 XAML，必須將 Visual Studio 專案的最低版本設定為組建 15063 (Creators Update) 或更新版本，並將目標版本設定成比最低版本更新的版本。 如需有關設定 Visual Studio 專案的詳細資訊，請參閱[版本調適型應用程式](version-adaptive-apps.md)。

> [!NOTE]
> 若要建立最低版本小於組建 15063 的版本調適型應用程式，您必須使用[版本調適型程式碼](version-adaptive-code.md)，而非 XAML。

如需關於 ApiInformation 和 API 協定的重要背景資訊，請參閱[版本調適型應用程式](version-adaptive-apps.md)。

## <a name="conditional-namespaces"></a>條件式命名空間

若要在 XAML 中使用條件式方法，您必須先在頁面頂端宣告條件式 [XAML 命名空間](../xaml-platform/xaml-namespaces-and-namespace-mapping.md)。 以下是條件式命名空間的虛擬程式碼範例：

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

條件式命名空間可分成兩個部分 (以 '?' 分隔符號分隔)。 

- 分隔符號之前的內容表示包含被參考 API 的命名空間或結構描述。 
- 在 '?' 分隔符號之後的內容表示判斷條件式命名空間評估為 **true** 或 **false** 的條件式方法。

在大部分情況下，結構描述會是預設的 XAML 命名空間：

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

條件式 XAML 支援下列條件式方法：

方法 | 反向方法
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

我們將在本文後續章節中進一步討論這些方法。

> [!NOTE]
> 建議您使用 IsApiContractPresent 和 IsApiContractNotPresent。 Visual Studio 設計體驗不完全支援其他條件式。

## <a name="create-a-namespace-and-set-a-property"></a>建立命名空間並設定屬性

在此範例中，如果應用程式在秋季版 Creators Update 或更新版本上執行，會顯示「條件式 XAML 您好」做為文字區塊的內容，如果在舊版執行，則依預設不顯示任何內容。

首先，定義以 contract5Present 為前置詞的自訂命名空間，並使用預設 XAML 命名空間 (https://schemas.microsoft.com/winfx/2006/xaml/presentation) ) 做為包含 [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.Text) 屬性的結構描述。 若要讓這個命名空間變成條件式命名空間，請在結構描述後面加上 '?' 分隔符號。

接著定義會在執行秋季版 Creators Update 或更新版本之裝置傳回 **true** 的條件式。 您可以使用 ApiInformation 方法 **IsApiContractPresent** 來檢查是否有第 5 版的 UniversalApiContract。 第 5 版 UniversalApiContract 已隨秋季版 Creators Update (SDK 16299) 一起發行。

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

定義命名空間之後，在 TextBox 的 Text 屬性前面加上命名空間前置詞，將其限定為應在執行階段有條件地加以設定的屬性。

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

以下是完整的 XAML。

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

在秋季版 Creators Update 上執行此範例時，將會顯示「條件式 XAML 您好」，如果是在 Creators Update 中執行，則不顯示任何文字。

條件式 XAML 讓您將可在程式碼中進行的 API 檢查標記改為在標記中執行。 以下是進行這項檢查的對等程式碼。

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

請注意，雖然 IsApiContractPresent 方法的 contractName 參數接受的是字串，但是在 XAML 命名空間宣告中，不要將此字串放在引號 (" ") 內。

## <a name="use-ifelse-conditions"></a>使用 if/else 條件

在前面的範例中，只有當應用程式在秋季版 Creators Update 上執行時才會設定 Text 屬性。 但若是您想要在 Creators Update 上執行時顯示不同的文字呢？ 您可以嘗試不使用條件式限定詞來設定 Text 屬性，就像這樣：

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

這在 Creators Update 上執行時會有作用，但在秋季版 Creators Update 上執行時，就會收到錯誤訊息，指出已設定 Text 屬性一次以上。

若要在應用程式於 Windows 10 執行時設定不同的文字，您需要其他條件。 條件式 XAML 提供為每個支援的 ApiInformation 方法提供反向方法，讓您建立 if/else 條件式案例，就像這樣：

如果目前裝置包含指定的協定和版本號碼，IsApiContractPresent 方法會傳回 **true**。 例如，假設應用程式正在含有第 4 版通用 API 協定的 Creators Update 上執行。

各種對 IsApiContractPresent 的呼叫都會產生下列結果：

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true

IsApiContractNotPresent returns the inverse of IsApiContractPresent 呼叫 IsApiContractNotPresent 會產生下列結果：

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

若要使用反向條件，請建立使用 **IsApiContractNotPresent** 條件式的第二個條件式 XAML 命名空間。 如下所示，其前置詞為 'contract5NotPresent'。

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

定義這兩個命名空間後，只要您在這些命名空間前面加上確保執行階段僅使用一個屬性設定的限定詞，即可設定 Text 屬性兩次，就像這樣：

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

以下是另一個設定按鈕背景的範例。 從秋季版 Creators Update 開始已提供[壓克力材質](../design/style/acrylic.md)功能，因此當應用程式在秋季版 Creators Update 上執行時，會使用壓克力做為背景。 舊版無法使用此功能，所以在這些情況下，會將背景設定為紅色。

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>建立控制項並繫結屬性

現在您已經了解如何使用條件式 XAML 設定屬性，但您也可以在執行階段根據可用的 API 協定有條件地具現化控制項。

以下範例中，當應用程式在有提供 [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker) 控制項的秋季版 Creators Update 上執行時，該控制項會具現化。 ColorPicker 無法在秋季版 Creators Update 以前的版本中使用，因此當應用程式在舊版中執行時，請使用 [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) 來為使用者提供簡化的色彩選擇。

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

您可以透過不同形式的 [XAML 屬性語法](../xaml-platform/xaml-syntax-guide.md)來使用條件式限定詞。 以下範例中，矩形的 Fill 屬性針對秋季版 Creators Update 使用屬性元素語法來設定，針對舊版則使用屬性語法來設定。

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

將屬性繫結到另一個相依於條件式命名空間的屬性時，必須在這兩個屬性上使用相同的條件。 以下範例中，`colorPicker.Color` 相依於 'contract5Present' 條件式命名空間，因此您也必須將 'contract5Present' 前置詞放在 SolidColorBrush.Color 屬性上。 (或者，您可以將 'contract5Present ' 前置詞放在 SolidColorBrush 上，而不是在 Color 屬性上。)如果沒有這麼做，您將會在編譯階段收到錯誤。

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

以下是示範這些案例的完整 XAML。 此範例包含矩形以及可讓您設定矩形色彩的 UI。

當應用程式在秋季版 Creators Update 上執行時，請使用 ColorPicker 來讓使用者設定色彩。 ColorPicker 無法在秋季版 Creators Update 以前的版本中使用，因此當應用程式在舊版中執行時，請使用下拉式方塊來為使用者提供簡化的色彩選擇。

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>相關文章

- [UWP 應用程式指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [利用 API 協定動態偵測功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API 協定](https://channel9.msdn.com/Events/Build/2015/3-733) (組建 2015 影片)
- [通用裝置系列 API 合約](/uwp/extension-sdks/windows-universal-sdk)
