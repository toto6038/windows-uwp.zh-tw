---
title: 使用連接儲存緩衝區
description: 了解如何使用連接儲存體的緩衝區。
ms.assetid: 1d9d1b52-4bfe-4cd9-8b80-50ca6c0e9ae1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，連接的儲存體
ms.localizationpriority: medium
ms.openlocfilehash: 3df95e4807e8d3457143e67eebfb62011bf365cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595023"
---
# <a name="working-with-connected-storage-buffers"></a>使用連接儲存緩衝區

連接儲存體 API 使用**Windows::Storage::Streams::Buffer**來傳遞資料與應用程式的執行個體。 WinRT 類型無法公開原始指標，因為緩衝區執行個體資料的存取權是透過**DataReader**並**資料寫入元**類別。 不過，**緩衝區**也會實作 COM 介面**IBufferByteAccess**，因此可以取得直接緩衝區資料的指標。

### <a name="to-get-a-pointer-to-a-buffer-instances-data"></a>若要取得的緩衝執行個體的資料指標

1.  使用**轉換\_cast**轉型的緩衝執行個體**IUnknown**。

```cpp
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
```

2.  查詢**IUnknown**介面**IBufferByteAccess** COM 介面。

```cpp
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
```

3.  使用**IBufferByteAccess::Buffer**取得直接指向緩衝區的資料指標。

```cpp
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
```

例如，下列程式碼範例示範如何建立保留目前的系統時間的緩衝區。 因為緩衝區有不同的容量和長度值就必須明確設定容量和長度。 根據預設，長度為 0。

```cpp
    inline byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer)
    {
        using namespace Windows::Storage::Streams;
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
        return bytes;
    }

    IBuffer^ WrapRawBuffer( void* ptr, size_t size )
    {
        using namespace Windows::Storage::Streams;

        //uint32 size = sizeof(FILETIME);
        Buffer^ buffer = ref new Buffer(size);
        buffer->Length = size;

        memcpy(GetBufferData(buffer),ptr,size);


        return buffer;

    };
```
