---
Description: 當您完成建立您的應用程式提交並按一下 提交至市集時，提交輸入憑證的步驟。
title: 應用程式認證程序
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10 uwp，發行，前置處理、 憑證、 發行，暫止，提交，發佈，狀態時間
ms.localizationpriority: medium
ms.openlocfilehash: 733d5ff882d7ed7c574f6fe6fedd28b79c3913d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597073"
---
# <a name="the-app-certification-process"></a>應用程式認證程序

當您建立完 App 的提交作業並且按一下 **\[提交至 Microsoft Store\]** 時，提交就進入認證步驟。 此程序通常會在幾個小時內完成，某些情況則需要三個工作天。 您的提交憑證之後，它可能需要 24 小時的時間讓客戶查看新的提交，或以變更更新提交至封裝的應用程式的清單。 如果您的更新只會變更清單詳細資訊的存放區，少於一小時就會完成發行程序。  您會收到通知時發行您的提交，並在儀表板中的應用程式的狀態將會**在存放區內**。

## <a name="preprocessing"></a>前置處理

順利上傳應用程式套件並提交應用程式以進行認證之後，套件就會進入測試的佇列中。 如果在前置處理期間偵測到任何錯誤，我們會顯示訊息。 如需關於可能錯誤的詳細資訊，請參閱[解決提交錯誤](resolve-submission-errors.md)。

## <a name="certification"></a>Certification

在此階段會進行幾項測試：

-   **安全性測試：** 此第一項測試會檢查您的應用程式套件，病毒和惡意程式碼。 如果應用程式未通過這個測試，您需要執行最新的防毒軟體來檢查您的開發系統，然後在全新的作業系統重建您的應用程式套件。
-   **技術相容性測試：** Windows 應用程式認證套件被測試技術相容性。 (將 app 提交至市集之前，請務必記得[使用 Windows 應用程式認證套件測試 app](../debug-test-perf/windows-app-certification-kit.md))。
-   **內容的合規性：** 這樣所花費的時間量是依複雜應用程式是多少，將視覺內容，最近已提交多少個應用程式而有所不同。 請務必在[認證注意事項](notes-for-certification.md)頁面中提供測試人員應該注意的任何資訊。

認證程序完成之後，您會收到一份認證報告，告知您的應用程式是否通過認證。 若 app 未通過認證，報告將指出哪個測試失敗或不符合哪個[原則](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 您修正問題之後，可以重新提交應用程式，再次進行認證程序。

## <a name="release"></a>發行

當您的應用程式通過認證時，就準備好移往**發佈**程序。

- 如果您已指定，應該儘可能 （預設選項） 發佈您的提交，就會立即開始發佈程序。
- 如果這是第一次您發行應用程式，而且您指定**發行日期**中[排程](configure-precise-release-scheduling.md#release)區段中，應用程式將可供根據您**發行日期**選取項目。
- 如果您使用過[發行保留選項](manage-submission-options.md#publishing-hold-options)若要指定，它應該不會釋放在特定日期之前，我們會等到該日期，若要開始發行程序中，除非您選取**變更發行日期**。
- 如果您使用過[發行保留選項](manage-submission-options.md#publishing-hold-options)若要指定您想要手動發佈提交，我們將不會啟動發佈程序，直到您選取**立即發行**(或選取**變更發行日期**挑選特定的日期)。


## <a name="publishing"></a>Publishing

應用程式發行之後，系統會對應用程式的套件加上數位簽章，以保護這些套件使其不受竄改。 此階段開始後，您就無法取消提交或變更發行日期。

對於新的應用程式和更新包括對應用程式的套件，發佈程序將在 24 小時內完成。 只變更選項，例如市集清單的詳細資訊，但不會變更應用程式套件的更新，發佈程序需要少於一小時。

在發行階段中，您的應用程式時**Podrobnosti**連結狀態 欄中，您的應用程式提交可讓您知道您的新套件和清單詳細資訊的存放區時提供給每個支援的 OS 上的客戶版本。 尚未完成的步驟會顯示為**擱置中**。 您的應用程式仍在發行階段的程序完成之前，也就是說，新的套件和/或清單詳細資訊可供所有的應用程式的潛在客戶。

## <a name="in-the-store"></a>在市集中 

成功進行上述步驟之後，提交的狀態會從 [發行中] 變成 [在市集內]。 客戶就能在 Microsoft Store 下載您的提交項目 (除非您選擇了其他[可搜尋性](choose-visibility-options.md#discoverability)選項)。 

> [!NOTE]
> 我們也會在應用程式發佈之後進行抽樣檢查以找出潛在的問題，並且確保您的應用程式符合所有的 [Microsoft Store 原則](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 如果我們發現任何問題，將會通知您相關問題和修正方式 (如果有)，或者問題是否已從市集移除。

 

 

 




