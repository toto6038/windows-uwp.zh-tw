---
author: QuinnRadich
Description: "這些指導方針描述如何為應用程式設計有效的說明內容。"
title: "應用程式說明的指導方針"
label: Guidelines for app help
template: detail.hbs
ms.sourcegitcommit: 0aa2db498ab7ec25839da259dd0026b0a7cd2b13
ms.openlocfilehash: f2522afa91abe26303a85cbfbabd5ec5b3dba59c

---

# App 說明的指導方針

\[ 針對 Windows 10 上的通用 Windows 平台 (UWP) app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

應用程式可能十分複雜，並提供有效的說明，讓使用者可以大幅改善其體驗。 並非所有的應用程式需要為其使用者提供說明，且根據應用程式以及人們使用的方式，所提供的說明類型變化很大。

如果您決定提供說明，請考慮此處所述的指導方針。 請記住，沒有什麼幫助的說明，比根本沒有說明還要糟。

## <span id="intuitive_design"></span><span id="INTUITIVE_DESIGN"></span>直覺式設計

就算說明內容十分有用，您的 app 也不能依賴它為使用者提供良好的體驗。 如果某人無法立即探索並使用您 app 的重要功能，表示該人員不會使用您的 app。 沒有任何說明內容可以變更該第一印象。

直覺且易於使用的設計是撰寫有用說明的第一個步驟。 這不僅可讓使用者持續使用以接觸更進階的功能，還可以讓他們了解 app 的核心功能，以做為基礎並繼續使用。

## <span id="general_instructions"></span><span id="GENERAL_INSTRUCTIONS"></span>一般指示

除非使用者已經遇到問題，否則不會尋找說明內容，因此說明需要為該問題提供快速且有效的回答。 如果說明不是立即有用，或者說明太過複雜，則使用者更有可能會忽略它。

所有說明 (不管是哪一種) 都應該遵循下列的基本原則︰

-   **容易了解：**混淆使用者的說明比根本沒有說明還要糟。

-   **易懂：**尋找說明的使用者希望直接向他們呈現清楚的解答。

-   **相關：**使用者不希望必須搜尋他們的特定問題。 他們希望先呈現最相關的說明 (這稱為「內容說明」)，或是想要可輕鬆瀏覽的介面。

-   **直接：**使用者尋求協助時，會想要查看說明。 如果您的 app 包含回報錯誤、提供意見反應、檢視服務條款或類似功能，它們應該包含為主要說明頁面上的連結或註腳，而不是重要項目。

-   **一致：**不管類型為何，說明仍然是您 app 的一部分，應與 UI 的任何其他部分一視同仁。 針對 app 其他部分的相同可用性、可存取性及樣式設計原則，也應該用在您提供的說明中。

## <span id="types_of_help"></span><span id="TYPES_OF_HELP"></span>說明類型

說明內容有三個主要類別，各有不同的優勢且適合不同的用途。 您可以視需求而定，在 app 中任意搭配這三個類別的內容。

### <span id="instructional_ui"></span><span id="INSTRUCTIONAL_UI"></span>指示性 UI

在理想的情況下，使用者應該不需要指示就能夠使用您 app 的所有核心功能，但您的 app 可能偶爾會根據特定的手勢使用，或者是您的 app 可能具有不甚明顯的第二個功能。 在這些情況下，指示性 UI 應該用來提供使用者有關如何執行特定工作的指示。

[請參閱指示性 UI 的指導方針](instructional-ui.md)

### <span id="in_app_help"></span><span id="IN_APP_HELP"></span>App 內說明

呈現說明的標準方法是依使用者要求在應用程式內顯示。 有數種可實作的方式 (例如透過說明頁面或資訊描述)。 這種方法十分適用於一般用途的說明，可直接回答使用者不太複雜的問題。

[請參閱 App 內說明的指導方針](in-app-help.md)

### <span id="external_help"></span><span id="EXTERNAL_HELP"></span>外部說明

針對因太大而無法放入應用程式內的詳細教學課程、進階功能，或說明主題庫，便應該使用能連至外部網頁的連結。 這些連結應該謹慎使用，因為它們會移除使用者的應用程式體驗。

[請參閱外部說明的指導方針。](external-help.md)

\[本文章包含通用 Windows 平台 (UWP) 應用程式與 Windows 10 專屬的資訊。 如需 Windows 8.1 指導方針，請下載 [Windows 8.1 指導方針 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]



<!--HONumber=Jun16_HO5-->


