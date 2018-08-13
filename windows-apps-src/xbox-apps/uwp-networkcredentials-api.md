---
author: WilliamsJason
title: 裝置入口網站網路認證 API 參考 （英文)
description: 了解如何新增、 移除或以程式設計方式更新網路認證。
ms.localizationpriority: medium
ms.openlocfilehash: 7e00169f92ee6f0aa48df64ec4a1186f9682b358
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "410124"
---
# <a name="network-credentials-api-reference"></a>網路認證 API 參考 （英文)
您可以新增、 移除或更新預存的網路認證上使用此 REST API 您 devkit。

## <a name="get-existing-credentials"></a>取得現有的認證

**要求**

您可以取得具有該網路共用的認證之使用者的使用者名稱與預存的共用清單。

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

- JSON 陣列格式如下：
* 認證
  * 網路路徑-網路共用的路徑。
  * Username-已儲存的認證的使用者名稱。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 個 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="add-or-update-stored-credentials-for-a-user"></a>新增或更新的使用者儲存的認證

**要求**

方法      | 要求 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| 網路路徑        | 共用的網路路徑您要新增的認證以存取。 |
<br>

**要求標頭**

- 無

**要求主體**

- 下列的 JSON 元素：
* 網路路徑-網路共用的路徑。
* Username-來儲存下的認證的使用者名稱。
* 密碼-這位使用者的全新或更新密碼。

**回應**   

- 無  

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="remove-stored-credentials-for-a-share"></a>移除共用的儲存的認證。

**要求**

方法      | 要求 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| 網路路徑        | 您要從中移除儲存的認證共用的網路路徑。 |
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
204 | 認證要求已成功。
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox


