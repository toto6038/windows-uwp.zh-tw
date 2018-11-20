---
author: Xansky
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: 使用本節的 Python 程式碼範例，深入了解如何使用 Microsoft Store 提交 API。
title: Python 範例 - 提交應用程式、附加元件與正式發行前小眾測試版
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 程式碼範例, python
ms.localizationpriority: medium
ms.openlocfilehash: 34d686b8e20d384da4a3db1ea3805ad082d5f8a8
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7296829"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Python 範例：提交應用程式、附加元件與正式發行前小眾測試版

本文提供 Python 程式碼範例，示範如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md)來執行這些任務：

* [取得 Azure AD 存取權杖](#token)
* [建立附加元件](#create-add-on)
* [建立套件正式發行前小眾測試版](#create-package-flight)
* [建立應用程式提交](#create-app-submission)
* [建立附加元件提交](#create-add-on-submission)
* [建立套件正式發行前小眾測試版提交](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>取得 Azure AD 存取權杖

下列範例示範如何[取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，您可以使用它來呼叫 Microsoft Store 提交 API 中的方法。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 提交 API。 權杖到期之後，您可以產生新的權杖。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>建立附加元件

下列範例示範如何[建立](create-an-add-on.md)然後[刪除](delete-an-add-on.md)套件正式發行前小眾測試版和附加元件。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>建立套件正式發行前小眾測試版

下列範例示範如何[建立](create-a-flight.md)然後[刪除](delete-a-flight.md)套件正式發行前小眾測試版。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>建立應用程式提交

下列範例示範如何使用 Microsoft Store 提交 API 中的幾個方法來建立應用程式提交。 若要這樣做，程式碼會建立新的提交當做複製的最後一個已發佈提交，然後更新，並將其認可至合作夥伴中心複製的提交。 具體來說，此範例會執行以下工作：

1. 一開始，此範例會[為指定的應用程式取得資料](get-an-app.md)。
2. 接下來，它會[刪除應用程式的擱置中提交](delete-an-app-submission.md) (如果有的話)。
3. 然後它會[為此應用程式建立新的提交](create-an-app-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會變更新提交的部分詳細資料，並將此提交的新套件上傳到 Azure Blob 儲存體。
5. 接下來，它[更新](update-an-app-submission.md)，然後[將其認可](commit-an-app-submission.md)至合作夥伴中心新的提交。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-an-app-submission.md)，直到此提交認可成功為止。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>建立附加元件提交

下列範例示範如何使用 Microsoft Store 提交 API 中的幾個方法來建立附加元件提交。 若要這樣做，程式碼會建立新的提交當做複製的最後一個已發佈提交，然後更新，並將其認可至合作夥伴中心複製的提交。 具體來說，此範例會執行以下工作：

1. 一開始，此範例會[為指定的附加元件取得資料](get-an-add-on.md)。
2. 接下來，它會[刪除附加元件的擱置中提交](delete-an-add-on-submission.md) (如果有的話)。
3. 然後它會[為此附加元件建立新的提交](create-an-add-on-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會將包含此提交圖示的 ZIP 封存上傳到 Azure Blob 儲存體。 如需詳細資訊，請參閱[建立附加元件提交](manage-add-on-submissions.md#create-an-add-on-submission)中有關上傳 ZIP 封存到 Azure Blob 儲存體的相關指示。
5. 接下來，它[更新](update-an-add-on-submission.md)，然後[將其認可](commit-an-add-on-submission.md)至合作夥伴中心新的提交。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-an-add-on-submission.md)，直到此提交認可成功為止。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>建立套件正式發行前小眾測試版提交

下列範例示範如何使用 Microsoft Store 提交 API 中的幾個方法來建立套件正式發行前小眾測試版提交。 若要這樣做，程式碼會建立新的提交當做複製的最後一個已發佈提交，然後更新，並將其認可至合作夥伴中心複製的提交。 具體來說，此範例會執行以下工作：

1. 一開始，此範例會[為指定的套件正式發行前小眾測試版取得資料](get-a-flight.md)。
2. 接下來，它會[刪除套件正式發行前小眾測試版的擱置中提交](delete-a-flight-submission.md) (如果有的話)。
3. 然後它會[為套件正式發行前小眾測試版建立新的提交](create-a-flight-submission.md) (新的提交是最後一個已發佈提交的複本)。
4. 它會將此提交的新套件上傳到 Azure Blob 儲存體。 如需詳細資訊，請參閱[建立套件正式發行前小眾測試版提交](manage-flight-submissions.md#create-a-package-flight-submission)中有關上傳 ZIP 封存到 Azure Blob 儲存體的相關指示。
5. 接下來，它[更新](update-a-flight-submission.md)，然後[將其認可](commit-a-flight-submission.md)至合作夥伴中心新的提交。
6. 最後，它會定期[檢查新提交的狀態](get-status-for-a-flight-submission.md)，直到此提交認可成功為止。

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
