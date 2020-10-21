---
Description: 當您建立完 App 的提交作業並且按一下 \[提交至 Microsoft Store\] 時，提交就進入認證步驟。
title: 應用程式認證程序
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、uwp、發行、前置處理、認證、發行、暫止、提交、發佈、狀態、時間
ms.localizationpriority: medium
ms.openlocfilehash: d88d8deeb467f186f120fb8c1e579d5c9222aaf1
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253798"
---
# <a name="the-app-certification-process"></a>應用程式認證程序

當您建立完 App 的提交作業並且按一下 **\[提交至 Microsoft Store\]** 時，提交就進入認證步驟。 此程序通常會在幾個小時內完成，某些情況則需要三個工作天。 提交通過認證之後，最多可能需要24小時的時間，客戶才能看到應用程式的新提交清單，或針對套件的變更進行更新提交。 如果您的更新只變更 Store 清單詳細資料，則發佈程式將會在一小時內完成。  當您發佈提交時，系統將會通知您，且儀表板中的應用程式狀態將會 **在存放區中**。

## <a name="preprocessing"></a>前置處理

順利上傳應用程式套件並提交應用程式以進行認證之後，套件就會進入測試的佇列中。 如果在前置處理期間偵測到任何錯誤，我們會顯示訊息。 如需關於可能錯誤的詳細資訊，請參閱[解決提交錯誤](resolve-submission-errors.md)。

## <a name="certification"></a>認證

在此階段會進行幾項測試：

-   **安全性測試：** 第一個測試會檢查您 app 的套件是否含有病毒或惡意程式碼。 如果應用程式未通過這個測試，您需要執行最新的防毒軟體來檢查您的開發系統，然後在全新的作業系統重建您的應用程式套件。
-   **技術規範測試：** 技術規範是由 Windows 應用程式認證套件進行測試。 (將 app 提交至市集之前，請務必記得[使用 Windows 應用程式認證套件測試 app](../debug-test-perf/windows-app-certification-kit.md))。
-   **內容規範：** 所需的時間取決於您應用程式的複雜程度、應用程式包含多少視覺化內容以及最近提交了多少應用程式。 請務必在[認證注意事項](notes-for-certification.md)頁面中提供測試人員應該注意的任何資訊。

認證程序完成之後，您會收到一份認證報告，告知您的應用程式是否通過認證。 若 app 未通過認證，報告將指出哪個測試失敗或不符合哪個[原則](store-policies.md)。 您修正問題之後，可以重新提交應用程式，再次進行認證程序。

## <a name="release"></a>版本

當您的應用程式通過認證時，即已準備好移至 **發佈** 流程。

- 如果您已指出應該儘快發佈提交 (預設選項) ，則會立即開始發行程式。
- 如果這是您第一次發行應用程式，而且您在 [[排程](configure-precise-release-scheduling.md#release)] 區段中指定了**發行日期**，則應用程式將會根據您的**發行日期**選項而變成可用。
- 如果您已使用 [發佈保留選項](manage-submission-options.md#publishing-hold-options) 來指定在特定日期之前不應發行，我們會等到該日期開始發佈程式，除非您選取 **變更發行日期**。
- 如果您已使用 [發行保留選項](manage-submission-options.md#publishing-hold-options) 來指定要手動發佈提交，除非您選取 [ **立即發佈** ]，否則不會啟動發佈程式 (或選取 [ **變更發行日期** ]，然後挑選特定的日期) 。


## <a name="publishing"></a>發佈

應用程式發行之後，系統會對應用程式的套件加上數位簽章，以保護這些套件使其不受竄改。 此階段開始後，您就無法取消提交或變更發行日期。

針對新的應用程式和更新（包括應用程式套件的變更），發佈程式將在24小時內完成。 針對只變更選項（例如 Store 清單詳細資料，但不變更應用程式封裝）的更新，發佈程式將花費不到一小時的時間。

當您的應用程式在發佈階段時，您應用程式提交的 [狀態] 資料行中的 [ **顯示詳細資料** ] 連結，可讓您知道新套件和 Store 清單詳細資料何時可供客戶在每個支援的作業系統版本上使用。 尚未完成的步驟會顯示為**擱置中**。 在程式完成之前，您的應用程式會保留在發佈階段中，這表示所有應用程式的潛在客戶都可使用新的套件及/或清單詳細資料。

## <a name="in-the-store"></a>在 Microsoft Store 中 

成功進行上述步驟之後，提交的狀態會從 **\[發行中\]** 變成 **\[在 Microsoft Store 內\]**。 客戶就能在 Microsoft Store 下載您的提交項目 (除非您選擇了其他[可搜尋性](choose-visibility-options.md#discoverability)選項)。 

> [!NOTE]
> 我們也會在應用程式發佈之後進行抽樣檢查以找出潛在的問題，並且確保您的應用程式符合所有的 [Microsoft Store 原則](store-policies.md)。 如果我們發現任何問題，將會通知您相關問題和修正方式 (如果有)，或者問題是否已從市集移除。

 

 

 




