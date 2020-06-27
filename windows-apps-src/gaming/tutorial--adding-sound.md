---
title: 加入聲音
description: 使用 XAudio2 Api 來開發簡單的音效引擎，以播放遊戲音樂和音效效果。
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 聲音
ms.localizationpriority: medium
ms.openlocfilehash: 0e624c750bfce0633bc91d440fd883341b831836
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409647"
---
# <a name="add-sound"></a>加入聲音

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列[建立簡單的通用 Windows 平臺（UWP）遊戲](tutorial--create-your-first-uwp-directx-game.md)的一部分。 該連結的主題會設定數列的內容。

在本主題中，我們會使用[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) api 來建立簡單的音效引擎。 如果您不熟悉__XAudio2__，我們在[音訊概念](#audio-concepts)底下已包含簡短的簡介。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至[Direct3D 範例遊戲](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

使用[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)將聲音新增至範例遊戲。

## <a name="define-the-audio-engine"></a>定義音訊引擎

在範例遊戲中，音訊物件和行為定義于三個檔案中：

* __/.Cpp [ ](#audioh) __：定義__音訊__物件，其中包含用於音效播放的__XAudio2__資源。 如果遊戲暫停或停用，它還會定義暫停和繼續音訊播放的方法。
* __ [MediaReader. h](#mediareaderh)/.cpp__：定義從本機儲存體讀取音訊 .wav 檔案的方法。
* __ [SoundEffect. h](#soundeffecth)/.cpp__：定義遊戲中音效播放的物件。

## <a name="overview"></a>概觀

有三個主要部分是為了讓遊戲中的音訊播放設定。

1. [建立並初始化音訊資源](#create-and-initialize-the-audio-resources)
2. [載入音訊檔案](#load-audio-file)
3. [將音效與物件產生關聯](#associate-sound-to-object)

它們全都定義于[Simple3DGame：： Initialize](#simple3dgameinitialize-method)方法中。 因此，讓我們先檢查這個方法，然後深入探討每個章節中的更多詳細資料。

在設定之後，我們會學習如何觸發音效效果以進行播放。 如需詳細資訊，請移至[播放音效](#play-the-sound)。

### <a name="simple3dgameinitialize-method"></a>Simple3DGame：： Initialize 方法

在__Simple3DGame：： Initialize__中， __m \_ 控制器__和__m \_ __轉譯器也會一併初始化，我們設定了音訊引擎並準備好播放聲音。

 * 建立__m \_ audioController__，這是[音訊](#audioh)類別的實例。
 * 使用[音訊：： CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法來建立所需的音訊資源。 這裡有兩個__XAudio2__物件， &mdash; 一個是音樂引擎物件和一個音效引擎物件，每個都是建立的。 音樂引擎物件可以用來播放遊戲的背景音樂。 音效引擎可以用來在遊戲中播放音效效果。 如需詳細資訊，請參閱[建立和初始化音訊資源](#create-and-initialize-the-audio-resources)。
 * 建立__mediaReader__，這是[mediaReader](#mediareaderh)類別的實例。 [MediaReader](#mediareaderh)是[SoundEffect](#soundeffecth)類別的 helper 類別，會從檔案位置同步讀取小型音訊檔案，並以位元組陣列的形式傳回音效資料。
 * 使用[MediaReader：： LoadMedia](#mediareaderloadmedia-method)從其位置載入音效檔，並建立__targetHitSound__變數來保存已載入的 .wav 聲音資料。 如需詳細資訊，請參閱[載入音訊](#load-audio-file)檔案。 

音效效果與遊戲物件相關聯。 因此當該遊戲物件發生衝突時，它會觸發要播放的音效效果。 在此範例遊戲中，我們會對 ammo （我們用來為目標進行的程式）和目標的音效產生效果。 
    
* 在__GameObject__類別中，有一個__HitSound__屬性，可用來將音效效果與物件產生關聯。
* 建立[SoundEffect](#soundeffecth)類別的新實例，並將它初始化。 在初始化期間，會建立音效效果的來源聲音。 
* 這個類別會使用[音訊](#audioh)類別所提供的主控語音來播放音效。 使用[MediaReader](#mediareaderh)類別從檔案位置讀取音效資料。 如需詳細資訊，請參閱[將音效與物件產生關聯](#associate-sound-to-object)。

>[!Note]
>播放音效的實際觸發程式是由這些遊戲物件的移動和衝突所決定。 因此，實際播放這些聲音的呼叫是在[Simple3DGame：： UpdateDynamics](#simple3dgameupdatedynamics-method)方法中定義。 如需詳細資訊，請移至[播放音效](#play-the-sound)。

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>建立並初始化音訊資源

* 使用[XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)（XAudio2 API）來建立兩個新的 XAudio2 物件，以定義音樂和音效效果引擎。 這個方法會傳回物件的[IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)介面指標，以管理所有音訊引擎狀態、音訊處理執行緒、語音圖形等等。
* 在引擎已具現化之後，請使用[IXAudio2：： CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice)來為每個音效引擎物件建立主控語音。

如需詳細資訊，請移至[如何：初始化 XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)。

### <a name="audiocreatedeviceindependentresources-method"></a>Audio：： CreateDeviceIndependentResources 方法

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>載入音訊檔案

在範例遊戲中，讀取音訊格式檔案的程式碼定義于[MediaReader](#mediareaderh)/cpp__。  若要讀取編碼的 .wav 音訊檔案，請呼叫[MediaReader：： LoadMedia](#mediareaderloadmedia-method)，並傳入 .wav 的檔案名做為輸入參數。

### <a name="mediareaderloadmedia-method"></a>MediaReader：： LoadMedia 方法

這個方法使用[媒體基礎](/windows/desktop/medfound/microsoft-media-foundation-sdk) API 當作脈衝碼調制 (PCM) 緩衝區來讀入 .wav 音訊檔案。

#### <a name="set-up-the-source-reader"></a>設定來源讀取器

1. 使用[MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl)來建立媒體來源讀取器（[IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)）。
2. 您可以使用[MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype)來建立媒體類型（[IMFMediaType](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)）物件 _（媒介_）。 它代表媒體格式的描述。 
3. 指定_媒體_的解碼輸出為 PCM 音訊，這是__XAudio2__可以使用的音訊類型。
4. 藉由呼叫[IMFSourceReader：： SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)，設定來源讀取器的已解碼輸出媒體類型。

如需為何使用來源讀取器的詳細資訊，請移至[來源讀取器](/windows/desktop/medfound/source-reader)。

#### <a name="describe-the-data-format-of-the-audio-stream"></a>描述音訊串流的資料格式

1. 使用[IMFSourceReader：： GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype)取得資料流程的目前媒體類型。
2. 使用[IMFMediaType：： MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) ，將目前的音訊媒體類型轉換成[WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)緩衝區，並使用先前作業的結果做為輸入。 此結構指定載入音訊之後所使用的 wave 音訊資料流程的資料格式。 

__WAVEFORMATEX__格式可以用來描述 PCM 緩衝區。 相較于[WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible)結構，它只能用來描述音訊 wave 格式的子集。 如需__WAVEFORMATEX__與__WAVEFORMATEXTENSIBLE__之間差異的詳細資訊，請參閱[可擴充的 Wave 格式描述](/windows-hardware/drivers/audio/extensible-wave-format-descriptors)元。

#### <a name="read-the-audio-stream"></a>讀取音訊串流

1.  藉由呼叫[IMFSourceReader：： GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute)取得音訊串流的持續時間（以秒為單位），然後將持續時間轉換成位元組。
2.  藉由呼叫[IMFSourceReader：： ReadSample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)，以資料流程的形式讀取中的音訊檔案。 __ReadSample__會從媒體來源讀取下一個範例。
3.  使用[IMFSample：： ConvertToContiguousBuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer)將「音訊範例緩衝區」（_範例_）的內容複寫到陣列（_mediaBuffer_）。

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>將音效與物件產生關聯

在[Simple3DGame：： Initialize](#simple3dgameinitialize-method)方法中，當遊戲初始化時，會將聲音與物件產生關聯。

Recap：
* 在__GameObject__類別中，有一個__HitSound__屬性，可用來將音效效果與物件產生關聯。
* 建立[SoundEffect](#soundeffecth)類別物件的新實例，並將它與遊戲物件產生關聯。 這個類別會使用__XAudio2__ api 來播放音效。  它會使用[音訊](#audioh)類別所提供的「主控語音」。 您可以使用[MediaReader](#mediareaderh)類別，從檔案位置讀取音效資料。

[SoundEffect：： Initialize](#soundeffectinitialize-method)是用來初始化具有下列輸入參數的__SoundEffect__實例：聲音引擎物件的指標（在[Audio：： CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)方法中建立的 IXAudio2 物件）、使用__MediaReader：： GetOutputWaveFormatEx__的 .Wav 檔案格式的指標，以及使用[MediaReader：： LoadMedia](#mediareaderloadmedia-method)方法載入的音效資料。 在初始化期間，也會建立音效效果的來源聲音。

### <a name="soundeffectinitialize-method"></a>SoundEffect：： Initialize 方法

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>播放音效

播放音效的觸發程式是在[Simple3DGame：： UpdateDynamics](#simple3dgameupdatedynamics-method)方法中定義的，因為這是更新物件的位置，並決定物件之間的衝突。

由於物件之間的互動會有很大的差異，視遊戲而定，我們不會在這裡討論遊戲物件的動態。 如果您有興趣瞭解其執行方式，請移至[Simple3DGame：： UpdateDynamics](#simple3dgameupdatedynamics-method)方法。

在原則上，當發生衝突時，會呼叫**SoundEffect：:P laysound**，觸發要播放的音效效果。 這個方法會停止目前現正播放的任何音效效果，並使用所需的音效資料將記憶體中的緩衝區排入佇列。 它會使用來源語音來設定磁片區、提交音效資料，然後開始播放。

### <a name="soundeffectplaysound-method"></a>SoundEffect：:P laySound 方法

* 使用來源語音物件**m \_ sourceVoice**來啟動音效資料緩衝區**m \_ soundData**的播放
* 建立[XAUDIO2 \_ 緩衝區](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)，它會提供對音效資料緩衝區的參考，然後使用[IXAudio2SourceVoice：： SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)的呼叫提交它。 
* 當聲音資料排入佇列時，**SoundEffect::PlaySound** 會呼叫 [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) 開始播放。

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame：： UpdateDynamics 方法

__Simple3DGame：： UpdateDynamics__方法會負責遊戲物件之間的互動和衝突。 當物件衝突（或 intersect）時，它會觸發相關聯的音效效果以進行播放。

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
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
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>後續步驟

我們已涵蓋 UWP 架構、圖形、控制項、使用者介面，以及 Windows 10 遊戲的音訊。 本教學課程的下一個部分[延伸了範例遊戲](tutorial-resources.md)，說明開發遊戲時可以使用的其他選項。

## <a name="audio-concepts"></a>音訊概念

針對 Windows 10 遊戲開發，請使用 XAudio2 版本2.9。 此版本隨附于 Windows 10。 如需詳細資訊，請移至[XAudio2 版本](/windows/desktop/xaudio2/xaudio2-versions)。

__AudioX2__是一個低層級的 API，可提供信號處理和混合基礎。 如需詳細資訊，請參閱[XAudio2 重要概念](/windows/desktop/xaudio2/xaudio2-key-concepts)。

### <a name="xaudio2-voices"></a>XAudio2 語音

XAudio2 語音物件有三種類型： [來源]、[submix] 和 [主控語音]。 語音是 XAudio2 用來處理、操作和播放音訊資料的物件。 
* 來源聲音在用戶端提供的音訊資料上操作。 
* 來源和次混音聲音會將它們的輸出傳送到一或多個次混音或主播放聲音。 
* 次混音和主播放聲音會將傳入的所有聲音的音訊混合在一起，然後在結果上操作。 
* 主控語音會接收來自來源語音和 submix 語音的資料，並將該資料傳送到音訊硬體。

如需詳細資訊，請移至[XAudio2 語音](/windows/desktop/xaudio2/xaudio2-voices)。

### <a name="audio-graph"></a>音訊圖表

音訊圖形是[XAudio2 語音](/windows/desktop/xaudio2/xaudio2-voices)的集合。 音訊會在來源語音的音訊圖形的一端開始，選擇性地通過一或多個 submix 語音，並以主控語音結束。 音訊圖表會包含每個目前播放音效的來源語音、零或多個 submix 語音，以及一個主控聲音。 最簡單的音訊圖表和在 XAudio2 中產生雜訊所需的最小值，是直接輸出到主控語音的單一來源聲音。 如需詳細資訊，請移至[音訊圖形](/windows/desktop/xaudio2/audio-graphs)。

### <a name="additional-reading"></a>其他閱讀資料

* [使用方法：初始化 XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [使用方法：在 XAudio2 中載入音訊資料檔](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [使用方法：使用 XAudio2 播放音效](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>重要的音訊 .h 檔案

### <a name="audioh"></a>Audio.h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>MediaReader。h

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect.h

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```
