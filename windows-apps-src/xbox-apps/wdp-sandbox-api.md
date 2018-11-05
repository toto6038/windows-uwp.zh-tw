---
author: payzer
title: 裝置入口網站 Xbox Live 沙箱 API 參考
description: 了解如何以程式設計方式存取 Xbox Live 沙箱。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: 6f1729f07734b181dc5e0e8c97d702d8592302c2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6052083"
---
# <a name="xbox-live-sandbox-api-reference"></a>Xbox Live 沙箱 API 參考   
您可以使用此 REST API 取得並設定您的 Xbox Live 沙箱。

## <a name="get-the-xbox-live-sandbox"></a>取得 Xbox Live 沙箱

**要求**

您可以使用下列要求讀取目前裝置的 Xbox Live 沙箱值：

方法      | 要求 URI
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
Sandbox - (字串) 目前裝置所在的沙箱。   

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 已授與存取檔案共用認證的要求。
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="set-the-xbox-live-sandbox"></a>設定 Xbox Live 沙箱
您可以使用下列要求變更裝置的 Xbox Live 沙箱。 請注意，在 Xbox One 上，裝置需要重新啟動，設定才會生效。

**要求**

您可以使用下列要求設定目前裝置的 Xbox Live 沙箱值：

方法      | 要求 URI
:------     | :-----
PUT | /ext/xboxlive/sandbox
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**   
要求主體是包含下列欄位的 JSON 物件：   
Sandbox - (字串) 要設定裝置沙箱的新值。

**回應**   
Sandbox - (字串) 目前裝置所在的沙箱。   

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 已授與存取檔案共用認證的要求。
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox

