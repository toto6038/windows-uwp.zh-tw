---
author: M-Stahl
title: 裝置入口網站 Xbox 資訊 API 參考
description: 了解如何存取 Xbox 裝置資訊。
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
keywords: windows 10、 uwp、 xbox 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 4b0e2bab0ce7d5525e8032809954ff656a74a61c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "6992133"
---
# <a name="xbox-info-api-reference"></a>Xbox 資訊 API 參考   
您可以存取 Xbox One 裝置的資訊，請使用此 API。

## <a name="get-xbox-one-device-information"></a>取得 Xbox One 裝置資訊

**要求**

您可以取得裝置的資訊有關您的 Xbox One。

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

* OsVersion-（字串） 的 os 版本。
* OsEdition-（字串） 版本的作業系統，例如 「 3 月 2017 」 或 「 3 月 2017 QFE 1 」。
* ConsoleId-（字串） 主機的識別碼。
* DeviceId-（字串） 主機的 Xbox Live 裝置 id。
* SerialNumber-（字串） 主機的序號。
* DevMode-（字串） 主機的目前開發人員模式，例如 「 無 」 或 「 零售 」。
* ConsoleType-（字串） 主機的類型，例如 「 Xbox One 」 或 「 Xbox One S 」。
* DevkitCertificateExpirationTime-（數字） 的 UTC 時間 （秒） 主機的開發人員套件的憑證將會到期。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 要求成功
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox