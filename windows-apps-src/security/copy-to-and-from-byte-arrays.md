---
title: 複製到位元組陣列和從位元組陣列中複製
description: 這個範例程式碼說明如何在通用 Windows 平台 (UWP) app 中複製到位元組陣列和從位元組陣列中複製。
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: 22ce577f7d70b3750a365462b64181a27e71428e
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "6669063"
---
# <a name="copy-to-and-from-byte-arrays"></a>複製到位元組陣列和從位元組陣列中複製



這個範例程式碼說明如何在通用 Windows 平台 (UWP) app 中複製到位元組陣列和從位元組陣列中複製。

```cs
public void ByteArrayCopy()
{
    // Initialize a byte array.
    byte[] bytes = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Create a buffer from the byte array.
    IBuffer buffer = CryptographicBuffer.CreateFromByteArray(bytes);

    // Encode the buffer into a hexadecimal string (for display);
    string hex = CryptographicBuffer.EncodeToHexString(buffer);

    // Copy the buffer back into a new byte array.
    byte[] newByteArray;
    CryptographicBuffer.CopyToByteArray(buffer, out newByteArray);
}
```