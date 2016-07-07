---
author: Jwmsft
Description: "輸入並編輯文字時，拼字檢查會以紅色波浪線醒目顯示某個單字，告知使用者這個字拼錯了，並為使用者提供修正拼字錯誤的方式。"
title: "拼字檢查和文字預測"
ms.assetid: B867C956-5AB2-4207-A8DE-179CE7871180
label: Spell checking and text prediction
template: detail.hbs
redirect_url: text-controls
ms.sourcegitcommit: 08f982f5c54d596a7624fe63528f91375e7761ca
ms.openlocfilehash: 8954b14a84241686a7c784c3d250f197bed7c8b6

---

# 拼字檢查的指導方針

輸入並編輯文字時，拼字檢查會以紅色波浪線醒目顯示某個單字，告知使用者這個字拼錯了，並為使用者提供修正拼字錯誤的方式。

**重要 API**

-   [**IsSpellCheckEnabled 屬性 (XAML)**](https://msdn.microsoft.com/library/windows/apps/br209688)


## <span id="checklist_section"></span><span id="CHECKLIST_SECTION"></span>建議事項


-   當使用者在文字輸入控制項中輸入單字或句子時，使用拼字檢查協助使用者。 拼字檢查可與觸控、滑鼠及鍵盤輸入搭配使用。
-   當單字不太可能出現在字典中或使用者不重視拼字檢查時，不要使用拼字檢查。 例如，對於密碼、電話號碼或姓名的輸入方塊，不要啟用拼字檢查。 這些控制項的拼字檢查預設為停用狀態。
-   不要只是因為目前的拼字檢查引擎不支援您的應用程式語言，就停用拼字檢查。 當拼字檢查工具不支援語言時，它不會執行任何動作，所以保留啟用該選項並不會造成損害。 另外，有些使用者可能會使用輸入法 (IME) 將另一種語言輸入您的應用程式中，可能就會支援該語言。 例如，建置日文應用程式時，雖然拼字檢查引擎目前無法辨識該語言，但是不要將拼字檢查關閉。 使用者可能會切換到英文 IME 並且在應用程式中輸入英文；如果啟用拼字檢查，就會進行英文拼字檢查。

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>其他用法指導方針


Windows 市集應用程式為多行和單行文字輸入方塊，以及 **contentEditable** 屬性設為 **true** 的元素，提供內建的拼字檢查工具。 這裡是內建拼字檢查工具的範例：

![內建拼字檢查工具](images/spellchecking.png)

如需詳細資訊，請參閱 [**TextBox 類別**](https://msdn.microsoft.com/library/windows/apps/br209683)。

使用拼字檢查搭配文字輸入控制項有兩個目的：

-   **自動校正拼字錯誤**

    如果確定能夠修正的話，拼字檢查引擎會自動校正拼字錯誤的字。 例如，引擎會自動將 "teh" 變更成 "the"。

-   **顯示替代拼法**

    拼字檢查引擎不確定修正是否正確時，會在拼字錯誤的字底下加上紅色底線，並且在您點選或以滑鼠右鍵按一下該字時顯示替代字。

對於 JavaScript 控制項，多行文字輸入控制項的拼字檢查預設為開啟狀態，單行控制項的拼字檢查則預設為關閉狀態。 您只要將控制項的 **spellcheck** 屬性設為 **true**，就可以手動開啟單行控制項的拼字檢查。 將 **spellcheck** 屬性設為 **false** 可以停用控制項的拼字檢查。

XAML TextBox 控制項的拼字檢查預設為關閉狀態。 您可以將 **IsSpellCheckEnabled** 屬性設為 **true** 來啟用它。



## <span id="related_topics"></span>相關文章

* [文字和文字控制項](text-controls.md)
* [文字輸入的指導方針](https://msdn.microsoft.com/library/windows/apps/hh750315)
* [文字與印刷樣式的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700394) 
           **適用於開發人員 (XAML)**
* [**TextBox.IsSpellCheckEnabled 屬性**](https://msdn.microsoft.com/library/windows/apps/br209688)
* [**TextBox 類別**](https://msdn.microsoft.com/library/windows/apps/br209683)

 







<!--HONumber=Jun16_HO5-->


