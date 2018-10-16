---
author: Jwmsft
Description: Use a tooltip to reveal more info about a control before asking the user to perform an action.
title: 工具提示
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5a61b8bdcfcfad490528cdceed5e732a6f5f3a89
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4687952"
---
# <a name="tooltips"></a>工具提示

工具提示是連結至另一個控制項或物件的簡短描述。 工具提示可協助使用者了解 UI 中未直接說明的不熟悉的物件。 當使用者將焦點移至控制項、按住控制項或將滑鼠指標暫留在控制項時，它們將會自動顯示。 當使用者移動手指、指標或鍵盤/遊戲台焦點數秒之後，工具提示就會消失。

![工具提示](images/controls/tool-tip.png)

> **重要 API**：[ToolTip 類別](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)、[ToolTipService 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltipservice)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用工具提示在要求使用者執行動作前顯示關於控制項的詳細資訊。 您應謹慎使用工具提示，只有當它們可以為試著完成工作的使用者明顯帶來效用時，才應使用工具提示。 通用的原則是，如果相同經驗的別處也提供某個資訊，就不需要工具提示。 有價值的工具提示將可釐清不清楚的動作。

什麼時候應該使用工具提示？ 若要決定使用時機，請考量下列問題：

- **資訊是否應隨著指標的暫留而變得可見？**
    如果不是，請使用其他控制項。 提示僅顯示為使用者互動的結果時，一律不應獨立顯示。

- **控制項是否有文字標籤？**
    如果沒有，請使用工具提示提供標籤。 理想的 UX 設計做法，是標記大部分的內嵌控制項以及不需要工具提示的控制項。 僅顯示圖示的工具列控制項和命令按鈕需要工具提示。

- **說明或進一步的資訊對了解物件有沒有幫助？**
    如果有，請使用工具提示。 不過，文字必須是補充性質的，也就是說，並非主要工作所必要的。 如果是必要的文字，請將它直接放入 UI 中，使用者就不必到處尋找這些文字。

- **補充資訊是否為錯誤、警告或狀態？**
    如果是，請使用其他 UI 元素，例如飛出視窗。

- **使用者是否需要與提示互動？**
    如果是，請使用其他控制項。 使用者無法與提示互動，因為移動滑鼠會讓提示消失。

- **使用者是否需要列印補充資訊？**
    如果是，請使用其他控制項。

- **使用者是否覺得提示會造成困擾或干擾？**
    如果是，請考慮使用其他解決方案，包括不執行任何動作。 如果您要在可能造成干擾的地方使用提示，請允許使用者將其關閉。

## <a name="example"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/ToolTip">開啟應用程式並查看 ToolTip 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Bing 地圖服務應用程式中的工具提示。

![Bing 地圖服務應用程式中的工具提示](images/control-examples/tool-tip-maps.png)

## <a name="create-a-tooltip"></a>建立工具提示

[工具提示](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)必須指派給其擁有者的另一個 UI 元素。 [ToolTipService](/uwp/api/windows.ui.xaml.controls.tooltipservice) 類別提供顯示工具提示的靜態方法。

在 XAML 中，使用 **ToolTipService.Tooltip** 連接的屬性，指定工具提示給擁有者。

```xaml
<Button Content="Submit" ToolTipService.ToolTip="Click to submit"/>
```

在程式碼中，使用 [ToolTipService.SetToolTip](/uwp/api/windows.ui.xaml.controls.tooltipservice.settooltip) 方法，指定工具提示給擁有者。

```xaml
<Button x:Name="submitButton" Content="Submit"/>
```

```csharp
ToolTip toolTip = new ToolTip();
toolTip.Content = "Click to submit";
ToolTipService.SetToolTip(submitButton, toolTip);
```

### <a name="content"></a>內容

您可以使用任何物件做為工具提示的[內容](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)。 以下是在工具提示中使用[影像](/uwp/api/windows.ui.xaml.controls.image)的範例。

```xaml
<TextBlock Text="store logo">
    <ToolTipService.ToolTip>
        <Image Source="Assets/StoreLogo.png"/>
    </ToolTipService.ToolTip>
</TextBlock>
```

### <a name="placement"></a>位置

根據預設，工具提示會顯示在指標上的置中位置。 位置並不受應用程式視窗的限制，因此工具提示可能會超出應用程式視窗範圍部分顯示或完整顯示。

廣泛的調整，使用[位置](/uwp/api/windows.ui.xaml.controls.tooltip.placement)屬性或**ToolTipService.Placement**附加屬性來指定工具提示是否應繪製上方、 下、 左或向右的指標。 您可以設定[VerticalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.verticaloffset)或[HorizontalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.horizontaloffset)的屬性來變更指標與工具提示之間的距離。 只有其中一個兩個位移值將會影響最後位置-VerticalOffset 時位置是頂端或底部，HorizontalOffset 時就會留位置或向右。

```xaml
<!-- An Image with an offset ToolTip. -->
<Image Source="Assets/StoreLogo.png">
    <ToolTipService.ToolTip>
        <ToolTip Content="Offset ToolTip."
                 Placement="Right"
                 HorizontalOffset="20"/>
    </ToolTipService.ToolTip>
</Image>
```

如果工具提示遮蔽其指出的內容是要您可以調整它準確地使用新的**PlacementRect**屬性的位置。 PlacementRect 錨定在工具提示的位置，並提供足夠的螢幕空間，以繪製工具提示此區域以外，也做工具提示將不會遮蓋到，一個區域。 您可以指定相對於工具提示的擁有者，和高度矩形的原點和排除區域的寬度。 如果工具提示應繪製上方、 下、 左或向右的 PlacementRect，將會定義的[配置](/uwp/api/windows.ui.xaml.controls.tooltip.placement)屬性。 

```xaml
<!-- An Image with a non-occluding ToolTip. -->
<Image Source="Assets/StoreLogo.png" Height="64" Width="96">
    <ToolTipService.ToolTip>
        <ToolTip Content="Non-occluding ToolTip."
                 PlacementRect="0,0,96,64"/>
    </ToolTipService.ToolTip>
</Image>
```

## <a name="recommendations"></a>建議事項

- 請謹慎使用工具提示 (或完全不使用)。 工具提示是一種干擾。 工具提示和快顯一樣讓人分心，所以除非它們具有重要價值，否則不要使用。
- 讓工具提示文字簡潔。 工具提示適合簡短句子和句子片段。 大型文字區塊可能太具壓迫性，且工具提示可能會在使用者完成閱讀之前即逾時。
- 建立有用、補充的工具提示文字。 工具提示必須具有資訊性。 不要讓文字太明顯，或只是重複畫面上已經有的內容。 由於工具提示文字不一定會顯示，因此它應該是使用者不一定要閱讀的補充資訊。 請使用一目了然的控制項標籤或就地補充文字來傳達重要資訊。
- 視需要使用影像。 有時候在工具提示中較適合使用影像。 例如，當使用者暫留在一個超連結的時候，您可以使用工具提示來顯示連結頁面的預覽。
- 不要使用工具提示顯示 UI 中已經顯示的文字。 例如，請不要在按鈕上放置與按鈕本身顯示相同文字的工具提示。
- 不要在工具提示內放置互動式控制項。
- 不要在工具提示內放置看起來像是互動式影像的影像。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [ToolTip 類別](https://msdn.microsoft.com/library/windows/apps/br227608)
