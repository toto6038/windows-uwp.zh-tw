---
Description: 在此逐步解說中，您將會使用 A/B 測試建立、執行和管理您的第一個實驗。
title: 建立和執行您的第一個實驗
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK A/B 測試, 實驗
ms.localizationpriority: medium
ms.openlocfilehash: de68b779fe8f02af6afdcb4d42ce0da77aeecc7d
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362451"
---
# <a name="create-and-run-your-first-experiment"></a>建立和執行您的第一個實驗

在本逐步解說中，您將：
* 在合作夥伴中心中建立測試 [專案](run-app-experiments-with-a-b-testing.md#terms) ，以定義數個代表應用程式按鈕文字和色彩的遠端變數。
* 使用可抓取遠端變數值的程式碼建立應用程式、使用此資料來變更按鈕的背景色彩，以及將 view 和轉換事件資料記錄回合作夥伴中心。
* 在專案中建立實驗，測試變更 App 按鈕的背景色彩是否會成功增加按鈕的點擊數。
* 執行 App 以收集實驗資料。
* 在合作夥伴中心中檢查實驗結果，選擇要為應用程式的所有使用者啟用的變化，並完成實驗。

如需使用合作夥伴中心進行 A/B 測試的總覽，請參閱 [使用 A/b 測試來執行應用程式實驗](run-app-experiments-with-a-b-testing.md)。

## <a name="prerequisites"></a>先決條件

若要遵循這個逐步解說，您必須有合作夥伴中心帳戶，且必須設定您的開發電腦，如 [使用 a/B 測試執行應用程式實驗](run-app-experiments-with-a-b-testing.md)中所述。

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>使用合作夥伴中心中的遠端變數建立專案

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard)。
2. 如果您在合作夥伴中心中已經有想要用來建立實驗的應用程式，請在合作夥伴中心中選取該應用程式。 如果您在合作夥伴中心中還沒有應用程式，請 [保留名稱來建立新的應用](../publish/create-your-app-by-reserving-a-name.md) 程式，然後在合作夥伴中心中選取該應用程式。
3. 在流覽窗格中，按一下 [ **服務** ]，然後按一下 [ **實驗**]。
4. 在下一個頁面的 [ **專案** ] 區段中，按一下 [ **新增專案** ] 按鈕。
5. 在 [ **新增專案** ] 頁面中，輸入新專案的 [名稱] **按鈕，然後按一下 [實驗** ]。
6. 展開 [ **遠端變數** ] 區段，然後按一下 [ **加入變數** 四次]。 您現在應該有四個空的變數列。
  * 在第一個資料列中，針對 [變數名稱] 輸入**buttonText** ，並在 [**預設值**] 資料行中輸入**灰色按鈕**。
  * 在第二列中，輸入**r**作為變數名稱，然後在 [**預設值**] 資料行中輸入**128** 。
  * 在第三個數據列中，在 [**預設值**] 資料行中輸入**g**作為變數名稱，然後輸入**128** 。
  * 在第四個數據列中，為變數名稱輸入**b** ，並在 [**預設值**] 資料行中輸入**128** 。
7. 按一下 [**儲存**]，記下 [ **SDK 整合**] 區段中顯示的[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)值。 在下一節中，您將會更新您的 App 程式碼，並在程式碼中參照這個值。

## <a name="code-the-experiment-in-your-app"></a>在您的 App 中編寫實驗程式碼

