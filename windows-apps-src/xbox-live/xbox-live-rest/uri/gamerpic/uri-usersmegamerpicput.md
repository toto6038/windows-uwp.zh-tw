---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
description: " PUT (/users/me/gamerpic)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7aedc7cbd8366c9cb8d3a60e2cb1f5e843b24a8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661653"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
上傳 1080 x 1080 gamerpic。 
  * [要求本文](#ID4EQ)
  * [HTTP 狀態碼](#ID4EZ)
  * [回應主體](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>要求本文
 
要求主體是 gamerpic （1080 x 1080 PNG 檔案）。
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | 
| 200| 確定| 成功的 GET。| 
| 201| 建立。| 上傳成功。| 
| 403| 已禁止| 撤銷權限。| 
| 500| 錯誤| 發生問題。| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>回應主體
 
回應主體會不傳送任何物件。
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>父系 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   