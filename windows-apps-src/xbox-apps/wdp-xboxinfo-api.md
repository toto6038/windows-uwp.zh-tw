---
title: Device Portal Xbox 資訊 API 參考
description: 瞭解如何使用 Xbox 裝置入口網站 REST API 的 GET 方法存取 Xbox One 的裝置資訊。
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10、uwp、xbox、裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: c2a4cecaf3340818b3679dfd3f64b9759ea46752
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174652"
---
# <a name="xbox-info-api-reference"></a>Xbox 資訊 API 參考   
您可以使用此 API 存取 Xbox One 裝置資訊。

## <a name="get-xbox-one-device-information"></a>取得 Xbox One 裝置資訊

## <a name="request"></a>要求

您可以取得您的 Xbox One 裝置資訊。

方法      | 要求 URI
:------     | :-----
GET | /ext/xbox/info

**URI 參數**

- 無

**要求標頭**

- 無

**要求本文**

- 無

## <a name="response"></a>回應
JSON 物件，包含下列欄位：

* OsVersion - (字串) 作業系統的版本。
* OsEdition -（字串）作業系統版本，例如「2017 年 3 月」或「2017 年 3 月 QFE 1」。
* ConsoleId -（字串）主機的識別碼。
* DeviceId -（字串）主機的 Xbox Live 裝置識別碼。
* SerialNumber -（字串）主機的序號。
* DevMode -（字串）主機目前開發人員模式，例如「None」或「Retail」。
* ConsoleType -（字串）主機的類型，例如「Xbox One」或「Xbox One S」。
* DevkitCertificateExpirationTime -（數字）主機開發套件憑證將到期的時間 (UTC 時間 (秒))。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

**可用裝置系列**

* Windows Xbox
