---
author: Xansky
Description: "描述使用高對比佈景主題時，確保通用 Windows 平台 (UWP) App 可以正常運作所需的步驟。"
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: "高對比佈景主題"
label: High-contrast themes
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 4201f5a0b08f1fc8d691218da0803ee04ab2c86a

---

# 高對比佈景主題  

描述使用高對比佈景主題時，確保通用 Windows 平台 (UWP) App 可以正常運作所需的步驟。

根據預設，UWP App 可支援高對比佈景主題。 如果使用者已選擇要讓系統使用來自系統設定或協助工具的高對比佈景主題，架構就會自動針對 UI 中的控制項和元件，使用產生高對比配置和呈現方式的色彩和樣式設定。

這項預設支援所根據的是使用預設佈景主題和範本。 這些佈景主題和範本會將系統色彩當作資源定義來參考，當系統使用高對比模式時，資源來源就會自動變更。 不過，如果您的控制項使用自訂範本、佈景主題以及樣式，請小心，不要停用內建的高對比支援。 如果設定樣式時使用了其中一個適用於 Microsoft Visual Studio 的 XAML 設計工具，只要您定義的範本與預設範本有明顯的差異，此設計工具除了產生主要的佈景主題之外，還會另外產生高對比佈景主題。 獨立的佈景主題字典會放到 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx) 集合，它是 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 元素的專用屬性。

如需佈景主題與控制項範本的詳細資訊，請參閱[快速入門：控制項範本](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374)。 這裡通常會提供非常有用的資訊，例如特定控制項的 XAML 資源字典以及佈景主題，也可以了解佈景主題的建構方式，以及參考資源類似但每個可能的高對比設定不同的方式。

## 佈景主題字典

當您需要變更系統預設色彩或需要新增影像做為裝飾 (例如背景影像)，請針對您的 app 建立 **ThemeDictionaries** 集合。

* 由建立適當的配置開始 (如果尚未存在)。 在 App.xaml 中，建立 **ThemeDictionaries** 集合：

``` xaml
 <Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">

            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">

            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources
```

* **HighContrast** 不是唯一可用的索引鍵名稱。 另外還有 **HighContrastBlack**、**HighContrastWhite** 以及 **HighContrastCustom**。 在大部分情況下，您只需要 **HighContrast**。
* 在 **Default** 中，建立您所需要的[**筆刷**](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx)類型，通常是 **SolidColorBrush**。 針對它的用途來指定 **x:Key** 名稱：<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" />`
* 指派您想要的**色彩**：<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />`
* 將該**筆刷**複製到 **HighContrast**：

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

* 決定您的**筆刷**應有的顏色，並在 **HighContrast** 中修改它。

決定高對比的色彩需要學習。 您在上方建立的配置可讓您更容易更新。

## 高對比色彩

使用者可以使用 [設定] 頁面切換到高對比。 根據預設，有 4 個高對比佈景主題。 一旦使用者選取一個選項，頁面會顯示 app 可能外觀的預覽。

![高對比設定](images/high-contrast-settings.png)<br/>
_高對比設定_

 按一下預覽上的每個方形，即可變更其值。 每個方形也會直接對應到系統資源。

![高對比資源](images/high-contrast-resources.png)<br/>
_高對比資源_

如果您將 _SystemColor_ 前置於上述叫出的名稱且後置 _Color_，例如：**SystemColorWindowTextColor**，這些名稱會動態更新成符合使用者指定的外觀。 這會讓您不用選取高對比的特定色彩。 相反地，改為挑選對應到已使用何種色彩的系統資源。 在上述範例中，我們將我們的網頁背景色彩命名為 **SolidColorBrushBrandedPageBackground**。 因為這將使用於背景，所以我們可以將此對應至高對比中的 **SystemColorWindowColor**：

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

如果您依照 8 個高對比色彩的調色盤，則您不需要建立任何其他的高對比 **ResourceDictionaries**。 這個有限的調色盤在呈現複雜視覺狀態時常會代表困難的挑戰。 通常，將框線新增到只在高對比中的區域，可協助釐清情況。

### 可行與禁止事項

* 請務必及早在高對比模式中測試，並經常測試。
* 請務必以其設計目的使用具名色彩。
* 請務必將基本型別 (例如 **Color**、**Brush** 與 **Thickness**) 放置於 **ThemeDictionaries** 內。 請避免將更複雜的資源 (例如 **Style** 元素) 放置在它們之中。 下列範例都能正常運作：

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>

        <Style x:Key="MyButtonStyle" TargetType="Button">
            <Setter Property="Foreground" Value="{ThemeResource BrandedPageBackground}" />
        </Style>
    </ResourceDictionary>
</Application.Resources>

...

<Button Style="{StaticResource MyButtonStyle}" />
```

