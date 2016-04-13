---
Description: 提供一份檢查清單，以協助確定您的通用 Windows 平台 (UWP) app 可提供無障礙功能。
title: 協助工具檢查清單
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
label: Checklist
template: detail.hbs
---

協助工具檢查清單
===================================================================================

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

提供一份檢查清單，以協助確定您的通用 Windows 平台 (UWP) app 可提供無障礙功能。

我們在此提供一份檢查清單，讓您用來確定 App 可提供無障礙功能。

1.  為 App 的內容和互動式 UI 元素設定無障礙名稱 (必要) 以及描述 (選用)。

    無障礙名稱是螢幕助讀程式用來宣告 UI 元素的簡單描述性文字字串。 某些像是 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 的 UI 元素會將自己的文字內容升級為預設的無障礙名稱；請參閱[基本的協助工具資訊](basic-accessibility-information.md#name_from_inner_text)。

    如果影像或其他控制項不會將內部文字內容升級為隱含的無障礙名稱，則您應該為它們設定明確的無障礙名稱。 您應該在表單元素使用標籤，這樣標籤文字就可以當作 Microsoft 使用者介面自動化模型的 [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 目標，負責將標籤和輸入建立關聯。 如果您想為使用者提供比典型無障礙名稱更多內容的 UI 指導方針，可以使用無障礙說明和工具提示來協助使用者了解 UI。

    如需詳細資訊，請參閱[無障礙名稱](basic-accessibility-information.md#accessible_name)和[無障礙說明](basic-accessibility-information.md)。

2.  實作鍵盤協助工具：


    -   Test the default tab index order for a UI. Adjust the tab index order if necessary, which may require enabling or disabling certain controls, or changing the default values of [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) on some of the UI elements.
    -   Use controls that support arrow-key navigation for composite elements. For default controls, the arrow-key navigation is typically already implemented.
    -   Use controls that support keyboard activation. For default controls, particularly those that support the UI Automation [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) pattern, keyboard activation is typically available; check the documentation for that control.
    -   Set access keys or implement accelerator keys for specific parts of the UI that support interaction.
    -   For any custom controls that you use in your UI, verify that you have implemented these controls with correct [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) support for activation, and defined overrides for key handling as needed to support activation, traversal and access or accelerator keys.

    For more info, see [Keyboard interactions](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  用肉眼檢查 UI，確定文字有適當的對比、元素可在高對比佈景主題中正確顯示以及使用正確的色彩。

    -   使用系統顯示選項來調整顯示器的 DPI 值，並確定在 DPI 值變更時，您的 app UI 能夠正確縮放。 (某些使用者會從協助工具選項中變更 DPI 值，您可以在 [輕鬆存取]**** 中使用這個選項)。
    -   使用色彩分析工具，確定視覺文字對比率至少是 4.5:1。
    -   切換到高對比佈景主題，確定應用程式的 UI 可清楚閱讀並可操作。
    -   確定 UI 不將顏色作為傳達資訊的唯一方式。

    如需詳細資訊，請參閱[高對比佈景主題](high-contrast-themes.md)和[協助工具文字的需求](accessible-text-requirements.md)。

4.  執行協助工具，解決報告的問題以及確定螢幕助讀正常運作。

    使用 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 這類的工具檢查程式存取，執行像是 [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) 的診斷工具來尋找常見錯誤，以及使用朗讀程式確定螢幕助讀正常運作。

    如需詳細資訊，請參閱[協助工具測試](accessibility-testing.md)。

5.  確定應用程式資訊清單的各項設定符合無障礙指導方針。

6.  在 Windows 市集中將您的 App 宣告為提供無障礙功能。

    如果您實作基本的協助工具支援，請在 Windows 市集中宣告您的 App 提供無障礙功能，這樣可以吸引更多用戶以及得到更多好評。

    如需詳細資訊，請參閱[市集中的協助工具](accessibility-in-the-store.md)。

<span id="related_topics"> </span>相關主題
-----------------------------------------------

* [協助工具](accessibility.md)
* [協助工具設計](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [應避免的做法](practices-to-avoid.md)
 

 





<!--HONumber=Mar16_HO3-->


