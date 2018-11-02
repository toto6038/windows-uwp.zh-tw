---
author: jwmsft
description: 透過評估對資源的參考，搭配會依據目前使用中的佈景主題抓取不同資源的額外系統邏輯，為所有 XAML 屬性提供一個值。
title: ThemeResource 標記延伸
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 024e48380941c0d79eef65780396ec9b89edc3c7
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5947361"
---
# <a name="themeresource-markup-extension"></a>{ThemeResource} 標記延伸

透過評估對資源的參考，搭配會依據目前使用中的佈景主題抓取不同資源的額外系統邏輯，為所有 XAML 屬性提供一個值。 與 [{StaticResource} 標記延伸](staticresource-markup-extension.md)類似，資源是在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中定義，而 **ThemeResource** 用法會參考該資源在 **ResourceDictionary** 中的索引鍵。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{ThemeResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 說明 |
|------|-------------|
| 索引鍵 | 要求的資源的索引鍵。 這個索引鍵最初是由 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 所指派。 資源索引鍵可以是定義在 XamlName 文法中的任何字串。 |

## <a name="remarks"></a>備註

**ThemeResource** 是一項技術，可以為 XAML 屬性取得定義在 XAML 資源字典中其他位置的值。 這個標記延伸的基本用途與 [{StaticResource} 標記延伸](staticresource-markup-extension.md)相同。 與 {StaticResource} 標記延伸在行為上的不同之處，在於 **ThemeResource** 參考可以依據系統目前所使用的佈景主題，動態使用不同的字典做為主要查詢位置。

當應用程式第一次啟動時，系統會根據啟動時的使用中佈景主題，評估 **ThemeResource** 參考所執行的所有資源參考。 但是，如果使用者後續在執行階段變更使用中的佈景主題，系統將會重新評估每一個 **ThemeResource** 參考、抓取可能不同的佈景主題特定資源，然後使用視覺化樹狀結構中所有適當位置的新資源值重新顯示應用程式。 **StaticResource** 是在 XAML 載入階段/App 啟動時所決定，在執行階段不會重新評估 (此外，還有其他技術，例如會動態重新載入 XAML 的視覺狀態，但是這些技術是在由 [{StaticResource} 標記延伸](staticresource-markup-extension.md)啟用基本資源評估的較高層級運作)。

**ThemeResource** 接受一個引數，這個引數指定了所要求的資源的索引鍵。 資源索引鍵一律是 Windows 執行階段 XAML 中的一個字串。 如需如何在一開始指定資源索引鍵的詳細資訊，請參閱 [x:Key 屬性](x-key-attribute.md)。

如需有關如何定義資源及正確使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) (包括範例程式碼) 的詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)。

**重要** 與使用 **StaticResource** 時一樣，**ThemeResource** 不得嘗試對 XAML 檔案中進一步定義詞彙的資源做向前參考。 不支援嘗試這樣的做法。 即使向前參考並未失敗，但是嘗試這樣做會導致效能降低。 為獲得最佳結果，請調整您資源字典的組合以避免向前參考。

嘗試將 **ThemeResource** 指定給無法解析的索引鍵，會導致在執行階段擲回 XAML 剖析例外狀況。 設計工具也可能發出警告或錯誤。

在 Windows 執行階段 XAML 處理器實作中，沒有 **ThemeResource** 的支援類別表示法。 在程式碼中最接近的相等做法是使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的集合 API，例如呼叫 [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) 或 [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139)。

**ThemeResource** 是一個標記延伸。 當有需要將屬性值逸出文字值或處理常式名稱時，通常就會實作標記延伸，而且這需求是全域性的，而不只是在特定類型或屬性放置類型轉換器。 XAML 的所有標記延伸會在屬性語法中使用「{」和「}」字元，這是慣例，XAML 處理器藉此來辨識必須處理屬性的標記延伸。

### <a name="when-and-how-to-use-themeresource-rather-than-staticresource"></a>使用 {ThemeResource} 而不是 {StaticResource} 的時機和方式

將 **ThemeResource** 解析成資源字典中的項目時所依據的規則，大致上與 **StaticResource** 相同。 **ThemeResource** 查詢可以延伸到 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807) 集合所參考的 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 檔案中，但是 **StaticResource** 也可以那麼做。 差別在於 **ThemeResource** 可以在執行階段重新評估，但 **StaticResource** 不可以。

每個佈景主題字典中的索引鍵組都應該提供一組相同的索引鍵資源，不論使用中的佈景主題是哪一個。 如果 **HighContrast** 佈景主題字典中有某個指定的索引鍵資源，則 **Light** 和 **Default** 中也應該要有另一個具有該名稱的資源。 如果不是這樣，當使用者切換佈景主題時，資源查詢可能會失敗，而您的應用程式會看起來不正常。 雖然佈景主題字典包含的索引鍵資源有可能只從相同範圍內參考以提供子值；這些在所有佈景主題中不一定要相等。

一般而言，您應該將資源放在佈景主題字典中，然後只當這些值可以在佈景主題之間變更或受變更值支援時，才使用 **ThemeResource** 參考這些資源。 這適用於下列資源類型：

-   筆刷 (使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 的特定色彩)。 這些構成預設 XAML 控制項範本 (generic.xaml) 中大約 80% 的 **ThemeResource** 使用。
-   框線、位移、邊界及邊框間距等等的像素值。
-   字型屬性 (例如，**FontFamily** 或 **FontSize**)。
-   限定數量之控制項的完整範本，這些控制項通常是系統樣式並且用於動態呈現 (例如，[**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 和 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919))。
-   文字顯示樣式 (通常用來變更字型色彩、背景，也可能用來變更字型大小)。

Windows 執行階段提供專供 **ThemeResource** 參照的一組資源。 這些都列在 XAML 檔案 themeresources.xaml 中，您可以從 Windows 軟體開發套件 (SDK) 所含的 include/winrt/xaml/design 資料夾中找到這個檔案。 如需有關佈景主題筆刷及 themeresources.xaml 中定義之其他樣式的文件，請參閱 [XAML 佈景主題資源](https://msdn.microsoft.com/library/windows/apps/mt187274)。 筆刷相關資訊列在表格中，其中說明三種可能的使用中佈景主題中每種筆刷的色彩值。

只要基礎資源可能因佈景主題變更而改變，控制項範本中視覺狀態的 XAML 定義就應該使用 **ThemeResource** 參考。 系統佈景主題變更通常不會同時導致視覺狀態變更。 在這種情況下，資源需要使用 **ThemeResource** 參考，才能針對仍在使用中的視覺狀態重新評估值。 例如，如果您有一個視覺狀態會變更特定 UI 部分的筆刷色彩及它的其中一個屬性，而該筆刷色彩依佈景主題而有不同，您應該使用 **ThemeResource** 參考來提供該屬性在預設範本中的值，並且也提供任何視覺狀態修改給該預設範本。

您可能會在一系列的相依值中看到 **ThemeResource** 的使用。 例如，[**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 使用的 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 值同時也是索引鍵資源，可能使用 **ThemeResource** 參考。 但是，所有使用索引鍵 **SolidColorBrush** 資源的 UI 屬性也會使用 **ThemeResource** 參考，因此具體說來，在佈景主題變更時啟用動態值變更的是每個 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 類型屬性。

**注意：** `{ThemeResource}`和執行階段資源評估上佈景主題切換是 Windows8.1 XAML 中支援，但 Windows8 為目標的應用程式在 XAML 中不支援。

### <a name="system-resources"></a>系統資源

有些佈景主題資源會將系統資源值當作基礎子值來參考。 系統資源是在任何 XAML 資源字典中都找不到的特殊資源值。 這些資源依賴 Windows 執行階段 XAML 支援中的行為來轉送來自系統本身的值，然後以 XAML 資源可以參考的形式呈現這些值。 例如，有一個名為「SystemColorButtonFaceColor」的系統資源，代表某個 RGB 色彩。 這個色彩來自系統色彩和佈景主題的層面，而這些系統色彩和佈景主題並非僅供 Windows 執行階段和 Windows 執行階段應用程式使用。

系統資源通常是高對比佈景主題的基礎值。 使用者可以控制其高對比佈景主題的色彩選擇，而且使用者在做這些選擇時，使用的也並非 Windows 執行階段應用程式特定的系統功能。 藉由以 **ThemeResource** 參考的形式來參考系統資源，Windows 執行階段應用程式之高對比佈景主題的預設行為，便可使用這些由使用者控制並由系統顯示的佈景主題特定值。 此外，這些參考現在已標示為要在系統偵測到執行階段佈景主題變更時重新評估。

### <a name="an-example-themeresource-usage"></a>{ThemeResource} 用法範例

這裡提供一些取自預設 generic.xaml 和 themeresources.xaml 檔案的範例 XAML，用來說明如何使用 **ThemeResource**。 我們只會看一個範本 (預設 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265))，以及如何宣告兩個屬性 ([**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 和 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414)) 以回應佈景主題變更。

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

在這裡，屬性採用 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 值，而使用 **ThemeResource** 建立對名為 `ButtonBackgroundThemeBrush` 和 `ButtonForegroundThemeBrush` 之 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 資源的參考。

[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的一些視覺狀態也會調整這些相同的屬性。 較明顯的就是當按一下按鈕時，背景色彩會變更。 同樣地，在這裡，視覺狀態腳本中的 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 和 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) 動畫會以 **ThemeResource** 做為主要畫面值，使用 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) 物件及對筆刷的參考。

```xml
<VisualState x:Name="Pressed">
  <Storyboard>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Border"
        Storyboard.TargetProperty="Background">
      <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedBackgroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter"
         Storyboard.TargetProperty="Foreground">
       <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedForegroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
  </Storyboard>
</VisualState>
```

這些筆刷中的每一個都是稍早在 generic.xaml 中定義：必須在使用這些筆刷的所有範本之前先行定義，才能避免 XAML 向前參考。 這裡針對「Default」佈景主題字典提供這些定義。

```xml
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="Transparent" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="#FFFFFFFF" />
...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="#FFFFFFFF" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="#FF000000" />
...
```

然後，每個其他的佈景主題字典也都定義這些筆刷，例如：

```xml
        <ResourceDictionary x:Key="HighContrast">
            <!-- High Contrast theme resources -->
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />

...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
```

這裡的 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 值是對系統資源的另一個 **ThemeResource** 參考。 如果您參考某個系統資源，並且希望它隨著佈景主題變更而改變，您應該使用 **ThemeResource** 進行參考。

## <a name="windows8-behavior"></a>Windows8 行為

Windows8 不支援**ThemeResource**標記延伸，它是從 Windows8.1 開始提供。 此外，Windows8 不支援動態切換與佈景主題相關的資源為 Windows 執行階段應用程式。 應用程式必須重新啟動，才能針對 XAML 範本和樣式挑選佈景主題變更。 這不是一個良好的使用者經驗，因此使用者可以使用樣式與**ThemeResource**使用方式，可以在使用者進行時動態切換佈景主題應用程式重新編譯和目標 Windows8.1 我們強烈建議。 針對 Windows8 但 Windows8.1 上執行繼續使用 Windows8 行為已編譯的應用程式。

## <a name="design-time-tools-support-for-the-themeresource-markup-extension"></a>**{ThemeResource}** 標記延伸的設計階段工具支援

Microsoft Visual Studio2013 可以在 Microsoft IntelliSense 下拉式清單中包含可能的索引鍵值，當您在 XAML 頁面中使用 **{ThemeResource}** 標記延伸。 例如，一旦輸入「{ThemeResource」之後，就會立即顯示所有來自 [XAML 佈景主題資源](https://msdn.microsoft.com/library/windows/apps/mt187274)的資源索引鍵。

一旦資源索引鍵存在於任何 **{ThemeResource}** 用法中，**[移至定義]** (F12) 功能就可以立即解析該資源，並為您顯示設計階段的 generic.xaml，這是定義佈景主題資源的位置。 因為已經多次定義佈景主題資源 (針對每個佈景主題)，所以 **[移至定義]** 會將您帶往檔案中找到的第一個定義，也就是 **Default** 的定義。 如果你需要其他定義，可以在檔案內搜尋索引鍵名稱，以尋找其他佈景主題的定義。

## <a name="related-topics"></a>相關主題

* [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [XAML 佈景主題資源](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [x:Key 屬性](x-key-attribute.md)
 

