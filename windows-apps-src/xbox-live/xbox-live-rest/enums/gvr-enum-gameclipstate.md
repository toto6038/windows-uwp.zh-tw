---
title: GameClipState 列舉
assetID: 97fe5c1e-f7b5-537e-69eb-8284b69cd3e1
permalink: en-us/docs/xboxlive/rest/gvr-enum-gameclipstate.html
description: " GameClipState 列舉"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f7b20224eeab1b98c7c80f0e4b551420b5a15e7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630713"
---
# <a name="gameclipstate-enumeration"></a>GameClipState 列舉
詳細說明 GameClipState 列舉型別。 
<a id="ID4ET"></a>

 
## <a name="gameclipstate"></a>GameClipState
 
| <b>列舉值</b>| <b>描述</b>| 
| --- | --- | 
| 無 | 遊戲的剪輯服務狀態是未知或未設定。| 
| PendingUpload | 遊戲的剪輯服務正在等候的資產上傳。| 
| PendingDelete | 遊戲的短片是為要刪除佇列中。 （實際上，表示它 「 已刪除 」）。| 
| 處理 | 所有處理都完成遊戲的剪輯。| 
| 正在處理| 正在處理遊戲剪輯 （編碼、 縮圖等。）。| 
| Publishing| 正在發行遊戲剪輯資產。| 
| Published| 發行遊戲剪輯資產 – 此狀態指出它已就緒，可檢視。| 
| 加上旗標| 遊戲的剪輯已標示為強制。| 
| 帳戶已被禁用| 已被禁用，但尚未刪除遊戲的剪輯。| 
| Uploaded| 遊戲的剪輯已完成上傳。| 
| 已刪除| 已刪除遊戲的剪輯。| 
| 錯誤| 遊戲的剪輯處於錯誤狀態且無法使用。| 
  