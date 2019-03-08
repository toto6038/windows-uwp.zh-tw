---
title: /titles/{titleId}/sessions/{sessionId}/allocationStatus
assetID: 55611f4b-4ba4-fa9a-ce44-fcc4a6df1b35
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus.html
description: " /titles/{titleId}/sessions/{sessionId}/allocationStatus"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aa66b81d1e363488a6969ab31d7091b695f09ae4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603303"
---
# <a name="titlestitleidsessionssessionidallocationstatus"></a>/titles/{titleId}/sessions/{sessionId}/allocationStatus
針對給定的書名識別碼和工作階段識別碼，取得票證要求的狀態。 這些 uri 的網域`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [URI 參數](#ID4EU)
  * [主機名稱](#ID4EPB)
  * [有效的方法](#ID4EWB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 描述| 
| --- | --- | 
| titleId| 要求應操作的標題的識別碼。| 
| sessionId| 工作階段的識別碼來查閱。| 
  
<a id="ID4EPB"></a>

 
## <a name="host-name"></a>主機名稱
 
gameserverms.xboxlive.com
  
<a id="ID4EWB"></a>

 
## <a name="valid-methods"></a>有效的方法
  
[GET](uri-titlestitleidsessionssessionidallocationstatus-get.md)
 
&nbsp;&nbsp;傳回由其 sessionId sessionhost 配置狀態。
   