* 請務必針對前景 UI 元素使用高對比前景色彩。
* 請務必搭配其定義的色彩組合使用高對比色彩。 例如，請一律搭配 **BUTTONFACE** 使用 **BUTTONTEXT**，特別是在前景/背景的情況下。
* 請務必針對特定 UI 元素使用建議的高對比色彩組合，以確定符合所需的 14:1 對比率。
* 請勿分割高對比色彩組合，或任意混合與比對高對比色彩。 這通常會對至少其中一個已預先安裝的高對比佈景主題建立不可見的 UI。
* 請勿將您建立的任何 **Brush** 物件放置於 **ThemeDictionaries** 集合外。
* 請勿使用 **StaticResource** 來參照 **ThemeDictionaries** 集合中的資源。 這似乎沒有問題，直到使用者在您的 app 執行時變更佈景主題。 請改為使用 **ThemeResource**。
* 請勿使用硬式編碼的色彩值。
* 請勿因為個人喜好就使用某個色彩。

如需詳細資訊，請參閱 [XAML 佈景主題資源](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)。

## 使用邊框的時機
在高對比模式中，在 UI 元素中需要框線的位置加上框線，以在項目上保留可辨識的界限形狀。 使用框線區分不同的瀏覽、動作和內容區域。

![與頁面的其餘部分分開的瀏覽窗格](images/high-contrast-actions-content.png)<br/>
_與頁面的其餘部分分開的瀏覽窗格_

如果預設 UI 元素「沒有」__框線或背景，請勿在高對比模式中對預設狀態新增框線或背景。

如果預設 UI 元素「有」__框線，則保留高對比模式中的框線。

重疊或相鄰色彩應該能夠互相區分，但它們不一定需要符合 14:1 的色彩對比率。 不過，針對這些類型的案例，最好的做法是 3:1 的色彩對比率。

如果高對比背景色彩用來區別重疊的 UI 元素，要確保這些項目之間的對比，唯一保證的方法就是加入框線。

## 偵測何時啟用高對比佈景主題  
使用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 類別的成員，偵測高對比佈景主題目前的設定。 [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrast) 屬性會判斷目前是否選取高對比佈景主題。 如果將 **HighContrast** 設定為 **true**，則下一步是檢查 [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrastscheme) 屬性的值，以取得使用的高對比佈景主題名稱。 「白底黑字」和「黑底白字」一般是程式碼應該回應的 **HighContrastScheme** 值。 XAML 定義的 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 索引鍵不可包含空格，因此在資源字典中，這些佈景主題的索引鍵通常是「HighContrastWhite」和 「HighContrastBlack」。 您也應該針對預設的高對比佈景主題準備後援邏輯，以便在這個值是其他字串時使用。 [XAML 高對比範例](http://go.microsoft.com/fwlink/p/?linkid=254993)會顯示這種情況下的邏輯。

> [!NOTE]
> 請確定您是從已將 App 初始化且已經顯示內容的範圍內呼叫 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 建構函式。

App 可在執行時，切換成使用高對比資源值。 只要在樣式或範本 XAML 中使用 [{ThemeResource} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt185591)來要求資源，就可以達到這個目的。 預設佈景主題 (generic.xaml) 都會使用這項 {ThemeResource} 標記延伸技巧，因此，如果您使用預設控制項佈景主題，就會獲得這項行為。 如果您在自訂範本和樣式中也使用了這項 {ThemeResource} 標記延伸資源技巧，則自訂控制項或自訂控制項樣式也能夠執行這項工作。

## 相關主題  
* [協助工具](accessibility.md)
* [UI 對比和設定範例](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 高對比範例](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)



<!--HONumber=Jul16_HO1-->


