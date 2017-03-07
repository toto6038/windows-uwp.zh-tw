---
author: mtoepke
title: "加入聲音"
description: "在這個步驟中，我們會檢視射擊遊戲範例如何使用 XAudio2 API 來建立播放聲音的物件。"
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 遊戲, 聲音"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 11553a22274a36094a3e839e8fda648f78cfaaf8
ms.lasthandoff: 02/07/2017

---

# <a name="add-sound"></a>加入聲音


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在這個步驟中，我們會檢視射擊遊戲範例如何使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) API 來建立播放聲音的物件。

## <a name="objective"></a>目標


-   使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) 加入聲音輸出。

在遊戲範例中，音訊物件和行為是在三個檔案中定義：

-   **Audio.h/.cpp**。 這個程式碼檔案會定義 **Audio** 物件，其中包含聲音播放的 XAudio2 資源。 如果遊戲暫停或停用，它還會定義暫停和繼續音訊播放的方法。
-   **MediaReader.h/.cpp**。 這個程式碼會定義從本機存放區讀取音訊 .wav 檔的方法。
-   **SoundEffect.h/.cpp**。 這個程式碼會定義遊戲內聲音播放的物件。

## <a name="defining-the-audio-engine"></a>定義聲音引擎


當遊戲範例啟動時，它會建立一個配置遊戲音訊資源的 **Audio** 物件。 宣告這個物件的程式碼看起來就像這樣：

```cpp
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

**Audio::MusicEngine** 和 **Audio::SoundEffectEngine** 方法會將參考傳回給定義每個音訊類型之主播放聲音的 [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) 物件。 主播放聲音是用來播放的音訊裝置。 聲音資料緩衝區無法直接提交到主播放聲音，但是提交到其他聲音類型的資料必須導向主播放聲音才能被聽到。

## <a name="initializing-the-audio-resources"></a>初始化音訊資源


範例會呼叫 [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212) 來初始化音樂和音效引擎的 [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) 物件。 在引擎具現化後，它會呼叫 [**IXAudio2::CreateMasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/hh405048) 為每個引擎建立主播放聲音，像這樣：

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

在載入音樂或音效的音訊檔案時，這個方法會呼叫主播放聲音上的 [**IXAudio2::CreateSourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418607) (它會為要播放的來源聲音建立執行個體)。 完成檢視遊戲範例如何載入音訊檔案後，我們就會說明這個程式碼。

## <a name="reading-an-audio-file"></a>讀取音訊檔案


在遊戲範例中，讀取音訊格式檔案的程式碼是在 **MediaReader.cpp** 中定義。 讀取編碼 .wav 音訊檔案的特定方法 **MediaReader::LoadMedia**，看起來就像這樣：

```cpp
Platform::Array<byte>^  MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
        );

    // Get the complete WAVEFORMAT from the Media Type.
    Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
        );

    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    // Get the total length of the stream in bytes.
    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &propVariant)
        );
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes;

    double durationInSeconds = (duration / static_cast<double>(10000 * 1000));
    maxStreamLengthInBytes = static_cast<unsigned int>(durationInSeconds * m_waveFormat.nAvgBytesPerSec);

    // Make the length a multiple of 4 bytes.
    maxStreamLengthInBytes = (maxStreamLengthInBytes + 3) / 4 * 4;

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        DX::ThrowIfFailed(
            reader->ReadSample(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, nullptr, &flags, nullptr, &sample)
            );

        if (sample != nullptr)
        {
            DX::ThrowIfFailed(
                sample->ConvertToContiguousBuffer(&mediaBuffer)
                );

            BYTE *audioData = nullptr;
            DWORD sampleBufferLength = 0;
            DX::ThrowIfFailed(
                mediaBuffer->Lock(&audioData, nullptr, &sampleBufferLength)
                );

            for (DWORD i = 0; i < sampleBufferLength; i++)
            {
                fileData[positionInData++] = audioData[i];
            }
        }
        if (flags & MF_SOURCE_READERF_ENDOFSTREAM)
        {
            done = true;
        }
    }

    // Fix up the array size on match the actual length.
    Platform::Array<byte>^ realfileData = ref new Platform::Array<byte>((positionInData + 3) / 4 * 4);
    memcpy(realfileData->Data, fileData->Data, positionInData);
    return realfileData;
}
```

這個方法使用[媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms694197) API 當作脈衝碼調制 (PCM) 緩衝區來讀入 .wav 音訊檔案。

1.  呼叫 [**MFCreateSourceReaderFromURL**](https://msdn.microsoft.com/library/windows/desktop/dd388110) 來建立媒體來源讀取器 ([**IMFSourceReader**](https://msdn.microsoft.com/library/windows/desktop/dd374655)) 物件。
2.  呼叫 [**MFCreateMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms693861) 來建立解碼音訊檔案的媒體類型 ([**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850))。 這個方法會將解碼輸出指定為 PCM 音訊，它是 XAudio2 可以使用的音訊類型。
3.  呼叫 [**IMFSourceReader::SetCurrentMediaType**](https://msdn.microsoft.com/library/windows/desktop/bb970432) 來設定讀取器的解碼輸出媒體類型。
4.  建立 [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799) 緩衝區，並複製在 [**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850) 物件上呼叫 [**IMFMediaType::MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177) 的結果。 在音訊檔案載入後，會格式化存放這個音訊檔案的緩衝區。
5.  呼叫 [**IMFSourceReader::GetPresentationAttribute**](https://msdn.microsoft.com/library/windows/desktop/dd374662) 來取得音訊串流的持續時間 (以秒為單位)，然後將持續時間轉換為位元組。
6.  呼叫 [**IMFSourceReader::ReadSample**](https://msdn.microsoft.com/library/windows/desktop/dd374665) 將音訊檔案當作串流讀入。
7.  將音訊範例緩衝區的內容複製到方法傳回的陣列。

**SoundEffect::Initialize** 中最重要的事是從主播放聲音建立來源聲音物件 **m\_sourceVoice**。 我們會在從 **MediaReader::LoadMedia** 取得的聲音資料緩衝區實際播放使用來源聲音。

範例遊戲會在初始化 **SoundEffect** 物件時呼叫這個方法，就像這樣：

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

呼叫 **Audio::SoundEffectEngine** (或 **Audio::MusicEngine**)、**MediaReader::GetOutputWaveFormatEx** 的結果會傳送給這個方法，而且呼叫 **MediaReader::LoadMedia** 會傳回緩衝區，就像在這裡看到的。

```cpp
MediaReader^ mediaReader = ref new MediaReader;
auto targetHitSound = mediaReader->LoadMedia("hit.wav");
```

```cpp
myTarget->HitSound(ref new SoundEffect());
myTarget->HitSound()->Initialize(
                m_audioController->SoundEffectEngine(),
                mediaReader->GetOutputWaveFormatEx(),
                targetHitSound);
