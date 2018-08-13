---
author: M-Stahl
title: 裝置入口網站 Xbox info API 參考 （英文)
description: 了解如何存取 Xbox 裝置資訊。
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 xbox、 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: db1df2418a2bb60de4a72f51ad01a0bfd547ec20
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "406249"
---
# <a name="xbox-info-api-reference"></a>Xbox Info API 參考 （英文)   
您可以存取使用此 API Xbox 一部裝置資訊。

## <a name="get-xbox-one-device-information"></a>取得一個 Xbox 裝置資訊

**要求**

您可以取得裝置資訊有關您 Xbox 一個。

方法      | 要求 URI
:------     | :-----
GET | /ext/xbox/info
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**

- 無

**回應**   
JSON 物件，包含下列欄位：

* OsVersion-(String) 的 OS 版本。
* OsEdition-(String) 版本的作業系統，例如"年 3 月 2017"或"年 3 月 2017 QFE 1"。
* ConsoleId-(String) 主控台的識別碼。
* DeviceId-(String) 主控台的 Xbox Live 裝置 id。
* SerialNumber-(String) 主控台的序號。
* DevMode-(String) 主控台的目前開發人員模式，例如"None"或"零售"。
* ConsoleType-(String) 主控台的類型，例如"Xbox 一"或"Xbox 一個 S"。
* DevkitCertificateExpirationTime-主控台的開發人員 kit 憑證到期時的秒數 （數字） UTC 時間。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 個 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox