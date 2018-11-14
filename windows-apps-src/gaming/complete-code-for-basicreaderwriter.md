---
author: mtoepke
title: BasicReaderWriter 的完整程式碼
description: 讀取和寫入二進位資料檔的一般類別與方法的完整程式碼。
ms.assetid: af968edd-df5c-b8e6-479e-bfa9689380fc
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, BasicReaderWriter
ms.localizationpriority: medium
ms.openlocfilehash: 7a5d644a2a141a83316575a235805fa56657bf3a
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6649055"
---
# <a name="complete-code-for-basicreaderwriter"></a>BasicReaderWriter 的完整程式碼



讀取和寫入二進位資料檔的一般類別與方法的完整程式碼。 專供 [BasicLoader](complete-code-for-basicloader.md) 類別使用。

這個主題包含這些小節：

-   [技術](#technologies)
-   [需求](#requirements)
-   [檢視程式碼 (C++)](#view-the-code-c)


## <a name="download-location"></a>下載位置

此範例不提供下載。


## <a name="technologies"></a>技術

**程式設計語言** - C++  
**程式設計模型** - Windows 執行階段


## <a name="requirements"></a>需求

 **最低支援的用戶端** - Windows 10       
 **最低支援的伺服器** - Windows Server 2016 Technical Preview 

## <a name="view-the-code-c"></a>檢視程式碼 (C++)


## <a name="basicreaderwriterh"></a>BasicReaderWriter.h


```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include <ppltasks.h>

// A simple reader/writer class that provides support for reading and writing
// files on disk. Provides synchronous and asynchronous methods.
ref class BasicReaderWriter
{
private:
    Windows::Storage::StorageFolder^ m_location;
    Platform::String^ m_locationPath;

internal:
    BasicReaderWriter();
    BasicReaderWriter(
        _In_ Windows::Storage::StorageFolder^ folder
        );

    Platform::Array<byte>^ ReadData(
        _In_ Platform::String^ filename
        );

    concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename
        );

    uint32 WriteData(
        _In_ Platform::String^ filename,
        _In_ const Platform::Array<byte>^ fileData
        );

    concurrency::task<void> WriteDataAsync(
        _In_ Platform::String^ filename,
        _In_ const Platform::Array<byte>^ fileData
        );
};
```

## <a name="basicreaderwritercpp"></a>BasicReaderWriter.cpp


```cpp
using namespace Microsoft::WRL;
using namespace Windows::Storage;
using namespace Windows::Storage::FileProperties;
using namespace Windows::Storage::Streams;
using namespace Windows::Foundation;
using namespace Windows::ApplicationModel;
using namespace concurrency;

BasicReaderWriter::BasicReaderWriter()
{
    m_location = Package::Current->InstalledLocation;
    m_locationPath = Platform::String::Concat(m_location->Path, "\\");
}

BasicReaderWriter::BasicReaderWriter(
    _In_ Windows::Storage::StorageFolder^ folder
    )
{
    m_location = folder;
    Platform::String^ path = m_location->Path;
    if (path->Length() == 0)
    {
        // Applications are not permitted to access certain
        // folders, such as the Documents folder, using this
        // code path.  In such cases, the Path property for
        // the folder will be an empty string.
        throw ref new Platform::FailureException();
    }
    m_locationPath = Platform::String::Concat(path, "\\");
}

Platform::Array<byte>^ BasicReaderWriter::ReadData(
    _In_ Platform::String^ filename
    )
{
    CREATEFILE2_EXTENDED_PARAMETERS extendedParams = {0};
    extendedParams.dwSize = sizeof(CREATEFILE2_EXTENDED_PARAMETERS);
    extendedParams.dwFileAttributes = FILE_ATTRIBUTE_NORMAL;
    extendedParams.dwFileFlags = FILE_FLAG_SEQUENTIAL_SCAN;
    extendedParams.dwSecurityQosFlags = SECURITY_ANONYMOUS;
    extendedParams.lpSecurityAttributes = nullptr;
    extendedParams.hTemplateFile = nullptr;

    Wrappers::FileHandle file(
        CreateFile2(
            Platform::String::Concat(m_locationPath, filename)->Data(),
            GENERIC_READ,
            FILE_SHARE_READ,
            OPEN_EXISTING,
            &extendedParams
            )
        );
    if (file.Get() == INVALID_HANDLE_VALUE)
    {
        throw ref new Platform::FailureException();
    }

    FILE_STANDARD_INFO fileInfo = {0};
    if (!GetFileInformationByHandleEx(
        file.Get(),
        FileStandardInfo,
        &fileInfo,
        sizeof(fileInfo)
        ))
    {
        throw ref new Platform::FailureException();
    }

    if (fileInfo.EndOfFile.HighPart != 0)
    {
        throw ref new Platform::OutOfMemoryException();
    }

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(fileInfo.EndOfFile.LowPart);

    if (!ReadFile(
        file.Get(),
        fileData->Data,
        fileData->Length,
        nullptr,
        nullptr
        ))
    {
        throw ref new Platform::FailureException();
    }

    return fileData;
}

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}

uint32 BasicReaderWriter::WriteData(
    _In_ Platform::String^ filename,
    _In_ const Platform::Array<byte>^ fileData
    )
{
    CREATEFILE2_EXTENDED_PARAMETERS extendedParams = {0};
    extendedParams.dwSize = sizeof(CREATEFILE2_EXTENDED_PARAMETERS);
    extendedParams.dwFileAttributes = FILE_ATTRIBUTE_NORMAL;
    extendedParams.dwFileFlags = FILE_FLAG_SEQUENTIAL_SCAN;
    extendedParams.dwSecurityQosFlags = SECURITY_ANONYMOUS;
    extendedParams.lpSecurityAttributes = nullptr;
    extendedParams.hTemplateFile = nullptr;

    Wrappers::FileHandle file(
        CreateFile2(
            Platform::String::Concat(m_locationPath, filename)->Data(),
            GENERIC_WRITE,
            0,
            CREATE_ALWAYS,
            &extendedParams
            )
        );
    if (file.Get() == INVALID_HANDLE_VALUE)
    {
        throw ref new Platform::FailureException();
    }

    DWORD numBytesWritten;
    if (
        !WriteFile(
            file.Get(),
            fileData->Data,
            fileData->Length,
            &numBytesWritten,
            nullptr
            ) ||
        numBytesWritten != fileData->Length
        )
    {
        throw ref new Platform::FailureException();
    }

    return numBytesWritten;
}

task<void> BasicReaderWriter::WriteDataAsync(
    _In_ Platform::String^ filename,
    _In_ const Platform::Array<byte>^ fileData
    )
{
    return task<StorageFile^>(m_location->CreateFileAsync(filename, CreationCollisionOption::ReplaceExisting)).then([=](StorageFile^ file)
    {
        FileIO::WriteBytesAsync(file, fileData);
    });
}
```

 

 