1. 在 Visual Studio 中，使用 Visual C# 建立新的通用 Windows 平台專案。 將專案命名為 **SampleExperiment**。
2. 在方案總管中，展開專案節點，以滑鼠右鍵按一下 [ **參考**]，然後按一下 [ **加入參考**]。
3. 在 **\[參考管理員\]** 中，展開 **\[通用 Windows\]**，然後按一下 **\[擴充功能\]**。
4. 在 Sdk 清單中，選取 [ **Microsoft Engagement Framework** ] 旁的核取方塊，然後按一下 **[確定]**。
5. 在 **方案總管**中，按兩下 [MainPage] 以開啟應用程式中主頁面的設計工具。
6. 將 **按鈕** 從 [ **工具箱** ] 拖曳至頁面。
7. 按兩下位於設計工具的按鈕，以開啟程式碼檔案並為**點選**事件新增事件處理常式。  
8. 使用下列程式碼取代整個程式碼檔案內容。 將 ```projectId``` 變數指派給您在上一節中從合作夥伴中心取得的 [專案識別碼](run-app-experiments-with-a-b-testing.md#terms) 值。
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentPage.xaml.cs" id="SampleExperiment":::

9. 儲存程式碼檔案，並建置專案。

## <a name="create-the-experiment-in-partner-center"></a>在合作夥伴中心中建立實驗

1. 返回合作夥伴中心中的 **按鈕 Click 實驗** 專案頁面。
2. 在 [ **實驗** ] 區段中，按一下 [ **新增實驗** ] 按鈕。
3. 在 [**實驗詳細資料**] 區段的 [**實驗名稱**] 欄位中，輸入 [優化的名稱]**按鈕**。
4. 在 [ **view** event] 區段的 [ **view event name** ] 欄位中，輸入**userViewedButton** 。 請注意這個名稱符合您在上一節新增的程式碼中記錄的檢視事件字串。
5. 在 [ **目標和轉換事件** ] 區段中，輸入下列值：
  * 在 [ **目標名稱** ] 欄位中，輸入 **增加按鈕的點擊**。
  * 在 [ **轉換事件名稱** ] 欄位中，輸入名稱 **userClickedButton**。 請注意這個名稱符合您在上一節新增的程式碼中記錄的轉換事件字串。
  * 在 [ **目標** ] 欄位中，選擇 [ **最大化**]。
6. 在 [ **遠端變數和變化** ] 區段中，確認已選取 [ **平均散發** ] 核取方塊，如此一來，就會將變化平均分散到您的應用程式。
7. 對您的實驗新增變數︰
    1. 按一下下拉式清單控制項，選擇 [ **buttonText**]，然後按一下 [ **加入變數**]。 字串 **灰色按鈕** 應該會自動出現在資料行 **變化** (此值衍生自專案設定) 。 在 [ **變化 B** ] 資料行中，輸入 **藍色按鈕**。
    2. 再次按一下下拉式清單控制項，選擇 [ **r**]，然後按一下 [ **加入變數**]。 字串 **128** 應該會自動出現在 **Variation A** 資料行中。 在 [ **Variation B** ] 資料行中，輸入 **1**。
    3. 再次按一下下拉式清單控制項，選擇 **g**，然後按一下 [ **加入變數**]。 字串 **128** 應該會自動出現在 **Variation A** 資料行中。 在 [ **Variation B** ] 資料行中，輸入 **1**。  
    4. 再次按一下下拉式清單控制項，選擇 **b**，然後按一下 [ **加入變數**]。 字串 **128** 應該會自動出現在 **Variation A** 資料行中。 在 [ **Variation B** ] 資料行中，輸入 **255**。  
8. 按一下 [ **儲存** ]，然後按一下 [ **啟動**]。

> [!IMPORTANT]
> 啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時按一下 **\[可編輯的實驗\]** 核取方塊。 我們通常會建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

## <a name="run-the-app-to-gather-experiment-data"></a>執行 App 以收集實驗資料

1. 執行您稍早建立的 **SampleExperiment** App。
2. 確認您看到灰色或藍色按鈕。 按一下該按鈕，然後關閉 app。
3. 在同部電腦上重複執行數次上述步驟，以確認 app 顯示相同的按鈕色彩。

## <a name="review-the-results-and-complete-the-experiment"></a>檢閱結果並完成實驗

完成上一節後，請至少等候幾個小時的時間，然後再遵循以下步驟檢閱您實驗的結果並完成實驗。

> [!NOTE]
> 一旦您啟用實驗，合作夥伴中心會立即開始從任何經檢測的應用程式收集資料，以記錄您實驗的資料。 不過，實驗資料可能需要數小時的時間才會出現在合作夥伴中心中。

1. 在合作夥伴中心中，返回您應用程式的**測試頁面。**
2. 在 [使用中的 **實驗** ] 區段中，按一下 [ **優化按鈕點擊** ] 以移至此實驗的頁面。
3. 確認 [ **結果摘要** ] 和 [ **結果詳細資料** ] 區段中顯示的結果符合您預期看到的結果。 如需這些區段的詳細資訊，請參閱 [合作夥伴中心管理您的實驗](manage-your-experiment.md#review-the-results-of-your-experiment)。
    > [!NOTE]
    > 合作夥伴中心只會在24小時的時間內報告每位使用者的第一個轉換事件。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止具有許多轉換事件的單一使用者扭曲一組範例使用者的實驗結果。

4. 您現已準備好結束實驗。 在 [ **結果摘要** ] 區段的 [ **Variation B** ] 資料行中，按一下 [ **切換**]。 這會將您 app 的所有使用者切換至藍色按鈕。
5. 按一下 **[確定]** ，確認您想要結束實驗。
6. 執行您在上一節中建立的 **SampleExperiment** app。
7. 確認顯示藍色按鈕。 請注意，app 可能需要兩分鐘時間來接收更新的變異指派。

## <a name="related-topics"></a>相關主題

* [在合作夥伴中心中建立專案並定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的應用程式程式碼](code-your-experiment-in-your-app.md)
* [在合作夥伴中心定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在合作夥伴中心管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試執行應用程式實驗](run-app-experiments-with-a-b-testing.md)
