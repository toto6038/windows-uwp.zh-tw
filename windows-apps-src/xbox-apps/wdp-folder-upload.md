---
author: WilliamsJason
title: 裝置入口網站資料夾上傳 API 參考
description: 了解如何以程式設計方式存取資料夾上傳 API。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 481ec666c327b15088d8e60577c51fa1697918fe
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5593575"
---
# <a name="upload-a-folder-to-the-development-directory"></a>將資料夾上傳到開發目錄

**要求**

您可以將整個資料夾同時上傳到 DevelopmentFiles 的已知資料夾識別碼 (或是該資料夾內的的子資料夾)。

方法      | 要求 URI
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數      | 說明
:------     | :-----
destinationFolder (必要) | 上傳資料夾的目的地資料夾名稱。 這個資料夾會放置在主機上的 d:\developmentfiles\LooseApps 底下。 這個資料夾名稱必須是 base64 編碼，因為它可能包含路徑分隔符號 (如果該資料夾是 LooseApps 下的子資料夾)。
<br />

**要求標頭**

- 無

**要求主體**

- 目錄內容的多部分合格 http 本文。

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 成功
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Xbox

