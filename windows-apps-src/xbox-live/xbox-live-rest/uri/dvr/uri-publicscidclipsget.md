---
title: GET (/public/scids/{scid}/clips)
assetID: 15b3e873-1f96-b1da-2f79-6dac1369a4c0
permalink: en-us/docs/xboxlive/rest/uri-publicscidclipsget.html
description: " GET (/public/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5bce1dd261e0ad1172068a0287519cd0480da85f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589803"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
列出公用的剪輯。 此 URI 的網域是`gameclipsmetadata.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4ECB)
  * [查詢字串參數](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
此 API 可讓清單剪輯所公開的各種方法。 剪輯清單會根據傳回隱私權檢查和針對要求 XUID 的內容隔離檢查。
 
查詢會最佳化每個服務設定識別元 (SCID)。 進一步指定篩選條件或下面所列的預設值以外的排序次序可能在某些情況下需要較長的時間傳回。 這是對於較大集的影片更明顯。 查詢不能指定以遞增的排序順序。
 
限定詞，才能取得特定集合 ofpublic 剪輯。 提出要求的使用者必須能夠存取要求 SCID，否則為 HTTP 403 會傳回。
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| 字串| 公用的剪輯的主要服務設定識別項。| 
| titleid| 字串| 公用的剪輯的 titleId。 無法為 SCID 相同的 URI 中指定。 如果指定，將用來查閱主要 SCID 中。| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| <b>?achievementId={achievementId}</b>| 符合指定的最新剪輯<b>achievementId</b>。| 不支援其他的排序/篩選。| 
| <b>?greatestMomentId={greatestMomentId}</b>| 符合指定的最新剪輯<b>greatestMomentId</b>。| 不支援其他的排序/篩選。| 
| <b>？ 限定詞 = 建立 </b>| 最新| 必要。| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>父系 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   