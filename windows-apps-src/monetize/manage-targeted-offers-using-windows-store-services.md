---
author: Xansky
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: 使用 Microsoft Store 針對性優惠 API，取得目前可用於您的應用程式的針對性優惠。
title: 使用Microsoft Store 服務管理針對性優惠
ms.author: mhopkins
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10 uwp, Microsoft Store 服務, Microsoft Store 針對性優惠 API, 針對性優惠
ms.localizationpriority: medium
ms.openlocfilehash: dbfefefdb7f7b96dbe99b35656b610b393ab3afa
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6204067"
---
# <a name="manage-targeted-offers-using-store-services"></a>使用Microsoft Store 服務管理針對性優惠

如果您建立內的*針對性的優惠***互動 > 針對性優惠**您在合作夥伴中心，使用*Microsoft Store 針對性優惠 API*在您的應用程式程式碼來抓取資訊，可協助您實作的應用程式內體驗的應用程式頁面針對性的優惠。 如需有關針對性優惠，以及如何在儀表板中建立它們的詳細資訊，請參閱[使用針對性優惠提高吸引力與轉換數](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)。

性優惠 App 是簡單的 REST API，可根據使用者是不是針對性優惠客戶區隔的一部分，取得適用於目前使用者的針對性優惠。 若要在 app 程式碼中使用這個 API，請依照下列步驟執行：

1.  為您的 app 目前已登入的使用者[取得 Microsoft 帳戶權杖](#obtain-a-microsoft-account-token)。
2.  [為目前使用者取得針對性優惠](#get-targeted-offers)。
3.  實作與其中一個針對性優惠相關聯的附加元件的 App 內購買體驗。 如需有關實作 App 內購買的詳細資訊，請參閱[此文章](enable-in-app-purchases-of-apps-and-add-ons.md)。

如需示範所有步驟的完整程式碼範例，請參閱本文結尾的[程式碼範例](#code-example)。 下列各節提供每個步驟的詳細資料。

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>為目前使用者取得 Microsoft 帳戶權杖

在您的 app 程式碼，為目前已登入的使用者取得 Microsoft 帳戶 (MSA) 權杖。 您必須針對 Microsoft Store 針對性優惠 API，在 ```Authorization``` 要求標頭中傳送此權杖。 Microsoft Store 會使用此權杖來擷取適用於目前使用者的針對性優惠。

若要取得 MSA 權杖，請使用[WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) 類別來要求使用範圍 ```devcenter_implicit.basic,wl.basic``` 的權杖。 以下範例示範操作方法。 此範例是[完整範例](#code-example)的摘要，並需要完整範例中提供的 **using** 陳述式。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

如需有關如何取得 MSA 權杖的詳細資訊，請[Web 帳戶管理員](../security/web-account-manager.md)。

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>為目前使用者取得針對性優惠

當您取得目前使用者的 MSA 權杖之後，呼叫 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI 的 GET 方法，取得適用於目前使用者的針對性優惠。 如需有關這個 REST 方法的詳細資訊，請參閱[取得針對性優惠](get-targeted-offers.md)。

這個方法傳回附加元件的產品識別碼，這些附加元件與適用於目前使用者的針對性優惠相關聯。 使用此資訊，您可以為使用者提供一或多個針對性優惠做為 App 內購買。

下列範例示範如何取得適用於目前使用者的針對性優惠。 此範例是[完整範例](#code-example)的摘要。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 程式庫和其他類別，以及完整範例中提供的 **using** 陳述式。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>完整程式碼範例

以下程式碼範例示範下列工作：

* 取得目前使用者的 MSA 權杖。
* 使用[取得針對性優惠](get-targeted-offers.md)方法，取得適用於目前使用者所有針對性優惠。
* 購買與針對性優惠相關聯的附加元件。

此範例需要來自 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 程式庫。 範例使用此程式庫來序列化和還原序列化 JSON 格式的資料。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>相關主題

* [使用針對性優惠提高吸引力與轉換數](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [取得針對性優惠](get-targeted-offers.md)
