---
Description: After you define your experiment in Partner Center and code your experiment in your app, you are ready to active your experiment and use Partner Center to review the results of your experiment.
title: 在合作夥伴中心管理您的實驗
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 6e5c0d0ca1b1d771df2b224cc41ec5a37e267bc9
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8207006"
---
# <a name="manage-your-experiment-in-partner-center"></a>在合作夥伴中心管理您的實驗

您可以[定義您在合作夥伴中心中的實驗](define-your-experiment-in-the-dev-center-dashboard.md)，並[編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)之後, 您準備好啟用實驗並使用合作夥伴中心來檢閱實驗的結果。 取得所需的全部資料之後，您可以結束實驗，並選擇是否要在所有 App 中繼續使用控制項變化中的變數值，或切換為使用您其中一個其他變化中的變數值。

> [!NOTE]
> 當您啟用實驗時，合作夥伴中心就會立即開始從任何已檢測以記錄您的實驗資料的 app 收集資料。 不過，它可能需要數小時的實驗資料才會出現在合作夥伴中心。

如需示範建立及執行實驗的端對端處理程序的逐步解說，請參閱[利用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="activate-your-experiment"></a>啟用您的實驗

當您滿意您的實驗，在合作夥伴中心的參數，且您已更新您的應用程式程式碼時，您已經準備好啟用您的實驗，以便您可以開始從您的 app 收集實驗資料。 實驗作用中時，您的應用程式可以擷取變化值，並向合作夥伴中心報告檢視和轉換事件。

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 在**您的應用程式** 之下，選取您要啟用實驗的應用程式。
3. 在瀏覽窗格中，選取 **\[服務\]**，然後選取 **\[實驗\]**。
4. 在 **\[專案\]** 區段的專案表格中，展開包含您實驗的專案，接著執行下列其中一項：
  * 按一下實驗的 **\[啟用\]** 連結。 您的實驗即會新增到接近頁面頂端的 **\[作用中的實驗\]** 區段。
  * 按一下實驗名稱、捲動到實驗頁面底部，然後按一下 **\[啟用\]**。

> [!IMPORTANT]
> 啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時按一下 **\[可編輯的實驗\]** 核取方塊。 我們建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

## <a name="review-the-results-of-your-experiment"></a>檢閱實驗的結果

1. 在合作夥伴中心，回到您的應用程式的**實驗**頁面。
2. 在 **\[作用中的實驗\]** 區段中，按一下作用中的實驗名稱以移至實驗頁面。
3. 若為作用中或已完成的實驗，此頁面中的前兩個區段會提供您的實驗結果︰
  * **\[結果摘要\]** 區段會列出您的實驗目標以及每個變化的轉換率百分比。
  * **\[結果詳細資料\]** 區段會為實驗中所有目標的每個變化提供更多詳細資料，包括檢視數、轉換數、不重複的使用者數、轉換率、差異 %、信賴和顯著性。 *「信賴」* 是估計值可靠性的統計測量，用來計算誤差範圍。 *「顯著性」* 是以樣本大小為基礎的統計測量，用來判斷結果的可能性不是因為機運，而是歸咎於特定原因。

> [!NOTE]
> 合作夥伴中心僅會回報第一個轉換事件為每個使用者在 24 小時內。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止具有許多轉換事件的單一使用者扭曲一組範例使用者的實驗結果。


## <a name="complete-your-experiment"></a>完成您的實驗

1. 在合作夥伴中心，回到您的實驗頁面。 如需相關指示，請參閱上一節。
2. 在 **\[結果摘要\]** 區段中，執行下列其中一項：
  * 如果您想要結束實驗並繼續在您的 App 中使用此控制項變化的變數值，按一下 **\[保留\]**。
  * 如果您想要結束實驗但切換為在您的 App 中使用不同變化的變數值，在您要切換的目標變化下方按一下 **\[切換\]**。
3. 按一下 **\[確定\]**，確認您要結束實驗。


## <a name="related-topics"></a>相關主題

* [建立專案與定義遠端變數在合作夥伴中心](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)
