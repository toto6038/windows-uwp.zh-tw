---
Description: 提供檢查清單以協助您確定可以存取您的 Windows 應用程式。
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 協助工具檢查清單
label: Accessibility checklist
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6b9340f55064ff89c0b047cbb6d7407574da3d6
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216391"
---
# <a name="accessibility-checklist"></a>協助工具檢查清單

提供檢查清單以協助您確定可以存取您的 Windows 應用程式。

我們在此提供一份檢查清單，讓您用來確定 App 可提供無障礙功能。

1. 為 App 的內容和互動式 UI 元素設定無障礙名稱 (必要) 以及描述 (選用)。

    無障礙名稱是螢幕助讀程式用來宣告 UI 元素的簡單描述性文字字串。 某些像是 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 和 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 的 UI 元素會將自己的文字內容升級為預設的無障礙名稱；請參閱[基本的協助工具資訊](basic-accessibility-information.md#name_from_inner_text)。

    如果影像或其他控制項不會將內部文字內容升級為隱含的無障礙名稱，則您應該為它們設定明確的無障礙名稱。 您應該在表單元素使用標籤，這樣標籤文字就可以當作 Microsoft 使用者介面自動化模型的 [**LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 目標，負責將標籤和輸入建立關聯。 如果您想為使用者提供比典型無障礙名稱更多內容的 UI 指導方針，可以使用無障礙說明和工具提示來協助使用者了解 UI。

    如需詳細資訊，請參閱[無障礙名稱](basic-accessibility-information.md#accessible_name)和[無障礙說明](basic-accessibility-information.md)。

2. 實作鍵盤協助工具：

    * 測試 UI 的預設標籤索引順序。 需要時調整標籤索引順序，這樣做可能需要啟用或停用某些控制項，或者您可以變更部分 UI 元素的 [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 預設值。
    * 使用支援複合元素方向鍵瀏覽的控制項。 預設的控制項通常已經實作方向鍵瀏覽。
    * 使用支援鍵盤啟動的控制項。 預設的控制項 (特別是支援使用者介面自動化 [**Invoke**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) 模式的控制項) 通常可以使用鍵盤啟動功能；請參閱該控制項的說明文件。
    * 為支援互動的 UI 特定組件，設定便捷鍵或實作快速鍵。
    * 對於 UI 中使用的任何自訂控制項，確定您已針對啟動功能使用正確的 [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 支援來實作這些控制項，並定義支援啟動、周遊、便捷鍵或快速鍵所需的按鍵處理覆寫項。

    如需詳細資訊，請參閱[鍵盤互動](../input/keyboard-interactions.md)。

3. 確定文字是可讀取的大小

    * Windows 包含各種協助工具工具和設定，使用者可利用這些工具和設定來調整，並根據自己的需求和喜好設定來閱讀文字。 其中包括：
        * 放大鏡工具，可放大 UI 的選取區域。 您應該確保應用程式中的文字版面配置不會讓您難以使用放大鏡進行讀取。
        * 設定中的全域規模和解析度設定 **->系統 >顯示 >縮放和**配置。 確切的調整大小選項有何不同，這取決於顯示裝置的功能。
        * 設定中的文字大小設定 **->輕鬆存取->顯示**。 調整 [ **讓文字變得更大** ] 設定，只指定在所有應用程式和螢幕上支援控制項的文字大小 (所有 UWP 文字控制項都能支援不含任何自訂或範本) 的文字縮放體驗。
        > [!NOTE]
        > [ **讓所有專案變得更大** ] 設定可讓使用者在主要畫面上，通常只會指定其慣用的文字和應用程式大小。

4. 用肉眼檢查 UI，確定文字有適當的對比、元素可在高對比佈景主題中正確顯示以及使用正確的色彩。

    * 使用色彩分析工具，確定視覺文字對比率至少是 4.5:1。
    * 切換到高對比佈景主題，確定應用程式的 UI 可清楚閱讀並可操作。
    * 確定 UI 不將顏色作為傳達資訊的唯一方式。

    如需詳細資訊，請參閱[高對比佈景主題](high-contrast-themes.md)和[協助工具文字的需求](accessible-text-requirements.md)。

5. 執行協助工具，解決報告的問題以及確定螢幕助讀正常運作。

    使用 [**Inspect**](/windows/desktop/WinAuto/inspect-objects) 這類的工具檢查程式存取，執行像是 [**AccChecker**](/windows/desktop/WinAuto/ui-accessibility-checker) 的診斷工具來尋找常見錯誤，以及使用朗讀程式確定螢幕助讀正常運作。

    如需詳細資訊，請參閱[協助工具測試](accessibility-testing.md)。

6. 確定應用程式資訊清單的各項設定符合協助工具指導方針。

7. 在 Microsoft Store 中將您的 App 宣告為提供無障礙功能。

    如果您實作基本的協助工具支援，請在 Microsoft Store 中宣告您的 App 提供無障礙功能，這樣可以吸引更多用戶以及得到更多好評。

    如需詳細資訊，請參閱[市集中的協助工具](accessibility-in-the-store.md)。

## <a name="related-topics"></a>相關主題  

* [協助工具文字的需求](accessible-text-requirements.md)
* [文字大小調整](../input/text-scaling.md)
* [協助工具](accessibility.md)
* [協助工具設計](./accessibility-overview.md)
* [應避免的做法](practices-to-avoid.md)
