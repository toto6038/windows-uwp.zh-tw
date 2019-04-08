---
Description: 在此逐步解說中，您將會使用 A/B 測試建立、執行和管理您的第一個實驗。
title: 建立和執行您的第一個實驗
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: 8dba9095326c01029e14742c98c1c368b896dfb8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660213"
---
# <a name="create-and-run-your-first-experiment"></a>建立和執行您的第一個實驗

在此逐步解說中，您將︰
* 建立實驗[專案](run-app-experiments-with-a-b-testing.md#terms)在合作夥伴中心，定義數個遠端變數代表的文字和應用程式按鈕的色彩。
* 建立應用程式程式碼來擷取遠端變數的值，使用此資料來變更背景色彩的按鈕，並記錄檢視與轉換資料送回合作夥伴中心的事件。
* 在專案中建立實驗，測試變更 App 按鈕的背景色彩是否會成功增加按鈕的點擊數。
* 執行 App 以收集實驗資料。
* 檢閱在合作夥伴中心實驗結果，選擇一種變化，若要啟用所有使用者的應用程式，並完成實驗。

概略 A / B 測試與合作夥伴中心，請參閱[執行應用程式的實驗，以 A / B 測試](run-app-experiments-with-a-b-testing.md)。

## <a name="prerequisites"></a>必要條件

若要依照本逐步解說中，您必須有合作夥伴中心帳戶和您必須設定您的開發電腦中所述[應用程式的實驗執行 A / B 測試](run-app-experiments-with-a-b-testing.md)。

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>建立專案，而在合作夥伴中心內的遠端變數

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 如果您已經在您想要用來建立實驗的合作夥伴中心內應用程式，請在合作夥伴中心內選取該應用程式。 如果您還沒有應用程式在合作夥伴中心[建立新的應用程式保留名稱](../publish/create-your-app-by-reserving-a-name.md)，然後在合作夥伴中心內選取該應用程式。
3. 在瀏覽窗格中，按一下 [服務]，然後再按一下 [實驗]。
4. 在下一頁的 [專案] 區段中，按一下 [新專案] 按鈕。
5. 在 [新專案] 頁面中，為新的專案輸入**按鈕按一下實驗**名稱。
6. 展開 [遠端變數] 區段，然後按一下 [加入變數] 四次。 您現在應該有四個空的變數列。
  * 在第一列中，輸入 **buttonText** 做為變數名稱，在 [預設值] 欄中輸入 **Grey Button**。
  * 在第二列中，輸入 **r** 做為變數名稱，在 [預設值] 欄中輸入 **128**。
  * 在第三列中，輸入 **g** 做為變數名稱，在 [預設值] 欄中輸入 **128**。
  * 在第四列中，輸入 **b** 做為變數名稱，在 [預設值] 欄中輸入 **128**。
7. 按一下 [儲存]，並且記下 [SDK 整合] 區段中出現的[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)值。 在下一節中，您將會更新您的 App 程式碼，並在程式碼中參照這個值。

## <a name="code-the-experiment-in-your-app"></a>在您的 App 中編寫實驗程式碼

1. 在 Visual Studio 中，使用 Visual C# 建立新的通用 Windows 平台專案。 將專案命名為 **SampleExperiment**。
2. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]，然後按一下 [加入參考]。
3. 在 \[參考管理員\] 中，展開 \[通用 Windows\]，然後按一下 \[擴充功能\]。
4. 在 SDK 清單中，選取 [Microsoft Engagement Framework] 旁邊的核取方塊，然後按一下 [確定]。
5. 在 [方案總管] 中，按兩下 MainPage.xaml 以在 App 中開啟主頁面的設計工具。
6. 將 [按鈕]  從 [工具箱] 拖曳至頁面。
7. 按兩下位於設計工具的按鈕，以開啟程式碼檔案並為**點選**事件新增事件處理常式。  
8. 使用下列程式碼取代整個程式碼檔案內容。 指派```projectId```變數設為[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)您從上一節中的合作夥伴中心取得的值。
    [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

9. 儲存程式碼檔案，並建置專案。

## <a name="create-the-experiment-in-partner-center"></a>在合作夥伴中心中建立實驗

1. 返回 **按鈕按一下實驗**在合作夥伴中心內的專案頁面。
2. 在 [實驗] 區段中，按一下 [新增實驗] 按鈕。
3. 在 [實驗詳細資料] 區段的 [實驗名稱] 欄位中輸入名稱：**最佳化按鈕點選次數**。
4. 在 [檢視事件] 區段的 [檢視事件名稱] 欄位中，輸入 **userViewedButton**。 請注意這個名稱符合您在上一節新增的程式碼中記錄的檢視事件字串。
5. 在 [目標和轉換事件] 區段中，輸入下列值：
  * 在 [目標名稱] 欄位中，輸入 [增加按鈕點選次數]。
  * 在 [轉換事件名稱] 欄位中，輸入名稱：**userClickedButton**。 請注意這個名稱符合您在上一節新增的程式碼中記錄的轉換事件字串。
  * 在 [目標] 欄位中，選擇 [最大化]。
6. 在 [遠端變數和變化] 區段中，確認選取 [平均散佈] 核取方塊，以平均散佈變化至您的 App。
7. 對您的實驗新增變數︰
    1. 按一下下拉式清單控制項，選擇 [buttonText]，然後按一下 [加入變數]。 字串 **Grey Button** 應該會自動出現在 [變化 A] 欄中 (此值衍生自專案設定)。 在 [變化 B] 欄中，輸入 **Blue Button**。
    2. 再按一下下拉式清單控制項，選擇 [r]，然後按一下 [加入變數]。 字串 **128** 應該會自動出現在 [變化 A] 欄中。 在 [變化 B] 欄中，輸入 **1**。
    3. 再按一下下拉式清單控制項，選擇 [g]，然後按一下 [加入變數]。 字串 **128** 應該會自動出現在 [變化 A] 欄中。 在 [變化 B] 欄中，輸入 **1**。  
    4. 再按一下下拉式清單控制項，選擇 [b]，然後按一下 [加入變數]。 字串 **128** 應該會自動出現在 [變化 A] 欄中。 在 [變化 B] 欄中，輸入 **255**。  
8. 按一下 [儲存]，然後再按一下 [啟用]。

> [!IMPORTANT]
> 啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時按一下 **\[可編輯的實驗\]** 核取方塊。 我們通常會建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

## <a name="run-the-app-to-gather-experiment-data"></a>執行 App 以收集實驗資料

1. 執行您稍早建立的 **SampleExperiment** App。
2. 確認您看到灰色或藍色按鈕。 按一下該按鈕，然後關閉 app。
3. 在同部電腦上重複執行數次上述步驟，以確認 app 顯示相同的按鈕色彩。

## <a name="review-the-results-and-complete-the-experiment"></a>檢閱結果並完成實驗

完成上一節後，請至少等候幾個小時的時間，然後再遵循以下步驟檢閱您實驗的結果並完成實驗。

> [!NOTE]
> 一旦您啟用實驗時，合作夥伴中心便會立即開始收集資料，用以記錄資料，提供您的實驗的任何應用程式。 不過，可能需要數小時才會出現在合作夥伴中心的實驗資料。

1. 在合作夥伴中心，返回**實驗**您應用程式頁面。
2. 在 [啟用實驗] 區段中，按一下 [最佳化按鈕點選次數] 以移至此實驗的頁面。
3. 確認在 [結果摘要] 和 [結果詳細資料] 區段中顯示的結果符合您的預期。 如需進一步瞭解這些區段的詳細資訊，請參閱[管理您的實驗，在合作夥伴中心](manage-your-experiment.md#review-the-results-of-your-experiment)。
    > [!NOTE]
    > 合作夥伴中心報告在 24 小時制的時間週期內的每一位使用者僅限第一次轉換事件。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止具有許多轉換事件的單一使用者扭曲一組範例使用者的實驗結果。

4. 您現已準備好結束實驗。 在 [結果摘要] 區段的 [變異 B] 欄中，按一下 [切換]。 這會將您 app 的所有使用者切換至藍色按鈕。
5. 按一下 [確定]，確認您要結束實驗。
6. 執行您在上一節中建立的 **SampleExperiment** app。
7. 確認顯示藍色按鈕。 請注意，app 可能需要兩分鐘時間來接收更新的變異指派。

## <a name="related-topics"></a>相關主題

* [建立專案，並在合作夥伴中心中定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [您的應用程式進行測試的程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心內定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [管理您的實驗，在合作夥伴中心](manage-your-experiment.md)
* [應用程式的實驗執行 A / B 測試](run-app-experiments-with-a-b-testing.md)
