---
author: Xansky
Description: 提供一份檢查清單，以協助確定您的通用 Windows 平台 (UWP) app 可提供無障礙功能。
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: 協助工具檢查清單
label: Accessibility checklist
template: detail.hbs
---

# 協助工具檢查清單



提供一份檢查清單，以協助確定您的通用 Windows 平台 (UWP) App 可提供無障礙功能。

我們在此提供一份檢查清單，讓您用來確定 App 可提供無障礙功能。

1.  為 App 的內容和互動式 UI 元素設定無障礙名稱 (必要) 以及描述 (選用)。

    無障礙名稱是螢幕助讀程式用來宣告 UI 元素的簡單描述性文字字串。 某些像是 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 的 UI 元素會將自己的文字內容升級為預設的無障礙名稱；請參閱[基本的協助工具資訊](basic-accessibility-information.md#name_from_inner_text)。

    如果影像或其他控制項不會將內部文字內容升級為隱含的無障礙名稱，則您應該為它們設定明確的無障礙名稱。 您應該在表單元素使用標籤，這樣標籤文字就可以當作 Microsoft 使用者介面自動化模型的 [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 目標，負責將標籤和輸入建立關聯。 如果您想為使用者提供比典型無障礙名稱更多內容的 UI 指引，可以使用無障礙描述和工具提示來協助使用者了解 UI。

    如需詳細資訊，請參閱[無障礙名稱](basic-accessibility-information.md#accessible_name)和[無障礙描述](basic-accessibility-information.md)。

2.  實作鍵盤協助工具：

    * 測試 UI 的預設標籤索引順序。 需要時調整標籤索引順序，這樣做可能需要啟用或停用某些控制項，或者您可以變更部分 UI 元素的 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 預設值。
    * 使用支援複合元素方向鍵瀏覽的控制項。 預設的控制項通常已經實作方向鍵瀏覽。
    * 使用支援鍵盤啟動的控制項。 預設的控制項 (特別是支援使用者介面自動化 [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) 模式的控制項) 通常可以使用鍵盤啟動功能；請參閱該控制項的說明文件。
    * 為支援互動的 UI 特定組件，設定便捷鍵或實作快速鍵。
    * 對於 UI 中使用的任何自訂控制項，確定您已針對啟動功能使用正確的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 支援來實作這些控制項，並定義支援啟動、周遊、便捷鍵或快速鍵所需的按鍵處理覆寫項。

    如需詳細資訊，請參閱[鍵盤互動](https://msdn.microsoft.com/library/windows/apps/Mt185607)。

3.  用肉眼檢查 UI，確定文字有適當的對比、元素可在高對比佈景主題中正確顯示以及使用正確的色彩。

    * 使用系統顯示選項來調整顯示器的 DPI 值，並確定在 DPI 值變更時，app UI 能夠正確縮放。 (某些使用者會變更 DPI 值來做為其無障礙輔助，可以在 [輕鬆存取]**** 中變更該選項)。
    * 使用色彩分析工具，確定視覺文字對比率至少是 4.5:1。
    * 切換到高對比佈景主題，確定應用程式的 UI 可清楚閱讀並可操作。
    * 確定 UI 不將顏色作為傳達資訊的唯一方式。

    如需詳細資訊，請參閱[高對比佈景主題](high-contrast-themes.md)和[協助工具文字的需求](accessible-text-requirements.md)。

4.  執行協助工具，解決報告的問題以及確定螢幕助讀正常運作。

    使用 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 這類的工具檢查程式存取，執行像是 [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) 的診斷工具來尋找常見錯誤，以及使用朗讀程式確定螢幕助讀正常運作。

    如需詳細資訊，請參閱[協助工具測試](accessibility-testing.md)。

5.  確定應用程式資訊清單的各項設定符合無障礙指導方針。

6.  在 Windows 市集中將您的 App 宣告為提供無障礙功能。

    如果您實作基本的協助工具支援，請在 Windows 市集中宣告您的 app 提供無障礙功能，這樣可以吸引更多用戶以及得到更多好評。

    如需詳細資訊，請參閱[市集中的協助工具](accessibility-in-the-store.md)。

<span id="related_topics"/>
## 相關主題  
* [協助工具](accessibility.md)
* [協助工具設計](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [應避免的做法](practices-to-avoid.md)


<!--HONumber=May16_HO2-->


