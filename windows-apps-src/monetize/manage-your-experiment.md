---
Description: 在合作夥伴中心中定義您的實驗，且您的應用程式中的程式碼實驗之後，您就準備好使用您的實驗，而且使用合作夥伴中心檢閱您的實驗的結果。
title: 在合作夥伴中心管理您的實驗
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 6e5c0d0ca1b1d771df2b224cc41ec5a37e267bc9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594923"
---
# <a name="manage-your-experiment-in-partner-center"></a>在合作夥伴中心管理您的實驗

之後您[在合作夥伴中心內定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)並[應用程式進行測試的程式碼](code-your-experiment-in-your-app.md)，即可啟用您的實驗，並使用合作夥伴中心 來檢閱您的實驗的結果。 取得所需的全部資料之後，您可以結束實驗，並選擇是否要在所有 App 中繼續使用控制項變化中的變數值，或切換為使用您其中一個其他變化中的變數值。

> [!NOTE]
> 當您啟動實驗時，合作夥伴中心便會立即開始收集資料，用以記錄資料，提供您的實驗的任何應用程式。 不過，可能需要數小時才會出現在合作夥伴中心的實驗資料。

如需示範建立及執行實驗的端對端程序的逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="activate-your-experiment"></a>啟用您的實驗

當您滿意您的實驗，在合作夥伴中心內的參數，並更新您的應用程式程式碼時，您已準備好啟動您的實驗，因此您可以開始從您的應用程式中收集的實驗資料。 啟用實驗時，您的應用程式可以擷取變化值，並向合作夥伴中心報告檢視 」 和 「 轉換 」 事件。

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 在**您的應用程式** 之下，選取您要啟用實驗的應用程式。
3. 在瀏覽窗格中，選取 **[服務]**，然後選取 **[實驗]**。
4. 在 **\[專案\]** 區段的專案表格中，展開包含您實驗的專案，接著執行下列其中一項：
  * 按一下實驗的 **\[啟用]\** 連結。 您的實驗即會新增到接近頁面頂端的 **\[作用中的實驗\]** 區段。
  * 按一下實驗名稱、捲動到實驗頁面底部，然後按一下 **\[啟用\]**。

> [!IMPORTANT]
> 啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時按一下 **\[可編輯的實驗\]** 核取方塊。 我們建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

## <a name="review-the-results-of-your-experiment"></a>檢閱實驗的結果

1. 在合作夥伴中心，返回**實驗**您應用程式頁面。
2. 在 **\[作用中的實驗\]** 區段中，按一下作用中的實驗名稱以移至實驗頁面。
3. 若為作用中或已完成的實驗，此頁面中的前兩個區段會提供您的實驗結果︰
  * **\[結果摘要\]** 區段會列出您的實驗目標以及每個變化的轉換率百分比。
  * **\[結果詳細資料\]** 區段會為實驗中所有目標的每個變化提供更多詳細資料，包括檢視數、轉換數、不重複的使用者數、轉換率、差異 %、信賴和顯著性。 *「信賴」* 是估計值可靠性的統計測量，用來計算誤差範圍。 *「顯著性」* 是以樣本大小為基礎的統計測量，用來判斷結果的可能性不是因為機運，而是歸咎於特定原因。

> [!NOTE]
> 合作夥伴中心報告在 24 小時制的時間週期內的每一位使用者僅限第一次轉換事件。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止具有許多轉換事件的單一使用者扭曲一組範例使用者的實驗結果。


## <a name="complete-your-experiment"></a>完成您的實驗

1. 在合作夥伴中心，返回您實驗 頁面上。 如需相關指示，請參閱上一節。
2. 在 **\[結果摘要\]** 區段中，執行下列其中一項：
  * 如果您想要結束實驗並繼續在您的 App 中使用此控制項變化的變數值，按一下 **\[保留\]**。
  * 如果您想要結束實驗但切換為在您的 App 中使用不同變化的變數值，在您要切換的目標變化下方按一下 **\[切換\]**。
3. 按一下 **\[確定\]**，確認您要結束實驗。


## <a name="related-topics"></a>相關主題

* [建立專案，並在合作夥伴中心中定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [您的應用程式進行測試的程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心內定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [建立並執行您的第一個實驗以 A / B 測試](create-and-run-your-first-experiment-with-a-b-testing.md)
* [應用程式的實驗執行 A / B 測試](run-app-experiments-with-a-b-testing.md)
