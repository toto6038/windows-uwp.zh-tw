---
author: mcleanbyron
Description: "在此逐步解說中，您將會使用 A/B 測試建立、執行和管理您的第一個實驗。"
title: "使用 A/B 測試建立和執行您的第一個實驗"
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
translationtype: Human Translation
ms.sourcegitcommit: bfe4862c441ca095a40df4f594fdf9b3e213d142
ms.openlocfilehash: ab15741531b829c496811cdfca35059cd113f91d

---

# 使用 A/B 測試建立和執行您的第一個實驗

在此逐步解說中，您將︰
* 在開發人員中心儀表板上建立實驗[專案](run-app-experiments-with-a-b-testing.md#terms)，定義幾個遠端變數代表 App 按鈕的文字和色彩。
* 建立 App 利用程式碼擷取遠端變數、使用此資料變更按鈕的背景色彩，以及將檢視和轉換事件資料記錄回開發人員中心。
* 在專案中建立實驗，測試變更 App 按鈕的背景色彩是否會成功增加按鈕的點擊數。
* 執行 App 以收集實驗資料。
* 在開發人員中心儀表板上檢閱實驗結果，選擇要為所有 app 使用者啟用的變異，然後完成實驗。

如需使用開發人員中心執行 A/B 測試的概觀，請參閱[使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)。

## 先決條件

為了遵循此逐步解說，您必須具有 Windows 開發人員中心帳戶，且必須按照[使用 A/B 測試執行 App 實驗](run-app-experiments-with-a-b-testing.md)所述，設定開發電腦。

## 在 Windows 開發人員中心建立包含遠端變數的專案

1. 登入[開發人員中心儀表板](https://dev.windows.com/overview)。
2. 若您在開發中心已有想要用來建立實驗的 App，請在您的儀表板中選取該 App。 若您在儀表板中尚無 App，請[保留名稱以建立新 App](../publish/create-your-app-by-reserving-a-name.md)，然後再於儀表板中選取該 App。
3. 在瀏覽窗格中，按一下 [服務]****，然後再按一下 [實驗]****。
4. 在下一頁的 [專案]**** 區段中，按一下 [新專案]**** 按鈕。
5. 在 [新專案]**** 頁面中，為新的專案輸入**按鈕按一下實驗**名稱。
6. 展開 [遠端變數]**** 區段，然後按一下 [加入變數]**** 四次。 您現在應該有四個空的變數列。
  * 在第一列中，輸入 **buttonText** 做為變數名稱，在 [預設值]**** 欄中輸入 **Grey Button**。
  * 在第二列中，輸入 **r** 做為變數名稱，在 [預設值]**** 欄中輸入 **128**。
  * 在第三列中，輸入 **g** 做為變數名稱，在 [預設值]**** 欄中輸入 **128**。
  * 在第四列中，輸入 **b** 做為變數名稱，在 [預設值]**** 欄中輸入 **128**。
7. 按一下 [儲存]****，並且記下 [SDK 整合]**** 區段中出現的[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)值。 在下一節中，您將會更新您的 App 程式碼，並在程式碼中參照這個值。

## 在您的 App 中編寫實驗程式碼

1. 在 Visual Studio 2015 中，使用 Visual C# 建立新的通用 Windows 平台專案。 將專案命名為 **SampleExperiment**。
2. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]****，然後按一下 [加入參考]****。
3. 在 [參考管理員]**** 中，展開 [通用 Windows]****，然後按一下 [擴充功能]****。
4. 在 SDK 清單中，選取 [Microsoft Engagement Framework]**** 旁邊的核取方塊，然後按一下 [確定]****。
5. 在 [方案總管]**** 中，按兩下 MainPage.xaml 以在 App 中開啟主頁面的設計工具。
6. 將 [按鈕] **** 從 [工具箱]**** 拖曳至頁面。
7. 按兩下位於設計工具的按鈕，以開啟程式碼檔案並為**點選**事件新增事件處理常式。  
8. 使用下列程式碼取代整個程式碼檔案內容。 將 ```projectId``` 值指派至您在上一節中從開發人員中心儀表板取得的[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)值。

  ```CSharp
  using System;
  using Windows.UI.Xaml;
  using Windows.UI.Xaml.Controls;
  using Windows.UI.Xaml.Media;
  using System.Threading.Tasks;
  using Windows.UI;
  using Windows.UI.Core;

  // Namespace for A/B testing.
  using Microsoft.Services.Store.Engagement;

  namespace SampleExperiment
  {  
     public sealed partial class MainPage : Page
     {
        private StoreServicesExperimentVariation variation;
        private StoreServicesCustomEventLogger logger;

        // Assign this variable to the project ID for your experiment from Dev Center.
        private string projectId = "";

        public MainPage()
        {
            this.InitializeComponent();

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
#pragma warning disable CS4014
            InitializeExperiment();
#pragma warning restore CS4014
        }

        private async Task InitializeExperiment()
        {
            // Get the current cached variation assignment for the experiment.
            var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
            variation = result.ExperimentVariation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
            {
                result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
                {
                    variation = result.ExperimentVariation;
                }
            }

            // Get remote variables named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the variables default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInt32("r", 128);
            var g = (byte)variation.GetInt32("g", 128);
            var b = (byte)variation.GetInt32("b", 128);

            // Assign button text and color.
            await button.Dispatcher.RunAsync(
                CoreDispatcherPriority.Normal,
                () =>
                {
                    button.Background = new SolidColorBrush(Color.FromArgb(255, r, g, b));
                    button.Content = buttonText;
                    button.Visibility = Visibility.Visible;
                });

            // Log the view event named "userViewedButton" to Dev Center.
            if (logger == null)
            {
                logger = StoreServicesCustomEventLogger.GetDefault();
            }

            logger.LogForVariation(variation, "userViewedButton");
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            if (logger == null)
            {
                logger = StoreServicesCustomEventLogger.GetDefault();
            }

            logger.LogForVariation(variation, "userClickedButton");
        }
     }
  }
  ```
10. 儲存程式碼檔案，並建置專案。

## 在 Windows 開發人員中心建立實驗

1. 回到 Windows 開發人員中心儀表板中的 [按鈕按一下實驗]**** 專案頁面。
2. 在 [實驗]**** 區段中，按一下 [新增實驗]**** 按鈕。
5. 在 [實驗詳細資料]**** 區段的 [實驗名稱]**** 欄位中輸入名稱：**最佳化按鈕點選次數**。
6. 在 [檢視事件]**** 區段的 [檢視事件名稱]**** 欄位中，輸入 **userViewedButton**。 請注意這個名稱符合您在上一節新增的程式碼中記錄的檢視事件字串。
7. 在 [目標和轉換事件]**** 區段中，輸入下列值：
  * 在 [目標名稱]**** 欄位中，輸入 [增加按鈕點選次數]****。
  * 在 [轉換事件名稱]**** 欄位中，輸入名稱：**userClickedButton**。 請注意這個名稱符合您在上一節新增的程式碼中記錄的轉換事件字串。
  * 在 [目標]**** 欄位中，選擇 [最大化]****。
8. 在 [遠端變數和變化]**** 區段中，確認選取 [平均散佈]**** 核取方塊，以平均散佈變化至您的 App。
9. 對您的實驗新增變數︰
  9. 按一下下拉式清單控制項，選擇 [buttonText]****，然後按一下 [加入變數]****。 字串 **Grey Button** 應該會自動出現在 [變化 A]**** 欄中 (此值衍生自專案設定)。 在 [變化 B]**** 欄中，輸入 **Blue Button**。
  9. 再按一下下拉式清單控制項，選擇 [r]****，然後按一下 [加入變數]****。 字串 **128** 應該會自動出現在 [變化 A]**** 欄中。 在 [變化 B]**** 欄中，輸入 **1**。
  9. 再按一下下拉式清單控制項，選擇 [g]****，然後按一下 [加入變數]****。 字串 **128** 應該會自動出現在 [變化 A]**** 欄中。 在 [變化 B]**** 欄中，輸入 **1**。  
  9. 再按一下下拉式清單控制項，選擇 [b]****，然後按一下 [加入變數]****。 字串 **128** 應該會自動出現在 [變化 A]**** 欄中。 在 [變化 B]**** 欄中，輸入 **255**。  
10. 按一下 [儲存]****，然後再按一下 [啟用]****。

> **重要**&nbsp;&nbsp;啟用實驗之後，您就無法再修改實驗參數，除非您在建立實驗時按下 [可編輯的實驗]**** 核取方塊。 我們通常會建議您在啟用實驗之前，先在您的 App 中編寫實驗程式碼。

## 執行 App 以收集實驗資料

1. 執行您稍早建立的 **SampleExperiment** App。
2. 確認您看到灰色或藍色按鈕。 按一下該按鈕，然後關閉 app。
3. 在同部電腦上重複執行數次上述步驟，以確認 app 顯示相同的按鈕色彩。

## 檢閱結果並完成實驗

完成上一節後，請至少等候幾個小時的時間，然後再遵循以下步驟檢閱您實驗的結果並完成實驗。

> **注意**&nbsp;&nbsp;當您啟用實驗時，開發人員中心會立即開始從任何已檢測的 App 收集資料，以記錄您的實驗資料。 不過，可能需要幾個小時的時間，實驗資料才會出現在儀表板中。

1. 在開發人員中心，回到您 App 的 [實驗]**** 頁面。
2. 在 [啟用實驗]**** 區段中，按一下 [最佳化按鈕點選次數]**** 以移至此實驗的頁面。
3. 確認在 [結果摘要]**** 和 [結果詳細資料]**** 區段中顯示的結果符合您的預期。 如需關於這些區段的更多詳細資料，請參閱[在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md#review-the-results-of-your-experiment)。

  >**注意**&nbsp;&nbsp;開發人員中心僅會回報 24 小時內每個使用者的第一個轉換事件。 如果使用者在 24 小時內觸發您 App 中多個轉換事件，則只會回報第一個轉換事件。 這是為了防止具有許多轉換事件的單一使用者扭曲一組範例使用者的實驗結果。
4. 您現已準備好結束實驗。 在 [結果摘要]**** 區段的 [變異 B]**** 欄中，按一下 [切換]****。 這會將您 app 的所有使用者切換至藍色按鈕。
5. 按一下 [確定]****，確認您要結束實驗。
6. 執行您在上一節中建立的 **SampleExperiment** app。
7. 確認顯示藍色按鈕。 請注意，app 可能需要兩分鐘時間來接收更新的變異指派。

## 相關主題

* [在開發人員中心儀表板中建立專案與定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [編寫實驗用的 App 程式碼](code-your-experiment-in-your-app.md)
* [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


