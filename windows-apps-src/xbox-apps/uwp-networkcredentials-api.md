---
title: 裝置入口網站網路認證的 API 參考
description: 了解如何新增、 移除或以程式設計方式更新網路憑證。
ms.localizationpriority: medium
ms.openlocfilehash: 2da8dae554a0dcbb84d3d3fc3873e2fb035175dc
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7974093"
---
# <a name="network-credentials-api-reference"></a>網路認證 API 參考
您可以新增、 移除或更新您使用此 REST API 的 devkit 上儲存的網路認證。

## <a name="get-existing-credentials"></a>取得現有的認證

**要求**

您可以取得儲存的共用以及具有該網路共用認證之使用者的使用者名稱的清單。

方法      | 要求 URI
:------     | :-----
GET | /ext/networkcredential
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**   

- 無

**回應**   

- 使用下列格式的 JSON 陣列：
* 認證
  * 網路路徑-網路共用的路徑。
  * 使用者名稱-這已儲存認證的使用者名稱。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="add-or-update-stored-credentials-for-a-user"></a>新增或更新儲存的認證的使用者

**要求**

方法      | 要求 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| 網路路徑        | 共用的網路路徑，您要新增認證，才能存取。 |
<br>

**要求標頭**

- 無

**要求主體**

- 下列 JSON 項目：
* 網路路徑-網路共用的路徑。
* 使用者名稱-儲存在認證的使用者名稱。
* 密碼-這位使用者的新的或或更新密碼。

**回應**   

- 無  

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="remove-stored-credentials-for-a-share"></a>移除儲存在共用的認證。

**要求**

方法      | 要求 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| 網路路徑        | 移除儲存的認證的共用網路路徑。 |
<br>

**要求標頭**

- 無

**要求主體**   

- 無

**回應**   

- 無 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 認證的要求成功。
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox


