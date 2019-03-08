---
title: Device Portal 網路認證 API 參考
description: 了解如何以程式設計方式新增、移除或更新網路認證。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: ac30d8db830c51ee40653feb49b443ed44502617
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659213"
---
# <a name="network-credentials-api-reference"></a>網路認證 API 參考
您可以使用此 REST API，新增、移除或更新開發套件上儲存的網路認證。

## <a name="get-existing-credentials"></a>取得現有認證

**要求**

您可以取得已儲存共用的清單，以及擁有該網路共用認證之使用者的使用者名稱。

方法      | 要求 URI
:------     | :-----
GET | /ext/networkcredential
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**   

- 無

**回應**   

- 下列格式的 JSON 陣列：
* 認證
  * NetworkPath - 網路共用的路徑。
  * Username - 擁有已儲存認證的使用者名稱。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="add-or-update-stored-credentials-for-a-user"></a>新增或更新使用者的已儲存認證

**要求**

方法      | 要求 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| NetworkPath        | 您要加入認證以存取之共用的網路路徑。 |
<br>

**要求標頭**

- 無

**要求本文**

- 下列 JSON 項目：
* NetworkPath - 網路共用的路徑。
* Username - 要儲存認證的使用者名稱。
* Password - 這位使用者的全新或已更新密碼。

**回應**   

- 無  

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="remove-stored-credentials-for-a-share"></a>移除共用的已儲存認證。

**要求**

方法      | 要求 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| NetworkPath        | 共用的網路路徑，您要從中移除已儲存的認證。 |
<br>

**要求標頭**

- 無

**要求本文**   

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
**可用的裝置系列**

* Windows Xbox


