---
title: 建立隨機數字
description: 這個範例程式碼說明如何在通用 Windows 平台 (UWP) 應用程式中建立要用於加密編譯的隨機數字或緩衝區。
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.localizationpriority: medium
ms.openlocfilehash: f36e50f2de36df39177370d688b9add5591403e1
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8473314"
---
# <a name="create-random-numbers"></a>建立隨機數字



這個範例程式碼說明如何在通用 Windows 平台 (UWP) 應用程式中建立要用於加密編譯的隨機數字或緩衝區。

```cs
public string GenerateRandomData()
{
    // Define the length, in bytes, of the buffer.
    uint length = 32;

    // Generate random data and copy it to a buffer.
    IBuffer buffer = CryptographicBuffer.GenerateRandom(length);

    // Encode the buffer to a hexadecimal string (for display).
    string randomHex = CryptographicBuffer.EncodeToHexString(buffer);

    return randomHex;
}

public uint GenerateRandomNumber()
{
    // Generate a random number.
    uint random = CryptographicBuffer.GenerateRandomNumber();
    return random;
}
```