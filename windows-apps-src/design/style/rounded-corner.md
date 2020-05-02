---
title: 圓角半徑
description: 了解圓角原則、設計方法和自訂選項。
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp, 邊角半徑, 修圓
ms.openlocfilehash: 134a49ac57678eea0da718e93a14e3d0cf8896d5
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "81001475"
---
# <a name="corner-radius"></a>圓角半徑

從 [Windows UI 程式庫](/uwp/toolkits/winui/) (WinUI) 2.2 版開始，許多控制項的預設樣式已更新為使用圓角。 這些新樣式主要是要喚起親切和信任的感覺，讓使用者更容易以視覺方式處理 UI。

以下是兩個按鈕控制項，第一個沒有圓角，而第二個採用新的圓角樣式。

![沒有和具有圓角的按鈕](images/rounded-corner/my-button.png)

當您安裝適用於 WinUI 2.2 或更新版本的 NuGet 套件時，會同時為 WinUI 控制項和平台控制項安裝新的預設樣式。 當您在應用程式中使用 WinUI 2.2 時，會自動使用這些樣式；您不需要採取任何進一步的動作，就能使用新的樣式。 不過，本文稍後會示範如何自訂圓角 (如果您需要這麼做)。

> [!IMPORTANT]
> 有些控制項同時適用於平台 ([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls)) 和 WinUI ([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2))，例如 **TreeView** 或 **ColorPicker**。 當您在應用程式中使用 WinUI 時，您應該使用控制項的 WinUI 版本。 搭配 WinUI 使用時，邊角修圓可能不會一致地套用於平台版本。

