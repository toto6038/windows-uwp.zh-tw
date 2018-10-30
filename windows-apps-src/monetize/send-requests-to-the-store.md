---
author: Xansky
Description: You can use the SendRequestAsync method to send requests to the Microsoft Store for operations that do not yet have an API available in the Windows SDK.
title: 傳送要求至 Microsoft Store
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, StoreRequestHelper, SendRequestAsync
ms.localizationpriority: medium
ms.openlocfilehash: 71247b8e04e63e5f792a872256dd79447c4d36cd
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5760784"
---
# <a name="send-requests-to-the-microsoft-store"></a>傳送要求至 Microsoft Store

從 Windows 10 版本 1607 開始，Windows SDK 在 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中針對 Microsoft Store 相關作業 (例如 App 內購買) 提供 API。 不過，雖然支援 Microsoft Store 的服務會在作業系統發行之間經常更新、擴展和改良，新 API 通常只在主要作業系統發行期間加入 Windows SDK。

我們提供 [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) 方法，以方便您在新版的 Windows SDK 發行之前，提供新的 Microsoft Store 作業給通用 Windows 平台 (UWP) 應用程式使用。 您可以使用此方法，針對 Windows SDK 最新版本中沒有對應 API 的新作業，將要求傳送給 Microsoft Store。

> [!NOTE]
> **SendRequestAsync** 方法僅適用於目標為 Windows 10 版本 1607 或更新版本的 app。 這個方法支援的一些要求，只在 Windows 10 版本 1607 之後版本中受支援。

