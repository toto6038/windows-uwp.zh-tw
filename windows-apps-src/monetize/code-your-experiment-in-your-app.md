---
author: mcleanbyron
Description: "若要在通用 Windows 平台 (UWP) app 中運用 A/B 測試執行實驗，您必須在 App 中編寫實驗的程式碼。"
title: "編寫實驗用的 App 程式碼"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: cc32e2688bce636e1f4bda02aade4ed1d94f3e28

---

# <a name="code-your-app-for-experimentation"></a>編寫實驗用的 App 程式碼

在您[在開發人員中心儀表板中建立專案並定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)之後，就可以準備更新通用 Windows 平台 (UWP) app 中的程式碼，以：
* 從 Windows 開發人員中心收到遠端變數值。
* 使用遠端變數為您的使用者設定 App 體驗。
* 將指出使用者何時檢視實驗並執行所需動作 (也稱為「轉換」) 的事件記錄至「開發人員中心」。

若要將此行為新增到您的 App 中，您將使用 Microsoft Store Services SDK 所提供的 API。

下列小節描述取得實驗變化及將事件記錄至「開發人員中心」的一般程序。 編寫實驗用的 App 程式碼之後，就可以[在開發人員中心儀表板中定義實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 如需示範建立及執行實驗的端對端程序的逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

>**注意**  Windows Store Services SDK 中的部分實驗 API 使用[非同步模式](../threading-async/asynchronous-programming-universal-windows-platform-apps.md)從「開發人員中心」擷取資料。 這意謂著這些方法的部分執行可能在方法被呼叫後才發生，因此您 App 的 UI 可以在作業完成時持續回應。 非同步模式會要求您的 App 在呼叫 API 時使用 **async** 關鍵字和 **await** 運算子，如本文的程式碼範例所示範。 根據慣例，非同步方法會以 **Async** 作為結尾。

## <a name="configure-your-project"></a>設定您的專案

若要開始著手，請在您的開發電腦上安裝 Microsoft Store Services SDK，然後將必要的參考新增到您的專案中。

1. [安裝 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。
2. 在 Visual Studio 中，開啟您的專案。
3. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]，然後按一下 [加入參考]。
3. 在 [參考管理員] 中，展開 [通用 Windows]，然後按一下 [擴充功能]。
4. 在 SDK 清單中，選取 [Microsoft Engagement Framework] 旁邊的核取方塊，然後按一下 [確定]。

>**注意**  本文中的程式碼範例假設您的程式碼檔案有 **System.Threading.Tasks** 和 **Microsoft.Services.Store.Engagement** 命名空間的 **using** 陳述式。

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>取得變化資料並記錄實驗的檢視事件

在專案中，找出您想要在實驗中修改之功能的程式碼。 新增會擷取變化資料的程式碼、使用此資料來修改您要測試之功能的行為，然後將您實驗的檢視事件記錄至「開發人員中心」中的 A/B 測試服務。

您所需的特定程式碼將取決於您的 App，但以下範例將示範基本程序。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

> [!div class="tabbedCodeSnippets"]
[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

下列步驟詳細說明此程序的重要部分。

1. 宣告一個代表目前變化指派的 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件，以及一個您將用來把檢視和轉換事件記錄至「開發人員中心」的 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 物件。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

1. 宣告一個指派給您所要擷取的實驗之[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)的字串變數。
  >**注意**  當您[在開發人員中心儀表板中建立專案](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)時，會取得一個專案識別碼。 以下所示的專案識別碼僅供範例用途使用。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

2. 呼叫靜態的 [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) 方法來取得實驗目前快取的變化指派，然後將實驗的專案識別碼傳遞給此方法。 這個方法會傳回 [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) 物件，此物件會透過 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx) 屬性提供對變化指派的存取權。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. 檢查 [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) 屬性，以判斷是否需要使用來自伺服器的遠端變化指派來重新整理快取的變化指派。 如果需要重新整理，請呼叫靜態的 [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) 方法，以檢查是否有來自伺服器的更新變化指派，並重新整理本機快取變化。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. 使用 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件的 [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx)、[GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx)、[GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx) 或 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) 方法，取得變化指派的值。 在每個方法中，第一個參數是您想要擷取的變化名稱 (與您在開發人員中心儀表板中輸入的變化名稱相同)。 第二個參數是如果方法無法從開發人員中心擷取指定值 (例如，如果沒有網路連線)，而且變化的快取版本無法使用，方法應傳回的預設值。

  下列範例使用 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) 取得名為 *buttonText* 的變數，並指定 **Grey Button** 的預設變數值。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

4. 在您的程式碼中，使用變數值來修改您要測試之功能的行為。 例如，下列程式碼會將 *buttonText* 值指派給您 App 中某個按鈕的內容。 此範例假設您已經在專案中的其他地方定義這個按鈕。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

5. 最後，將實驗的[檢視事件](run-app-experiments-with-a-b-testing.md#terms)記錄至「開發人員中心」的 A/B 測試服務。 請將 ```logger``` 欄位初始化為 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 物件，並呼叫 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法。 傳遞代表目前變化指派的 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件 (此物件會將事件的相關內容提供給「開發人員中心」) 以及實驗的檢視事件名稱。 這必須符合您在「開發人員中心」儀表板中為實驗輸入的檢視事件名稱。 您的程式碼應該在使用者開始檢視屬於實驗一部分的變化時記錄檢視事件。

  下列範例說明如何記錄名為 **userViewedButton** 的檢視事件。 在此範例中，實驗的目標是讓使用者按一下 App 中的按鈕，以便在 App 擷取到變化資料 (在此案例中為按鈕文字) 之後記錄檢視事件，然後將它指派給按鈕的內容。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-dev-center"></a>將轉換事件記錄至開發人員中心

接下來，新增會將[轉換事件](run-app-experiments-with-a-b-testing.md#terms)記錄至「開發人員中心」中 A/B 測試服務的程式碼。 您的程式碼應該在使用者達到實驗目標時記錄轉換事件。 您所需的特定程式碼將取決於您的 App，但以下是一般步驟。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 在當使用者達到其中一個實驗目標的目標時會執行的程式碼中，再次呼叫 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法，並傳遞 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件以及實驗的轉換事件名稱。 這必須符合您在「開發人員中心」儀表板中為實驗輸入的其中一個轉換事件名稱。

  下列範例會記錄來自按鈕之 **Click** 事件處理常式且名為 **userClickedButton** 的轉換事件。 在此範例中，實驗的目標是讓使用者按一下按鈕。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>後續步驟

在您的 App 中編寫實驗程式碼之後，您就準備好進行下列步驟︰
1. [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 建立定義檢視事件、轉換事件以及 A/B 測試唯一變化的實驗。
2. [在開發人員中心儀表板中執行和管理您的實驗](manage-your-experiment.md)。


## <a name="related-topics"></a>相關主題

* [在開發人員中心儀表板中建立專案與定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Dec16_HO1-->


