---
Description: 使用工具提示在要求使用者執行動作前顯示關於控制項的詳細資訊。
title: 工具提示
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a9a5c6deb2ad7ea2b4dad8e7db3e9513700c2c3e
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750624"
---
# <a name="tooltips"></a>工具提示

工具提示是連結至另一個控制項或物件的簡短描述。 工具提示可協助使用者了解 UI 中未直接說明的不熟悉的物件。 當使用者將焦點移至控制項、按住控制項或將滑鼠指標暫留在控制項時，它們將會自動顯示。 當使用者移動手指、指標或鍵盤/遊戲台焦點數秒之後，工具提示就會消失。

![工具提示](images/controls/tool-tip.png)

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](../style/rounded-corner.md)。 WinUI 是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **平台 API**：[ToolTip 類別](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)、[ToolTipService 類別](/uwp/api/windows.ui.xaml.controls.tooltipservice)

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
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/ToolTip">開啟應用程式並查看 ToolTip 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Windows Maps 應用程式中的工具提示。

![Windows Maps 應用程式中的工具提示](images/control-examples/tool-tip-maps.png)

## <a name="create-a-tooltip"></a>建立工具提示

[ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) 必須指派給其擁有者的另一個 UI 元素。 [ToolTipService](/uwp/api/windows.ui.xaml.controls.tooltipservice) 類別提供顯示 ToolTip 的靜態方法。

在 XAML 中，使用 **ToolTipService.Tooltip** 連接的屬性，將 ToolTip 指定給擁有者。

```xaml
<Button Content="Submit" ToolTipService.ToolTip="Click to submit"/>
```

在程式碼中，使用 [ToolTipService.SetToolTip](/uwp/api/windows.ui.xaml.controls.tooltipservice.settooltip) 方法，將 ToolTip 指派給擁有者。

```xaml
<Button x:Name="submitButton" Content="Submit"/>
```

```csharp
ToolTip toolTip = new ToolTip();
toolTip.Content = "Click to submit";
ToolTipService.SetToolTip(submitButton, toolTip);
```

### <a name="content"></a>內容

您可以使用任何物件作為 ToolTip 的[內容](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)。 以下是在 ToolTip 中使用[影像](/uwp/api/windows.ui.xaml.controls.image)的範例。

```xaml
<TextBlock Text="store logo">
    <ToolTipService.ToolTip>
        <Image Source="Assets/StoreLogo.png"/>
    </ToolTipService.ToolTip>
</TextBlock>
```

### <a name="placement"></a>放置

根據預設，ToolTip 會顯示在指標上的置中位置。 位置並不受應用程式視窗的限制，因此 ToolTip 可能會超出應用程式視窗範圍部分顯示或完整顯示。

如需廣泛調整，請使用 [Placement](/uwp/api/windows.ui.xaml.controls.tooltip.placement) 屬性或 **ToolTipService.Placement** 附加屬性，指定應將 ToolTip 拖曳到指標的上、下、左或右。 您可以設定 [VerticalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.verticaloffset) 或 [HorizontalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.horizontaloffset) 屬性來變更指標與 ToolTip 之間的距離。 兩個位移值當中只有一個值會影響最終位置 - Placement 是 Top 或 Bottom 時為 VerticalOffset，Placement 是 Left 或 Right 時為 HorizontalOffset。

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

如果 ToolTip 遮蔽它所參考的內容，您可以使用新的 **PlacementRect** 屬性精確地調整其位置。 PlacementRect 會固定 ToolTip 的位置，也可做為 ToolTip 不會遮蓋的區域，但前提是有足夠的螢幕空間可將 ToolTip 拖曳到此區域外部。 您可以指定 ToolTip 擁有者的相關矩形來源，以及排除區域的高度和寬度。 [Placement](/uwp/api/windows.ui.xaml.controls.tooltip.placement)屬性會定義 ToolTip 應拖曳到 PlacementRect. 的上、下、左或右。 

```xaml
<!-- An Image with a non-occluding ToolTip. -->
<Image Source="Assets/StoreLogo.png" Height="64" Width="96">
    <ToolTipService.ToolTip>
        <ToolTip Content="Non-occluding ToolTip."
                 PlacementRect="0,0,96,64"/>
    </ToolTipService.ToolTip>
</Image>
```

## <a name="recommendations"></a>建議

- 請謹慎使用工具提示 (或完全不使用)。 工具提示是一種干擾。 工具提示和快顯一樣讓人分心，所以除非它們具有重要價值，否則不要使用。
- 讓工具提示文字簡潔。 工具提示適合簡短句子和句子片段。 大型文字區塊可能太具壓迫性，且工具提示可能會在使用者完成閱讀之前即逾時。
- 建立有用、補充的工具提示文字。 工具提示必須具有資訊性。 不要讓文字太明顯，或只是重複畫面上已經有的內容。 由於工具提示文字不一定會顯示，因此它應該是使用者不一定要閱讀的補充資訊。 請使用一目了然的控制項標籤或就地補充文字來傳達重要資訊。
- 視需要使用影像。 有時候在工具提示中較適合使用影像。 例如，當使用者暫留在一個超連結的時候，您可以使用工具提示來顯示連結頁面的預覽。
- 不要使用工具提示顯示 UI 中已經顯示的文字。 例如，請不要在按鈕上放置與按鈕本身顯示相同文字的工具提示。
- 不要在工具提示內放置互動式控制項。
- 不要在工具提示內放置看起來像是互動式影像的影像。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [ToolTip 類別](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)
