---
title: 媒體擷取 API 參考
description: 瞭解如何使用 Xbox 裝置入口網站 REST API 來捕捉目前畫面的 PNG 標記法。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: ee1ccba3fe2a3f83a95c3538cb267730f7770c4c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172732"
---
# <a name="media-capture-api-reference"></a>媒體擷取 API 參考 #

## <a name="request"></a>要求

您可以使用下列要求格式擷取代表目前畫面的 PNG 檔案。

| 方法        | 要求 URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |


**URI 參數**

您可以在要求 URI 上指定下列其他參數：


| URI 參數      | 說明     | 
| ------------------ |-----------------|
| download (選擇性)| 指出 HTTP 回應標頭是否應該設定為指示主機瀏覽器應該以附件形式下載螢幕擷取畫面 (而不是在瀏覽器中呈現) 的布林值。  |

**要求標頭**

* 無

**要求本文**

* 無

## <a name="response"></a>回應

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼   | 描述     | 
| ------------------ |-----------------|
| 200                | 螢幕擷取畫面要求成功且已傳回擷取 |
| 5XX                | 未預期失敗的錯誤代碼 |
<br>

**可用裝置系列**

* Windows Xbox

