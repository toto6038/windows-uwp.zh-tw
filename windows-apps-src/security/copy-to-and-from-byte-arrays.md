---
title: 複製到位元組陣列和從位元組陣列中複製
description: 這個範例程式碼說明如何在通用 Windows 平台 (UWP) app 中複製到位元組陣列和從位元組陣列中複製。
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 066ce6f058bae42144be0ba9523db72716544543
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689004"
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