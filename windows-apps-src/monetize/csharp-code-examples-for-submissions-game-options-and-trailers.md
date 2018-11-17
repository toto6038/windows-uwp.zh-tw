---
author: Xansky
description: 使用本節的 C# 程式碼範例，深入了解如何使用 Microsoft Store 提交 API 來提交遊戲選項和預告片。
title: C# 範例 - 提交含遊戲選項與預告片的應用程式
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 提交 API, 程式碼範例, 遊戲選項, 預告片, 進階清單, C#
ms.localizationpriority: medium
ms.openlocfilehash: 61d2a919d6bebcb1807d8084dda39a2e6d660ca5
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7156924"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>C\# 範例：提交含遊戲選項與預告片的應用程式

本文提供 C# 程式碼範例，示範如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md)來執行這些任務：

* 取得 Azure AD 存取權杖以便用於 Microsoft Store 提交 API。
* 建立 App 提交
* 設定 App 提交的 Store 清單資料，包括[遊戲](manage-app-submissions.md#gaming-options-object)和[預告片](manage-app-submissions.md#trailer-object)進階清單選項。
* 上傳包含 App 提交的套件、清單影像和預告片檔案的 ZIP 檔案。
* 認可 App 提交。

您可以檢閱每個範例以深入了解示範的工作，或者您可以將本篇文章中的所有程式碼範例建置到主控台應用程式。 若要建置範例，請在 Visual Studio 中建立名為 **DevCenterApiSample** 的 C# 主控台應用程式，分別將每個範例複製到專案中單獨的程式碼檔案，然後建置專案。

## <a name="prerequisites"></a>必要條件

這些範例包含下列必要條件：

* 新增參考至您專案中的 System.Web 組件。
* 從 Newtonsoft 安裝 [Newtonsoft.Json](http://www.newtonsoft.com/json) NuGet 套件到您的專案。

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>建立 App 提交

```CreateAndSubmitSubmissionExample``` 類別定義公用 ```Execute``` 方法，此方法會呼叫其他範例方法，以使用 Microsoft Store 提交 API 來建立並認可包含遊戲選項和預告片的 App 提交。 若要自行調整這個程式碼：

* 將 ```tenantId``` 變數指派給應用程式的租用戶識別碼，並指派 ```clientId``` 和 ```clientSecret``` 變數給應用程式的用戶端識別碼和金鑰。 如需詳細資訊，請參閱[如何將 Azure AD 應用程式，您的合作夥伴中心帳戶產生關聯](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* 將 ```applicationId``` 變數指派給您要建立提交之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs#CreateAndSubmitSubmissionExample)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>取得 Azure AD 存取權杖

```DevCenterAccessTokenClient``` 類別定義協助程式方法，使用您的 ```tenantId```、```clientId``` 和 ```clientSecret``` 值來建立 Azure AD 存取權杖以搭配 Microsoft Store 提交 API 使用。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs#DevCenterAccessTokenClient)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>叫用提交 API 並上傳提交檔案的協助程式方法

```DevCenterClient``` 類別定義協助程式方法，叫用 Microsoft Store 提交 API 中的各種不同方法，並上傳包含 App 提交的套件、清單影像和預告片檔案的 ZIP 檔案。

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs#DevCenterClient)]

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
