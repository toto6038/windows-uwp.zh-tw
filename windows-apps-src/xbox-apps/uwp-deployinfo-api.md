---
title: 裝置入口網站部署資訊 API 參考
description: 了解如何以程式設計方式存取部署資訊 API。
ms.localizationpriority: medium
ms.openlocfilehash: c44089313b100880b419e9b55a26101e877496f3
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8712174"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>對於一或多個安裝的套件要求部署資訊。

**要求**

方法      | 要求 URI
:------     | :------
POST | /ext/app/deployinfo
<br />
**URI 參數**

 - 無

**要求標頭**

- 無

**要求主體**

下列格式的 JSON 陣列：

* DeployInfo
  * PackageFullName - 我們正在要求相關資訊的套件名稱。
  * OverlayFolder - 如果使用這項功能選用的路徑重疊資料夾路徑。

###<a name="response"></a>回應

**回應本文**

下列格式的 JSON 陣列 (部分欄位是選用的)：

* DeployInfo
  * PackageFullName - 我們正在接收相關資訊的套件名稱。
  * DeployType - 部署的類型。
  * DeployPathOrSpecifiers - 鬆散部署的部署路徑，或封裝部署的已安裝規範。
  * DeployDrive - 針對適用的部署類型，部署套件到磁碟機。
  * DeploySizeInBytes - 針對適用的部署類型，套件的大小 (以位元組為單位)。
  * OverlayFolder - 支援此功能之部署的重疊資料夾。

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