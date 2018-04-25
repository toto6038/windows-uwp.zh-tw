---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: 應用程式認證程序
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 03/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 發佈, 前置處理, 認證, 發行, 擱置中, 提交, 發佈, 狀態
ms.localizationpriority: high
ms.openlocfilehash: 0b2191808457401a41fe6bb0996d3f5a5ed4943d
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="the-app-certification-process"></a>應用程式認證程序

當您建立完 App 的提交作業並且按一下 **\[提交至 Microsoft Store\]** 時，提交就進入認證步驟。 此程序通常會在幾個小時內完成，某些情況則需要三個工作天。 當您的提交通過驗證之後，最多可能需要 24 小時，客戶才會在市集中看到 App 的清單 (或先前發行之 App 的更新)。 當您的提交已發行且可供客戶取得時，您會看到通知，且 app 在儀表板中的狀態會是 **\[在市集內\]**。

## <a name="preprocessing"></a>前置處理

順利上傳應用程式套件並提交應用程式以進行認證之後，套件就會進入測試的佇列中。 如果在前置處理期間偵測到任何錯誤，我們會顯示訊息。 如需關於可能錯誤的詳細資訊，請參閱[解決提交錯誤](resolve-submission-errors.md)。

## <a name="certification"></a>認證

在此階段會進行幾項測試：

-   **安全性測試：** 第一個測試會檢查您 app 的套件是否含有病毒或惡意程式碼。 如果應用程式未通過這個測試，您需要執行最新的防毒軟體來檢查您的開發系統，然後在全新的作業系統重建您的應用程式套件。
-   **技術規範測試：** 技術規範是由 Windows 應用程式認證套件進行測試。 (將 app 提交至市集之前，請務必記得[使用 Windows 應用程式認證套件測試 app](../debug-test-perf/windows-app-certification-kit.md))。
-   **內容規範：**所需的時間取決於您應用程式的複雜程度、應用程式包含多少視覺化內容以及最近提交了多少應用程式。 請務必在[認證注意事項](notes-for-certification.md)頁面中提供測試人員應該注意的任何資訊。

認證程序完成之後，您會收到一份認證報告，告知您的應用程式是否通過認證。 若 app 未通過認證，報告將指出哪個測試失敗或不符合哪個[原則](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 您修正問題之後，可以重新提交應用程式，再次進行認證程序。

## <a name="release"></a>發行

您的應用程式通過認證之後，就可以進行**發佈**程序。 如果您之前指定提交項目應盡快發佈，就會立刻發佈。 如果您之前指定要到某個日期之後才發行，除非您按一下 **\[變更發行日期\]** 連結，否則我們會等到該日期才發佈。 如果您之前指定要手動發佈提交項目，我們就會等到您按一下 **\[立即發佈\]** 按鈕，或按一下 **\[變更發行日期\]** 的連結並挑選特定日期之後，才會發佈提交項目。

## <a name="publishing"></a>發佈

應用程式發行之後，系統會對應用程式的套件加上數位簽章，以保護這些套件使其不受竄改。 此階段開始後，您就無法取消提交或變更發行日期。

當您的應用程式處於發佈階段時，應用程式提交之 [狀態] 欄位中的 **\[顯示詳細資料\]** 連結，會在您的新套件和市集清單詳細資料可供您每個支援 OS 版本的客戶使用時，讓您得知。 尚未完成的步驟會顯示為**擱置中**。 在完成程序之前，您的 app 將處於發行階段，表示新套件和清單詳細資料可供您所有的 app 潛在客戶使用。 最多可能需要 24 小時。 

## <a name="in-the-store"></a>在 Microsoft Store 中 

成功進行上述步驟之後，提交的狀態會從 **\[發行中\]** 變成 **\[在 Microsoft Store 內\]**。 客戶就能在 Microsoft Store 下載您的提交項目 (除非您選擇了其他[可搜尋性](choose-visibility-options.md#discoverability)選項)。 

> [!NOTE]
> 我們也會在應用程式發佈之後進行抽樣檢查以找出潛在的問題，並且確保您的應用程式符合所有的 [Microsoft Store 原則](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 如果我們發現任何問題，將會通知您相關問題和修正方式 (如果有)，或者問題是否已從市集移除。

 

 

 




