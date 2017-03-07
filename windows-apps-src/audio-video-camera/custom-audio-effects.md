---
author: drewbatgit
Description: "本文章明如何建立能實作 IBasicAudioEffect 介面以允許您為音訊串流建立自訂效果的 Windows 執行階段元件。"
title: "自訂音訊效果"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 360faf3f-7e73-4db4-8324-3391f801d827
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1d8ced4a3dfcf0994d969ad045550ea0cc44a2af
ms.lasthandoff: 02/08/2017

---

# <a name="custom-audio-effects"></a>自訂音訊效果

本文章說明如何建立能實作 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 介面來為音訊串流建立自訂效果的 Windows 執行階段元件。 自訂效果可以搭配數個不同的 Windows 執行階段 API 使用，其中包括能提供裝置相機存取的 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124)、能允許您由媒體剪輯建立複雜組合的 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646)，以及能允許您快速組合由不同的音訊輸入、輸出及次混音節點所組成之圖形的 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph)。

## <a name="add-a-custom-effect-to-your-app"></a>將自訂效果新增到您的 App


自訂音訊訊效果是在實作 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 介面之類別中定義。 此類別不能直接包含在您 App 的專案中。 您必須改為使用 Windows 執行階段元件來裝載您的音訊效果類別。

**為您的音訊效果新增 Windows 執行階段元件**

1.  在 Microsoft Visual Studio 中，將您的方案開啟，並移至 **\[檔案\]** 功能表，然後選取 **\[新增\] -&gt; \[新專案\]**。
2.  選取 **\[Windows 執行階段元件 (通用 Windows)\]** 專案類型。
3.  針對此範例，請將專案命名為 *AudioEffectComponent*。 此名稱將會由稍後的程式碼所參考。
4.  按一下 **\[確定\]**。
5.  專案範本會建立名為 Class1.cs 的類別。 在 **\[方案總管\]** 中，以滑鼠右鍵按一下 Class1.cs 圖示，然後選取 **\[重新命名\]**。
6.  將檔案重新命名為 *ExampleAudioEffect.cs*。 Visual Studio 將會顯示提示，詢問您是否要將所有參照更新為新的名稱。 按一下 \[是\]****。
7.  開啟 **ExampleAudioEffect.cs** 並更新類別定義以實作 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 介面。


[!code-cs[ImplementIBasicAudioEffect](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetImplementIBasicAudioEffect)]

您必須在您的效果類別檔案中包含下列命名空間，以存取本文章的範例中所使用的所有類型。

[!code-cs[EffectUsing](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetEffectUsing)]

## <a name="implement-the-ibasicaudioeffect-interface"></a>實作 IBasicAudioEffect 介面

您的音訊效果必須實作 [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) 介面的所有方法和屬性。 本節將引導您完成此介面的簡單實作，以建立基本的回音效果。

### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties 屬性

系統將會檢查 [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.SupportedEncodingProperties) 屬性來判斷您效果所支援的編碼屬性。 請注意，如果您效果的取用者無法使用您所指定的屬性來編碼音訊，系統將會在您的效果上呼叫 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) 並將您的效果從音訊管線中移除。 在這個範例中，[**AudioEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.AudioEncodingProperties) 物件會建立並新增到傳回的清單，以支援 44.1 kHz 及 48 kHz 的 32 位元浮點單聲道編碼。

[!code-cs[SupportedEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSupportedEncodingProperties)]

### <a name="setencodingproperties-method"></a>SetEncodingProperties 方法

系統會在您的效果上呼叫 [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) 來讓您知道效果正在運作之音訊串流的編碼屬性。 為了實作回音效果，本範例會使用緩衝區以儲存一秒的音訊資料。 這個方法能根據音訊編碼的取樣率，提供針對一秒音訊中的範例數目初始化緩衝區大小的機會。 延遲效果也會使用整數計數器以追蹤延遲緩衝區中的目前位置。 由於系統會在效果新增到音訊管道時呼叫 **SetEncodingProperties**，這是將該值初始化為 0 的好時機。 您也可以擷取傳遞到此方法的 **AudioEncodingProperties** 物件，以用於效果中的其他地方。

[!code-cs[DeclareEchoBuffer](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDeclareEchoBuffer)]
[!code-cs[SetEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetEncodingProperties)]


### <a name="setproperties-method"></a>SetProperties 方法

[**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) 方法允許使用您效果的 App 調整效果參數。 屬性將會以包含屬性名稱和值的 [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) 對應進行傳遞。

[!code-cs[SetProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetProperties)]

這個簡單的範例會根據 **Mix** 屬性的值，將目前的音訊範例與來自延遲緩衝區的值混合。 將會宣告屬性，並使用 TryGetValue 來取得由呼叫 App 所設定的值。 如果沒有設定任何值，將會使用 0.5 的預設值。 請注意，這是唯讀的屬性。 屬性值必須使用 **SetProperties** 進行設定。

[!code-cs[MixProperty](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetMixProperty)]

### <a name="processframe-method"></a>ProcessFrame 方法

[**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) 方法可以讓您的效果修改串流的音訊資料。 此方法會於每個框架呼叫一次，並會被傳遞 [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext) 物件。 此物件包含輸入 [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioFrame) 物件 (該物件包含要處理的傳入框架)，以及輸出 **AudioFrame** 物件 (您將會針對該物件寫入會傳遞至剩餘音訊管線的音訊資料)。 音訊框架是代表音訊資料片段的音訊範例緩衝區。

存取 **AudioFrame** 的資料緩衝區需要 COM Interop，因此您應該將 **System.Runtime.InteropServices** 命名空間包含在效果類別檔案中，然後將下列程式碼新增到命名空間內，使您的效果能匯入存取音訊緩衝區的介面。

[!code-cs[COMImport](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetComImport)]

> [!NOTE]
> 因為此技術會存取原生、未受管理的影像緩衝區，您必須將您的專案設定為允許不安全的程式碼。
> 1.  在 \[方案總管\] 中，以滑鼠右鍵按一下 \[AudioEffectComponent\] 專案，然後選取 \[屬性\]****。
> 2.  選取 \[建置\]**** 索引標籤。
> 3.  選取 \[允許 Unsafe 程式碼\]**** 核取方塊。

 

現在您可以將 **ProcessFrame** 方法實作新增到效果。 首先，此方法會同時從輸入和輸出音訊框架取得 [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioBuffer) 物件。 請注意，輸出框架會針對寫入開啟，而輸入框架則會針對讀取開啟。 接下來，將會透過呼叫 [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046) 來為每個緩衝區取得 [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671)。 然後，透過將 **IMemoryBufferReference** 物件轉型為 **IMemoryByteAccess** (於上方定義的 COM interop 介面)，然後呼叫 **GetBuffer**，來取得實際的資料緩衝區。

現在您已取得資料緩衝區，您可以從輸入緩衝區讀取，並寫入至輸出緩衝區。 針對輸入緩衝區中的每個範例，將會取得其值並乘以 1 - **Mix** 以設定效果的乾聲訊號值。 接下來，將會從目前在回音緩衝區中的位置擷取範例，並乘以 **Mix** 以設定效果的濕聲值。 輸出範例將會以乾聲和濕聲值的總和設定。 最後，每個輸入範例都會儲存在回音緩衝區中，且目前的範例索引會遞增。

[!code-cs[ProcessFrame](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetProcessFrame)]



### <a name="close-method"></a>Close 方法

系統將會在效果應該關閉時，呼叫您類別上的 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) 方法。 您應該使用此方法來處置您已建立的任何資源。 方法的引數為 [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason)，可讓您知道效果是否正常關閉、是否有發生錯誤，或是效果是否不支援所需的編碼格式。

[!code-cs[Close](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetClose)]

### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames 方法

在您的效果應該重設時，便會呼叫 [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) 方法。 此情況的其中一個典型案例是您的效果會儲存先前已處理的畫面，以用於處理目前的畫面之上。 呼叫此方法時，您應該處置先前已儲存的框架組合。 除了針對累計的音訊框架之外，此方法也可以用來重設任何與先前框架相關的狀態。

[!code-cs[DiscardQueuedFrames](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDiscardQueuedFrames)]

### <a name="timeindependent-property"></a>TimeIndependent 屬性

[**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803) 屬性能讓系統知道您的效果不需要統一計時。 當設定為 true 時，系統將可以使用能增強效果效能的最佳化功能。

[!code-cs[TimeIndependent](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetTimeIndependent)]

### <a name="useinputframeforoutput-property"></a>UseInputFrameForOutput 屬性

將 [**UseInputFrameForOutput**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.UseInputFrameForOutput) 屬性設定為 **true**，以告訴系統您的效果會將其輸出寫入到傳入 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) 之 [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext) 的 [**InputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.InputFrame)，而非寫入 [**OutputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.OutputFrame)。 

[!code-cs[UseInputFrameForOutput](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetUseInputFrameForOutput)]


## <a name="adding-your-custom-effect-to-your-app"></a>將您的自訂效果新增到您的 App


如果要從您的 App 使用您的音訊效果，您必須將針對效果專案的參照新增到您的 App。

1.  在 \[方案總管\] 中，於您的專案下方，以滑鼠右鍵按一下 \[參考\]****，然後選取 \[加入參考\]****。
2.  展開 \[專案\]**** 索引標籤，選取 \[方案\]****，然後選取您效果專案名稱的核取方塊。 針對此範例，該名稱為 *AudioEffectComponent*。
3.  按一下 \[確定\]****

如果您的音訊效果類別已宣告為不同的命名空間，請務必將該命名空間包含在程式碼檔案中。

[!code-cs[UsingAudioEffectComponent](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUsingAudioEffectComponent)]

### <a name="add-your-custom-effect-to-an-audiograph-node"></a>將自訂效果新增到 AudioGraph 節點
如需使用音訊圖的一般資訊，請參閱[音訊圖](audio-graphs.md)。 下列程式碼片段會示範如何將本文的範例回音效果新增到音訊圖節點。 首先，將會建立 [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet)，並設定由效果所定義的 **Mix** 值。 接下來，將會呼叫 [**AudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.AudioEffectDefinition) 建構函式，以傳遞自訂效果類型和屬性集的完整類別名稱。 最後，效果定義會新增到現有 [**FileInputNode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.CreateAudioFileInputNodeResult.FileInputNode) 的 [**EffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioFileInputNode.EffectDefinitions) 屬性，使自訂效果可以處理發出的音訊。 

[!code-cs[AddCustomEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddCustomEffect)]

在將自訂效果新增到節點之後，可以透過呼叫 [**DisableEffectsByDefinition**](https://msdn.microsoft.com/library/windows/apps/dn958480) 並傳入 **AudioEffectDefinition** 物件來停用它。 如需在 App 中使用音訊圖的詳細資訊，請參閱 [AudioGraph](audio-graphs.md)。

### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>將您的自訂效果新增到 MediaComposition 中的短片

下列程式碼片段會示範如何將自訂音訊效果新增到媒體組合中的視訊短片和背景曲目。 如需從視訊短片建立媒體組合並新增背景曲目的一般指導方針，請參閱[媒體組合和編輯](media-compositions-and-editing.md)。

[!code-cs[AddCustomAudioEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddCustomAudioEffect)]



## <a name="related-topics"></a>相關主題
* [簡單的相機預覽存取](simple-camera-preview-access.md)
* [媒體組合和編輯](media-compositions-and-editing.md)
* [Win2D 文件](http://go.microsoft.com/fwlink/p/?LinkId=519078)
* [媒體播放](media-playback.md)

 