**SendRequestAsync** 是 [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper) 類別的靜態方法。 若要呼叫這個方法，您必須將下列資訊傳遞給此方法：
* [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 物件，提供您想要為其執行作業的使用者相關資訊。 如需有關這個物件的詳細資訊，請參閱[開始使用 StoreContext 類別](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class)。
* 整數，辨識您想要傳送給 Microsoft Store 的要求。
* 如果要求支援任何引數，您也可以傳遞 JSON 格式化字串，其中包含與要求一起傳遞的引數。

下列範例示範如何呼叫這個方法。 這個範例需要 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空間的 using 陳述式。

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

請參閱下列章節以取得目前可透過 **SendRequestAsync** 方法使用之要求的詳細資訊。 新要求的支援新增時，我們將會更新本文。

## <a name="request-for-in-app-ratings-and-reviews"></a>要求應用程式內評分與評論

您能以程式設計方式從您的應用程式啟動對話方塊，其會要求您客戶針對您的應用程式做評分和評論，方法是將要求整數 16 傳送到 **SendRequestAsync** 方法。 如需詳細資訊，請參閱[顯示您的應用程式中的評分並評論對話方塊](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)。

## <a name="requests-for-flight-group-scenarios"></a>正式發行前小眾測試版群組案例的要求

> [!IMPORTANT]
> 本節所描述的所有正式發行前小眾測試版群組要求目前並未提供給大部分開發人員帳戶使用。 除非您的開發人員帳戶是由 Microsoft 特別佈建，否則這些要求將會失敗。

**SendRequestAsync** 方法支援正式發行前小眾測試版群組案例的一組要求，例如將使用者或裝置新增至正式發行前小眾測試版群組。 若要提交這些要求，傳送值 7 或 8 至 *requestKind* 參數，並將 JSON 格式化字串傳送至 *parametersAsJson* 參數，表示您想要與任何相關引數一起提交的要求。 這些 *requestKind* 值在下列方面不同。

|  要求類型值  |  描述  |
|----------------------|---------------|
|  7                   |  要求是在目前裝置的內容中執行。 這個值僅能在 Windows 10 版本 1703 或更新版本中使用。  |
|  8                   |  要求是在目前已登入 Microsoft Store 之使用者的內容中執行。 這個值可以在 Windows 10 版本 1607 或更新版本中使用。  |

目前實作下列正式發行前小眾測試版群組要求。

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>擷取最高排名之正式發行前小眾測試版群組的遠端變數

> [!IMPORTANT]
> 此要求目前並未提供給大部分開發人員帳戶使用。 除非您的開發人員帳戶是由 Microsoft 特別佈建，否則這個要求將會失敗。

此要求擷取目前使用者或裝置所屬最高排名之正式發行前小眾測試版群組的遠端變數。 若要傳送這個要求，傳遞下列資訊到 **SendRequestAsync** 方法的 *requestKind* 和 *parametersAsJson* 參數。

|  參數  |  描述  |
|----------------------|---------------|
|  *requestKind*                   |  指定 7，傳回裝置所屬最高排名之正式發行前小眾測試版群組，或指定 8 傳回目前使用者與裝置所屬最高排名之正式發行前小眾測試版群組。 我們建議 *requestKind* 參數使用值 8，因為這個值會跨目前使用者及裝置的成員資格，傳回最高排名之正式發行前小眾測試版群組。  |
|  *parametersAsJson*                   |  傳遞 JSON 格式化字串，包含下列範例中顯示的資料。  |

下列範例示範傳送至 *parametersAsJson* 的 JSON 資料格式。 *type* 欄位必須指派給字串 *GetRemoteVariables*。 將 *projectId* 欄位指派到專案識別碼，您在 Windows 開發人員中心儀表板中於該專案中定義遠端變數。

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

您提交此要求後，[StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 傳回值的 [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.Response) 屬性包含有下列欄位的 JSON 格式化字串。

|  欄位  |  描述  |
|----------------------|---------------|
|  *anonymous*                   |  布林值，**true** 表示使用者或裝置身分識別未出現在要求中，**false** 表示使用者或裝置身分識別出現在要求中。  |
|  *name*                   |  字串，包含裝置或使用者所屬最高排名之正式發行前小眾測試版群組的名稱。  |
|  *settings*                   |  索引鍵/值組的字典，包含開發人員為正式發行前小眾測試版群組設定的遠端變數的名稱及值。  |

以下範例示範此要求的傳回值。

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>將目前的裝置或使用者加入正式發行前小眾測試版群組

> [!IMPORTANT]
> 此要求目前並未提供給大部分開發人員帳戶使用。 除非您的開發人員帳戶是由 Microsoft 特別佈建，否則這個要求將會失敗。

若要傳送這個要求，傳遞下列資訊到 **SendRequestAsync** 方法的 *requestKind* 和 *parametersAsJson* 參數。

|  參數  |  描述  |
|----------------------|---------------|
|  *requestKind*                   |  指定 7 將裝置新增到正式發行前小眾測試版群組中，或指定 8 將目前已登入 Microsoft Store 之使用者新增到正式發行前小眾測試版群組中。  |
|  *parametersAsJson*                   |  傳遞 JSON 格式化字串，包含下列範例中顯示的資料。  |

下列範例示範傳送至 *parametersAsJson* 的 JSON 資料格式。 *type* 欄位必須指派給字串 *AddToFlightGroup*。 將 *flightGroupId* 欄位指派到正式發行前小眾測試版群組識別碼，您想要在此群組中新增裝置或使用者。

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

如果要求發生錯誤，[StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 傳回值的 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 屬性包含回應碼。

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>從正式發行前小眾測試版群組移除目前裝置或使用者

> [!IMPORTANT]
> 此要求目前並未提供給大部分開發人員帳戶使用。 除非您的開發人員帳戶是由 Microsoft 特別佈建，否則這個要求將會失敗。

若要傳送這個要求，傳遞下列資訊到 **SendRequestAsync** 方法的 *requestKind* 和 *parametersAsJson* 參數。

|  參數  |  描述  |
|----------------------|---------------|
|  *requestKind*                   |  指定 7 從正式發行前小眾測試版群組中移除裝置，或指定 8 從正式發行前小眾測試版群組中移除目前已登入 Microsoft Store 之使用者。  |
|  *parametersAsJson*                   |  傳遞 JSON 格式化字串，包含下列範例中顯示的資料。  |

下列範例示範傳送至 *parametersAsJson* 的 JSON 資料格式。 *type* 欄位必須指派給字串 *RemoveFromFlightGroup*。 將 *flightGroupId* 欄位指派到正式發行前小眾測試版群組識別碼，您想要從此群組中移除裝置或使用者。

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

如果要求發生錯誤，[StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) 傳回值的 [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) 屬性包含回應碼。

## <a name="related-topics"></a>相關主題

* [顯示您的應用程式中的評分並評論對話方塊](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [SendRequestAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)
