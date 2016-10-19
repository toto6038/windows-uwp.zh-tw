---
title: "建立隨機數字"
description: "這個範例程式碼說明如何在通用 Windows 平台 (UWP) 應用程式中建立要用於加密編譯的隨機數字或緩衝區。"
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 71ac4a6bffe5004a044ee6feba21eb44762ab781

---

# 建立隨機數字


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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


<!--HONumber=Aug16_HO3-->


