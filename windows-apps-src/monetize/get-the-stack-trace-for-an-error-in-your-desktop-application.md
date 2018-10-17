---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法，以取得傳統型應用程式中錯誤的堆疊追蹤。
title: 取得傳統型應用程式中錯誤的堆疊追蹤
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 堆疊追蹤, 錯誤, 詳細資料, 傳統型應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 60f4ed7251fa934190a96e8c7d6f0edd21520980
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2018
ms.locfileid: "4746302"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>取得傳統型應用程式中錯誤的堆疊追蹤

在 Microsoft Store 分析 API 使用此方法，取得已加入到 [Windows 傳統型應用程式](https://msdn.microsoft.com/library/windows/desktop/mt826504)之傳統型應用程式的錯誤堆疊追蹤。 這個方法只可以下載最近 30 天發生之錯誤的堆疊追蹤。 「Windows 開發人員中心」儀表板中傳統型應用程式的[健康情況報告](https://msdn.microsoft.com/library/windows/desktop/mt826504)也提供堆疊追蹤。

使用這個方法之前，您必須先使用[取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)方法來擷取與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的識別碼雜湊。

## <a name="prerequisites"></a>先決條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的識別碼雜湊。 若要取得此值，請使用[取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)方法，以擷取您的 App 中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabIdHash** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |
 

### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  |
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要取得堆疊追蹤的傳統型應用程式的產品識別碼。 若要取得傳統型應用程式的產品識別碼，請開啟任何[傳統型應用程式的開發人員中心分析報告](https://msdn.microsoft.com/library/windows/desktop/mt826504) (例如**健康報告**)，並從 URL 擷取產品識別碼。 |  是  |
| cabIdHash | string | 與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的唯一識別碼雜湊。 若要取得此值，請使用[取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)方法，以擷取您的應用程式中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabIdHash** 值。 |  是  |

 
### <a name="request-example"></a>要求範例

下列範例示範如何使用此方法取得堆疊追蹤。 將 *applicationId* 和 *cabIdHash* 參數以傳統型應用程式的適當值取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 說明                  |
|------------|---------|--------------------------------|
| 值      | 陣列   | 物件的陣列，每個物件都包含一個堆疊追蹤資料框架。 如需有關每個物件中資料的詳細資訊，請參閱下方的[堆疊追蹤值](#stack-trace-values)一節。 |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。          |


### <a name="stack-trace-values"></a>堆疊追蹤值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述      |
|-----------------|---------|----------------|
| level            | 字串  |  此元素在呼叫堆疊中所代表的框架數目。  |
| image   | 字串  |   可執行檔名稱或程式庫映像，包含在此堆疊框架中呼叫的函式。           |
| function | 字串  |  在此堆疊框架中呼叫的函式名稱。 只有當您的 App 包含可執行檔或程式庫的符號時才能使用。              |
| offset     | 字串  |  相對於函式開始之目前指示的位元組位移。      |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>相關主題

* [健康情況報告](../publish/health-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得傳統型應用程式的錯誤報告資料](get-desktop-application-error-reporting-data.md)
* [取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)
* [下載傳統型應用程式中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-desktop-application.md)
