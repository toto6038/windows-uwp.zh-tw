---
title: 裝置入口網站 SSH Pin API 參考
description: 瞭解如何使用/ext/app/sshpins Xbox 裝置入口網站 REST API，以程式設計方式移除所有信任的安全殼層 (SSH) pin。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 307af4cdd4e998832f4a2fe7a8f874615fe10ad7
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043530"
---
# <a name="ssh-pins-api-reference"></a>SSH Pin API 參考
您可以使用此 REST API 在您的 devkit 上移除所有受信任的 SSH Pin 碼。

## <a name="remove-trusted-ssh-pins"></a>移除受信任的 SSH Pin 碼

**要求**

方法      | 要求 URI
:------     | :-----
刪除 | /ext/app/sshpins
<br />
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

HTTP 狀態碼      | 描述
:------     | :-----
204 | 清除 Pin 的要求成功。
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox

