---
title: /global/scids/{scid}/data/{pathAndFileName},{type}
assetID: 774ce2dc-15c5-fe12-42b9-4e040bd4d2cf
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapathandfilenametype.html
description: " /global/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8c58ee4888cbbe7d9a752531c489b1da3fdde86
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656233"
---
# <a name="globalscidssciddatapathandfilenametype"></a>/global/scids/{scid}/data/{pathAndFileName},{type}
將檔案下載。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| guid| 要查閱的服務組態的識別碼。| 
| pathAndFileName| 字串| 要存取之項目的路徑和檔案名稱。 有效的字元 （最多且包含在最後斜線） 的路徑部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)，並正斜線 （/）。路徑部分可能是空的。有效的字元 （在最後斜線之後的所有內容） 中的檔案名稱部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)、 句號 （.），以及連字號 （-）。 檔案名稱不能是空的、 以句號結束或包含兩個連續的句號。| 
| type| 字串| 資料的格式。 可能的值為： 二進位檔、 組態或 json。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-globalscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;將檔案下載。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>父系 

[標題儲存體 Uri](atoc-reference-storagev2.md)

   