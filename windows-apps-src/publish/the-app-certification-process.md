---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: 應用程式認證程序
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10，uwp，發佈，前置處理，認證，發行，擱置中，提交，發佈，狀態，時間
ms.localizationpriority: medium
ms.openlocfilehash: 24eec7e0f9b52d83d9cda5b964e6b23274a5e6e6
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5549680"
---
# <a name="the-app-certification-process"></a>應用程式認證程序

當您建立完 App 的提交作業並且按一下 **\[提交至 Microsoft Store\]** 時，提交就進入認證步驟。 此程序通常會在幾個小時內完成，某些情況則需要三個工作天。 您的提交通過認證後，它可能需要 24 小時，讓客戶可以看到新的提交，或使用變更更新提交套件的應用程式清單。 如果您的更新只變更市集清單詳細資料，會少於每小時內完成發行程序。  您的提交發佈，並在儀表板中的應用程式的狀態將會**在市集**時，您將會收到通知。

## <a name="preprocessing"></a>前置處理

順利上傳應用程式套件並提交應用程式以進行認證之後，套件就會進入測試的佇列中。 如果在前置處理期間偵測到任何錯誤，我們會顯示訊息。 如需關於可能錯誤的詳細資訊，請參閱[解決提交錯誤](resolve-submission-errors.md)。

## <a name="certification"></a>認證

在此階段會進行幾項測試：

-   **安全性測試：** 第一個測試會檢查您 app 的套件是否含有病毒或惡意程式碼。 如果應用程式未通過這個測試，您需要執行最新的防毒軟體來檢查您的開發系統，然後在全新的作業系統重建您的應用程式套件。
-   **技術規範測試：** 技術規範是由 Windows 應用程式認證套件進行測試。 (將 app 提交至市集之前，請務必記得[使用 Windows 應用程式認證套件測試 app](../debug-test-perf/windows-app-certification-kit.md))。
-   **內容規範：** 所需的時間取決於您應用程式的複雜程度、應用程式包含多少視覺化內容以及最近提交了多少應用程式。 請務必在[認證注意事項](notes-for-certification.md)頁面中提供測試人員應該注意的任何資訊。

認證程序完成之後，您會收到一份認證報告，告知您的應用程式是否通過認證。 若 app 未通過認證，報告將指出哪個測試失敗或不符合哪個[原則](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 您修正問題之後，可以重新提交應用程式，再次進行認證程序。

## <a name="release"></a>發行

當您的應用程式通過認證之後時，它已經準備好移到**發佈**程序。

- 如果您之前指定，應該儘可能 （預設選項） 即將發行您的提交，就會立即開始發佈程序。
- 如果這是第一次您已經發佈應用程式，並在 [[排程](configure-precise-release-scheduling.md#release)\] 區段中所指定的**發行日期**，應用程式也會提供根據您的**發行日期**選擇。
- 如果您曾使用[發佈暫停選項](manage-submission-options.md#publishing-hold-options)來指定，它應該不會釋放直到某個日期，我們會等到該日期開始發佈程序，除非您選取 [**變更發行日期**。
- 如果您曾使用[發佈暫停選項](manage-submission-options.md#publishing-hold-options)來指定您想要手動發佈提交，我們將不會開始發行程序，直到您選取 [**立即發佈**（或選取 [**變更發行日期**並挑選特定日期）。


## <a name="publishing"></a>發佈

應用程式發行之後，系統會對應用程式的套件加上數位簽章，以保護這些套件使其不受竄改。 此階段開始後，您就無法取消提交或變更發行日期。

新的應用程式，以及更新其中包括應用程式套件的變更，將會在 24 小時內完成發行程序。 對於只變更選項，例如 [市集清單詳細資料，但不會變更應用程式套件的更新，發佈程序會引導小於一小時。

當您的應用程式處於發佈階段時，在您的應用程式提交的狀態欄中**顯示詳細資料**連結可讓您知道當您的新套件和市集清單詳細資料可供您每個支援 OS 版本的客戶。 尚未完成的步驟會顯示為**擱置中**。 您的 app 將處於發行階段處理程序完成之前，表示新套件和/或清單詳細資料可供所有的應用程式的潛在客戶。

## <a name="in-the-store"></a>在 Microsoft Store 中 

成功進行上述步驟之後，提交的狀態會從 **\[發行中\]** 變成 **\[在 Microsoft Store 內\]**。 客戶就能在 Microsoft Store 下載您的提交項目 (除非您選擇了其他[可搜尋性](choose-visibility-options.md#discoverability)選項)。 

> [!NOTE]
> 我們也會在應用程式發佈之後進行抽樣檢查以找出潛在的問題，並且確保您的應用程式符合所有的 [Microsoft Store 原則](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 如果我們發現任何問題，將會通知您相關問題和修正方式 (如果有)，或者問題是否已從市集移除。

 

 

 




