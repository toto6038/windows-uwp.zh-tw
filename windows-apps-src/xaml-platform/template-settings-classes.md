---
description: 瞭解如何使用 TemplateSettings 類別來提供一組定義新控制項範本的屬性。
title: 範本設定類別
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 23418233769d893e755ee8981aa9f66aa9404758
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154962"
---
# <a name="template-settings-classes"></a>範本設定類別


## <a name="prerequisites"></a>先決條件

假設您可以將控制項新增至 UI、設定其屬性，以及附加事件處理常式。 如需將控制項新增至 App 的指示，請參閱[新增控制項和處理事件](../design/controls-and-patterns/controls-and-events-intro.md)。 我們也假設您知道如何製作預設範本的複本並加以編輯，來定義控制項自訂範本的基本知識。 如需詳細資訊，請參閱[快速入門：控制項範本](/previous-versions/windows/apps/hh465374(v=win.10))。

## <a name="the-scenario-for-templatesettings-classes"></a>**TemplateSettings** 類別的案例

**TemplateSettings** 類別提供一組屬性，用於定義控制項的新控制項範本。 這些屬性對於特定 UI 元素組件有像素度量之類的值。 這些值有時是依據控制項邏輯而計算的值，通常不容易覆寫或存取。 某些屬性則是做為 **From** 和 **To** 值，控制組件的轉場和動畫，因此相關的 **TemplateSettings** 屬性必須成對。

**TemplateSettings** 有數種類別。 所有類別都位於 [**Windows.UI.Xaml.Controls.Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) 命名空間中。 以下是類別清單，以及相關控制項之 **TemplateSettings** 屬性的連結。 此 **TemplateSettings** 屬性是您如何存取控制項的 **TemplateSettings** 值，並且可以建立範本繫結至它的屬性：

-   [**ComboBoxTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings)：[**ComboBox.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.combobox.templatesettings) 的值
-   [**GridViewItemTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings)：[**GridViewItem.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings) 的值
-   [**ListViewItemTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings)：[**ListViewItem.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings) 的值
-   [**ProgressBarTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings)：[**ProgressBar.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings) 的值
-   [**ProgressRingTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings)：[**ProgressRing.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.progressring.templatesettings) 的值
-   [**SettingsFlyoutTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings)：[**SettingsFlyout.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings) 的值
-   [**ToggleSwitchTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings)：[**ToggleSwitch.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings) 的值
-   [**ToolTipTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings)：[**ToolTip.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings) 的值

**TemplateSettings** 屬性一律用於 XAML 中，而不是在程式碼中。 這些是父項控制項唯讀 **TemplateSettings** 屬性的唯讀子屬性。 在進階的自訂控制項案例中，例如如果建立的新 [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) 基底類別會影響控制項邏輯，請考慮在控制項上定義自訂的 **TemplateSettings** 屬性，以便傳遞可能對任何人都有用的資訊，讓其他人也能重新製作控制項的範本。 由於該屬性的值是唯讀的，所以請為每個與範本度量、動畫定位等相關的資訊項目，定義一個新的 **TemplateSettings** 類別，與您具唯讀屬性的控制項相關，並且提供呼叫者使用您的控制項邏輯初始化該類別的執行階段執行個體。 **TemplateSettings** 類別衍生自 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)，因此屬性可以使用相依性屬性系統做為屬性變更回呼。 但是屬性的相依性屬性識別碼並未公開為公用 API，因為 **TemplateSettings** 屬性對於呼叫者應該是唯讀的。

## <a name="how-to-use-templatesettings-in-a-control-template"></a>如何在控制項範本中使用 **TemplateSettings**

以下範例來自預設 XAML 控制項範本的開頭部份。 這個特殊的範本來自 [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 的預設範本：

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

[**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 範本的完整 XAML 有數百行，這裡只摘錄一小部份。 這個 XAML 定義一個控制項組件，這是六個用來描繪不確定進度之旋轉動畫的 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 元素中的其中一個。 身為開發人員，您可能不喜歡圓圈，可能會使用不同的圖形或不同的基本形狀顯示動畫進行的方式。 例如，您可以撰寫一個 **ProgressRing**，改用一組正方形排列的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 元素。 如果是，新範本的每個個別 **Rectangle** 元件可能看起來像這樣：

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

此時 **TemplateSettings** 屬性就非常實用，因為這些屬性的值是從 [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 的基本控制項邏輯計算而得。 計算分為 **ProgressRing** 整體的 [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 和 [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight)，並且在其範本中為每個動作元素分配計算的測量結果，以便範本組件可以調整為內容的大小。

以下是另一個預設 XAML 控制項範本的範例用法，這次顯示動畫的 **From** 和 **To** 的其中一個屬性集。 這是來自 [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 預設範本：

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

同樣地，範本中有很多 XAML，所以我們只顯示摘錄。 這只是其中一種狀態和佈景主題動畫，每一種都使用相同的 [**ComboBoxTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings) 屬性。 如果是 [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)，在範本中透過繫結使用 **ComboBoxTemplateSettings** 值強制執行相關的動畫，將會在根據共用值計算的位置停止並開始，因此可以順暢地轉換。

**注意**   當您使用**TemplateSettings**值做為控制項範本的一部分時，請確定您正在設定符合數值型別的屬性。 如果不是，您可能需要建立繫結的值轉換器，以便繫結的目標類型可以從 **TemplateSettings** 值的不同來源類型轉換。 如需詳細資訊，請參閱 [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter)。

## <a name="related-topics"></a>相關主題

* [快速入門：控制項範本](/previous-versions/windows/apps/hh465374(v=win.10))