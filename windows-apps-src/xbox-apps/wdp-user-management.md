---
title: Xbox Live 測試使用者管理 API 參考
description: 了解如何以程式設計方式存取使用者管理 API。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 52f333af73084ed14982b9d09b6770c8294980f7
ms.sourcegitcommit: 6169660ea437915265165c4631d9702587e4793d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74902531"
---
# <a name="xbox-live-user-management"></a>Xbox Live 使用者管理

## <a name="request"></a>要求

您可以取得主機上的使用者清單，或是透過新增、移除、登入、登出或修改現有使用者來更新清單。

| 方法        | 要求 URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |


**URI 參數**

* 無

**要求標頭**

* 無

**要求本文**

針對 PUT 的呼叫應該包含具有下列結構的 JSON 陣列：

* 使用者
  * AutoSignIn (選擇性)：停用或啟用以 EmailAddress 或 UserId 所指定帳戶自動登入的布林值。
  * EmailAddress (選擇性 - 如果未提供 UserId 則必須提供，登入贊助使用者的情況除外)：指定要修改/新增/刪除使用者的電子郵件地址。
  * Password (選擇性 - 如果使用者目前不在主機上則必須提供)：用來將新使用者新增到主機的密碼。
  * SignedIn (選擇性)：指定所提供帳戶是否應登入或登出的布林值。
  * UserId (選擇性 - 如果未提供 EmailAddress 則必須提供，登入贊助使用者的情況除外)：指定要修改/新增/刪除使用者的使用者識別碼。
  * SponsoredUser (選擇性)：指定是否要新增贊助使用者的布林值。
  * Delete （選擇性）：指定要從主控台刪除此使用者的 bool

## <a name="response"></a>回應

**回應主體**

針對 GET 的呼叫會傳回具有下列屬性的 JSON 陣列：

* 使用者
  * AutoSignIn (選擇性)
  * EmailAddress (選擇性)
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser (選擇性)
  
**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼   | 說明     | 
| ------------------ |-----------------|
| 200                | 針對 GET 的呼叫成功，且回應主體中已傳回使用者的 JSON 陣列 |
| 204                | 針對 PUT 的呼叫成功，且已更新主機上的使用者 |
| 4XX                | 無效的要求資料或格式的各種錯誤 |
| 5XX                | 未預期失敗的錯誤代碼 |
