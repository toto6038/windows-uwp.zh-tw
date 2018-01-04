---
title: "編碼及解碼資料"
description: "這個範例程式碼說明如何在通用 Windows 平台 (UWP) 應用程式中編碼及解碼 base64 和十六進位資料。"
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: e7b80edf6f0434e336b6b146ca4a9bc737d262ba
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: zh-TW
---
# <a name="encode-and-decode-data"></a>編碼及解碼資料


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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
