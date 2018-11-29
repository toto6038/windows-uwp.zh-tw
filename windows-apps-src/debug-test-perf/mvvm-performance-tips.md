---
ms.assetid: 159681E4-BF9E-4A57-9FEE-EC7ED0BEFFAD
title: MVVM 和語言效能祕訣
description: 本主題討論與您的軟體設計模式及程式設計語言選擇相關的一些效能考量。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9027362eccfb8130b181bee26a57f13ce1e1af66
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7972668"
---
# <a name="mvvm-and-language-performance-tips"></a>MVVM 和語言效能祕訣


本主題討論與您的軟體設計模式及程式設計語言選擇相關的一些效能考量。

## <a name="the-model-view-viewmodel-mvvm-pattern"></a>Model-View-ViewModel (MVVM) 模式

Model-View-ViewModel (MVVM) 模式在許多 XAML 應用程式中相當常見。 (MVVM 與 Fowler 的 Model-View-Presenter 模式描述非常相似，但是它是專為 XAML 量身打造)。 MVVM 模式的問題在於它可能會不小心導致產生有太多層和太多配置的應用程式。 以下是使用 MVVM 的動機。

-   **區分不同的考量**。 將問題分割成較小片段一律都是有幫助的，而像 MVVM 或 MVC 之類的模式即是一個將應用程式 (或甚至是單一控制項) 分割成下列較小片段的方式：實際檢視、檢視的邏輯模型 (檢視模型)，以及與檢視無關的應用程式邏輯 (模型)。 尤其，讓擁有檢視的設計人員使用一種工具，讓擁有模型的開發人員使用另一種工具，以及讓擁有檢視模型的設計整合人員同時使用這兩種工具，是相當常見的工作流程。
-   **單元測試**。 您可以在不考量檢視的情況下，對檢視模型進行單元測試 (進而測試模型)，因此不需倚賴建立視窗、促成輸入等等。 藉由維持小型檢視，您完全不需建立視窗，即可測試應用程式的大部分。
-   **靈活因應使用者體驗的改變**。 檢視一般傾向於顯示最頻繁的變更和最新的變更，因為使用者體驗是根據使用者意見反應塑造的。 藉由個別維持檢視，便可更快因應變更，並且對應用程式造成較少的影響。

MVVM 模式有多個具體的定義，以及協助實作此模式的協力廠商架構。 但是嚴格遵守此模式的任何變化，可能會導致應用程式產生遠遠超出所能合理處理的額外負荷。

-   XAML 資料繫結 ({Binding} 標記延伸) 的部分設計目的是為了啟用模型/檢視模式。 但是 {Binding} 帶來不小的工作集和 CPU 額外負荷。 建立 {Binding} 會造成一系列的配置，而更新繫結目標則可能造成反射和 Boxing。 這些問題目前正藉由 {x:bind} 標記延伸來解決，它會在建置階段編譯繫結。 **建議：** 使用 {x:Bind}。
-   在 MVVM 中，使用 ICommand (例如常見的 DelegateCommand 或 RelayCommand 協助程式) 將 Button.Click 連接到檢視模型是常見的做法。 這些命令是額外的配置，不過，包括 CanExecuteChanged 事件接聽程式、新增到工作集，以及新增到頁面的啟動/瀏覽時間。 **建議：** 做為便利的 ICommand 介面的替代方案，請考慮將事件處理常式放在您的程式碼後置中並將它們連結到檢視事件，然後在這些事件被引發時，在您的檢視模型上呼叫命令。 您還需要新增額外的程式碼，以在無法使用命令時停用「按鈕」。
-   在 MVVM 中，建立一個含有所有可能的 UI 設定的「頁面」，然後藉由將 Visibility 屬性繫結到 VM 中的屬性來摺疊樹狀結構的組件，是相當常見的做法。 這會使得啟動時間產生不必要的增加，並且也可能導致工作集變大 (因為樹狀結構的某些組件可能永遠不會顯示)。 **建議：** 使用 [x:Load 屬性](../xaml-platform/x-load-attribute.md)或 [x:DeferLoadStrategy 屬性](../xaml-platform/x-deferloadstrategy-attribute.md)功能來延遲樹狀結構的非必要部分，使其不參與啟動。 此外，請為頁面的不同模式建立個別的使用者控制項，然後使用程式碼後置以只載入必要控制項。

## <a name="ccx-recommendations"></a>C++/CX 建議

-   **使用最新版本**。 針對 C + + /CX 編譯器，有提供持續的效能改進。 請務必使用最新的工具組來建立您的應用程式。
-   **停用 RTTI (/GR-)**。 編譯器中預設為開啟 RTTI，因此，除非您的建置環境將它關閉，否則您可能已經在使用它。 RTTI 會帶來明顯的額外負荷，除非您的程式碼對其有相當深度的倚賴，否則應該將它關閉。 XAML 架構完全不要求您的程式碼要使用 RTTI。
-   **避免大量使用 ppltasks**。 呼叫非同步 WinRT API 時，Ppltasks 非常便利，但是它們會帶來明顯的程式碼大小額外負荷。 C++/CX 小組正在設想一個將可提供更佳效能的語言功能 – await。 在此同時，請針對您程式碼最忙碌路徑中的 ppltasks 使用取得平衡。
-   **避免在應用程式的「商務邏輯」中使用 C++/CX**。 C++/CX 的設計目的是要做為一個從 C++ 應用程式存取 WinRT API 的便利方式。 它會利用具有額外負荷的包裝函式。 您應該避免在類別的商務邏輯/模型內使用 C++/CX，而將其保留以供在您的程式碼與 WinRT 之間的界限使用。