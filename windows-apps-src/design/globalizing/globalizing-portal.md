---
author: stevewhims
Description: Learn about the benefits of globalizing and localizing your app, and exactly what these terms mean.
Search.SourceType: Video
title: 全球化和當地語系化
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: b437055704c307dc3e3fa506a9885ff892585503
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038957"
---
# <a name="globalization-and-localization"></a>全球化和當地語系化

全球不同語言、地區和文化的對象都使用 Windows。 您的使用者使用各種不同的語言，居住在各種不同的國家和地區。 有些使用者甚至會說一種以上的語言。 因此，您的應用程式是根據包含許多語言、地區及文化系統設定變更的設定執行的。 您可以藉由使用「全球化」** 和「當地語系化」** 來設計您的應用程式，使其能做出調整，提升應用程式的潛在市場。

這部影片提供如何為全球準備您的應用程式的簡介：[全球化與當地語系化的簡介](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization)。

**「全球化」** 是設計及開發您應用程式的一段過程，使其能在不同的全球市場 (在不同語言和文化設定的系統上) 正常運作，而無需針對文化進行特定變更或自訂。

- 在操縱字串時將文化納入考量，例如不要在比較他們之前變更字串的大小寫。
- 使用適用於目前文化的曆法。
- 使用全球化 API 顯示針對國家或地區適當格式化的資料，例如數字、日期、時間和貨幣。
- 考慮不同文化在整理 (排序) 文字和其他資料時有不同的規則。

您的程式碼必須在任何您應用程式決定支援的文化中都能正常運作。 理想情況下，您的程式碼會在「任何」** 語言、地區或文化內容中正常運作。 全球化您應用程式功能最有效率的方法，便是使用文化/地區設定概念。 文化/地區設定是一組規則和一組資料，專屬於特定語言和地理區域。 這些規則和資料包括下列資訊。

- 字元分類
- 書寫系統
- 日期和時間格式
- 數字，貨幣、重量和度量慣例
- 排序規則

**可當地語系化性**是為全球化應用程式準備當地語系化，和/或驗證該應用程式已準備好進行當地語系化的過程。 正確的使應用程式可當地語系化，表示之後的當地語系化過程不會在應用程式中發現任何功能缺失。 可當地語系化應用程式最重要的屬性，便是其可執行的程式碼和應用程式的可當地語系化資源完全分離。

- 翻譯成不同語言的字串長度變化可能會相當大。 因此，建議您將您的 UI 設計為可容納不同文字長度和文字大小的標籤和文字輸入控制項。
- 嘗試避免在影像中包含文字和/或文化敏感的材料。
- 不要在您的應用程式程式碼和標記中硬式編碼字串和文化相依影像。 相反的，請將他們儲存為字串和影像資源，使其可獨立於您應用程式的建置二進位檔之外，適應不同的本地市場。
- 虛擬當地語系化您的應用程式，以揭露任何可當地語系化性問題。

**當地語系化**是適應和翻譯您應用程式的可當地語系化資源，使其符合該應用程式意圖支援之特定本地市場的語言、文化和政治需求。 在您到達當地語系化應用程式的階段時，或您的應用程式可當地語系化，您便不需要在此過程中修改任何邏輯。

- 為新的市場翻譯應用程式的字串資源和其他資產。
- 視需要修改任何文化相依影像。
- 檔案也可能會根據使用者的地區而有所不同，而與其語言無關。 例如，地圖可能會根據使用者的位置而有不同的邊界，但標籤應遵循使用者的慣用語言。

大部分的當地語系化團隊都會使用特殊的工具，協助這段過程。 例如，回收重複出現文字的翻譯。

| 文章 | 描述 |
|---------|-------------|
| [全球化指導方針](guidelines-and-checklist-for-globalizing-your-app.md) | 設計及開發您的應用程式，使其能夠在不同語言和文化設定的系統上正常運作。 |
| [了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md) | 此主題會定義「使用者設定檔語言清單」、「應用程式資訊清單語言清單」及「應用程式執行階段語言清單」等三個術語。 我們會在本主題及位於此功能區域的其他主題中使用這些術語，因此請務必了解他們的意義。 |
| [全球化您的日期/時間/數字格式](use-global-ready-formats.md) | 藉由為日期、時間、數字、電話號碼及貨幣進行適當的格式設定，將您的應用程式設計為可全球通用。 您的應用程式在將來便可因應全球市場中的其他文化特性、地區及語言。 |
| [使用範本和模式來設定日期和時間的格式](use-patterns-to-format-dates-and-times.md) | 使用 [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 命名空間中的類別搭配自訂範本和模式，以您想要的格式來顯示日期和時間。 |
| [調整配置和字型並支援 RTL](adjust-layout-and-fonts--and-support-rtl.md) | 設計您的應用程式以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。 |
| [NumeralSystem 值](glob-numeralsystem-values.md) | 本主題列出 [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) 命名空間中各種類別可用的 **NumeralSystem** 屬性值。 |
| [讓您的應用程式可當地語系化](prepare-your-app-for-localization.md) | 當地語系化的應用程式是可為其他市場、語言或地區當地語系化，而不會使應用程式產生任何功能缺失的應用程式。 可當地語系化應用程式最重要的屬性，便是其可執行的程式碼和可當地語系化的資源完全分離。 |
| [國際字型](loc-international-fonts.md) | 本主題列出當地語系化為美國英文以外語言的 UWP app 可用的字型。 |
| [將您的應用程式設計為支援雙向文字](design-for-bidi-text.md) | 設計您的應用程式，使其提供雙向文字支援 (BiDi)，組合由左至右和由右至左書寫系統的文字。 |
| [使用多語應用程式工具組 4.0](use-mat.md) | 多語應用程式工具組 (MAT) 4.0 與 Microsoft Visual Studio 2017 整合，提供 UWP app 翻譯支援、翻譯檔案管理及編輯器工具。 |
| [多語應用程式工具組 4.0 常見問題集與疑難排解](mat-faq-troubleshooting.md) | 本主題提供與多語應用程式工具組 (MAT) 4.0 相關的常見問題解答。 |
