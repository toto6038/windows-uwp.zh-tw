---
Description: 提供一份檢查清單，以協助確定您的通用 Windows 平台 (UWP) app 可提供無障礙功能。
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 協助工具檢查清單
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9b789102b90e63e5456c030f6eb90df372354d7a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319004"
---
# <a name="accessibility-checklist"></a>協助工具檢查清單

提供一份檢查清單，以協助確定您的通用 Windows 平台 (UWP) App 可提供無障礙功能。

我們在此提供一份檢查清單，讓您用來確定 App 可提供無障礙功能。

1. 為 App 的內容和互動式 UI 元素設定無障礙名稱 (必要) 以及描述 (選用)。

    無障礙名稱是螢幕助讀程式用來宣告 UI 元素的簡單描述性文字字串。 某些像是 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 和 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 的 UI 元素會將自己的文字內容升級為預設的無障礙名稱；請參閱[基本的協助工具資訊](basic-accessibility-information.md#name_from_inner_text)。

    如果影像或其他控制項不會將內部文字內容升級為隱含的無障礙名稱，則您應該為它們設定明確的無障礙名稱。 您應該在表單元素使用標籤，這樣標籤文字就可以當作 Microsoft 使用者介面自動化模型的 [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 目標，負責將標籤和輸入建立關聯。 如果您想為使用者提供比典型無障礙名稱更多內容的 UI 指導方針，可以使用無障礙說明和工具提示來協助使用者了解 UI。

    如需詳細資訊，請參閱[無障礙名稱](basic-accessibility-information.md#accessible_name)和[無障礙說明](basic-accessibility-information.md)。

2. 實作鍵盤協助工具：

    * 測試 UI 的預設標籤索引順序。 需要時調整標籤索引順序，這樣做可能需要啟用或停用某些控制項，或者您可以變更部分 UI 元素的 [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) 預設值。
    * 使用支援複合元素方向鍵瀏覽的控制項。 預設的控制項通常已經實作方向鍵瀏覽。
    * 使用支援鍵盤啟動的控制項。 預設的控制項 (特別是支援使用者介面自動化 [**Invoke**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) 模式的控制項) 通常可以使用鍵盤啟動功能；請參閱該控制項的說明文件。
    * 為支援互動的 UI 特定組件，設定便捷鍵或實作快速鍵。
    * 對於 UI 中使用的任何自訂控制項，確定您已針對啟動功能使用正確的 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 支援來實作這些控制項，並定義支援啟動、周遊、便捷鍵或快速鍵所需的按鍵處理覆寫項。

    如需詳細資訊，請參閱[鍵盤互動](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)。

3. 請確定文字是可讀取的大小

    * Windows 包含各種協助工具和設定，使用者可以利用和調整，以自己的需求和喜好設定，用於讀取文字。 它們包括：
        * [放大鏡] 工具，可以放大 UI 中的所選取的區域。 您應該確定在您的應用程式中的文字版面配置不會造成難以使用 [放大鏡] 進行讀取。
        * 中的全域規模和解析度設定**設定]-> [系統]-> [顯示比例和版面配置]-> [** 。 確實有哪些調整大小選項可能會不同，因為這取決於顯示裝置的功能。
        * 中的文字大小設定**設定]-> [輕鬆存取]-> [顯示**。 調整**使文字變大**設定來指定文字的大小在支援跨所有應用程式和畫面 （所有 UWP 文字控制項都支援規模調整體驗，而不需要任何自訂或樣板化的文字） 的控制項。
        > [!NOTE]
        > **更大的每個項目**設定可讓使用者，只有其主畫面上的一般情況下指定慣用的文字和應用程式的大小。

4. 用肉眼檢查 UI，確定文字有適當的對比、元素可在高對比佈景主題中正確顯示以及使用正確的色彩。

    * 使用色彩分析工具，確定視覺文字對比率至少是 4.5:1。
    * 切換到高對比佈景主題，確定應用程式的 UI 可清楚閱讀並可操作。
    * 確定 UI 不將顏色作為傳達資訊的唯一方式。

    如需詳細資訊，請參閱[高對比佈景主題](high-contrast-themes.md)和[協助工具文字的需求](accessible-text-requirements.md)。

5. 執行協助工具，解決報告的問題以及確定螢幕助讀正常運作。

    使用 [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) 這類的工具檢查程式存取，執行像是 [**AccChecker**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker) 的診斷工具來尋找常見錯誤，以及使用朗讀程式確定螢幕助讀正常運作。

    如需詳細資訊，請參閱[協助工具測試](accessibility-testing.md)。

6. 確定應用程式資訊清單的各項設定符合無障礙指導方針。

7. 在 Microsoft Store 中將您的 App 宣告為提供無障礙功能。

    如果您實作基本的協助工具支援，請在 Microsoft Store 中宣告您的 App 提供無障礙功能，這樣可以吸引更多用戶以及得到更多好評。

    如需詳細資訊，請參閱[市集中的協助工具](accessibility-in-the-store.md)。

## <a name="related-topics"></a>相關主題  

* [協助工具文字的需求](accessible-text-requirements.md)
* [文字大小調整](../input/text-scaling.md)
* [協助工具](accessibility.md)
* [協助工具的設計](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-overview)
* [應避免的做法](practices-to-avoid.md)
