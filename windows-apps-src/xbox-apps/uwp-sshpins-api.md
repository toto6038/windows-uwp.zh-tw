---
title: 裝置入口網站 SSH Pin API 參考
description: 了解如何以程式設計方式移除所有信任的 SSH Pin 碼。
ms.localizationpriority: medium
ms.openlocfilehash: 1ddf15d3cdb4089a8ef010a4ae46d247a06a10d7
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741206"
---
# <a name="ssh-pins-api-reference"></a>SSH Pin API 參考
您可以使用此 REST API 在您的 devkit 上移除所有受信任的 SSH Pin 碼。

## <a name="remove-trusted-ssh-pins"></a>移除受信任的 SSH Pin 碼

**要求**

方法      | 要求 URI
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**URI 參數**

- 無

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
204 | 清除 Pin 的要求成功。
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox

