---
author: payzer
title: 裝置入口網站 SMB API 參考
description: 了解如何以程式設計方式存取 SMB API。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: 2a337fe722d73a08c1c75a84478fc31e5bdf6b03
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5763225"
---
# <a name="developer-folder-api-reference"></a>開發人員資料夾 API 參考   
您可以使用標準的 &#91;檔案總管&#93; 存取您 Xbox One 上的開發相關檔案。 這可讓您從電腦針對主機輕鬆檢視並取代檔案。

**要求**

您可以使用下列要求存取開發人員資料夾。 要求會傳回：    
* 檔案共用的位置。 此位置可以輸入至 [檔案總管] 的位址列中。
* 存取檔案共用的使用者名稱。
* 存取檔案共用的密碼。

方法      | 要求 URI
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
路徑 - 檔案開發人員檔案共用的路徑。   
使用者名稱 - 存取開發人員檔案共用所需要的使用者名稱。   
密碼 - 存取開發人員檔案共用所需要的密碼。   

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
