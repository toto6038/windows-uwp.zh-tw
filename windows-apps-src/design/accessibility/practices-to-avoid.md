---
author: Xansky
Description: Lists the practices to avoid if you want to create an accessible Universal Windows Platform (UWP) app.
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: 協助工具應避免的做法
label: Accessibility practices to avoid
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9505dfc666042c22e6f77ed02ffca7c5973d4fba
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6196614"
---
# <a name="accessibility-practices-to-avoid"></a>協助工具應避免的做法

如果您想要建立無障礙的通用 Windows 平台 (UWP) App，請查看這份清單以了解應避免的做法： 

* **避免建立自訂的 UI 元素，盡可能使用預設的 Windows 控制項**或已經實作 Microsoft UI 自動化支援的控制項。 根據預設，標準的 Windows 控制項都具備無障礙功能，通常只需要新增一些 App 特定的協助工具屬性即可。 相反地，為真正的自訂控制項實作 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 支援時，涉及的因素更多 (請參閱[自訂自動化對等](custom-automation-peers.md))。
* **請勿將靜態文字或其他非互動元素放入 Tab 順序中** (例如，透過為非互動式的元素設定 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 屬性)。 如果將非互動式元素設定在 Tab 順序中，會違反鍵盤協助工具指導方針，因為這樣做會降低使用者的鍵盤瀏覽效率。 許多輔助技術針對如何顯示輔助技術使用者的應用程式介面使用 Tab 順序，並且著重其元素的邏輯能力。 Tab 順序中只有文字的元素，會讓只預期和 Tab 順序中的元素 (按鈕、核取方塊、文字輸入欄位、下拉式方塊、清單等) 互動的使用者感到混淆。
* **避免使用 UI 元素的絕對位置** (例如，在 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267) 元素中)，因為顯示順序通常與子元素宣告順序不同 (後者為實際上的邏輯順序)。 可以的話，按照文件順序或邏輯順序編排 UI 元素，確保螢幕助讀程式可以按照正確的順序閱讀這些元素。 如果 UI 元素的顯示順序不同於文件順序或邏輯順序，請使用明確的 Tab 索引值 (設定 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)) 來定義正確的閱讀順序。
* **不要將顏色當作唯一的資訊傳達方式。** 有色盲問題的使用者無法接收純粹透過顏色傳達的資訊，例如彩色的狀態指標。 包含其他的視覺提示，最好是文字，以確保提供無障礙的資訊。
* **不要自動重新整理整個應用程式畫布**，除非應用程式功能有此需要。 如果您需要自動重新整理頁面內容，請只更新頁面的特定區域。 輔助技術通常必須假設重新整理的應用程式畫布是全新的結構，即使有效的變更非常少。 對於輔助技術使用者而言，這樣做的成本是只要重新整理應用程式，就必須重新建立任何文件檢視或描述，然後再次向使用者呈現。
  
  使用者起始的明確頁面瀏覽，是重新整理應用程式結構的合理情況。 但是，請確定起始瀏覽的 UI 項目已經正確識別或者命名，以提供關於叫用項目將產生內容變更以及頁面重新載入的指示。

  > [!NOTE]
  > 如果您重新整理區域中的內容，請考量將該元素上的 [**AccessibilityProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516) 協助工具屬性設定為非預設設定 **Polite** 或 **Assertive** 中的其中一種。 部分輔助技術會將此設定對應到即時區域的無障礙豐富網際網路應用程式 (ARIA) 概念，因此可通知使用者該內容區域已經變更。

* **不要使用每秒閃爍 3 次以上的 UI 元素。** 閃爍的元素會造成某些人癲癇發作。 最好避免使用閃爍的 UI 元素。
* **勿自動變更使用者的上下文或自動啟動功能。** 應該只在使用者在焦點所在的 UI 元素上採取直接的動作之後，才變更上下文或啟動某項功能。 變更使用者上下文包括變更焦點、顯示新內容以及瀏覽其他頁面。 沒有使用者的介入而變更上下文，會造成殘障者的混亂。 此需求的例外情況包括顯示子功能表、查驗表單正確性、在其他控制項顯示說明內容，以及回應非同步事件。

<span id="related_topics"/>

## <a name="related-topics"></a>相關主題  
* [協助工具](accessibility.md)
* [市集中的協助工具](accessibility-in-the-store.md)
* [協助工具檢查清單](accessibility-checklist.md)
