---
title: Xbox Live 測試使用者管理 API 參考
description: 了解如何以程式設計方式存取使用者管理 API。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 71c47767cf026b962f682fb30ca93758dbd5e227
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244074"
---
#<a name="xbox-live-user-management"></a>Xbox Live 的使用者管理 #

## <a name="request"></a>要求

您可以取得主機上的使用者清單，或是透過新增、移除、登入、登出或修改現有使用者來更新清單。

| 方法        | 要求 URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |


**URI 參數**

* None

**要求標頭**

* None

**要求本文**

針對 PUT 的呼叫應該包含具有下列結構的 JSON 陣列：

* 使用者
  * AutoSignIn (選擇性)：停用或啟用以 EmailAddress 或 UserId 所指定帳戶自動登入的布林值。
  * EmailAddress (選用-必須提供除非贊助商的使用者在登入，如果未提供使用者識別碼):指定修改/新增/移除使用者的電子郵件地址。
  * 密碼 (選用-必須提供使用者目前無法在主控台上):所使用的主控台中新增新使用者的密碼。
  * SignedIn (選擇性)：指定所提供帳戶是否應登入或登出的布林值。
  * 使用者識別碼 (選用-除非贊助商的使用者在登入，如果未提供 EmailAddress 必須提供):指定修改/新增/移除使用者的使用者識別碼。
  * SponsoredUser (選擇性)：指定是否要新增贊助使用者的布林值。
  * （選擇性） 刪除： 指定要從主控台刪除這位使用者的 bool

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

| HTTP 狀態碼   | 描述     | 
| ------------------ |-----------------|
| 200                | 針對 GET 的呼叫成功，且回應主體中已傳回使用者的 JSON 陣列 |
| 204                | 針對 PUT 的呼叫成功，且已更新主機上的使用者 |
| 4XX                | 無效的要求資料或格式的各種錯誤 |
| 5XX                | 未預期失敗的錯誤代碼 |
