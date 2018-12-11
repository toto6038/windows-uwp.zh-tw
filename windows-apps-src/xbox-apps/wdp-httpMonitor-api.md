---
title: 裝置入口網站 HTTP 監視 API 參考
description: 了解如何在 Xbox 主機上存取來自焦點應用程式的 HTTP 流量。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 8b8828b060e0401e7938517e497bae20e1234baf
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883621"
---
# <a name="http-monitor-api-reference"></a>HTTP 監視 API 參考   
您可以使用此 API 存取焦點應用程式的即時 HTTP 流量，如果 Xbox 主機上已啟用 HTTP 監視 (開發人員首頁中核取方塊)。

## <a name="get-if-the-http-monitor-is-enabled"></a>取得是否啟用 HTTP 監視

**要求**

您可以在開發人員首頁取得是否已啟用的 HTTP 監視。

方法      | 要求 URI
:------     | :-----
GET | /ext/httpmonitor/sessions
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
JSON 物件，包含下列欄位：

* Enabled - (Bool) Xbox 主機上是否已啟用 HTTP 監視，透過在開發人員首頁中核取方塊。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="get-http-traffic-from-the-focused-app"></a>取得來自焦點應用程式的 HTTP 流量
**要求**

在 Xbox 上即時取得來自焦點應用程式的 HTTP 流量，只要它不是系統應用程式，如果已從開發人員首頁啟用 HTTP 監視。

方法      | 要求 URI
:------     | :-----
Websocket | /ext/httpmonitor/sessions
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
JSON 物件，包含下列欄位：

* 工作階段
    * RequestHeaders - (JSON 物件) HTTP 要求的要求標頭。
    * RequestContentHeaders - (JSON 物件) HTTP 要求的要求內容標頭。
    * RequestURL -（字串）要求 URL。
    * RequestMethod -（字串）要求方法。
    * RequestMessage -（字串）要求訊息，目前僅支援 JSON 和文字內容。
    * ResponseHeaders -（JSON 物件）HTTP 回應的回應標頭。
    * ResponseContentHeaders -（JSON 物件）HTTP 回應的回應內容標頭。
    * StatusCode - (數字) 回應狀態碼。
    * ReasponsePhrase -（字串）回應原因句子。
    * ResponseMessage -（字串）回應訊息，目前僅支援 JSON 和文字內容。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
403 | HTTP 監視停用，必須在開發人員首頁中啟用
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox