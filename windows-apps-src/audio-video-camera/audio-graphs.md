---
author: drewbatgit
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: "此文章說明如何使用 Windows.Media.Audio 命名空間中的 API 來建立音訊路由傳送、混音及處理案例的音訊圖。"
title: "音訊圖"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7e8df66a1fc4c95cb8b0b4be9eded8ef58b6803a

---

# 音訊圖

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


此文章說明如何使用 [**Windows.Media.Audio**](https://msdn.microsoft.com/library/windows/apps/dn914341) 命名空間中的 API 來建立音訊路由傳送、混音及處理案例的音訊圖。

音訊圖是一組用以傳送音訊資料的互連音訊節點。 音訊輸入節點會從音訊輸入裝置、音訊檔案，或從自訂程式碼將音訊資料提供給此圖形。 音訊輸出節點是此圖形所處理之音訊的目的地。 音訊可以從此圖形路由傳送至音訊輸出裝置、音訊檔案或自訂程式碼。 最後一種節點類型是副混音節點，它會從一或多個節點取得音訊，然後將它們結合成可路由傳送至圖形中其他節點的單一輸出。 已建立所有節點並設定它們之間的連線之後，您只需啟動音訊圖性，音訊資料就會從輸入節點經由任何副混音節點傳送至輸出節點。 此模型可讓您快速而輕鬆地實作案例，例如：從裝置的麥克風錄製到音訊檔、將檔案中的音訊播放到裝置的喇叭，或混合來自多個來源的音訊。

將音訊效果加入至音訊圖，促成其他案例。 音訊圖中的每個節點可以填入零個或多個效果，以對透過節點傳遞的音訊執行音訊處理。 內建效果有好幾個：例如等化器、限制，以及可利用少數幾行程式碼附加到音訊節點的殘響效果。 您也可以建立自己的自訂音訊效果，其運作方式與內建效果完全相同。

**注意**  
[AudioGraph UWP 範例](http://go.microsoft.com/fwlink/?LinkId=619481)實作本概觀文章中所討論的程式碼。 您可以下載範例以查看內容中的程式碼，或做為您自己 app 的初期基礎。

## 選擇 Windows 執行階段 AudioGraph 或 XAudio2

Windows 執行階段音訊圖 API 提供的功能也可使用以 COM 為基礎的 [XAudio2 API](https://msdn.microsoft.com/library/windows/desktop/hh405049) 實作。 以下是與 XAudio2 不同的 Windows 執行階段音訊圖架構功能。

-   Windows 執行階段音訊圖 API 明顯比 XAudio2 容易使用。
-   可以從 C# 使用 Windows 執行階段音訊圖 API - 除了對 C++ 提供支援以外。
-   Windows 執行階段音訊圖 API 可以直接使用包含壓縮檔案格式的音訊檔案。 XAudio2 只能在音訊緩衝區上操作，並不提供任何檔案 I/O 功能。
-   Windows 執行階段音訊圖 API 可以在 Windows 10 中使用低延遲音訊管線。
-   使用預設端點參數時，Windows 執行階段音訊圖 API 支援自動端點切換。 例如，如果使用者從裝置的喇叭切換到耳機時，則音訊會自動重新導向至新的輸入。

## AudioGraph 類別

[**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/dn914176) 類別是構成圖形之所有節點的父項。 使用此物件來建立所有音訊節點類型的執行個體。 初始化 [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) 物件，接著呼叫 [**AudioGraph.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn914216)，以建立 **AudioGraph** 類別的執行個體，包含圖形的組態設定。 傳回的 [**CreateAudioGraphResult**](https://msdn.microsoft.com/library/windows/apps/dn914273) 可供存取建立的音訊圖，或在音訊圖建立失敗時提供錯誤值。

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   使用 **AudioGraph** 類別的 Create\* 方法，建立所有音訊節點類型。
-   [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244) 方法會導致音訊圖開始處理音訊資料。 [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245) 方法會停止音訊處理。 執行圖形時，可以獨立啟動和停止圖形中的每個節點，但圖形停止時，就沒有作用中的節點。 [ **ResetAllNodes** ](https://msdn.microsoft.com/library/windows/apps/dn914242) 會導致圖形中的所有節點捨棄目前在其音訊緩衝區中的所有資料。
-   當圖形開始處理新的音訊資料配量，會發生 [**QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn914241) 事件。 當配量處理完成時，會發生 [**QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) 事件。

-   唯一必要的 [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) 屬性是 [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724)。 指定這個值可讓系統最佳化指定類別的音訊管線。
-   音訊圖的配量大小會決定一次處理的範例數目。 根據預設，配量大小採用預設取樣率並以 10 毫秒為基礎。 如果您藉由設定 [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205) 屬性來指定自訂配量大小，您也必須將 [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) 屬性設定為 **ClosestToDesired**，否則會忽略所提供的值。 如果使用這個值，則系統會選擇儘可能接近您指定之配量大小的大小。 若要判斷實際配量大小，請在建立完成後檢查 **AudioGraph** 的 [**SamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914243)。
-   如果您只打算搭配使用音訊圖和檔案，而不打算輸出到音訊裝置，建議您不要設定 [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205) 屬性，以使用預設配量大小。
-   [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522) 屬性決定主要轉譯裝置對音訊圖輸出執行的處理量。 **Default** 設定可讓系統對指定的音訊轉譯類別使用預設音訊處理。 此處理可大幅改善某些裝置上的音訊聲音，特別是具有小型喇叭的行動裝置。 **Raw** 設定可將執行的訊號處理量降至最低，進而提高效能，但可能會導致某些裝置上的音效品質較差。
-   如果 [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) 設定為 **LowestLatency**，則音訊圖會自動為 [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522) 使用 **Raw**。
-   [**EncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn958523) 可判斷圖形所使用的音訊格式。 僅支援 32 位元的浮點格式。
-   [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) 會設定音訊圖的主要轉譯裝置。 如果您未設定此項，則會使用預設系統裝置。 主要轉譯裝置用來計算圖形中的其他節點的配量大小。 如果系統上未出現任何音訊轉譯裝置，則音訊圖建立將會失敗。

您可以讓音訊圖使用預設音訊轉譯裝置，或使用 [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 類別來取得系統可用的音訊轉譯裝置清單，其作法是呼叫 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 並傳入 [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817) 所傳回的音訊轉譯裝置選取器。 您可以程式設計方式選擇其中一個傳回的 **DeviceInformation** 物件，或顯示 UI 讓使用者選取裝置，然後使用它來設定 [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) 屬性。

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  裝置輸入節點

裝置輸入節點會將音訊從連接到系統的音訊擷取裝置 (例如麥克風) 送入圖形中。 建立 [**DeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) 物件，其藉由呼叫 [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218) 來使用系統的預設音訊擷取裝置。 提供 [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724)，讓系統最佳化指定類別的音訊管線。

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

若要為裝置輸入節點指定特定音訊擷取裝置，您可以使用 [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 類別來取得系統可用的音訊擷取裝置清單，其作法是呼叫 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 並傳入 [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817) 所傳回的音訊轉譯裝置選取器。 您能以程式設計方式選擇其中一個傳回的 **DeviceInformation** 物件，或顯示 UI 讓使用者選取裝置，然後將它傳遞到 [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218) 中。

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  裝置輸出節點

裝置輸出節點會將音訊從圖形推送至音訊轉譯裝置，例如喇叭或耳機。 透過呼叫 [**CreateDeviceOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn958525) 建立 [**DeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098)。 輸出節點會使用音訊圖的 [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524)。

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  檔案輸入節點

檔案輸入節點可讓您將資料從音訊檔案送入圖形中。 透過呼叫 [**CreateFileInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914226) 建立 [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108)。

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   檔案輸入節點支援下列檔案格式：mp3、wav、wma、m4a
-   設定 [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) 屬性，以在檔案中指定應該開始播放的時間位移。 如果這個屬性為 Null，則會使用檔案的開頭。 設定 [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) 屬性，以在檔案中指定應該結束播放的時間位移。 如果這個屬性為 Null，則會使用檔案的結尾。 開始時間值必須小於結束時間值，而結束時間值必須小於或等於音訊檔案的持續時間，檢查 [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn914116) 屬性值即可判斷此值。
-   呼叫 [**Seek**](https://msdn.microsoft.com/library/windows/apps/dn914127) 並指定檔案中播放位置應移至的時間位移，以尋找音訊檔案中的位置。 指定的值必須在 [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) 與 [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) 範圍內。 使用唯讀的 [**Position**](https://msdn.microsoft.com/library/windows/apps/dn914124) 屬性，取得節點的目前播放位置。
-   設定 [**LoopCount**](https://msdn.microsoft.com/library/windows/apps/dn914120) 屬性，啟用音訊檔案的循環播放。 若不為 Null，這個值表示初始播放後將要播放該檔案的次數。 所以，例如將 **LoopCount** 設定為 1 會導致檔案總計播放 2 次，而設定為 5 則會導致檔案總計播放 6 次。 將 **LoopCount** 設定為 Null 會導致檔案無限循環播放。 若要停止循環播放，將此值設定為 0。
-   設定 [**PlaybackSpeedFactor**](https://msdn.microsoft.com/library/windows/apps/dn914123)，以調整音訊檔案的速度播放。 值為 1 表示檔案的原始速度、0.5 為一半速度，而 2 則是雙倍速度。

##  檔案輸出節點

檔案輸出節點可讓您將音訊資料從圖形引導至音訊檔中。 透過呼叫 [**CreateFileOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914227) 建立 [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133)。

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   檔案輸出節點支援下列檔案格式：mp3、wav、wma、m4a
-   呼叫 [**AudioFileOutputNode.FinalizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914140) 之前，您必須呼叫 [**AudioFileOutputNode.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914144) 來停止節點的處理程序，否則會擲回例外狀況。

##  音訊框架輸入節點

音訊框架輸入節點可讓您將在自己的程式碼中產生的音訊資料推送至音訊圖中。 這會促成一些案例，例如建立自訂軟體合成器。 透過呼叫 [**CreateFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914230) 建立 [**AudioFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914147)。

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

當音訊圖準備開始處理音訊資料的下一個配量時，會引發 [**FrameInputNode.QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn958507) 事件。 您可在處理常式中將自訂產生的音訊資料提供給此事件。

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   傳遞至 **QuantumStarted** 事件處理常式的 [**FrameInputNodeQuantumStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958533) 物件會公開 [**RequiredSamples**](https://msdn.microsoft.com/library/windows/apps/dn958534) 屬性，以指出音訊圖需要多少樣本才能填滿所要處理的配量。
-   呼叫 [**AudioFrameInputNode.AddFrame**](https://msdn.microsoft.com/library/windows/apps/dn914148)，將填滿音訊資料的 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 物件傳遞至圖形中。
-   **GenerateAudioData** 協助程式方法的範例實作如下所示。

若要在 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 中填入音訊資料，您必須能夠存取音訊框架的底層記憶體緩衝區。 若要這麼做，您必須在您的命名空間內新增下列程式碼，以初始化 **IMemoryBufferByteAccess** COM 介面。

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

下列程式碼顯示 **GenerateAudioData** 協助程式方法的範例實作，它可以建立 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 並填入音訊資料。

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   因為此方法會存取底層 Windows 執行階段類型的原始緩衝區，所以必須使用 **unsafe** 關鍵字來宣告它。 您也必須在 Microsoft Visual Studio 中設定您的專案，以允許不安全的程式碼編譯，其做法是開啟專案的 [屬性]**** 頁面、按一下 [建置]**** 屬性頁，然後選取 [容許 Unsafe 程式碼]**** 核取方塊。
-   將所需的緩衝區大小傳入至建構函式，以在 **Windows.Media** 命名空間中初始化 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 的新執行個體。 緩衝區大小是樣本數目乘以每個樣本的大小。
-   透過呼叫 [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)，以取得音訊框架的 [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454)。
-   呼叫 [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457)，從音訊緩衝區取得 [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) COM 介面的執行個體。
-   呼叫 [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506)，取得原始音訊緩衝區資料的指標並將它轉換為音訊資料的範例資料類型。
-   使用資料填滿緩衝區，並傳回 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 以便提交至音訊圖中。

##  音訊框架輸出節點

音訊框架輸出節點可讓您使用所建立的自訂程式碼，接收和處理來自音訊圖的音訊資料輸出。 其中一個範例案例就是執行音訊輸出的訊號分析。 透過呼叫 [**CreateFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914233) 以建立 [**AudioFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914166)。

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

當音訊圖已完成音訊資料的配量處理時，會引發 [**AudioGraph.QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) 事件。 您可以在此事件的處理常式中存取音訊資料。

[!code-cs[QuantumProcessed](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumProcessed)]

-   呼叫 [**GetFrame**](https://msdn.microsoft.com/library/windows/apps/dn914171)，以取得填滿圖形中音訊資料的 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) 物件。
-   **ProcessFrameOutput** 協助程式方法的範例實作如下所示。

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   如上述的音訊框架輸入節點範例，您必須宣告 **IMemoryBufferByteAccess** COM 介面並設定您的專案允許不安全的程式碼，才能存取底層音訊緩衝區。
-   透過呼叫 [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)，以取得音訊框架的 [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454)。
-   透過呼叫 [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457) 以從音訊緩衝區取得 **IMemoryBufferByteAccess** COM 介面的執行個體。
-   透過呼叫 **IMemoryBufferByteAccess.GetBuffer** 以取得原始音訊緩衝區資料的指標，並將它轉換為音訊資料的範例資料類型。

## 節點連線和副混音節點

所有輸入節點類型都會公開 **AddOutgoingConnection**方法，此方法會將節點所產生的音訊路由傳送至傳入方法中的節點。 下列範例會將 [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) 連接到 [**AudioDeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098)，這是可供在裝置的喇叭上播放音訊檔案的簡單設定。

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

您可以建立從一個輸入節點到其他節點的多個連線。 下列範例會新增從 [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) 到 [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133) 的另一個連線。 現在，音訊檔案的音訊會播放到裝置的喇叭，也會寫出至音訊檔案。

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

輸出節點也可以接收來自其他節點的多個連線。 下列範例會建立從 [**AudioDeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) 到 [**AudioDeviceOutput**](https://msdn.microsoft.com/library/windows/apps/dn914098) 節點的連線。 因為輸出節點有來自檔案輸入節點和裝置輸入節點的連線，所以輸出會包含這兩個來源的音訊混合。 **AddOutgoingConnection** 提供的多載可讓您為通過連線的訊號指定增益值。

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

雖然輸出節點可以接受來自多個節點的連線，但您可以從一或多個節點建立訊號的中繼混音，再將混音傳遞至輸出。 例如，您可以設定層級或將效果套用至圖形中音訊訊號的子集。 若要這麼做，請使用 [**AudioSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914247)。 您可以從一或多個輸入節點或其他副混音節點連接到某個副混音節點。 在下列範例中，會使用 [**AudioGraph.CreateSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914236) 建立新的副混音節點。 然後，會新增從檔案輸入節點和框架輸出節點至副混音節點的連線。 最後，副混音節點會連接到檔案輸出節點。

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## 開始和停止音訊圖節點

呼叫 [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244) 時，音訊圖會開始處理音訊資料。 每個節點類型都提供可導致個別節點開始或停止處理資料的 **Start** 和 **Stop** 方法。 呼叫 [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245) 時，在所有節點中處理的所有音訊都會停止 (不管個別節點的狀態為何)，但在音訊圖停止時可以設定每個節點的狀態。 例如，您可以在圖形停止時在個別節點上呼叫 **Stop**，然後再呼叫 **AudioGraph.Start**，而個別節點會維持在停止的狀態。

所有節點類型都會公開 **ConsumeInput** 屬性，若設定為 false，則可允許節點繼續音訊處理，但使它無法取用從其他節點輸入的任何音訊資料。

所有節點類型都會公開 **Reset** 方法，該方法會導致節點捨棄目前在其緩衝區中的所有音訊資料。

## 新增音訊效果

音訊圖 API 可讓您將音訊效果新增到圖形中每一種類型的節點。 輸出節點、輸入節點和副混音節點都可擁有不限數量的音訊效果，但僅限於硬體的功能。下列範例示範如何將內建回音效果加入至副混音節點。

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   所有音訊效果都會實作 [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044)。 每個節點都會公開 **EffectDefinitions** 屬性，以表示套用至該節點的效果清單。 將效果的定義物件新增至清單，即可新增該效果。
-   **Windows.Media.Audio** 命名空間中提供了數個效果定義類別。 其中包括：
    -   [**EchoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914276)
    -   [**EqualizerEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914287)
    -   [**LimiterEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914306)
    -   [**ReverbEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914313)
-   您可以建立自己的音訊效果以實作 [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044)，然後將這些效果套用到音訊圖中的任何節點。
-   每個節點類型都會公開一個 **DisableEffectsByDefinition** 方法，以停用節點的 **EffectDefinitions** 清單中使用指定的定義新增的所有效果。 **EnableEffectsByDefinition** 會透過指定的定義啟用效果。

 

 







<!--HONumber=Jun16_HO4-->