```

會從 **Simple3DGame:Initialize** 方法呼叫 **SoundEffect::Initialize**，這個方法會初始化主要的遊戲物件。

現在範例遊戲在記憶體中有音訊檔案了，我們再來看看如何在遊戲時播放它！

## <a name="playing-back-an-audio-file"></a>播放音訊檔案


```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

若要播放聲音，這個方法會使用來源聲音物件 **m\_sourceVoice**，以啟動播放聲音資料緩衝區 **m\_soundData**。 它會建立一個 [**XAUDIO2\_BUFFER**](https://msdn.microsoft.com/library/windows/desktop/ee419228)，並對它提供一個聲音資料緩衝區的參考，然後再呼叫 [**IXAudio2SourceVoice::SubmitSourceBuffer**](https://msdn.microsoft.com/library/windows/desktop/ee418473) 來提交它。 當聲音資料排入佇列時，**SoundEffect::PlaySound** 會呼叫 [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471) 開始播放。

現在只要子彈和目標發生撞擊，呼叫 **SoundEffect::PlaySound** 都會播放噪音。

## <a name="next-steps"></a>後續步驟


這是對通用 Windows 平台 (UWP) DirectX 遊戲開發的一大改革！ 到目前為止，您對於如何為 Windows 8 建立出色的遊戲應該已經有些概念了。 不過請記得，您的遊戲可以在多種 Windows 8 裝置和平台上進行，所以設計元件時，請盡可能讓您的圖形、控制項，使用者介面適用於廣泛的組態中！

如需修改這些文件所提供之範例的詳細資訊，請參閱[延伸遊戲範例](tutorial-resources.md)。

## <a name="complete-sample-code-for-this-section"></a>這個章節的完整範例程式碼


Audio.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

Audio.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Audio.h"
#include "DirectXSample.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

Audio::Audio():
    m_audioAvailable(false)
{
}

void Audio::Initialize()
{
}

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

#if defined(_DEBUG)
    XAUDIO2_DEBUG_CONFIGURATION debugConfiguration = {0};
    debugConfiguration.BreakMask = XAUDIO2_LOG_ERRORS;
    debugConfiguration.TraceMask = XAUDIO2_LOG_ERRORS;
    m_musicEngine->SetDebugConfiguration(&debugConfiguration);
#endif


    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

#if defined(_DEBUG)
    m_soundEffectEngine->SetDebugConfiguration(&debugConfiguration);
#endif

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}

IXAudio2* Audio::MusicEngine()
{
    return m_musicEngine.Get();
}

IXAudio2* Audio::SoundEffectEngine()
{
    return m_soundEffectEngine.Get();
}

void Audio::SuspendAudio()
{
    if (m_audioAvailable)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }
}

void Audio::ResumeAudio()
{
    if (m_audioAvailable)
    {
        DX::ThrowIfFailed(m_musicEngine->StartEngine());
        DX::ThrowIfFailed(m_soundEffectEngine->StartEngine());
    }
}
```

SoundEffect.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected:
    bool                    m_audioAvailable;
    IXAudio2SourceVoice*    m_sourceVoice;
    Platform::Array<byte>^  m_soundData;
};
```

SoundEffect.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "SoundEffect.h"
#include "DirectXSample.h"

SoundEffect::SoundEffect():
    m_audioAvailable(false)
{
}
//----------------------------------------------------------------------
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
//----------------------------------------------------------------------
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

 

 





