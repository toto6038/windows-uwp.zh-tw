---
author: mcleanbyron
Description: "在開發人員中心儀表板中定義實驗之後，您就已經準備好在 app 中編寫實驗用的程式碼。"
title: "編寫實驗用的 app 程式碼"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: d403e78b775af0f842ba2172295a09e35015dcc8
ms.openlocfilehash: 4e6706624e71c6d448a3d457c27d11c9f6ecc156

---

# 編寫實驗用的 app 程式碼

當您[在開發人員中心儀表板中定義實驗](define-your-experiment-in-the-dev-center-dashboard.md)之後，您就已經準備好更新通用 Windows 平台 (UWP) app 中的程式碼，以取得實驗的變化資料、使用此資料來修改您正在測試的功能行為，以及將檢視事件及轉換事件記錄到開發人員中心。

下列小節描述取得實驗變化和將事件記錄到開發人員中心的一般程序。 如需示範建立及執行實驗之端對端處理程序的逐步解說，請參閱[利用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## 設定您的專案

一開始，在開發電腦上安裝 Microsoft Store Engagement and Monetization SDK，並將必要的參考新增到您的專案。

1. 安裝 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。
2. 在 Visual Studio 中，開啟您的專案。
3. 在 [方案總管] 中，展開您的專案節點、以滑鼠右鍵按一下 [參考]****，然後按一下 [加入參考]****。
3. 在 [參考管理員]**** 中，展開 [通用 Windows]****，然後按一下 [擴充功能]****。
4. 在 SDK 清單中，選取 [Microsoft Store Engagement SDK]**** 旁邊的核取方塊，然後按一下 [確定]****。

## 新增程式碼以取得變化設定

在專案中，找出您想要在實驗中修改之功能的程式碼。 新增要擷取變化設定的程式碼，並使用此資料來修改您要測試之功能的行為。 您所需的特定程式碼取決於您的 app，但以下為一般步驟。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 宣告 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 物件，您將使用此物件來擷取實驗的變化，以及宣告 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 物件來代表目前的變化指派。
```CSharp
private readonly ExperimentClient experiment;
private ExperimentVariation variation;
```

2. 初始化 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 物件，並將您從儀表板的 [**實驗**] 頁面擷取的 API 金鑰傳遞到建構函式。 如需有關 API 金鑰的詳細資訊，請參閱[在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md#generate-an-api-key)。 以下所示的 API 金鑰僅供範例使用。
```CSharp
experiment = new ExperimentClient("F48AC670-4472-4387-AB7D-D65B095153FB");
```

3. 呼叫 [GetVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.getvariationasync.aspx) 方法，以取得目前針對實驗快取的變化指派。 這個方法會傳回 [ExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.aspx) 物件，可透過 [Variation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.variation.aspx) 屬性存取變化指派。
```CSharp
ExperimentVariationResult result = await experiment.GetVariationAsync();
variation = result.Variation;
```

4. 檢查 [NeedsRefresh](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.needsrefresh.aspx) 屬性，以判斷是否需要重新整理快取的變化指派。 如果需要重新整理，可呼叫 [RefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.refreshvariationasync.aspx) 方法，以檢查是否有來自伺服器的更新變化指派，並重新整理本機快取變化。
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
{
      result = await experiment.RefreshVariationAsync();

      // If the call succeeds, use the new result. Otherwise, use the
      // cached value we retrieved earlier.
      if (result.ErrorCode == EngagementErrorCode.Success)
      {
          variation = result.Variation;
      }
}
```

5. 使用 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 物件的 [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getboolean.aspx)、[GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getdouble.aspx)、[GetInteger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getinteger.aspx) 或 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) 方法 ，以取得變化指派的設定。 在每個方法中，第一個參數是您想要擷取的設定名稱 (就像您在開發人員中心儀表板中輸入的)。 第二個參數是預設值，如果方法無法從開發人員中心擷取特定值 (例如，如果沒有網路連線)，而且變化的快取版本無法使用，即應傳回此值。

  下列範例使用 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) 來取得名為 *buttonText* 的設定，並指定 **Grey Button** 的預設設定值。
```CSharp
var buttonText = currentVariation.GetString("buttonText", "Grey Button");
```
4. 在您的程式碼中，使用設定值來修改您要測試之功能的行為。 例如，下列程式碼會將 *buttonText* 值指派到按鈕的內容。
```CSharp
button.Content = buttonText;
```

## 新增程式碼以將檢視和轉換事件記錄到開發人員中心

接下來，新增程式碼，以將檢視和轉換事件記錄到服務開發人員中心的 A/B 測試服務。 您的程式碼應該在使用者開始檢視屬於您實驗一部分的變化時記錄檢視事件，而且應該在使用者到達實驗目標時記錄轉換事件。

您所需的特定程式碼取決於您的 app，但以下為一般步驟。 如需完整的程式碼範例，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

1. 在使用者開始檢視變化時執行的程式碼中，呼叫 [StoreServicesCustomEvents](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.aspx) 物件的靜態 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法 。 傳遞您在開發人員中心儀表板上定義於實驗中的檢視事件名稱，以及代表目前變化指派的 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 物件 (此物件會將事件相關內容提供給開發人員中心)。 下列範例會記錄名為 **userViewedButton** 的檢視事件。
```CSharp
StoreServicesCustomEvents.Log("userViewedButton", variation);
```
2. 在使用者到達其中一個實驗目標的目標時執行的程式碼中，再次呼叫 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法，並傳遞您在實驗中定義的轉換事件名稱和 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 物件。 下列範例會記錄名為 **userClickedButton** 的轉換事件。
```CSharp
StoreServicesCustomEvents.Log("userClickedButton", variation);
```

## 後續步驟

在開發人員儀表板中定義實驗並在 app 中編寫實驗用的程式碼之後，就已經準備好[在開發人員儀表板中執行和管理您的實驗](manage-your-experiment.md)。

## 相關主題

  * [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
  * [在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md)
  * [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [使用 A/B 測試執行 app 實驗](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


