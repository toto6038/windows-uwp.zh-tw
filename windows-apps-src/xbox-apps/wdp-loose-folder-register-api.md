---
author: WilliamsJason
title: 裝置入口網站鬆散資料夾註冊 API 參考
description: 了解如何以程式設計方式存取鬆散資料夾登錄 API。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: efdf4214-9738-4df6-bf1f-ed7141696ef6
ms.localizationpriority: medium
ms.openlocfilehash: cb80e2dbd7ebdfbb05bd642b9875a9cd7cc356f3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5988252"
---
# <a name="register-an-app-in-a-loose-folder"></a>登錄鬆散資料夾中的 App  

**要求**

您可以使用下列要求格式登錄鬆散資料夾中的 App。

方法      | 要求 URI
:------     | :------
POST | /api/app/packagemanager/register
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數      | 說明
:------     | :-----
folder (必要) | 登錄套件的目的地資料夾名稱。 這個資料夾必須存在主機上的 d:\developmentfiles\LooseApps 底下。 這個資料夾名稱必須是 base64 編碼，因為它可能包含路徑分隔符號 (如果該資料夾位於 LooseApps 下的子資料夾中)。
<br />

**要求標頭**

- 無

**要求主體**

- 無

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 部署要求已受理並正在處理
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Xbox

**注意事項**

至少有三種不同的方式，可以取得主機上所需資料夾中的鬆散 App。 最簡單的方式是透過 SMB 將檔案複製到 \\&lt;IP_Address&gt;\DevelopmentFiles\LooseApps。 這將需要 UWA 套件上的使用者名稱和密碼 (可以透過 [/ext/smb/developerfolder](wdp-smb-api.md) 取得)。 

第二種方法是針對 /api/filesystem/apps/file 執行 POST (其中 knownfolderid 是 DevelopmentFiles、packagefullname 是空的，且已適當提供檔案名稱和路徑 (路徑開頭應為 LooseApps))，來將個別檔案複製到正確的位置。

第三種方法是透過 [/api/app/packagemanager/upload](wdp-folder-upload.md) (其中 destinationFolder 是要放置於 d:\developmentfiles\looseapps 底下資料夾的名稱，且裝載是目錄內容的多部分合格 http 本文)，同時複製整個資料夾。