> **重要 API**：[Control.CornerRadius 屬性](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>預設控制項設計

控制項有三個區域使用了圓角樣式：矩形元素、飛出視窗元素和條狀元素。

### <a name="corners-of-rectangle-ui-elements"></a>矩形 UI 元素的邊角

- 這些 UI 元素包含基本控制項，例如使用者隨時在畫面上看到的按鈕。
- 我們用於這些 UI 元素的預設半徑值為 **2px**。

![醒目提示圓角的按鈕](images/rounded-corner/button.png)

**控制項**

- AutoSuggestBox
- 按鈕
  - ContentDialog 按鈕
- CalendarDatePicker
- CheckBox
  - TreeView 多選核取方塊
- ComboBox
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>飛出視窗和重疊 UI 元素的邊角

- 這些可以是暫時出現在畫面上的暫時性 UI 元素 (例如 MenuFlyout)，或是會覆疊其他 UI 的元素 (例如 TabView 索引標籤)。
- 我們用於這些 UI 元素的預設半徑值為 **4px**。

![飛出視窗範例](images/rounded-corner/flyout.png)

**控制項**

- CommandBarFlyout
- ContentDialog
- 飛出視窗
- MenuFlyout
- TabView 索引標籤
- TeachingTip
- ToolTip
- 飛出視窗部分 (開啟時)
  - AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>條狀元素

- 這些 UI 元素的形狀類似條狀或線條，例如 ProgressBar。
- 我們在這裡使用的預設半徑值為 **2px**。

![進度列範例](images/rounded-corner/bars.png)

**控制項**

- NavigationView 選取指標
- 樞紐選取指標
- ProgressBar
- ScrollBar (當 `IndicatorMode=TouchIndicator` 時)
- 滑桿
  - ColorPicker 色彩滑杆
  - MediaTransportControls 搜尋列滑杆

## <a name="customization-options"></a>自訂選項

我們提供的預設圓角半徑值並非一程不變，您有幾種方式可輕鬆地修改邊角的修圓量。 視您想要的自訂細微性層級而定，透過兩個全域資源，或直接在控制項上透過 [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) 屬性即可達成。

### <a name="when-not-to-round"></a>何時不要修圓

在某些情況下，不得將控制項的邊角修圓，且預設不會將其修圓。

- 當容器內的多個 UI 元素彼此接觸時，例如 SplitButton 的兩個部分。 當其碰觸時，應該就沒有任何間隔。

![SplitButton](images/rounded-corner/split-button-2.png)

- 當控制項容納於另一個容器內時，例如 ScrollBar 的條狀和屬於 ScrollBar 容器的按鈕，這也是 ScrollViewer 的一部分。

![ScrollBar](images/rounded-corner/scrollbar.png)

- 當飛出視窗 UI 元素連結到可在一側叫用飛出視窗的 UI 時。

![AutoSuggest](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>鍵盤焦點矩形和陰影

我們的預設設計並不會執行任何特殊工作來修圓鍵盤焦點矩形或控制項陰影的邊角。 使用較高的圓角半徑值並不會使其沒有作用。不過，請注意這一點，以避免較大的值可能帶來的不必要視覺問題。

以下是較大圓角半徑如何讓陰影看起來沒有必要的範例：

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>圓角和效能

呈現圓角自然會使用比呈現方角更多的繪製功能。 選取預設圓角半徑值時，我們不只考量到設計原則，而且還小心翼翼地確保當您在應用程式中使用這些值時，我們的預設控制項執行得很好。

在這種情況下考慮應用程式效能時，您應該主要考量頁面載入時間和應用程式啟動時間。 請考量較大 UI 介面上的圓角會對效能造成較大的影響。 避免在全螢幕應用程式 UI 上繪製圓角。 如果 UI 短暫顯示，且在頁面 (例如 ContentDialog) 載入之後，就不成問題。

### <a name="page-or-app-wide-cornerradius-changes"></a>整頁或全應用程式的 CornerRadius 變更

有 2 個應用程式資源可控制所有控制項的圓角半徑：

- `ControlCornerRadius` - 預設值為 2px。
- `OverlayCornerRadius` - 預設值為 4px。

如果您覆寫任何範圍內這些資源的值，因此會影響該範圍內的所有控制項。

這表示如果您想對所有可套用圓度的控制項變更其圓度，可以在應用程式層級使用新的 CornerRadius 值來定義這兩個資源，如下所示：

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">0</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">0</CornerRadius>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

或者，如果您想變更特定範圍 (例如頁面或容器層級) 內所有控制項的圓度，可以遵循類似的模式：

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> `OverlayCornerRadius` 資源必須在應用程式層級定義，才會生效。
>
>這是因為快顯和飛出視窗都是動態的，而且建立於視覺化樹狀結構的根元素，因此其使用的任何資源也必須在該處定義。 否則會超出範圍。

### <a name="per-control-cornerradius-changes"></a>每個控制項的 CornerRadius 變更

如果您只想變更所選控制項數目的圓度，可以直接在控制項上修改 [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) 屬性。

|Default | 已修改屬性 |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

並非所有控制項的邊角都會回應其進行修改的 `CornerRadius` 屬性。 若要確保您想修圓其邊角的控制項確實會以您預期的方式回應其 `CornerRadius` 屬性，請先檢查 `ControlCornerRadius` 或 `OverlayCornerRadius` 全域資源是否會影響有問題的控制項。 如果不會，請檢查您想修圓的控制項到底是否有邊角。 我們有許多控制項都不會呈現實際的邊緣，因此無法正確使用 `CornerRadius` 屬性。

### <a name="basing-custom-styles-on-winui"></a>在 WinUI 上建立自訂樣式的基礎

您可以在樣式中指定正確的 `BasedOn` 屬性，將自訂樣式建立在 WinUI 圓角樣式的基礎上。 例如，若要以 WinUI 按鈕樣式為基礎，建立自訂按鈕樣式，請執行下列動作：

```xaml
<Style x:Key="MyCustomButtonStyle" BasedOn="{StaticResource DefaultButtonStyle}">
   ...
</Style>
```

通常，WinUI 控制項樣式遵循一致的命名慣例："DefaultXYZStyle"，其中 "XYZ" 是控制項的名稱。 如需完整參考，您可以瀏覽 WinUI 存放庫中的 XAML 檔案。
