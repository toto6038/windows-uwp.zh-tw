---
description: 使用本節的 Python 程式碼範例，深入了解如何使用 Microsoft Store 提交 API 來提交遊戲選項和預告片。
title: Python 範例 - 提交含遊戲選項與預告片的應用程式
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 提交 API, 程式碼範例, 遊戲選項, 預告片, 進階清單, python
ms.localizationpriority: medium
ms.openlocfilehash: 59306e32fe1fcc68978c977b89934e64d85b8cc8
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8796956"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Python 範例：提交含遊戲選項與預告片的應用程式

本文提供 Python 程式碼範例，示範如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md)來執行這些任務：

* 取得 Azure AD 存取權杖以便用於 Microsoft Store 提交 API。
* 建立 App 提交
* 設定 App 提交的 Store 清單資料，包括[遊戲](manage-app-submissions.md#gaming-options-object)和[預告片](manage-app-submissions.md#trailer-object)進階清單選項。
* 上傳包含 App 提交的套件、清單影像和預告片檔案的 ZIP 檔案。
* 認可 App 提交。

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>建立 App 提交

此程式碼會呼叫其他範例類別和函式，以使用 Microsoft Store 提交 API 來建立並認可包含遊戲選項和預告片的 App 提交。 若要自行調整這個程式碼：

* 將 ```tenant``` 變數指派給應用程式的租用戶識別碼，並指派 ```client``` 和 ```secret``` 變數給應用程式的用戶端識別碼和金鑰。 如需詳細資訊，請參閱[如何將 Azure AD 應用程式，您的合作夥伴中心帳戶產生關聯](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* 將 ```application_id``` 變數指派給您要建立提交之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py#L1-L74)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>取得 Azure AD 存取權杖並叫用提交 API

以下範例定義下列類別：

* ```DevCenterAccessTokenClient``` 類別定義協助程式方法，使用您的 ```tenantId```、```clientId``` 和 ```clientSecret``` 值來建立 Azure AD 存取權杖以搭配 Microsoft Store 提交 API 使用。
* ```DevCenterClient``` 類別定義協助程式方法，叫用 Microsoft Store 提交 API 中的各種不同方法，並上傳包含 App 提交的套件、清單影像和預告片檔案的 ZIP 檔案。

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py#L1-L126)]

<span id="token" />

## <a name="get-app-submission-listing-data"></a>取得 App 提交清單資料

下例範例定義協助程式函式，傳回 JSON 格式清單資料以用於新範本 App 提交。

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py#L1-L170)]

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
