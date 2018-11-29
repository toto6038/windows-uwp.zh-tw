---
Description: Learn about several ways you can programmatically enable customers to rate and review your app.
title: 為您的應用程式作評等與評論
ms.date: 06/15/2018
ms.topic: article
keywords: Windows 10、uwp、評分、評論
ms.localizationpriority: medium
ms.openlocfilehash: 377b71dba2fb62dfc562b56d40e65e43b0bd49c9
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "7986612"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>為您的應用程式作評等與評論

您可以將程式碼新增到通用 Windows 平台 (UWP) 應用程式，以程式設計方式提示您的客戶對您的應用程式進行評分或評論。 有數種方式可以執行這項作業。
* 您可以直接在應用程式的環境中顯示評等與評論對話方塊。
* 您可以以以程式設計方式在 Microsoft Store 中打開您的應用程式的評等與評論頁面。

當您準備好分析評等與評論資料時，您可以在合作夥伴中心中檢視資料，或使用 「 Microsoft Store 分析 API 以程式設計方式擷取此資料。

> [!IMPORTANT]
> 當您新增您的應用程式內的評等函式，所有評論必須在市集中評分機制，無論/星級評等所選傳送都給使用者。 如果您從使用者收集意見反應或註解，它必須是清除它不相關的應用程式評等或評論存放區中的，但會直接傳送到應用程式開發人員。 請參閱 「 開發人員管理辦法[Fraudulent](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)或誠實活動的相關資訊。

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>在您的應用程式中顯示評等與評論對話方塊

要以程式設計方式顯示應用程式中的對話方塊，要求客戶評分應用程式並提交評論，請在 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中呼叫 [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) 方法。 如此程式碼範例所示，將整數 16 傳送到 *requestKind* 參數，並將空白字串傳送到  *parametersAsJson* 參數。 這個範例需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 程式庫，需要使用**Windows.Services.Store**、**System.Threading.Tasks** 和 **Newtonsoft.Json.Linq** 命名空間的陳述式。

> [!IMPORTANT]
> 必須在您的應用程式 UI 執行緒上呼叫顯示評等與評論對話方塊的要求。

```csharp
public async Task<bool> ShowRatingReviewDialog()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 16, String.Empty);

    if (result.ExtendedError == null)
    {
        JObject jsonObject = JObject.Parse(result.Response);
        if (jsonObject.SelectToken("status").ToString() == "success")
        {
            // The customer rated or reviewed the app.
            return true;
        }
    }

    // There was an error with the request, or the customer chose not to
    // rate or review the app.
    return false;
}
```

**SendRequestAsync** 方法使用簡單整數型要求系統和以 JSON 為基礎的資料參數將其他 Store 作業公開給應用程式。 當您將整數 16 傳送至 *requestKind* 參數時，您將發出要求以顯示評等與評論對話方塊，並將資料傳送至 Microsoft Store。 此方法在 Windows 10 版本 1607 中導入，並且只能在 Visual Studio 中以 **Windows 10 年度更新版 (10.0; 組建 14393)** 或更新版本為目標的專案中使用。 有關此方法的一般概述，請參閱 [傳送要求至 Microsoft Store ](send-requests-to-the-store.md)。

### <a name="response-data-for-the-rating-and-review-request"></a>評等與評論要求的回應資料

提交要求以顯示評等與評論話方塊後，[StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 傳回值的[回應](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) 屬性包含一個 JSON 格式的字串，該字串表示要求是否成功。

以下範例展示在客戶成功提交評分或評論後該要求的傳回值。

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

以下範例展示在客戶選擇提交評分或評論後該要求的傳回值。

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

下表描述 JSON 格式的資料字串中的欄位。

|  欄位  |  描述  |
|----------------------|---------------|
|  *狀態*                   |  表示客戶是否成功提交評等或評論的字串。 支援的值為**成功**和**中止**。   |
|  *資料*                   |  此物件包含名為*已更新*的單一布林值。 此值表示客戶是否更新現有評等或評論。 *資料*物件只包含在成功回應中。   |
|  *errorDetails*                   |  字串包含要求的錯誤詳細資料。 |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>在 Microsoft Store 中啟動您的應用程式的評等與評論頁面

如果您想以程式設計方式打開 Microsoft Store 中應用程式的評等與評論頁面，則可以使用具有 ```ms-windows-store://review```  URI 方案的 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 方法，如本程式碼範例所示。

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

如需詳細資訊，請參閱：[啟動 Microsoft Store 應用程式](../launch-resume/launch-store-app.md)。

## <a name="analyze-your-ratings-and-reviews-data"></a>分析您的評分與評論資料

要分析客戶的評分與評論資料，您有數個選項：
* 您可以使用合作夥伴中心中的 [[評論](../publish/reviews-report.md)] 報告，以查看從您的客戶的評分與評論。 您也可以下載此報告來離線檢視。
* 您可以使用 Microsoft Store 分析 API 中的[取得應用程式評分](get-app-ratings.md) 和 [取得應用程式評論](get-app-reviews.md) 方法以程式設計方式擷取 JSON 格式的客戶的評分與評論。

## <a name="related-topics"></a>相關主題

* [傳送要求至 Microsoft Store ](send-requests-to-the-store.md)
* [啟動 Microsoft Store 應用程式](../launch-resume/launch-store-app.md)
* [評論報告](../publish/reviews-report.md)
