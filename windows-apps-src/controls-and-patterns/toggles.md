---
Description: 切換開關相當於實體開關，讓使用者能夠開啟或關閉選項。
title: 切換開關控制項的指導方針
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: 切換開關
template: detail.hbs
---
# 切換開關

切換開關相當於實體開關，讓使用者能夠開啟或關閉選項。 使用 **ToggleSwitch** 控制項呈現兩個互斥的選項 (例如開/關)，選擇其中一個選項就會立即執行動作。

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**ToggleSwitch 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx)
-   [**IsOn 屬性**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx)
-   [**Toggled 事件**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx)

## 這是正確的控制項嗎？

針對在使用者輕觸切換開關之後立即生效的二元運算，使用切換開關。 例如，使用切換開關來開啟或關閉服務或硬體元件，例如 WiFi：

![WiFi 切換開關，開啟和關閉](images/toggleswitches01.png)

如果實體開關適用於該動作，則切換開關可能是要使用的最佳控制項。

在使用者切換開關後，建議您立即執行對應的動作。

### 選擇切換開關或核取方塊

對於某些動作，切換開關或核取方塊可能都能運作。 若要決定哪一個控制項較為適用，請遵循下列祕訣：

-   針對二元設定使用切換開關，在使用者變更後即可立即生效。

    ![切換開關與核取方塊的比較](images/toggleswitches02.png)

    在上述範例中，在使用切換開關時，很明顯地是將無線設定為「開啟」。 但透過核取方塊，使用者需要思考現在是否已將無線設定為開啟，或者他們是否需要核取方塊來開啟無線。

-   當使用者需要執行額外步驟以便讓變更生效時，請使用核取方塊。 例如，如果使用者必須按一下 [提交] 或 [下一步] 按鈕以套用變更時，請使用核取方塊。

    ![核取方塊和 [提交] 按鈕](images/submitcheckbox.png)

-   當使用者可以選取多個項目時，請使用核取方塊或[清單方塊](lists.md)：

    ![已選取多個項目的核取方塊](images/guidelines_and_checklist_for_toggle_switches_checkbox_multi_select.png)

## 範例

新聞應用程式之一般設定中的切換開關。

![新聞應用程式之一般設定中的切換開關](images/control-examples/toggle-switch-news.png)

Windows 開始功能表設定中的切換開關。

![Windows 開始功能表設定中的切換開關](images/control-examples/toggle-switch-start-settings.png)

## 建立切換開關

以下說明如何建立簡易切換開關。 此 XAML 會建立如前述內容的 WiFi 切換開關。

```xaml
<ToggleSwitch x:Name="wiFiToggle" Header="Wifi"/>
```
以下說明如何在程式碼中建立相同的切換開關。

```csharp
ToggleSwitch wiFiToggle = new ToggleSwitch();
wiFiToggle.Header = "WiFi";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(wiFiToggle);
```

### IsOn

此開關可為開啟或關閉。 使用 [**IsOn**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx) 屬性判斷開關的狀態。 若使用此開關控制另一個二進位屬性的狀態時，則您可如此處所示使用繫結。

```
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### Toggled

在其他情況下，您可處理 [**Toggled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx) 事件以回應狀態的變更。

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

### 開啟/關閉標籤

根據預設，切換開關包含已自動當地語系化的「開啟」與「關閉」標籤常值。 您可以藉由設定 [**OnContent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontent.aspx) 和 [**OffContent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontent.aspx) 屬性來取代這些標籤。

此範例會將「開啟/關閉」標籤取代為「顯示/隱藏」標籤。  

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide" 
              Toggled="ToggleSwitch_Toggled"/>
```

您亦可藉由設定 [**OnContentTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontenttemplate.aspx) 和 [**OffContentTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontenttemplate.aspx) 屬性，使用更複雜的內容。

## 建議

-   當設定有更明確的標籤時，可用來取代「開」和「關」標籤。 如果有簡短標籤 (3 到 4 個字元) 來代表更適用於特定設定的二元對立時，請使用這類標籤。 舉例而言，若設定為「顯示影像」，則您可使用「顯示/隱藏」。 使用更明確的標籤可協助進行 UI 當地語系化。
-   如非必要，請避免取代「開」和「關」標籤：盡量使用預設的標籤，除非狀況會呼叫自訂的標籤。
-   標籤長度應小於 4 個字元。

## 相關文章

[**ToggleSwitch**](https://msdn.microsoft.com/library/windows/apps/hh701411)
- [選項按鈕](radio-button.md)
- [切換開關](toggles.md)
- [核取方塊](checkbox.md)

**適用於開發人員 (XAML)**
- [**ToggleSwitch 類別**](https://msdn.microsoft.com/library/windows/apps/br209712)


<!--HONumber=Mar16_HO1-->


