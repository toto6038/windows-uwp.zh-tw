---
author: mcleanbyron
Description: "若要在通用 Windows 平台 (UWP) app 中運用 A/B 測試執行實驗，您必須在 App 中編寫實驗的程式碼。"
title: "編寫實驗用的 App 程式碼"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# 編寫實驗用的 App 程式碼

在您[在開發人員中心儀表板中建立專案並定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)之後，就可以準備更新通用 Windows 平台 (UWP) app 中的程式碼，以：
* 從 Windows 開發人員中心收到遠端變數值。
* 使用遠端變數為您的使用者設定 App 體驗。
* 將指出使用者何時檢視實驗並執行所需動作 (也稱為「轉換」**) 的事件記錄到開發人員中心。

下列小節描述取得實驗變化和將事件記錄到開發人員中心的一般程序。 編寫實驗用的 App 程式碼之後，就可以[在開發人員中心儀表板中定義實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 如需示範建立及執行實驗的端對端處理程序的逐步解說，請參閱[利用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## 設定您的專案

一開始，在開發電腦上安裝 Microsoft Store Services SDK，並將必要的參考新增到您的專案。

1. 安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。
2. 在 Visual Studio 中，開啟您的專案。
3. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]****，然後按一下 [加入參考]****。
3. 在 [參考管理員]**** 中，展開 [通用 Windows]****，然後按一下 [擴充功能]****。
4. 在 SDK 清單中，選取 [Microsoft Engagement Framework]**** 旁邊的核取方塊，然後按一下 [確定]****。

## 新增程式碼以取得變化資料

在專案中，找出您想要在實驗中修改之功能的程式碼。 新增要擷取變化資料的程式碼，並使用此資料來修改您要測試之功能的行為。 您所需的特定程式碼取決於您的 App，但以下為一般步驟。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 宣告一個代表目前變化指派的 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件，以及一個您用來將檢視和轉換事件記錄到開發人員中心的 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 物件。
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. 呼叫靜態的 [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) 方法取得實驗目前快取的變化指派，然後傳遞實驗的[專案識別碼](run-app-experiments-with-a-b-testing.md#terms)到此方法。 這個方法會傳回 [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) 物件，透過 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx) 屬性存取變化指派。
  >**注意**&nbsp;&nbsp;當您[在開發人員中心儀表板中建立專案](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)時會取得專案識別碼。 以下所示的專案識別碼僅供範例使用。

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. 檢查 [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) 屬性，以判斷是否需要使用來自伺服器的遠端變化指派重新整理快取的變化指派。 如果需要重新整理，請呼叫靜態的 [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) 方法，以檢查是否有來自伺服器的更新變化指派，並重新整理本機快取變化。
```CSharp
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
```

5. 使用 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件的 [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx)、[GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx)、[GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx) 或 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) 方法，取得變化指派的值。 在每個方法中，第一個參數是您想要擷取的變化名稱 (與您在開發人員中心儀表板中輸入的變化名稱相同)。 第二個參數是如果方法無法從開發人員中心擷取指定值 (例如，如果沒有網路連線)，而且變化的快取版本無法使用，方法應傳回的預設值。

  下列範例使用 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) 取得名為 *buttonText* 的變數，並指定 **Grey Button** 的預設變數值。
```CSharp
var buttonText = variation.GetString("buttonText", "Grey Button");
```
4. 在您的程式碼中，使用變數值來修改您要測試之功能的行為。 例如，下列程式碼會將 *buttonText* 值指派到按鈕的內容。
```CSharp
button.Content = buttonText;
```

## 新增程式碼以將檢視和轉換事件記錄到開發人員中心

接下來，新增程式碼以將[檢視事件](run-app-experiments-with-a-b-testing.md#terms)和[轉換事件](run-app-experiments-with-a-b-testing.md#terms)記錄到開發人員中心的 A/B 測試服務。 您的程式碼應該在使用者開始檢視屬於您實驗一部分的變化時記錄檢視事件，而且應該在使用者達到實驗目標時記錄轉換事件。

您所需的特定程式碼取決於您的 app，但以下為一般步驟。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 在使用者開始檢視變化時執行的程式碼中，請將 ```logger``` 欄位初始化為 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 物件並呼叫 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法。 傳遞代表目前變化指派的 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件 (此物件將事件相關內容提供給開發人員中心) 以及檢視事件的名稱 (與您在開發人員中心儀表板中輸入的檢視事件名稱相同)。 下列範例會記錄名為 **userViewedButton** 的檢視事件。

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. 在使用者達到其中一個實驗目標的目標時執行的程式碼中，再次呼叫 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 方法，並傳遞 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 物件和轉換事件的名稱 (與您在開發人員中心儀表板中輸入的轉換事件名稱相同)。 下列範例會記錄名為 **userClickedButton** 的轉換事件。
```CSharp
logger.LogForVariation(variation, "userClickedButton");
```

## 後續步驟

在您的 App 中編寫實驗程式碼之後，您就準備好進行下列步驟︰
1. [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 建立定義檢視事件、轉換事件以及 A/B 測試唯一變化的實驗。
2. [在開發人員中心儀表板中執行和管理您的實驗](manage-your-experiment.md)。


## 相關主題

* [在開發人員中心儀表板中建立專案與定義遠端變數](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


