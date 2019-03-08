---
title: 了解追蹤建構和設定表單
description: 了解您需要執行什麼來建立應用程式中健全的表單。
ms.date: 05/07/2018
ms.topic: article
keywords: 開始使用, uwp, windows 10, 了解曲目, 版面配置, 表單
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 45c4acda953808a85363d1cec7c623dd79cc08f1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656443"
---
# <a name="create-and-customize-a-form"></a>建立和自訂表單

如果您要建立的應用程式需要使用者輸入大量資訊，有可能會是您會想要建立他們填寫的表單。這篇文章將說明您需要知道，才能建立有用且強固的表單。

這不是教學課程。 如果您需要，請參閱我們的[調適型版面配置教學課程](../design/basics/xaml-basics-adaptive-layout.md)，將為您提供逐步導覽體驗。

我們將討論哪些 **XAML 控制項** 要放進表單，如何在頁面上做最佳排列，以及如何最佳化您的表單以符合變更螢幕大小。 但因為表單與視覺元素相關，讓我們先討論使用 XAML 的版面配置。

## <a name="what-do-you-need-to-know"></a>您需要知道哪些事項？

UWP 沒有明確的表單控制項，您可以將其新增到應用程式並進行設定。 相反地，您將需要透過排列頁面上 UI 元素的集合來建立表單。

若要這樣做，您必須了解 **版面配置面板**。 其為保留應用程式 UI 元素的容器，允許您排列和群組它們。 將版面配置面板放置於其他版面配置面板中，可讓您控制您的項目。要在哪裡以及如何顯示彼此之間的關係。 這也可以讓您的應用程式更容易適應變更螢幕大小。

閱讀[版面配置面板上的此份文件](../design/layout/layout-panels.md)。 通常會在一個或多個垂直欄位中顯示表單，因此您要在 **StackPanel** 中群組相似的項目，並在 **RelativePanel** 中排列，如果您有需要的話。 現在開始將一些版面放在一起，如果您需要參考資料，以下是兩欄表單的基本版面配置架構：

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Customer">
        <!--Save and Cancel buttons-->
    </StackPanel>
</RelativePanel>
```

## <a name="what-goes-in-a-form"></a>有什麼項目會在表單中？

您將需要各種 [XAML 控制項](../design/controls-and-patterns/controls-and-events-intro.md) 來填入表單。 您可能對此很熟悉了，但若您需要複習，請隨時閱讀。 尤其是，您會需要控制項，可讓您的使用者輸入文字，或從值的清單中選擇。 這是基本的清單，您可以新增的選項的 – 您不需要讀取其相關的所有項目只，使您了解它們的外觀和運作方式。

* [文字方塊](../design/controls-and-patterns/text-box.md)可讓您的應用程式的使用者輸入的文字。
* [ToggleSwitch](../design/controls-and-patterns/toggles.md) 可讓使用者在兩個選項中選擇。
* [DatePicker](../design/controls-and-patterns/date-picker.md) 可讓使用者選取一個日期的值。
* [TimePicker](../design/controls-and-patterns/time-picker.md) 可讓使用者選取一個日期的值。
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 展開，以顯示可選取項目的清單。 您也可以在[這裡](../design/controls-and-patterns/lists.md#drop-down-lists) 了解更多資訊。

您可能也會想要新增[按鈕](../design/controls-and-patterns/buttons.md)，讓使用者可以儲存或取消。

## <a name="format-controls-in-your-layout"></a>版面配置中的格式控制項

您知道如何排列版面配置面板和擁有您想要新增的項目，但應如何將它們格式化呢？ [表單](../design/controls-and-patterns/forms.md) 頁面有一些特定的設計指導方針。 閱讀**表單類型**和**版面配置**上的章節，取得有用的建議。 我們稍後將討論協助工具與相對版面配置。

請牢記該建議，您應該開始將選擇的控制項新增至您的版面配置，確保給其正確的標籤與間隔。 例如，以下是使用上述版面配置、控制項和設計指導方針的單一頁面表單基本外框：

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" HorizontalAlignment="Left" />
            <RelativePanel>
                <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" HorizontalAlignment="Left" />
                <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0" RelativePanel.RightOf="City">
                    <!--List of valid states-->
                </ComboBox>
            </RelativePanel>
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.Below="Customer">
        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Associate">
        <Button Content="Save" Margin="24" />
        <Button Content="Cancel" Margin="24" />
    </StackPanel>
</RelativePanel>
```

歡迎自訂您控制項與更多的屬性，以獲得更好的視覺體驗。

## <a name="make-your-layout-responsive"></a>讓您的版面配置回應

使用者可能會在各種不同螢幕寬度的裝置上檢視您的 UI。 不論他們的螢幕為何，為了確保他們有良好的體驗，您應該使用[回應式設計](../design/layout/responsive-design.md)。 仔細閱讀該頁面，以獲得設計原則的良好建議，在您繼續時牢記在心。

[使用 XAML 的回應式版面配置](../design/layout/layouts-with-xaml.md) 頁面提供如何實作這個的詳細概觀。 現在，我們會著重於 **流暢版面配置** 和 **XAML 中的視覺狀態**。

我們已整合的基本表單外框已是 **流暢版面配置**，因為它主要根據控制項的相對位置，只需使用特定的像素大小與位置 儘管如此，為了將來可能建立更多的 UI，請記住此指導方針。

