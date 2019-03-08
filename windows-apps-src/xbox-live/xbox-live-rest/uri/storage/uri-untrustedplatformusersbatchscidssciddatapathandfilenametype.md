---
title: /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
assetID: c2909e75-e108-3555-c157-f2d552f10e9f
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.html
description: " /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bc57ce4cd80a33b831be3eb3615489cec5a633b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599733"
---
# <a name="untrustedplatformusersbatchscidssciddatapathandfilenametype"></a>/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
下載多個檔案從多個使用者具有相同的檔案名稱。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| guid| 要查閱的服務組態的識別碼。| 
| pathAndFileName| 字串| 要存取之項目的路徑和檔案名稱。 有效的字元 （最多且包含在最後斜線） 的路徑部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)，並正斜線 （/）。路徑部分可能是空的。有效的字元 （在最後斜線之後的所有內容） 中的檔案名稱部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)、 句號 （.），以及連字號 （-）。 檔案名稱不能是空的、 以句號結束或包含兩個連續的句號。| 
| type| 字串| 資料的格式。 可能的值為二進位的 json。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;下載多個檔案從多個使用者具有相同的檔案名稱。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>父系 

[標題儲存體 Uri](atoc-reference-storagev2.md)

   