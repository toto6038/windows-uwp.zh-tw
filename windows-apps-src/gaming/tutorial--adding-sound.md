---
title: 加入聲音
description: 開發簡單的聲音引擎使用 XAudio2 Api，以播放遊戲音樂和音效。
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 聲音
ms.localizationpriority: medium
ms.openlocfilehash: 945270247b8a288554e1910ac1c6f8e5c1ec1619
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367847"
---
# <a name="add-sound"></a>加入聲音

在本主題中，我們會建立簡單的聲音引擎，使用[XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction) Api。 如果您是新手__XAudio2__，我們已包含在簡短的簡介[音訊概念](#audio-concepts)。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

將音效新增至範例遊戲 using [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)。

## <a name="define-the-audio-engine"></a>定義音訊引擎

在遊戲範例中，音訊物件和行為是在三個檔案中定義：

* __[Audio.h](#audioh)/.cpp__:定義__音訊__物件，其中包含__XAudio2__音訊播放的資源。 如果遊戲暫停或停用，它還會定義暫停和繼續音訊播放的方法。
* __[MediaReader.h](#mediareaderh)/.cpp__:定義用於從本機儲存體讀取音訊.wav 檔案的方法。
* __[SoundEffect.h](#soundeffecth)/.cpp__:定義在遊戲音效播放的物件。

## <a name="overview"></a>總覽

取得設定至您的遊戲的音訊播放中有三個主要部分。

1. [建立和初始化的音訊的資源](#create-and-initialize-the-audio-resources)
2. [載入音訊檔案](#load-audio-file)
3. [建立關聯物件的音效](#associate-sound-to-object)

所有以定義它們[Simple3DGame::Initialize](#simple3dgameinitialize-method)方法。 讓我們先檢查這個方法，然後再深入探討各節中的更多詳細資料。

設定好之後，我們了解如何觸發播放音效。 如需詳細資訊，請移至[播放聲音](#play-the-sound)。

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize 方法

在  __Simple3DGame::Initialize__，其中__m\_控制器__和__m\_轉譯器__是也已初始化，我們設定音訊引擎並取得它準備好播放音效。

 * 建立__m\_audioController__，這是執行個體[音訊](#audioh)類別。
 * 建立所需使用的音訊資源[Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法。 這裡，兩個__XAudio2__物件&mdash;音樂引擎物件和一個音訊引擎物件和精通的語音，為每個所建立。 音樂引擎物件可用來播放背景音樂，您的遊戲。 音效引擎可用來在您的遊戲中播放音效。 如需詳細資訊，請參閱 <<c0> [ 建立和初始化的音訊資源](#create-and-initialize-the-audio-resources)。
 * 建立__mediaReader__，這是執行個體[MediaReader](#mediareaderh)類別。 [MediaReader](#mediareaderh)，這是一個協助程式類別[SoundEffect](#soundeffecth)類別，讀取小型的音訊檔案以同步方式從檔案位置，並傳回為位元組陣列的音效資料。
 * 使用[MediaReader::LoadMedia](#mediareaderloadmedia-method)從它的位置載入音效檔，並建立__targetHitSound__變數載入的.wav 音效資料。 如需詳細資訊，請參閱 <<c0> [ 負載音訊檔](#load-audio-file)。 

音效是遊戲物件相關聯。 所以與該遊戲的物件發生衝突時，它會觸發要播放的音效。 在這個遊戲的範例中，我們有音效的彈藥 （我們用來傳送含有目標），以及適用於目標。 
    
* 在  __GameObject__類別，所以會__HitSound__用來建立關聯的音效物件的屬性。
* 建立的新執行個體[SoundEffect](#soundeffecth)類別，並將它初始化。 在初始化期間，會建立來源的聲音的音效。 
* 這個類別會播放音效，使用所提供的精通語音[音訊](#audioh)類別。 音效資料讀取檔案的位置使用[MediaReader](#mediareaderh)類別。 如需詳細資訊，請參閱 <<c0> [ 建立關聯物件的聲音](#associate-sound-to-object)。

>[!Note]
>播放音效的實際觸發程序取決於移動和這些遊戲物件的衝突。 因此，呼叫真正開始玩這些聽起來定義於[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法。 如需詳細資訊，請移至[播放聲音](#play-the-sound)。

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>建立和初始化的音訊的資源

* 使用[XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)，XAudio2 API，以建立兩個新的 「 XAudio2 」 物件，其定義的音樂和音效的效果引擎。 這個方法傳回的物件的指標[IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)來管理所有音訊引擎介面所述，處理執行緒、 語音圖形和更多功能的音訊。
* 選取的引擎具現化之後，請使用[IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice)音效引擎物件的每個建立精通的語音。

如需詳細資訊，請移至[How to:初始化 XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)。

### <a name="audiocreatedeviceindependentresources-method"></a>Audio::CreateDeviceIndependentResources 方法

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

## <a name="load-audio-file"></a>載入音訊檔案

在遊戲的範例中，用於讀取音訊格式檔案的程式碼會定義於[MediaReader.h](#mediareaderh)/cpp__。  若要讀取編碼的.wav 音效檔，請呼叫[MediaReader::LoadMedia](#mediareaderloadmedia-method)，並傳入輸入參數為.wav 的檔案名稱。

### <a name="mediareaderloadmedia-method"></a>MediaReader::LoadMedia 方法

這個方法使用[媒體基礎](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) API 當作脈衝碼調制 (PCM) 緩衝區來讀入 .wav 音訊檔案。

#### <a name="set-up-the-source-reader"></a>設定來源讀取器

1. 使用[MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl)建立的媒體來源讀取器 ([IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader))。
2. 使用[MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype)若要建立的媒體類型 ([IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) 物件 (_mediaType_)。 它代表的媒體格式的描述。 
3. 指定_mediaType_的已解碼的輸出是 PCM 音訊，也就是音訊型別__XAudio2__可以使用。
4. 將解碼的輸出媒體類型的來源讀取器藉由呼叫[IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)。

如需為什麼我們要使用來源讀取器的詳細資訊，請移至[來源讀取器](https://docs.microsoft.com/windows/desktop/medfound/source-reader)。

#### <a name="describe-the-data-format-of-the-audio-stream"></a>描述資料格式的音訊資料流

1. 使用[IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype)為目前的媒體類型取得資料流。
2. 使用[IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype)要轉換的目前音訊的媒體類型[WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)緩衝區，使用先前作業的結果做為輸入。 此結構會指定 wave 音訊資料流載入音效之後使用的資料格式。 

__WAVEFORMATEX__格式可以用來描述 PCM 緩衝區。 相較[WAVEFORMATEXTENSIBLE](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible)結構，它只能用來描述音訊 wave 格式的子集。 如需差異的詳細資訊__WAVEFORMATEX__和__WAVEFORMATEXTENSIBLE__，請參閱[可擴充聲波格式描述元](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors)。

#### <a name="read-the-audio-stream"></a>讀取的音訊資料流

1.  取得持續時間 （秒），藉由呼叫的音訊資料流[IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) ，然後將持續時間轉換為位元組。
2.  讀取做為資料流中的音訊檔案，藉由呼叫[IMFSourceReader::ReadSample](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)。 __ReadSample__讀取媒體來源中的下一個範例。
3.  使用[IMFSample::ConvertToContiguousBuffer](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer)音訊範例緩衝區的內容複製 (_範例_) 成陣列 (_mediaBuffer_)。

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>建立關聯物件的音效

在遊戲初始化時建立關聯物件的聲音會發生[Simple3DGame::Initialize](#simple3dgameinitialize-method)方法。

重點：
* 在  __GameObject__類別，所以會__HitSound__用來建立關聯的音效物件的屬性。
* 建立的新執行個體[SoundEffect](#soundeffecth)類別物件，並將它與遊戲物件產生關聯。 這個類別會播放音效 using __XAudio2__ Api。  它會使用所提供的精通語音[音訊](#audioh)類別。 音效資料可以讀取檔案的位置使用[MediaReader](#mediareaderh)類別。

[SoundEffect::Initialize](#soundeffectinitialize-method)是用來初始化__SoundEffect__具有下列輸入參數的執行個體： 音效引擎物件指標 (IXAudio2 物件中建立[音訊::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法)，指向格式.wav 檔案使用的__MediaReader::GetOutputWaveFormatEx__，並使用音效資料載入[MediaReader::LoadMedia](#mediareaderloadmedia-method)方法。 在初始化期間，也會建立來源的聲音的音效。

### <a name="soundeffectinitialize-method"></a>SoundEffect::Initialize 方法

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>播放音效

中所定義的觸發程序來播放音效[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)決定方法因為這是會在已更新之物件的移動與物件之間的衝突。

因為物件之間的互動而大幅，有所不同遊戲，我們不會討論的遊戲的物件。 如果您想要了解它的實作，請移至[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法。

基本上，當衝突發生時，它會觸發藉由呼叫播放的音效**SoundEffect::PlaySound**。 此方法會停止目前正在播放任何聲音效果排入佇列中的記憶體緩衝區中，使用所需的聲音資料。 它會使用來源語音將磁碟區設定、 提交音效的資料，並開始播放。

### <a name="soundeffectplaysound-method"></a>SoundEffect::PlaySound 方法

* 使用來源語音物件**m\_sourceVoice**開始播放的音效資料緩衝區**m\_soundData**
* 會建立[XAUDIO2\_緩衝區](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)，它提供完善的資料緩衝區的參考以及然後將它提交呼叫[IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)。 
* 音效資料排入佇列， **SoundEffect::PlaySound**開始播放藉由呼叫[IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start)。

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

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 方法

__Simple3DGame::UpdateDynamics__方法會負責的互動性和遊戲物件之間的衝突。 當物件衝突 （或 intersect） 時，它就會觸發相關聯的音效播放。

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
           // Start playing the sounds for the impact between the two balls.
              m_ammo[one]->PlaySound(impact, m_player->Position());
              m_ammo[two]->PlaySound(impact, m_player->Position());

       }
    }
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.
    if (impact > 0.0f)
    {
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>後續步驟

我們已涵蓋 UWP 架構、 圖形、 控制項、 使用者介面和 Windows 10 遊戲的音訊。 本教學課程的下一個部分[擴充遊戲範例](tutorial-resources.md)，說明開發遊戲時，可以使用其他選項。

## <a name="audio-concepts"></a>音訊的概念

Windows 10 的遊戲程式開發中，使用 XAudio2 版本為 2.9。 此版本隨附於 Windows 10。 如需詳細資訊，請移至[XAudio2 版本](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-versions)。

__AudioX2__是低階 API，提供訊號處理和混合的基礎。 如需詳細資訊，請參閱 < [XAudio2 Key Concepts](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts)。

### <a name="xaudio2-voices"></a>XAudio2 語音

有三種類型的 XAudio2 語音物件： 來源、 submix，和要精通語音。 語音都是物件 XAudio2 使用來處理、 管理，以及播放音訊資料。 
* 來源聲音在用戶端提供的音訊資料上操作。 
* 來源和次混音聲音會將它們的輸出傳送到一或多個次混音或主播放聲音。 
* 次混音和主播放聲音會將傳入的所有聲音的音訊混合在一起，然後在結果上操作。 
* 精通語音 submix 語音，語音來源與接收資料，並將該資料傳送至的音訊硬體。

如需詳細資訊，請移至[XAudio2 語音](/windows/desktop/xaudio2/xaudio2-voices)。

### <a name="audio-graph"></a>音訊的圖形

音訊的圖形是一堆[XAudio2 語音](/windows/desktop/xaudio2/xaudio2-voices)。 音訊來源語音中音訊圖形的一方、 選擇性地通過一或多個 submix 語音，在開始和結束精通的語音。 為目前正在播放，零或多個 submix 語音，每個音效來源語音和一個精通的聲音，會包含音訊的圖形。 最簡單的音訊圖形中，並需要製造聲音 XAudio2 中的最小值是單一來源的語音輸出直接至精通的語音。 如需詳細資訊，請移至[音訊圖形](https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs)。

### <a name="additional-reading"></a>其他閱讀資料

* [如何：Initialize XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [如何：載入 XAudio2 中的音訊資料檔案](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [如何：播放使用 XAudio2 音效](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>索引鍵的音訊.h 檔案

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```