對於回應式版面配置更重要的是 **視覺狀態。** 當指定的條件為 true 時，視覺狀態會定義要套用到指定元素的屬性值。 [請閱讀如何在 xaml 中執行此操作](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup)，然後將它們實作到您的表單。 以下是我們之前範例中 *最* 基本的一個：

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="640" />
            </VisualState.StateTriggers>

            <VisualState.Setters>
                <Setter Target="Associate.(RelativePanel.RightOf)" Value="Customer"/>
                <Setter Target="Associate.(RelativePanel.Below)" Value=""/>
                <Setter Target="Save.(RelativePanel.Below)" Value="Customer"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

<RelativePanel>
    <!--Previous 3 stack panels-->
</RelativePanel>
```

為各種螢幕大小建立視覺狀態是不切實際的，對於您應用程式的使用者體驗也不太可能產生顯著的影響。 我們建議設計用於幾個重要中斷點，做為替代方案，您可以 [在這裡閱讀更多資訊](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

## <a name="add-accessibility-support"></a>新增協助工具支援

既然您的版面配置建構良好，可回應螢幕大小的變更，您可以改善使用者體驗的最後一件事就是 [讓您的應用程式可存取](../design/accessibility/accessibility-overview.md)。 有許多可以討論的，但像這樣的表單，比外觀看起來要簡單的多。 著重於下列動作︰

* 鍵盤支援 - 確保 UI 面板中的元素順序，與顯示在螢幕上的相符，讓使用者可以輕鬆透過這些元素索引標籤。
* 螢幕助讀程式支援 - 確保您所有的控制項都有描述的名稱。

當您使用更多視覺元素建立更複雜的版面配置時，您會想要參閱 [協助工具的檢查清單](../design/accessibility/accessibility-checklist.md) 獲得詳細資訊。 畢竟，雖然協助工具對應用程式來說不是必要的，但它有助於應用程式觸達並與較廣的對象互動。

## <a name="going-further"></a>更進一步

雖然您在此建立一個表單，但版面配置與控制項的概念仍適用於您可能建立的所有 XAML UI。 放心地回頭檢查文件，我們已連結到您和試驗您有，加入新的 UI 功能，並進一步精簡的使用者體驗的表單。 如果您想透過更詳細的版面配置功能的逐步指引，請參閱我們[調適性的版面配置教學課程](../design/basics/xaml-basics-adaptive-layout.md)

表單也不一定要與世隔絕，您可以往前一個步驟，將您的表單嵌入 [主要/詳細資料模式](../design/controls-and-patterns/master-details.md) 或 [樞紐控制項](../design/controls-and-patterns/pivot.md)。 或如果您想要讓表單在程式碼後置上工作，您會需要開始使用我們的 [事件概觀](../xaml-platform/events-and-routed-events-overview.md)。

## <a name="useful-apis-and-docs"></a>實用的 API 和文件

以下是 API 的快速摘要，以及其他實用的文件，有助於您開始使用資料繫結。

### <a name="useful-apis"></a>實用的 API

| API | 描述 |
|------|---------------|
| [適用於表單的控制項](../design/controls-and-patterns/forms.md#input-controls) | 用於建立表單的實用輸入控制項清單，以及在什麼地方使用它們的基本指導方針。 |
| [方格](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | 用於在多列與多欄版面配置中排列控制項的面板。 |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | 適用於排列對應於其他元素與面板界限的項目的面板。 |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | 適用於在單一水平或垂直列中排列元素的面板。 |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | 當他們在特定狀態下，可讓您設定的 UI 元素的外觀。 |

### <a name="useful-docs"></a>實用的文件

| 主題 | 描述 |
|-------|----------------|
| [協助工具概觀](../design/accessibility/accessibility-overview.md) | 應用程式中協助工具選項的廣泛縮放概觀。 |
| [協助工具檢查清單](../design/accessibility/accessibility-checklist.md) | 實用的檢查清單以確保您的應用程式符合協助工具標準。 |
| [事件概觀](../xaml-platform/events-and-routed-events-overview.md) | 新增與組織事件以處理 UI 動作的詳細資訊。 |
| [表單](../design/controls-and-patterns/forms.md) | 建立表單的整體指導方針。 |
| [版面配置面板](../design/layout/layout-panels.md) | 提供版面配置面板類型的概觀，以及使用這些類型的位置。 |
| [主版/詳細資料模式](../design/controls-and-patterns/master-details.md) | 設計模式可在一個或多個表單中實作。 |
| [樞紐分析控制項](../design/controls-and-patterns/pivot.md) | 一個控制項可包含一個或多個表單。 |
| [回應式設計](../design/layout/responsive-design.md) | 大型回應式設計原則的概觀。 | 
| [使用 XAML 的回應式版面配置](../design/layout/layouts-with-xaml.md) | 視覺狀態和其他回應式設計實作的特定資訊。 |
| [回應式設計的螢幕大小](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | 哪些螢幕大小應將回應式版面配置的範圍限制在哪個範圍內的指導方針。 |

## <a name="useful-code-samples"></a>實用的程式碼範例

| 程式碼範例 | 描述 |
|-----------------|---------------|
| [自動調整的版面配置教學課程](../design/basics/xaml-basics-adaptive-layout.md) | 透過調適型版面配置和回應式設計的逐步引導體驗。 | 
| [客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | 請參閱多頁面企業範例中的版面配置和表單的實際運作。 |
| [XAML 控制項陳列庫](https://github.com/Microsoft/Xaml-Controls-Gallery) | 請參閱 XAML 控制項的選取和實作方式。 |
| [額外的程式碼範例](https://developer.microsoft.com//windows/samples) | 在類別下拉式清單中選擇 **控制項、版面配置和文字** 以參閱相關的程式碼範例。 |
