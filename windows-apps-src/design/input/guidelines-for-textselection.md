---
description: 本主題說明選取和操作文字、影像和控制項的新的 Windows UI，並提供在 Windows 應用程式中使用這些新的選取專案和操作機制時應考慮的使用者經驗指導方針。
title: 選取文字和影像
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: 鍵盤、文字、輸入、使用者互動
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 24ce46c367203a25a212df17c326c463e3ca9ac4
ms.sourcegitcommit: 4fffc66fac18fc4c80281e2a4afa9c4f2e1f7551
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94513707"
---
# <a name="selecting-text-and-images"></a>選取文字和影像


本文描述如何選取及操作文字、影像以及控制項，並提供在應用程式中使用這些機制時，所應考慮的使用者經驗指導方針。

> **重要 API** ： [**Windows.UI.Xaml.Input**](/uwp/api/Windows.UI.Xaml.Input)、 [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>可行與禁止事項


-   在實作自己的移駐夾 UI 時，使用字型字符。 移駐夾為兩個全系統可用的 Segoe UI 字型組合。 使用字型資源可簡化不同 dpi 的呈現問題，以及配合各種不同 UI 的縮放倍數使用。 實作自己的移駐夾時，這些移駐夾必須共有以下的 UI 特點：

    -   圓形
    -   在任何背景均可見
    -   大小一致
-   在可選取內容的周圍提供邊界，以容納移駐夾 UI。 如果您的應用程式允許在無法移動瀏覽/捲動的區域中選取文字，請在文字區域的左右邊留 1/2 的移駐夾邊界，並在文字區域的上下邊留 1 個移駐夾的高度 (如下列影像所示)。 這可確保讓使用者見到整個移駐夾 UI，並且將與其他以邊緣為基礎之 UI 的非預期互動減至最低。

    ![文字選取移駐夾邊界](images/textselection-gripper-margins.png)

-   進行互動時隱藏移駐夾 UI。 消除進行互動時移駐夾造成的閉塞。 這在移駐夾未完全被手指所掩蓋或有多個文字選取移駐夾時相當有用。 這樣可在顯示子項視窗時消除視覺誤差。

-   不要允許選取 UI 元素，例如控制項、標籤、影像、機密內容等等。 一般來說，Windows 應用程式只允許在特定控制項內選取內容。 按鈕、標籤以及標誌之類的控制項是不可選取的。 請評估選取對於您的應用程式是否會構成問題，如果是，請識別應該禁止選取的 UI 區域。 

## <a name="additional-usage-guidance"></a>其他用法指導方針


文字選取和操作特別容易受到觸控互動所帶來的使用者經驗挑戰影響。 滑鼠、畫筆/手寫筆以及鍵盤輸入都是極細微的：滑鼠點選或畫筆/手寫筆接觸一般都是對應單一像素，按鍵則不是按下就是未按下。 觸控輸入並不精細；很難將指尖的整個表面對應螢幕上特定的 x-y 位置來精確放置一個文字插入點。

**考量與建議**

使用 Windows 語言架構公開的內建控制項，建立提供完整平台使用者互動經驗 (包括選取和操作行為) 的應用程式。 您可以找到內建控制項的互動功能，足以滿足大部分的 Windows 應用程式。

使用標準的 Windows 文字控制項時，無法自訂此主題中所述的選取行為和視覺效果。

**文字選取**

如果應用程式需要可支援文字選取的自訂 UI，建議您依循此處描述的 Windows 選取行為。

**可編輯和不可編輯的內容**


使用觸控時，選取互動主要是透過手勢來執行的，例如點選來設定插入游標，或選取一個文字並滑動來修改選取範圍。 就像其他 Windows 觸控互動，計時互動也僅限於顯示資訊 UI 的長按手勢。 如需詳細資訊，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md)。

Windows 可以辨識兩種可能的選取互動狀態：可編輯和不可編輯；然後可以根據狀態來調整選取 UI、回饋以及功能。

**可編輯的內容**

若在文字的左半部內點選，游標的位置就會在文字的左邊；若在右半部內點選，游標的位置就會在文字的右邊。

下列影像示範如何透過點選文字的開頭或結尾附近，以使用移駐夾放置初始插入游標。

![點選 (或長按) 文字的左邊，來在該文字的開頭放置一個插入點和移駐夾。 點選 (或長按) 文字的右邊，來在該文字的結尾放置一個插入點和移駐夾。](images/textselection-place-caret.png)

下列影像示範如何拖曳移駐夾來調整選取範圍。

![朝其中一個方向拖曳移駐夾來調整選取範圍 (第一個移駐夾會保持定住，並且會顯示第二個移駐夾)。 拖曳其中一個移駐夾來進行後續的調整。](images/adjust-selection.png)

下列影像示範如何在選取範圍內或在移駐夾上進行點選 (或使用長按) 來叫用操作功能表。

![在選取範圍內或在移駐夾上進行點選 (或長按)，以叫用操作功能表。](images/textselection-show-context.png)

**備註** 針對文字拼錯的情況，這些互動會表現得有些不同。 點選標示為拼錯的文字會將整個文字反白，並叫用建議拼法操作功能表。

 

**不可編輯的內容**

下列影像示範如何在文字內點選以選取文字 (初始選取範圍中不包含任何空格)。

![在文字內點選以選取文字 (初始選取範圍中不包含任何空格)。](images/select-word.png)

對於可編輯的文字，請遵循相同的程序來調整選取範圍及顯示操作功能表。

**物件操作**

在可能的情況下，在 Windows 應用程式中執行自訂物件操作時，請使用相同的 (或類似的) 控制項資源作為文字選取範圍。 這樣可讓平台的互動體驗保持一致。

例如，您也可以在支援大小調整和裁剪的影像處理應用程式，或提供可調整進度列的媒體播放程式應用程式中使用移駐夾，如下列影像中所示。

![具備進度移駐夾的媒體播放程式](images/gripper-mediaplayer.png)

*具備可調整進度列的媒體播放程式。*

![具備裁剪移駐夾的影像](images/gripper-imagemanip.png)

*具備裁剪移駐夾的影像編輯器。*

## <a name="related-articles"></a>相關文章

### <a name="for-developers"></a>開發人員

- [自訂使用者互動](../layout/index.md)

### <a name="samples"></a>範例

- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

### <a name="archive-samples"></a>封存範例

- [輸入：XAML 使用者輸入事件範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [輸入：裝置功能範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [輸入：觸控點擊測試範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 滾動、移動流覽和縮放範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [輸入：簡化的筆跡範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)

- [輸入：操作和手勢範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX 觸控輸入範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
