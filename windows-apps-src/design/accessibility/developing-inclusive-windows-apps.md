---
author: Xansky
Description: Learn to develop accessible Windows 10 UWP apps that include keyboard navigation, color and contrast settings, and support for assistive technologies.
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: 開發全人 Windows 10 應用程式
label: Developing inclusive Windows 10 apps
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2adb3c67c4c7c1d024cd969af15cc12baa424511
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "6446969"
---
# <a name="developing-inclusive-windows-apps"></a>開發全人 Windows 應用程式  

本文討論如何開發無障礙的通用 Windows 平台 (UWP) App。 具體而言，本文章將假設您已了解設計 App 邏輯階層的方法。 瞭解如何開發無障礙的 Windows 10 UWP app，包括鍵盤瀏覽、色彩和對比設定以及輔助技術支援。

如果您還沒了解這部分的內容，請先閱讀[設計通用軟體](designing-inclusive-software.md)。

若要確保您 App 為無障礙，您必須執行三件事項：

1. 針對[程式設計存取](#programmatic-access)公開您的 UI 元素。
2. 確保您的 app 支援[鍵盤瀏覽](#keyboard-navigation)，以供無法使用滑鼠或觸控螢幕的人使用。
3. 確定您的 app 支援無障礙的[色彩和對比](#color-and-contrast)設定。

## <a name="programmatic-access"></a>程式設計存取  
程式設計存取對於在 App 中建立協助工具是非常重要的。 這是透過為 App 的內容和互動式 UI 元素設定無障礙名稱 (必要) 及描述 (選用) 來達成。 這將能確保 UI 控制項皆已公開給螢幕助讀程式 (例如朗讀器) 等的輔助技術 (AT)，或是替代輸出裝置 (例如點字顯示)。 在沒有程式設計存取的情況下，輔助技術的 API 將無法正確解譯資訊，讓使用者無法充分使用產品，或迫使 AT 使用並非用來做為協助工具介面使用的未記載程式設計介面或技術。 當 UI 控制項針對輔助技術公開時，AT 將能夠判斷可供使用者使用的動作和選項。  

如需使 app 的 UI 元素可供輔助技術 (AT) 使用的詳細資訊，請參閱[公開基本的協助工具資訊](basic-accessibility-information.md)。

## <a name="keyboard-navigation"></a>鍵盤瀏覽  
讓視障或行動不便的使用者能夠透過鍵盤瀏覽 UI，是一件非常重要的事。 不過，只有需要使用者互動以運作的 UI 控制項才需要鍵盤焦點。 不需要動作的元件 (例如靜態影像) 並不需要鍵盤焦點。  

請務必記得，鍵盤瀏覽和使用滑鼠或觸控瀏覽的不同之處，在於鍵盤瀏覽是線性的。 考慮鍵盤瀏覽時，請思考您的使用者與產品互動的方式，以及什麼樣的瀏覽才是合乎邏輯的方式。 在西方文化中，人們的閱讀方式是從左到右，從上到下。 因此，常見的鍵盤瀏覽做法便是跟隨這個模式。  

設計鍵盤瀏覽時，請檢查您的 UI，並思考下列問題：
* 控制項是如何在 UI 中進行配置或分組的？
* 是否有幾個較為重要的控制項群組？
    * 如果是，那些群組是否包含另一個階層的群組？
*   在對等的控制項中，瀏覽是否應該透過使用 Tab 鍵進行、透過特殊的瀏覽 (例如方向鍵) 進行，還是兩者皆是？

您的目標，是協助使用者了解 UI 配置的方式，並識別可執行動作的控制項。 如果您發現使用者在完成瀏覽迴圈之前，所需經歷的定位停駐點太多，請考慮將相關的控制項結合為相同的群組。 某些相關的控制項 (例如混合式控制項)，可能需要在初期的探索階段便進行處理。 當您開始開發產品時，重新處理鍵盤瀏覽將會變得困難，因此請務必仔細並提早計畫！  

如需深入了解使用鍵盤瀏覽 UI 元素的方式，請參閱[鍵盤協助工具](keyboard-accessibility.md)。  

此外，[針對協助工具的軟體工程設計](https://www.microsoft.com/download/details.aspx?id=19262)電子書，針對這個主題有一篇名叫_設計邏輯階層_的絕佳章節。

## <a name="color-and-contrast"></a>色彩和對比  
Windows 的其中一個內建協助工具功能便是高對比模式，此模式能增加螢幕上文字和影像的色彩對比。 對於一些人來說，提升色彩對比能降低眼睛的疲勞度，並使畫面更容易閱讀。 當您在高對比中驗證 UI 時，應該要檢查控制項的程式碼是否有一致，且依照系統色彩 (而非硬式編碼色彩) 進行撰寫，以確保使用者能和未使用高對比之使用者看到一樣的控制項。  

XAML
```xaml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
如需使用系統色彩和資源的詳細資訊，請參閱 [XAML 佈景主題資源](../controls-and-patterns/xaml-theme-resources.md)。

只要您沒有覆寫系統色彩，UWP App 預設可支援高對比佈景主題。 如果使用者已選擇要讓系統使用來自系統設定或協助工具的高對比佈景主題，架構就會自動針對 UI 中的控制項和元件，使用產生高對比配置和呈現方式的色彩和樣式設定。   

如需詳細資訊，請參閱[高對比佈景主題](high-contrast-themes.md)。  

如果您已決定使用自己的色彩，而非系統色彩，請考慮下列指導方針：  

**色彩對比率** - 已更新的美國身心障礙者法第 508 項及其他法律，要求文字和其背景之間的預設色彩對比必須為 5:1。 針對較大的文字 (18 點字型大小，或 14 點及粗體)，預設對比必須為 3:1。  

**色彩組合** - 大約有 7% 的男性 (以及少於 1 % 的女性) 患有某種形式的色盲。 患有色盲的使用者將無法區分部分顏色，因此請務必不要只使用色彩來傳達應用程式中的狀態或意義。 針對裝飾性的影像 (例如圖示或背景)，色彩組合應該要以色盲使用者能夠最大限度地認知該影像為前提做出選擇。  

## <a name="accessibility-checklist"></a>協助工具檢查清單  
下列為協助工具檢查清單的簡短版本：

1. 為 App 的內容和互動式 UI 元素設定無障礙名稱 (必要) 以及描述 (選用)。
2. 實作鍵盤協助工具。
3. 用肉眼檢查 UI，確定文字有適當的對比、元素可在高對比佈景主題中正確顯示以及使用正確的色彩。
4. 執行協助工具，解決報告的問題以及確定螢幕助讀正常運作。 (請參閱＜協助工具測試＞主題)。
5. 確定應用程式資訊清單的各項設定符合協助工具指導方針。
6. 在 Microsoft Store 中將您的 App 宣告為提供無障礙功能。 (請參閱 [Store 中的協助工具](accessibility-in-the-store.md)主題)。

如需詳細資料，請參閱完整的[協助工具檢查清單](accessibility-checklist.md)主題。

## <a name="related-topics"></a>相關主題  
* [設計通用軟體](designing-inclusive-software.md)  
* [通用設計](http://design.microsoft.com/inclusive)
* [協助工具應避免的做法](practices-to-avoid.md)
* [針對協助工具的軟體工程設計](https://www.microsoft.com/download/details.aspx?id=19262)
* [Microsoft 協助工具開發人員中樞](https://msdn.microsoft.com/enable)
* [協助工具](accessibility.md)
