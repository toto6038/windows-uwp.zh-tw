---
Description: 在此逐步解說中，您將會使用 A/B 測試建立和執行您的第一個實驗。
title: 使用 A/B 測試建立和執行您的第一個實驗
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
---

# 使用 A/B 測試建立和執行您的第一個實驗

在此逐步解說中，您將會執行以下動作 ︰
* 在 Windows 開發人員中心儀表板上建立實驗，以測試變更 app 按鈕的背景顏色是否會成功增加按鈕的點擊數。
* 建立 app 以從開發人員中心擷取變異設定、使用此資料變更按鈕的背景色彩，以及將檢視和轉換事件資料記錄至開發人員中心。
* 執行 app 以收集實驗資料。
* 在開發人員中心儀表板上檢閱實驗結果，選擇要為所有 app 使用者啟用的變異，然後完成實驗。

如需使用開發人員中心執行 A/B 測試的概觀，請參閱[使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)。

## 先決條件

為了遵循此逐步解說，您必須具有 Windows 開發人員中心帳戶，且必須按照[使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)所述，設定開發電腦。

## 在 Windows 開發人員中心建立實驗

1. 登入[開發人員中心儀表板](https://dev.windows.com/overview)。
2. 若您在開發中心已有想要用來建立實驗的 app，請在 [您的應用程式]**** 下方，選取 app。 若您在儀表板中尚無 app，請[保留名稱以建立新應用程式](../publish/create-your-app-by-reserving-a-name.md)，然後再於儀表板中選取該 app。
3. 在瀏覽窗格中，按一下 [服務]****，然後再按一下 [實驗]****。
4. 在 [API 金鑰]**** 區段中，選取 [新 API 金鑰]**** 以產生新的 API 金鑰，然後再針對 API 金鑰輸入名稱：**我的第一個實驗**。 您將在此逐步解說的下一個區段中，使用此 API 金鑰。
5. 在 [實驗]**** 區段中，按一下 [新增實驗]****。 在 [實驗名稱]**** 欄位中，輸入名稱：**最佳化按鈕點選次數**。
6. 在 [檢視事件名稱]**** 欄位中，輸入名稱：**userViewedButton**。 在本逐步解說的後面內容中，您將會新增程式碼，以在初始化 app 主頁面且使用者會看見按鈕時，記錄此檢視事件。
7. 在 [目標和轉換事件]**** 區段中，輸入下列值：
  * 在 [目標名稱]**** 欄位中，輸入 [增加按鈕點選次數]****。
  * 在 [轉換事件名稱]**** 欄位中，輸入名稱：**userClickedButton**。 在本逐步解說的後面內容中，您將新增程式碼，以在按鈕的點選事件處理常式中記錄此轉換事件。
  * 在 [Objective]**** 欄位中，選擇 [最大化]****。
8. 在 [變異和設定]**** 區段中，按三次 [新增設定]****。 您現在應有四列空白設定。
  * 在第一列中，針對設定名稱輸入 **buttonText**，在 [變異 A]**** 欄中輸入 **Grey Button**，然後在 [變異 B]**** 欄中輸入 **Blue Button**。
  * 在第二列中，針對設定名稱輸入 **r**，在 [變異 A]**** 欄中輸入 **128**，然後在 [變異 B]**** 欄中輸入 **1**。
  * 在第三列中，針對設定名稱輸入 **g**，在 [變異 A]**** 欄中輸入 **128**，然後在 [變異 B]**** 欄中輸入 **1**。
  * 在第四列中，針對設定名稱輸入 **b**，在 [變異 A]**** 欄中輸入 **128**，然後在 [變異 B]**** 欄中輸入 **255**。  
9. 確認選取 [平均散佈]**** 核取方塊，以平均散佈變異至您的 app。
10. 按一下 [儲存]****，然後再按一下 [啟用]****。

> **重要** 啟用實驗之後，您可以不用再修改實驗參數，除非這是測試實驗 (您在建立實驗時按下 [測試實驗]**** 核取方塊)。 通常我們建議您在啟用實驗之前，先在您的 app 中編寫實驗用的程式碼。 為了簡要起見，您現可在本逐步解說中啟用實驗。

## 在您的 app 中編寫實驗程式碼

1. 在 Visual Studio 2015 中，使用 Visual C# 建立新的通用 Windows 平台專案。 將專案命名為 **SampleExperiment**。
2. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]****，然後按一下 [加入參考]****。
3. 在 [參考管理員]**** 中，展開 [通用 Windows]****，然後按一下 [擴充功能]****。
4. 在 SDK 清單中，選取 [Microsoft Store Engagement SDK]**** 旁邊的核取方塊，然後按一下 [確定]****。
5. 在 [方案總管]**** 中，按兩下 MainPage.xaml 以在 app 中開啟主頁面的設計工具。
6. 將 [按鈕] **** 從 [工具箱]**** 拖曳至頁面。
7. 按兩下位於設計工具的按鈕，以開啟程式碼檔案並為**點選**事件新增事件處理常式。  
8. 使用下列程式碼取代整個程式碼檔案內容。

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
        private readonly ExperimentClient experiment;
        private ExperimentVariation variation;

        // Assign this variable to your API key from Dev Center. The API key
        // shown below is for example purposes only.
        private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";    

        public MainPage()
        {
            this.InitializeComponent();

            // Initialize the ExperimentClient for A/B testing.
            experiment = new ExperimentClient(apiKey);

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
            #pragma warning disable CS4014
            Initialize();
            #pragma warning restore CS4014
        }

        private async Task Initialize()
        {
            // Get the current cached variation assignment for the experiment.
            ExperimentVariationResult result = await experiment.GetVariationAsync();
            variation = result.Variation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
            {
                result = await experiment.RefreshVariationAsync();

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == EngagementErrorCode.Success)
                {
                    variation = result.Variation;
                }
            }

            // Get settings named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the settings default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInteger("r", 128);
            var g = (byte)variation.GetInteger("g", 128);
            var b = (byte)variation.GetInteger("b", 128);

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
            StoreServicesCustomEvents.Log("userViewedButton", variation);
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userClickedButton", variation);
        }
     }
  }
  ```
9. 在下列程式碼行，將 *apiKey* 變數指派至您在上一節中透過開發人員中心儀表板取得的 API 金鑰。 以下所示的 API 金鑰僅供範例使用。
```CSharp
private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";
```
10. 儲存程式碼檔案，並建置專案。

## 執行應用程式以收集實驗資料

1. 執行您在上一節中建立的 **SampleExperiment** app。
2. 確認顯示灰色或藍色按鈕。 按一下該按鈕，然後關閉 app。
3. 在同部電腦上重複執行數次上述步驟，以確認 app 顯示相同的按鈕色彩。

## 檢閱結果並完成實驗

完成上一節後，請等待至少數小時，然後再遵循以下步驟檢閱您實驗的結果並完成實驗。

> **注意**：當您啟用實驗時，開發人員中心會立即開始從任何已檢測的應用程式收集資料，以記錄您的實驗資料。 不過，可能需要幾個小時的時間，實驗資料才會出現在儀表板中。

1. 在開發人員中心，回到您應用程式的 [實驗]****頁面。
2. 在 [實驗]**** 區段中，按一下 [使用中]**** 篩選工具，然後再按一下 [最佳化按鈕點選次數]**** 以移至此實驗的頁面。
3. 確認在 [結果摘要]**** 和 [結果詳細資料]**** 區段中，顯示符合您預期會看到的結果。 如需關於這些章節的更多詳細資料，請參閱[在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md#review-the-results-of-your-experiment)。

  >**注意**：開發人員中心僅會回報 24 小時內每個使用者的第一個轉換事件。 如果使用者在 24 小時內觸發您的應用程式中的多個轉換事件，則只會回報第一個轉換事件。 這是為了防止具有許多轉換事件的單一使用者扭曲一組範例使用者的實驗結果。

4. 您現已準備好結束實驗。 在 [結果摘要]**** 區段的 [變異 B]**** 欄中，按一下 [切換]****。 這會將您 app 的所有使用者切換至藍色按鈕。
5. 按一下 [確定]****，確認您要結束實驗。
6. 執行您在上一節中建立的 **SampleExperiment** app。
7. 確認顯示藍色按鈕。 請注意，app 可能需要兩分鐘時間來接收更新的變異指派。

## 相關主題

  * [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
  * [編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)
  * [在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md)
  * [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


