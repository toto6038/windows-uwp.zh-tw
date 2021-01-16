---
title: 裝置入口網站 SSH Pin API 參考
description: 瞭解如何使用/ext/app/sshpins Xbox 裝置入口網站 REST API，以程式設計方式移除所有信任的安全殼層 (SSH) pin。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 999a77051b0eebe6ae5d8be9e4c2640709431b6d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254203"
---
# <a name="ssh-pins-api-reference"></a>SSH Pin API 參考

您可以使用此 REST API 在您的 devkit 上移除所有受信任的 SSH Pin 碼。

## <a name="remove-trusted-ssh-pins"></a>移除受信任的 SSH Pin 碼

**要求**

| 方法 | 要求 URI |
|--------|-------------|
| 刪除 | /ext/app/sshpins |

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

**回應**

- 無

**狀態碼**

此 API 具有下列預期狀態碼。

| HTTP 狀態碼 | 描述 |
|------------------|-------------|
| 204 | 清除 Pin 的要求成功。 |
| 4XX | 錯誤碼 |
| 5XX | 錯誤碼 |

**可用裝置系列**

* Windows Xbox
