---
author: WilliamsJason
title: 裝置入口網站 SSH Pin API 參考
description: 了解如何以程式設計方式移除所有信任的 SSH Pin 碼。
ms.localizationpriority: medium
ms.openlocfilehash: 88ba9d3e35650c8c581b9ddb76911636fc18c72e
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921231"
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
