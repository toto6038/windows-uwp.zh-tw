---
title: 編碼及解碼資料
description: 這個範例程式碼說明如何在通用 Windows 平台 (UWP) 應用程式中編碼及解碼 base64 和十六進位資料。
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 3c4e694dca3c84c7e94e513d8bb10a3f405bbc86
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8709747"
---
# <a name="encode-and-decode-data"></a>編碼及解碼資料



這個範例程式碼說明如何在通用 Windows 平台 (UWP) 應用程式中編碼及解碼 base64 和十六進位資料。

```cs
public void EncodeDecodeBase64()
{
    // Define a Base64 encoded string.
    String strBase64 = "uiwyeroiugfyqcajkds897945234==";

    // Decoded the string from Base64 to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromBase64String(strBase64);

    // Encode the buffer back into a Base64 string.
    String strBase64New = CryptographicBuffer.EncodeToBase64String(buffer);
}

public void EncodeDecodeHex()
{
    // Define a hexadecimal string.
    String strHex = "30310AFF";

    // Decode a hexadecimal string to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromHexString(strHex);

    // Encode the buffer back into a hexadecimal string.
    String strHexNew = CryptographicBuffer.EncodeToHexString(buffer);
}
```
