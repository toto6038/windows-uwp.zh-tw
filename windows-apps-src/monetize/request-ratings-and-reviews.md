---
Description: 瞭解幾種可透過程式設計方式讓客戶對您的應用程式評分及進行檢查的方式。
title: 為您的應用程式作評等與評論
ms.date: 01/22/2019
ms.topic: article
keywords: Windows 10、uwp、評分、評論
ms.localizationpriority: medium
ms.openlocfilehash: b167f4cc40ee72e6405436bacee28f2f20b4623c
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210714"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>為您的應用程式作評等與評論

您可以將程式碼新增到通用 Windows 平台 (UWP) 應用程式，以程式設計方式提示您的客戶對您的應用程式進行評分或評論。 有數種方式可以執行這項作業。
* 您可以直接在應用程式的環境中顯示評等與評論對話方塊。
* 您可以以以程式設計方式在 Microsoft Store 中打開您的應用程式的評等與評論頁面。

當您準備好要分析您的評等和評論資料時，您可以在合作夥伴中心查看資料，或使用 Microsoft Store 的分析 API 以程式設計方式抓取此資料。

> [!IMPORTANT]
> 在您的應用程式中新增評等函式時，不論選擇的星級評等為何，所有評論都必須將使用者傳送至商店的評等機制。 如果您向使用者收集意見或意見，必須清楚指出它與商店中的應用程式評等或評論無關，但會直接傳送給應用程式開發人員。 如需[詐騙或 Dishonest 活動](https://docs.microsoft.com/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities)的相關詳細資訊，請參閱開發人員程式碼。

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>顯示您的應用程式中的評分並評論對話方塊

若要以程式設計方式向您的應用程式顯示對話方塊，要求您的客戶對應用程式進行評分並提交評論，請呼叫[RequestRateAndReviewAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync)方法（在[Windows. Store](https://docs.microsoft.com/uwp/api/windows.services.store)命名空間中）。 

> [!IMPORTANT]
> 必須在您的應用程式 UI 執行緒上呼叫顯示評等與評論對話方塊的要求。

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

**RequestRateAndReviewAppAsync**方法是在 windows 10 版本1809中引進，而且只能用於以**Windows 10 2018 年10月更新（10.0;）為目標的專案。組建17763）** 或 Visual Studio 的更新版本。

### <a name="response-data-for-the-rating-and-review-request"></a>評等與評論要求的回應資料

提交要求以顯示 [評等] 和 [評論] 對話方塊之後， [StoreRateAndReviewResult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult)類別的[ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata)屬性會包含 JSON 格式的字串，指出要求是否成功。

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

| 欄位          | 描述                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *狀態*       | 表示客戶是否成功提交評等或評論的字串。 支援的值為**成功**和**中止**。 |
| *data*         | 此物件包含名為*已更新*的單一布林值。 此值表示客戶是否更新現有評等或評論。 *資料*物件只包含在成功回應中。 |
| *errorDetails* | 字串包含要求的錯誤詳細資料。                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>在 Microsoft Store 中啟動您的應用程式的評等與評論頁面

如果您想以程式設計方式打開 Microsoft Store 中應用程式的評等與評論頁面，則可以使用具有 [  URI 方案的 ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)LaunchUriAsync```ms-windows-store://review``` 方法，如本程式碼範例所示。

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

如需詳細資訊，請參閱：[啟動 Microsoft Store 應用程式](../launch-resume/launch-store-app.md)。

## <a name="analyze-your-ratings-and-reviews-data"></a>分析您的評分與評論資料

要分析客戶的評分與評論資料，您有數個選項：
* 您可以使用合作夥伴中心的[評論](../publish/reviews-report.md)報告來查看客戶的評等和評論。 您也可以下載此報告來離線檢視。
* 您可以使用 Microsoft Store 分析 API 中的[取得應用程式評分](get-app-ratings.md) 和 [取得應用程式評論](get-app-reviews.md) 方法以程式設計方式擷取 JSON 格式的客戶的評分與評論。

## <a name="related-topics"></a>相關主題

* [將要求傳送至存放區](send-requests-to-the-store.md)
* [啟動 Microsoft Store 應用程式](../launch-resume/launch-store-app.md)
* [評論報告](../publish/reviews-report.md)
