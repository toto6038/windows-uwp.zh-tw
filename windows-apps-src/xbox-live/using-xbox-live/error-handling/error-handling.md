---
title: 錯誤處理
description: 了解如何進行呼叫的 Xbox Live 服務時，處理錯誤。
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 錯誤、 服務呼叫
ms.localizationpriority: medium
ms.openlocfilehash: 637b9da84ef3ae50ea6235ca86e6318ae62acd11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637003"
---
# <a name="error-handling"></a>錯誤處理

開發人員應該特別注意，當發出服務呼叫。 進行網路呼叫時，您的標題應該一律會處理錯誤。 針對長久以來一直在開發期間的服務呼叫，即使服務呼叫在真實世界環境中失敗的數種不同的原因。 例如，您的呼叫可能會因為失敗：

* 無法使用網路。
* 服務過於忙碌，傳回 503 HTTP 狀態碼。
* 要求不是有效的傳回 400 的 HTTP 狀態碼。
* 使用者沒有正確的權限，傳回 403 HTTP 狀態碼。
* 使用者已被禁用，傳回 401 HTTP 狀態碼。
IXMLHTTPRequest2 WinRT 服務 Api 呼叫是無法傳送要求 （ERROR_TIMEOUT、 RPC_S_CALL_FAILED_DNE 等等）
* 上述清單並不詳盡。 大部分的這些問題會導致來自服務的 API 層擲回例外狀況。 您的標題應該擷取這些例外狀況，並適當地處理它們。

那里兩個不同的樣式的錯誤處理中 Xbox 服務 API (XSAPI) API 根據您使用：

請參閱[c + + API 錯誤處理](error-handling-cpp.md)。

請參閱[WinRT API 錯誤處理](error-handling-winrt.md)。
