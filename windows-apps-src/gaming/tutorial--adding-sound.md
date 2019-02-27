---
title: 新增音效
description: 開發的簡單的聲音引擎，使用 XAudio2 Api 來播放遊戲的音樂和音效。
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 聲音
ms.localizationpriority: medium
ms.openlocfilehash: 8d5a976ef65bee5efc3329afc98bf198d094b037
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117838"
---
# <a name="add-sound"></a>加入聲音

本主題中，我們會建立一個簡單的聲音引擎，使用[XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) Api。 如果您是__XAudio2__的新手，我們已包含簡短底下[音訊概念](#audio-concepts)簡介。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

將音效新增到範例遊戲中使用[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)。

## <a name="define-the-audio-engine"></a>定義的音訊引擎

在遊戲範例中，音訊物件和行為是在三個檔案中定義：

* __[Audio.h](#audioh)/.cpp__： 定義__音訊__物件，其中包含聲音播放的__XAudio2__資源。 如果遊戲暫停或停用，它還會定義暫停和繼續音訊播放的方法。
* __ [MediaReader.h](#mediareaderh)/.cpp__： 定義從本機存放區讀取音訊.wav 檔的方法。
* __ [SoundEffect.h](#soundeffecth)/.cpp__： 定義遊戲內聲音播放的物件。

## <a name="overview"></a>概觀

在開始播放到您的遊戲音訊設定有三個主要部分。

1. [建立和初始化音訊資源](#create-and-initialize-the-audio-resources)
2. [載入音訊檔案](#load-audio-file)
3. [建立關聯到物件的音效](#associate-sound-to-object)

所有定義這些[simple3dgame:: initialize](#simple3dgameinitialize-method)方法中。 讓我們先檢查這個方法，然後再深入了解更多詳細資料中的各節。

設定好之後，我們了解如何觸發程序來播放音效。 如需詳細資訊，請移至[播放聲音](#play-the-sound)。

### <a name="simple3dgameinitialize-method"></a>Simple3dgame:: initialize 方法

在__simple3dgame:: initialize__，其中也初始化__m\_controller__和__m\_renderer__ ，我們會設定音訊引擎，並準備好播放音效。

 * 建立__m\_audioController__，也就是[音訊](#audioh)類別的執行個體。
 * 建立所需使用[Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法的音訊資源。 這裡，兩個__XAudio2__物件&mdash;建立音樂引擎物件和音效引擎物件，並針對每個主播放聲音。 音樂引擎物件可用來播放背景音樂，為您的遊戲。 音效引擎可用來在遊戲中播放音效。 如需詳細資訊，請參閱[建立和初始化音訊資源](#create-and-initialize-the-audio-resources)。
 * 建立__mediaReader__，也就是[MediaReader](#mediareaderh)類別的執行個體。 [MediaReader](#mediareaderh)，也就是[SoundEffect](#soundeffecth)類別的協助程式類別，會從檔案位置同步讀取音訊的小檔案，並傳回做為位元組陣列的聲音資料。
 * 使用[mediareader:: Loadmedia](#mediareaderloadmedia-method)從其位置載入音效的檔案，並建立__targetHitSound__變數來保存.wav 載入音效資料。 如需詳細資訊，請參閱[載入音訊檔案](#load-audio-file)。 

音效是遊戲物件與相關聯。 因此當與該遊戲物件，會發生衝突，它會觸發要播放的音效。 在這個遊戲範例中，我們已經針對 （什麼我們使用射擊目標與） 子彈和目標的音效。 
    
* 在__GameObject__類別中，沒有__HitSound__屬性，用來將物件的音效產生關聯。
* 建立[SoundEffect](#soundeffecth)類別的新執行個體，並將它初始化。 在初始化期間，會建立來源音音效。 
* 這個類別會播放音效使用主播放聲音的[音訊](#audioh)類別所提供。 從檔案位置使用[MediaReader](#mediareaderh)類別讀取音效的資料。 如需詳細資訊，請參閱[將音效物件產生關聯](#associate-sound-to-object)。

>[!Note]
>實際觸發程序來播放音效是由移動與這些遊戲物件的碰撞決定。 因此，呼叫實際播放這些音效定義於[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法。 如需詳細資訊，請移至[播放聲音](#play-the-sound)。

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

## <a name="create-and-initialize-the-audio-resources"></a>建立和初始化音訊資源

* 您可以使用[XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212)，XAudio2 API，來建立兩個新的 XAudio2 物件定義音樂和音效引擎。 這個方法會傳回來管理所有的音訊引擎狀態，音訊處理執行緒，語音圖形，與更多的物件的[IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908)介面指標。
* 之後引擎具現化，使用[ixaudio2:: Createmasteringvoice](https://msdn.microsoft.com/library/windows/desktop/hh405048)為每個音效引擎物件建立主播放聲音。

如需詳細資訊，請移至[如何： 初始化 XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)。

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

在遊戲範例中，讀取音訊格式檔案的程式碼被定義在[MediaReader.h](#mediareaderh)/cpp__。  若要讀取編碼的.wav 音訊檔案，請呼叫[mediareader:: Loadmedia](#mediareaderloadmedia-method)，傳入.wav 做為輸入參數的檔名。

### <a name="mediareaderloadmedia-method"></a>Mediareader:: Loadmedia 方法

這個方法使用[媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms694197) API 當作脈衝碼調制 (PCM) 緩衝區來讀入 .wav 音訊檔案。

#### <a name="set-up-the-source-reader"></a>設定來源讀取器

1. 您可以使用[MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110)來建立媒體來源讀取器 ([IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655))。
2. 您可以使用[Imfmediatype](https://msdn.microsoft.com/library/windows/desktop/ms693861)來建立的媒體類型 ([Mfcreatemediatype](https://msdn.microsoft.com/library/windows/desktop/ms704850)) 物件 (_mediaType_)。 它代表媒體格式的描述。 
3. 指定_mediaType_解碼的輸出為 PCM 音訊，它是__XAudio2__可以使用的音訊類型。
4. 來源讀取藉由呼叫[imfsourcereader:: Setcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx)集合的解碼的輸出媒體類型。

如需我們為什麼要使用來源讀取器的詳細資訊，請移至[來源讀取器](https://msdn.microsoft.com/library/windows/desktop/dd940436.aspx)。

#### <a name="describe-the-data-format-of-the-audio-stream"></a>描述資料格式的音訊資料流

1. 您可以使用[imfsourcereader:: Getcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374660)來取得資料流目前的媒體類型。
2. 使用[Imfmediatype](https://msdn.microsoft.com/library/windows/desktop/ms702177)來將目前的音訊媒體類型轉換成[WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799)緩衝區中，做為輸入使用較舊版本的作業的結果。 此結構指定後載入音訊時，使用波音訊資料流的資料格式。 

__WAVEFORMATEX__格式可用來描述 PCM 緩衝區。 相較於[WAVEFORMATEXTENSIBLE](https://msdn.microsoft.com/library/windows/hardware/ff538802)結構中，它只可用來描述音訊分批格式的子集。 如需__WAVEFORMATEX__和__WAVEFORMATEXTENSIBLE__之間的差異的詳細資訊，請參閱[「 可延伸的音訊格式描述元](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors)。

#### <a name="read-the-audio-stream"></a>讀取音訊資料流

1.  取得的持續時間，以秒為單位，藉由呼叫[imfsourcereader:: Getpresentationattribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) ]，然後按一下 [轉換為位元組的持續時間的音訊資料流。
2.  藉由呼叫[imfsourcereader:: Readsample](https://msdn.microsoft.com/library/windows/desktop/dd374665)資料流讀取音訊檔案中。 __ReadSample__會從媒體來源讀取下一個範例。
3.  使用[IMFSample::ConvertToContiguousBuffer](https://msdn.microsoft.com/library/windows/desktop/ms698917.aspx)將音訊範例緩衝區 （_範例_） 的內容複製到陣列 (_mediaBuffer_)。

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
## <a name="associate-sound-to-object"></a>建立關聯到物件的音效

當遊戲初始化時， [simple3dgame:: initialize](#simple3dgameinitialize-method)方法中，建立聲音的物件的關聯會發生。

重點：
* 在__GameObject__類別中，沒有__HitSound__屬性，用來將物件的音效產生關聯。
* 建立[SoundEffect](#soundeffecth)類別物件的新執行個體，並將它與遊戲物件建立關聯。 這個類別會播放音效使用__XAudio2__ Api。  它會使用主播放聲音的[音訊](#audioh)類別所提供。 聲音資料可以讀取檔案位置使用[MediaReader](#mediareaderh)類別。

[Soundeffect:: Initialize](#soundeffectinitialize-method)是用來初始化__SoundEffect__執行個體以下列輸入參數： 音效引擎物件 （IXAudio2 建立物件[Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法中），指標指標 」 來設定格式的.wav 檔案使用__mediareader:: Getoutputwaveformatex__和聲音資料載入使用[mediareader:: Loadmedia](#mediareaderloadmedia-method)方法。 在初始化期間，也會建立音效的來源音。

### <a name="soundeffectinitialize-method"></a>Soundeffect:: Initialize 方法

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

## <a name="play-the-sound"></a>播放聲音

因為這是其中移動的物件會更新，而決定物件之間的碰撞[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法中定義觸發程序來播放音效。

因為物件之間的互動不同大幅，根據遊戲時，我們不會討論的遊戲物件。 如果您有興趣，若要了解它的實作，請移至[Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method)方法。

原則上，發生衝突時，它會觸發藉由呼叫**soundeffect:: Playsound**播放音效。 這個方法會阻止任何音效，目前正在播放，並會將所需的聲音資料的記憶體緩衝區的佇列。 它會使用來源音來設定磁碟區、 送出音效的資料，並開始播放。

### <a name="soundeffectplaysound-method"></a>Soundeffect:: Playsound 方法

* 若要開始播放的聲音資料緩衝區**m\_soundData**會使用來源聲音物件**m\_sourceVoice**
* 建立[XAUDIO2\_BUFFER](https://msdn.microsoft.com/library/windows/desktop/ee419228)，要在其中提供聲音資料緩衝區，參考，並呼叫[ixaudio2sourcevoice:: Submitsourcebuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473)送出。 
* 當聲音資料排入佇列時，**SoundEffect::PlaySound** 會呼叫 [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471) 開始播放。

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

__Simple3DGame::UpdateDynamics__方法會負責處理的互動和遊戲物件之間的碰撞。 當物件相碰撞 （或交集） 時，它會觸發相關聯來播放音效。

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

我們已討論過的 UWP 架構、 圖形、 控制項、 使用者介面和 Windows 10 遊戲的音訊。 [延伸遊戲範例](tutorial-resources.md)中，這個教學課程的下一個部分說明開發遊戲時，可以使用的其他選項。

## <a name="audio-concepts"></a>音訊概念

適用於 Windows 10 遊戲開發，使用 XAudio2 版本 2.9。 此版本會隨附於 Windows 10。 如需詳細資訊，請移至[XAudio2 版本](https://msdn.microsoft.com/library/windows/desktop/ee415802.aspx)。

__AudioX2__是低階 API，可提供訊號處理與混音的基礎。 如需詳細資訊，請參閱[XAudio2 主要概念](https://msdn.microsoft.com/library/windows/desktop/ee415764.aspx)。

### <a name="xaudio2-voices"></a>XAudio2 聲音

有三種類型的 XAudio2 聲音物件： 來源音、 副混音和主播放聲音。 聲音則是 XAudio2 的物件使用處理、 操作，及播放音訊資料。 
* 來源聲音在用戶端提供的音訊資料上操作。 
* 來源和次混音聲音會將它們的輸出傳送到一或多個次混音或主播放聲音。 
* 次混音和主播放聲音會將傳入的所有聲音的音訊混合在一起，然後在結果上操作。 
* 主播放聲音接收來源音和副混音的資料，並將該資料傳送至音訊硬體。

如需詳細資訊，請移至[XAudio2 聲音](/windows/desktop/xaudio2/xaudio2-voices)。

### <a name="audio-graph"></a>音訊圖

音訊圖會是[XAudio2 聲音](/windows/desktop/xaudio2/xaudio2-voices)的集合。 音訊開頭的音訊圖來源音中的另一側、 選擇性地通過一或多個副混音和主播放聲音在結束。 音訊圖會包含來源音每個音效目前正在播放零或多個副混音，以及一個主控音。 最簡單的音訊圖，並在 XAudio2 中，進行噪音所需的最小值是單一來源聲音輸出直接至一個主控音。 如需詳細資訊，請移至[音訊圖](https://msdn.microsoft.com/library/windows/desktop/ee415739.aspx)。

### <a name="additional-reading"></a>其他閱讀

* [使用方法：初始化 XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)
* [使用方法：在 XAudio2 中載入音訊資料檔](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [使用方法：使用 XAudio2 播放音效](https://msdn.microsoft.com/library/windows/desktop/ee415787.aspx)

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


