---
description: 當您在合作夥伴中心中定義實驗，並在應用程式中撰寫實驗的程式碼之後，您就可以開始使用您的實驗，並使用合作夥伴中心來檢查實驗的結果。
title: 在合作夥伴中心管理您的實驗
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: dfcd9819940d21dcc81c5ac698b76381adf05af6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030551"
---
# <a name="manage-your-experiment-in-partner-center"></a>在合作夥伴中心管理您的實驗

在 [合作夥伴中心中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md) 並撰寫 [應用程式程式碼以進行實驗](code-your-experiment-in-your-app.md)之後，您就可以開始啟用您的實驗，並使用合作夥伴中心來檢查實驗的結果。 取得所需的全部資料之後，您可以結束實驗，並選擇是否要在所有 App 中繼續使用控制項變化中的變數值，或切換為使用您其中一個其他變化中的變數值。

> [!NOTE]
> 當您啟用實驗時，合作夥伴中心會立即開始從任何經檢測的應用程式收集資料，以記錄您實驗的資料。 不過，實驗資料可能需要數小時的時間才會出現在合作夥伴中心中。

如需示範建立及執行實驗的端對端處理程序的逐步解說，請參閱[利用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="activate-your-experiment"></a>啟用您的實驗

當您滿意合作夥伴中心中的實驗參數，且您已更新應用程式程式碼時，就可以開始啟用您的實驗，以便開始從您的應用程式收集實驗資料。 當實驗處於作用中狀態時，您的應用程式可以將變化值和報表檢視和轉換事件取出至合作夥伴中心。

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **您的應用程式** 之下，選取您要啟用實驗的應用程式。
3. 在流覽窗格中，選取 [ **服務** ]，然後選取 [ **實驗** ]。
4. 在 [ **專案** ] 區段的專案表格中，展開包含您實驗的專案，然後執行下列其中一項：
  * 按一下實驗的 [ **啟動** ] 連結。 您的實驗會新增至接近頁面頂端的 [作用中 **實驗** ] 區段。
  * 按一下實驗名稱，然後在 [實驗] 頁面的底部，按一下 [ **啟動** ]。

> [!IMPORTANT]
> 啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時按一下 **\[可編輯的實驗\]** 核取方塊。 我們建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

## <a name="review-the-results-of-your-experiment"></a>檢閱實驗的結果

1. 在合作夥伴中心中，返回您應用程式的 **測試頁面。**
2. 在 [使用中的 **實驗** ] 區段中，按一下使用中的實驗名稱，移至 [實驗] 頁面。
3. 若為作用中或已完成的實驗，此頁面中的前兩個區段會提供您的實驗結果︰
  * [ **結果摘要** ] 區段會列出您的實驗目標，以及每個變化的轉換率百分比。
  * [ **結果詳細資料** ] 區段會針對您實驗中所有目標的變化提供更多詳細資料，包括視圖、轉換、唯一使用者、轉換率、差異%、信賴度和重要性。 *信賴度* 是估計值之可靠性的統計量值，其會計算誤差的邊界。 *重要性* 是以樣本大小為基礎的統計量值，可判斷結果不是因為有機會而產生的可能性，而是將屬性設為特定的原因。

> [!NOTE]
> 合作夥伴中心只會在24小時的時間內報告每位使用者的第一個轉換事件。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止具有許多轉換事件的單一使用者扭曲一組範例使用者的實驗結果。


## <a name="complete-your-experiment"></a>完成您的實驗

1. 在合作夥伴中心中，返回您的實驗頁面。 如需相關指示，請參閱上一節。
2. 在 [ **結果摘要** ] 區段中，執行下列其中一項：
  * 如果您想要結束實驗，並繼續在您的應用程式中使用控制項變異的變數值，請按一下 [ **保留** ]。
  * 如果您想要結束實驗，但切換至在您的應用程式中使用不同變化的變數值，請按一下您要切換的變化下的 [ **切換** ]。
3. 按一下 **[確定]** ，確認您想要結束實驗。


## <a name="related-topics"></a>相關主題

* [在合作夥伴中心中建立專案並定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的應用程式程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行應用程式實驗](run-app-experiments-with-a-b-testing.md)
