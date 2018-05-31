---
author: mcleanbyron
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: 在 Microsoft Store 中使用此方法來判斷您是否能回應特定評論，或是否可以回應特定應用程式的所有評論。
title: 取得評論的回應資訊
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 評論 API, 回應資訊
ms.localizationpriority: medium
ms.openlocfilehash: 4cc3bae99aebaf26074ba4f8b8a38e1a6e0ac428
ms.sourcegitcommit: cceaf2206ec53a3e9155f97f44e4795a7b6a1d78
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2018
ms.locfileid: "1701084"
---
# <a name="get-response-info-for-reviews"></a>取得評論的回應資訊

如果您想要以程式設計方式回應客戶對您的應用程式的評論，可以在 Microsoft Store 評論 API 中使用此方法先判斷自己是否有權限可回應評論。 您無法回應已選擇不要接收評論回應的客戶所提交的評論。 在您確認可回應評論後，以程式設計方式使用[提交應用程式評論的回應](submit-responses-to-app-reviews.md)方法回應。


## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](respond-to-reviews-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得您想要查看的評論的識別碼，判斷您是否可回應。 評論識別碼是在 Microsoft Store 分析 API [取得 app 評論](get-app-reviews.md)方法的回應資料中，以及[評論報告](../publish/reviews-report.md)的[離線下載](../publish/download-analytic-reports.md)中。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   | 描述                                     |  必要  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | 字串 | 包含您想知道是否可回應之評論的應用程式 Store 識別碼。  Store 識別碼可在開發人員中心儀表板的[應用程式身分識別頁面](../publish/view-app-identity-details.md)取得。 舉例來說，Store 識別碼可以是「9WZDNCRFJ3Q8」。 |  是  |
| reviewId | 字串 | 您想要回應評論的識別碼 (這是 GUID)。 評論識別碼是在 Microsoft Store 分析 API [取得 app 評論](get-app-reviews.md)方法的回應資料中，以及[評論報告](../publish/reviews-report.md)的[離線下載](../publish/download-analytic-reports.md)中。 <br/>如果您省略此參數，這個方法的回應本文將會指示您是否有權限可回應指定應用程式的任何評論。 |  否  |


### <a name="request-example"></a>要求的範例

下列範例說明如何使用此方法判斷您是否可以回應特定評論。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述    |  
|------------|--------|-----------------------|
| CanRespond      | 布林值  | 值為 **true**，表示您可以回應指定的評論，或是您有權限可回應指定應用程式的任何評論。 否則，這個值是 **false**。       |
| DefaultSupportEmail  | 字串 |  您的應用程式的[支援電子郵件地址](../publish/enter-app-properties.md#support-contact-info)，如同您的應用程式 Store 清單中所指定。 如果您未指定支援電子郵件地址，這個欄位是空的。    |

 
### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 分析 API 提交評論的回應](submit-responses-to-app-reviews.md)
* [使用開發人員中心儀表板回應客戶評論](../publish/respond-to-customer-reviews.md)
* [使用 Microsoft Store 服務回應評論](respond-to-reviews-using-windows-store-services.md)
* [取得應用程式評論](get-app-reviews.md)
