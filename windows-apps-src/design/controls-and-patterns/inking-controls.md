---
author: Karl-Bridge-Microsoft
Description: Ink tools described
title: 筆跡控制項
label: Inking Controls
template: detail.hbs
ms.author: kbridge
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 97eae5f3-c16b-4aa5-b4a1-dd892cf32ead
ms.localizationpriority: medium
ms.openlocfilehash: ed358f88470dfe1ba1c48cd3daf1ed54135ed987
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6653278"
---
# <a name="inking-controls"></a>筆跡控制項



有兩個不同的控制項可協助在通用 Windows 平台 (UWP) app 中使用手寫筆跡：[InkCanvas](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 和 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)。

InkCanvas 控制項可將手寫筆輸入轉譯為筆墨筆劃 (使用色彩與粗細的預設設定) 或擦去筆劃。 此控制項為完全透明的重疊，不包含任何用於變更預設筆墨筆劃屬性的內建 UI。

> [!NOTE]
> InkCanvas 可以同時針對滑鼠與觸控輸入設定，以支援類似的功能。

因為 InkCanvas 控制項不包含變更預設筆墨筆劃設定的支援，所以它可以與 InkToolbar 控制項配對。 InkToolbar 包含了可自訂與可擴充的按鈕集合，可在相關聯的 InkCanvas 中啟用筆跡相關的功能。

根據預設，InkToolbar 包含用於繪圖、清除、反白顯示，以及顯示尺規的按鈕。 根據功能而定，其他設定和命令 (例如筆跡色彩、筆劃粗細、清除所有筆跡) 將會在飛出視窗中提供。

> [!NOTE]
> InkToolbar 支援手寫筆以及滑鼠輸入，並可設定為辨識觸控輸入設定。

<img src="images/ink-tools-invoked-toolbar.png" width="300" alt="InkToolbar palette flyout">

> **重要 API**：[InkCanvas 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)、[InkToolbar 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)、[InkPresenter 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)、[Windows.UI.Input.Inking](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您的應用程式中需要在不提供任何筆跡設定給使用者的情況下支援基本手寫筆跡功能時，請使用 InkCanvas。

根據預設，使用筆尖時會轉譯為筆跡 (粗細為 2 像素的黑色鋼珠筆) 而使用橡皮擦時會轉譯為橡皮擦。 如果橡皮擦不存在，InkCanvas 可以設定為將筆尖輸入做為擦去筆劃處理。

將 InkCanvas 與 InkToolbar 配對來提供用於啟用筆跡功能和設定基本筆跡屬性 (例如筆觸大小、色彩及筆尖形狀) 的 UI。

> [!NOTE] 
> 如需更多 InkCanvas 轉譯的筆墨筆劃自訂項目，請使用基礎 [InkPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) 物件。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/InkCanvas">開啟應用程式並查看 InkCanvas 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Microsoft Edge**

Microsoft Edge 針對**網頁筆記**使用了 InkCanvas 與 InkToolbar。  
![InkCanvas 是用於 Microsoft Edge 中的筆跡](images/ink-tools-edge.png)

**Windows Ink 工作區**

InkCanvas 與 InkToolbar 也會用於 **Windows Ink 工作區**中的**繪圖板**和**螢幕繪圖**。  
![Windows Ink 工作區中的 InkToolbar](images/ink-tools-ink-workspace.png)

## <a name="create-an-inkcanvas-and-inktoolbar"></a>建立 InkCanvas 與 InkToolbar

將 InkCanvas 新增到您的應用程式只需要一行的標記︰

```xaml
<InkCanvas x:Name=“myInkCanvas”/>
```

> [!NOTE]
> 如需使用 InkPresenter 自訂 InkCanvas 的詳細說明，請參閱 [UWP 應用程式中的畫筆和手寫筆互動](http://windowsstyleguide/input/pen-and-stylus-interactions/)一文。

InkToolbar 控制項必須與 InkCanvas 搭配使用。 將 InkToolbar (與所有內建工具) 併入您的應用程式時需要額外的一行標記︰

 ```xaml
<InkToolbar TargetInkCanvas=“{x:Bind myInkCanvas}”/>
 ```

這會顯示下列 InkToolbar：
<img src="images/ink-tools-uninvoked-toolbar.png" width="250" alt="Basic InkToolbar">

### <a name="built-in-buttons"></a>內建按鈕

InkToolbar 包含下列內建按鈕︰

**畫筆**

- 鋼珠筆 - 以圓形筆尖繪製實心、不透明的筆觸。 筆觸大小是根據偵測到的手寫筆壓力。
- 鉛筆 - 以圓形筆尖繪製柔邊、具紋理，以及半透明的筆觸 (適用於分層的陰影效果)。 筆觸色彩 (濃度) 是根據偵測到的手寫筆壓力。
- 螢光筆 – 以矩形筆尖繪製半透明的筆觸。

您可以在各個畫筆的飛出視窗中自訂色彩調色盤和大小屬性 (最小值、最大值、預設值)。

**工具**

- 橡皮擦 – 刪除任何觸碰到的筆墨筆劃。 請注意，這會刪除整個筆墨筆劃，而不只是橡皮擦筆觸下的部分。

**切換**

- 尺規 – 顯示或隱藏尺規。 在尺規邊緣繪製會造成筆墨筆劃貼齊尺規。  
 ![與 InkToolbar 相關聯的尺規視覺效果](images/inking-tools-ruler.png)

雖然這是預設設定，但對於您應用程式的 InkToolbar 所包含的內建按鈕，您還是擁有完整的控制權。

### <a name="custom-buttons"></a>自訂按鈕

InkToolbar 包含兩個不同群組的按鈕類型︰

1. 包含內建繪圖、清除以及醒目提示按鈕的「工具」按鈕群組。 在此處新增自訂的畫筆與工具。
> [!NOTE]
> 功能選項互斥。

2. 包含內建尺規按鈕的「切換」按鈕群組。 在此處新增自訂的切換。
> [!NOTE]
> 功能不會互斥，因此可以和其他使用中的工具同時使用。

根據您的應用程式和所需的手寫筆跡功能，您可以將下列任何按鈕 (繫結到您自訂的筆跡功能) 新增到 InkToolbar：

- 自訂畫筆 – 適用於由主控應用程式定義的筆跡調色盤和筆尖屬性 (例如圖形、旋轉和大小) 的畫筆。
- 自訂工具 – 由主控應用程式定義的非畫筆工具。
- 自訂切換 – 將應用程式定義功能的狀態設定為開啟或關閉。 開啟時，功能會與使用中的工具搭配使用。

> [!NOTE]
> 您無法變更內建按鈕的顯示順序。 預設的顯示順序是︰鋼珠筆、鉛筆、螢光筆、橡皮擦和尺規。 自訂畫筆會附加到最後一個預設畫筆，自訂工具按鈕會新增到最後一個畫筆按鈕與橡皮擦按鈕之間，而自訂切換按鈕會新增到尺規按鈕之後。 (自訂按鈕會以指定的順序新增。)

雖然 InkToolbar 可能是最上層項目，但它通常會透過「手寫筆跡」按鈕或命令公開。 建議您使用 Segoe MLD2 Assets 字型的 EE56 字符做為最上層圖示。

## <a name="inktoolbar-interaction"></a>InkToolbar 互動

所有的內建畫筆和工具按鈕都包含飛出視窗功能表，其中可以設定筆跡屬性、筆尖形狀與大小。 「延伸字符」 ![InkToolbar 字符](images/ink-tools-glyph.png) 會顯示在按鈕上，以表示飛出視窗存在。

再次選取作用中工具的按鈕時，會顯示飛出視窗。 色彩或大小變更時，飛出視窗會自動關閉並且可以繼續使用手寫筆跡。 自訂畫筆和工具可以使用預設飛出視窗，或指定自訂飛出視窗。

橡皮擦也有飛出視窗，可提供 **\[清除所有筆跡\]** 命令。  
![叫用橡皮擦飛出視窗的 InkToolbar](images/ink-tools-erase-all-ink.png)

 如需自訂項目及擴充功能的資訊，請查看 [SimpleInk 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)。

## <a name="dos-and-donts"></a>可行與禁止事項

- InkCanvas (以及一般的手寫筆跡) 最能夠透過主動式手寫筆來體驗。 不過，如果您的應用程式要求，建議您使用滑鼠與觸控式輸入 (包括被動式手寫筆) 支援手寫筆跡。
- 搭配 InkCanvas 使用 InkToolbar 控制項，以提供基本的手寫筆跡功能與設定。 InkCanvas 與 InkToolbar 兩者都可透過程式設計的方式自訂。
- InkToolbar (以及一般的手寫筆跡) 最能夠透過主動式手寫筆來體驗。 不過，如果您的應用程式要求，也可支援使用滑鼠與觸控的手寫筆跡。
- 如果支援使用觸控輸入的手寫筆跡，建議您針對切換按鈕 (包含「觸控書寫」工具提示) 使用 Segoe MLD2 Assets 字型的 ED5F 圖示。
- 如果提供筆觸選取項目，建議您針對工具按鈕 (包含「選取工具」工具提示) 使用 Segoe MLD2 Assets 字型的 EF20 圖示。
- 如果使用一個以上的 InkCanvas，建議您使用單一 InkToolbar 來控制所有畫布上的手寫筆跡。
- 為獲得最佳效能，建議您更改預設飛出視窗，而不是針對預設與自訂工具建立自訂飛出視窗。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [SimpleInk 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)：示範 InkCanvas 與 InkToolbar 控制項的自訂項目和擴充功能的 8 種案例。 每個案例都提供了常見手寫筆跡情況與控制項實作的基本指導方針。
- [ComplexInk 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)：示範更新進階手寫筆跡案例。
- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [UWP 應用程式中的畫筆和手寫筆互動](http://windowsstyleguide/input/pen-and-stylus-interactions/)
- [辨識筆墨筆劃](http://windowsstyleguide/input/convert-ink-to-text/)
- [儲存和擷取筆墨筆劃](http://windowsstyleguide/input/save-and-load-ink/)
