---
author: Xansky
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: 使用本節的 Java 程式碼範例，深入了解如何使用 Microsoft Store 提交 API。
title: Java 範例 - 提交應用程式、附加元件與正式發行前小眾測試版
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 程式碼範例, java
ms.localizationpriority: medium
ms.openlocfilehash: 4a0df9fe873ab7d7330e06a18bb1816df3157d7a
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5970792"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Java 範例：提交應用程式、附加元件與正式發行前小眾測試版

本文提供 java 程式碼範例，示範如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md)來執行這些任務：

* [取得 Azure AD 存取權杖](#token)
* [建立附加元件](#create-add-on)
* [建立套件正式發行前小眾測試版](#create-package-flight)
* [建立應用程式提交](#create-app-submission)
* [建立附加元件提交](#create-add-on-submission)
* [建立套件正式發行前小眾測試版提交](#create-flight-submission)

您可以檢閱每個範例以深入了解示範的工作，或者您可以將本篇文章中的所有程式碼範例建置到主控台應用程式。 如需完整程式碼清單，請參閱本文結尾的[程式碼清單](java-code-examples-for-the-windows-store-submission-api.md#code-listing)一節。

## <a name="prerequisites"></a>必要條件

這些範例使用下列程式庫：

* [Apache Commons Logging 1.2](http://commons.apache.org/proper/commons-logging) (commons-logging-1.2.jar)。
* [Apache HttpComponents Core 4.4.5 和 Apache HttpComponents Client 4.5.2](https://hc.apache.org/) (httpcore-4.4.5.jar 和 httpclient-4.5.2.jar)。
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) 和 [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0.jar 和 javax.json-1.0.4.jar)。

## <a name="main-program-and-imports"></a>主要程式與匯入

下列範例示範所有程式碼範例所使用的匯入陳述式，並實作命令列程式來呼叫其他範例方法。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/MainExample.java#L1-L64)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>取得 Azure AD 存取權杖

下列範例示範如何[取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，您可以使用它來呼叫 Microsoft Store 提交 API 中的方法。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 提交 API。 權杖到期之後，您可以產生新的權杖。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L65-L95)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>建立附加元件

下列範例示範如何[建立](create-an-add-on.md)然後[刪除](delete-an-add-on.md)套件正式發行前小眾測試版和附加元件。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L310-L345)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>建立套件正式發行前小眾測試版

下列範例示範如何[建立](create-a-flight.md)然後[刪除](delete-a-flight.md)套件正式發行前小眾測試版。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L185-L221)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>建立應用程式提交

下列範例示範如何使用 Microsoft Store 提交 API 中的幾個方法來建立應用程式提交。 若要這樣做，```SubmitNewApplicationSubmission```方法會建立新的提交當做複製的最後一個已發佈提交，並再更新，並將其認可至合作夥伴中心複製的提交。 具體來說，```SubmitNewApplicationSubmission``` 方法會執行以下工作：

1. 一開始，此方法[為指定的應用程式取得資料](get-an-app.md)。
2. 接下來，它會[刪除應用程式的擱置中提交](delete-an-app-submission.md) (如果有的話)。
3. 然後它會[為此應用程式建立新的提交](create-an-app-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會變更新提交的部分詳細資料，並將此提交的新套件上傳到 Azure Blob 儲存體。
5. 接下來，它[更新](update-an-app-submission.md)，然後[將其認可](commit-an-app-submission.md)至合作夥伴中心新的提交。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-an-app-submission.md)，直到此提交認可成功為止。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L97-L183)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>建立附加元件提交

下列範例示範如何使用 Microsoft Store 提交 API 中的幾個方法來建立附加元件提交。 若要這樣做，```SubmitNewInAppProductSubmission```方法會建立新的提交當做複製的最後一個已發佈提交，然後更新，並將其認可至合作夥伴中心複製的提交。 具體來說，```SubmitNewInAppProductSubmission``` 方法會執行以下工作：

1. 一開始，此方法會[針對指定的附加元件取得資料](get-an-add-on.md)。
2. 接下來，它會[刪除附加元件的擱置中提交](delete-an-add-on-submission.md) (如果有的話)。
3. 然後它會[為此附加元件建立新的提交](create-an-add-on-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會將包含此提交圖示的 ZIP 封存上傳到 Azure Blob 儲存體。
5. 接下來，它[更新](update-an-add-on-submission.md)，然後[將其認可](commit-an-add-on-submission.md)至合作夥伴中心新的提交。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-an-add-on-submission.md)，直到此提交認可成功為止。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L347-L431)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>建立套件正式發行前小眾測試版提交

下列範例示範如何使用 Microsoft Store 提交 API 中的幾個方法來建立套件正式發行前小眾測試版提交。 若要這樣做，```SubmitNewFlightSubmission```方法會建立新的提交當做複製的最後一個已發佈提交，然後更新，並將其認可至合作夥伴中心複製的提交。 具體來說，```SubmitNewFlightSubmission``` 方法會執行以下工作：

1. 一開始，此方法會[為指定的套件正式發行前小眾測試版取得資料](get-a-flight.md)。
2. 接下來，它會[刪除套件正式發行前小眾測試版的擱置中提交](delete-a-flight-submission.md) (如果有的話)。
3. 然後它會[為套件正式發行前小眾測試版建立新的提交](create-a-flight-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會將此提交的新套件上傳到 Azure Blob 儲存體。
5. 接下來，它[更新](update-a-flight-submission.md)，然後[將其認可](commit-a-flight-submission.md)至 PartnerCenter 新的提交。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-a-flight-submission.md)，直到此提交認可成功為止。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L223-L308)]

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>上傳提交檔案和處理要求回應的公用程式方法

這些公用程式方法示範這些工作︰

* 如何將包含應用程式或附加元件提交之新資產的 ZIP 封存上傳到 Azure Blob 儲存體。 如需將 ZIP 封存上傳到 Azure Blob 儲存體進行應用程式和附加元件提交的詳細資訊，請參閱[建立應用程式提交](manage-app-submissions.md#create-an-app-submission)、[建立附加元件提交](manage-add-on-submissions.md#create-an-add-on-submission)和[建立套件正式發行前小眾測試版提交](manage-flight-submissions.md#create-a-package-flight-submission)中的相關指示。
* 如何處理要求回應

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L433-L490)]

<span id="code-listing" />

## <a name="complete-code-listing"></a>完整的程式碼清單

下列程式碼清單包含先前所有的範例，並已組織為單一原始程式檔。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L1-L491)]

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
