---
Description: The toggle switch represents a physical switch that allows users to turn things on or off.
title: 切換開關控制項的指導方針
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d0436f360facceb4a7ee0d02defd5fac2e33eaae
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8800896"
---
# <a name="toggle-switches"></a>切換開關

切換開關相當於實體開關，讓使用者可以開啟或關閉選項，就像燈光開關一樣。 使用切換開關控制項向使用者顯示兩個互斥的選項 (例如開啟/關閉)，選擇其中一個選項就會立即給出結果。

若要建立切換開關控制項，請使用 [ToggleSwitch 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)。

> **重要 API**：[ToggleSwitch 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)、[IsOn 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison)、[Toggled 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

將切換開關用於立即在使用者輕觸切換開關後產生效果的二元操作。

![Wi-Fi 切換開關, 開啟和關閉](images/toggleswitches01.png)

將切換開關想象成裝置的電源開關：要啟用或停用裝置所執行的動作時，您會將它打開或關掉。

為了讓切換開關容易了解，請用一兩個描述它所控制之功能的文字來加以標示，最好是名詞。 例如，「Wi-Fi」或「廚房燈」。 

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> 或 <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>選擇切換開關或核取方塊

對於某些動作，切換開關或核取方塊可能都能運作。 若要決定哪一個控制項較為適用，請遵循下列祕訣：

- 針對二元設定使用切換開關，在使用者變更後即可立即生效。

    ![切換開關與核取方塊的比較](images/toggleswitches02.png)

    在此範例中，從切換開關可以很清楚看出，廚房燈已設定為「開啟」。 但透過核取方塊，使用者需要思考現在是否已將廚房燈設定為開啟，或者他們是否需要核取方塊來開啟這些燈。

- 將核取方塊用於選擇性 (「有了也不錯」) 項目。
- 當使用者需要執行額外步驟讓變更生效時，請使用核取方塊。 例如，如果使用者必須按一下 [提交] 或 [下一步] 按鈕以套用變更時，請使用核取方塊。
- 當使用者可以選取多個與單一設定或功能相關的項目時，請使用核取方塊。

## <a name="toggle-switches-in-the-windows-ui"></a>Windows UI 中的切換開關

下列影像示範 Windows UI 如何使用切換開關。 以下是 [智慧存放裝置設定] 畫面使用切換開關的方式：

![智慧存放裝置中的切換開關](images/SmartStorageToggle.png)

此範例取自 [夜間光線設定] 頁面：

![Windows 開始功能表設定中的切換開關](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>建立切換開關

以下說明如何建立簡易切換開關。 此 XAML 會建立先前顯示的切換開關。

```xaml
<ToggleSwitch x:Name="lightToggle" Header="Kitchen Lights"/>
```

以下說明如何在程式碼中建立相同的切換開關。

```csharp
ToggleSwitch lightToggle = new ToggleSwitch();
lightToggle.Header = "Kitchen Lights";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(lightToggle);
```

### <a name="ison"></a>IsOn

此開關可為開啟或關閉。 使用 [IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison) 屬性判斷開關的狀態。 若使用此開關控制另一個二進位屬性的狀態時，則您可如此處所示使用繫結。

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

在其他情況下，您可處理 [Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled) 事件以回應狀態的變更。

此範例說明如何在 XAML 和程式碼中新增 Toggled 事件處理常式。 處理 Toggled 事件以開啟或關閉進度環，並變更其可見度。

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True"
              Toggled="ToggleSwitch_Toggled"/>
```

以下說明如何在程式碼中建立相同的切換開關。

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

以下是 Toggled 事件的處理常式。

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### <a name="onoff-labels"></a>開啟/關閉標籤

根據預設，切換開關包含已自動當地語系化的「開啟」與「關閉」標籤常值。 您可以藉由設定 [OnContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) 和 [OffContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent) 屬性來取代這些標籤。

此範例會將「開啟/關閉」標籤取代為「顯示/隱藏」標籤。

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

您也可以藉由設定 [OnContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) 和 [OffContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate) 屬性，使用更複雜的內容。

## <a name="recommendations"></a>建議事項

- 盡可能使用預設的 [開] 和 [關] 標籤。只有在切換開關不需要表達什麼意義時，才取代這些標籤。 如果您加以取代，就使用單獨一個更準確描述開關的字。 一般而言，如果「開」和「關」這兩個字無法描述繫結至切換開關的動作，您可能需要不同的控制項。
- 如非必要，請避免取代 [開] 和 [關] 標籤；除非情況需要自訂標籤，否則繼續使用預設的標籤。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [ToggleSwitch 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [選項按鈕](radio-button.md)
- [切換開關](toggles.md)
- [核取方塊](checkbox.md)
