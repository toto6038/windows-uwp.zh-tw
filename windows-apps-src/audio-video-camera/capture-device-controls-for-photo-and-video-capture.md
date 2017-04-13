---
author: drewbatgit
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: "本文示範如何使用手動裝置控制項來啟用美化的相片和視訊擷取案例，包括光學防手震和平滑變焦。"
title: "相片和視訊擷取的手動相機控制項"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: d0d7a429cf702455d969e1ac1c62def6181e8dd0
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>相片和視訊擷取的手動相機控制項

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文示範如何使用手動裝置控制項來啟用美化的相片和視訊擷取案例，包括光學防手震和平滑變焦。

本文中討論的控制項全都會使用相同的模式新增到您的 app。 首先，檢查 app 目前執行所在的裝置是否支援此控制項。 如果支援此控制項，則為控制項設定所需的模式。 通常，如果目前的裝置不支援特定的控制項，您應該停用或隱藏可讓使用者啟用該功能的 UI 元素。

本文中的程式碼是從[相機手動控制項 SDK 範例](https://go.microsoft.com/fwlink/?linkid=845228)改編而來。 您可以下載範例以查看內容中使用的程式碼，或以此範例做為自己的 app 起點。

> [!NOTE]
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 我們建議您先熟悉該文章中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文章中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

此文章中討論的所有裝置控制項 API 都是 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 命名空間的成員。

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

## <a name="exposure"></a>曝光

[**ExposureControl**](https://msdn.microsoft.com/library/windows/apps/dn278910) 可讓您設定擷取相片或視訊時使用的快門速度。

這個範例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控制項來調整目前的曝光值，以及使用核取方塊來切換自動曝光調整。

[!code-xml[ExposureXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetExposureXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297710) 屬性以查看目前的擷取裝置是否支援 **ExposureControl**。 如果支援此控制項，您可以顯示和啟用此功能的 UI。 將用來指出目前是否使用自動曝光調整的核取方塊的核取狀態設定為 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn278911) 屬性的值。

曝光值必須在裝置所支援的範圍內，並且必須是所支援之分段大小的增量。 檢查 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278919)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn278914) 及 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278930) 屬性 (用來設定對應的滑桿控制項屬性) 以取得目前裝置支援的值。

取消註冊 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件處理常式之後，將滑桿控制項的值設定為 **ExposureControl** 目前的值，如此才不會在設定值時觸發事件。

[!code-cs[ExposureControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureControl)]

在 **ValueChanged** 事件處理常式中，藉由呼叫 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927) 來取得控制項目前的值並設定曝光值。

[!code-cs[ExposureSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureSlider)]

在自動曝光核取方塊的 **CheckedChanged** 事件處理常式中，藉由呼叫 [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn278920) 並傳入布林值來開啟或關閉自動曝光調整。

[!code-cs[ExposureCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureCheckBox)]

> [!IMPORTANT]
> 只有當預覽串流處於執行狀態時，才支援自動曝光模式。 開啟自動曝光之前，請先檢查以確定預覽串流處於執行狀態。

## <a name="exposure-compensation"></a>曝光補償

[**ExposureCompensationControl**](https://msdn.microsoft.com/library/windows/apps/dn278897) 可讓您設定擷取相片或視訊時使用的曝光補償。

這個範例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控制項來調整目前的曝光補償值。

[!code-xml[EvXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetEvXAML)]

請檢查 [Supported](supported-codecs.md) 屬性以查看目前的擷取裝置是否支援 **ExposureCompensationControl**。 如果支援此控制項，您可以顯示和啟用此功能的 UI。

曝光補償值必須在裝置所支援的範圍內，並且必須是所支援之分段大小的增量。 檢查 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278901)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn278899) 及 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278904) 屬性 (用來設定對應的滑桿控制項屬性) 以取得目前裝置支援的值。

取消註冊 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件處理常式之後，將滑桿控制項的值設定為 **ExposureCompensationControl** 目前的值，如此才不會在設定值時觸發事件。

[!code-cs[EvControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvControl)]

在 **ValueChanged** 事件處理常式中，藉由呼叫 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278903) 來取得控制項目前的值並設定曝光值。

[!code-cs[EvValueChanged](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvValueChanged)]

## <a name="flash"></a>閃光燈

[**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) 可讓您啟用或停用閃光燈，或是啟用自動閃光燈，讓系統自動判斷是否要使用閃光燈。 這個控制項也可讓您在支援自動消除紅眼的裝置上使用該功能。 這些設定全部都可套用至相片擷取。 [**TorchControl**](https://msdn.microsoft.com/library/windows/apps/dn279077) 是個別的控制項，可針對視訊擷取開啟或關閉手電筒。

這個範例使用一組選項按鈕，可讓使用者在開啟、關閉及自動閃光燈設定之間做切換。 此外也提供一個核取方塊，可切換消除紅眼和視訊手電筒。

[!code-xml[FlashXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFlashXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837) 屬性以查看目前的擷取裝置是否支援 **FlashControl**。 如果支援此控制項，您可以顯示和啟用此功能的 UI。 如果支援 **FlashControl**，不一定支援自動消除紅眼，因此在啟用 UI 之前，請先檢查 [**RedEyeReductionSupported**](https://msdn.microsoft.com/library/windows/apps/dn297766) 屬性。 由於 **TorchControl** 是與閃光燈控制項分開的，因此，使用它之前，也必須先檢查其 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279081) 屬性。

在每個閃光燈選項按鈕的 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 事件處理常式中，啟用或停用適當的對應閃光燈設定。 請注意，若要設定為一律使用閃光燈，您必須將 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn297733) 屬性設為 true，將 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn297728) 屬性設為 false。

[!code-cs[FlashControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashControl)]

[!code-cs[FlashRadioButtons](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashRadioButtons)]

在消除紅眼核取方塊的處理常式中，將 [**RedEyeReduction**](https://msdn.microsoft.com/library/windows/apps/dn297758) 屬性設定為適當的值。

[!code-cs[紅眼](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetRedEye)]

最後，在視訊手電筒核取方塊的處理常式中，將 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) 屬性設定為適當的值。

[!code-cs[手電筒](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTorch)]

> [!NOTE] 
>  在某些裝置上，除非裝置的預覽串流處於執行狀態且正在主動擷取視訊，否則即使 [**TorchControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) 設定為 true，手電筒也不會發光。 建議的操作順序是開啟視訊預覽，接著將 **Enabled** 設定為 true 來開啟手電筒，然後起始視訊擷取。 在某些裝置上，要在啟動預覽後，手電筒才會亮起。 在其他裝置上，則是在啟動視訊擷取後，手電筒才會亮起。

## <a name="focus"></a>對焦

[**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) 物件支援三種不同的常用方法來調整相機的焦點：連續自動對焦、點選以對焦以及手動對焦。 相機 App 可支援這三種方法全部，但為了方便閱讀起見，本文將分開討論每一種技術。 本節也會討論如何啟用對焦輔助燈。

### <a name="continuous-autofocus"></a>連續自動對焦

啟用連續自動對焦可指示相機動態調整焦點，以嘗試將焦點對準相片或視訊主體。 這個範例使用選項按鈕來開啟和關閉連續自動對焦。

[!code-xml[CAFXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCAFXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 屬性以查看目前的擷取裝置是否支援 **FocusControl**。 接著，檢查 [**SupportedFocusModes**](https://msdn.microsoft.com/library/windows/apps/dn608079) 清單是否包含 [**FocusMode.Continuous**](https://msdn.microsoft.com/library/windows/apps/dn608084) 值以判斷是否支援連續自動對焦，如果包含該值，便顯示連續自動對焦選項按鈕。

[!code-cs[CAF](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCAF)]

在連續自動對焦選項按鈕的 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 事件處理常式中，使用 [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) 屬性來取得控制項的執行個體。 呼叫 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) 來解除鎖定控制項，以防萬一您的 App 先前已呼叫 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) 來啟用其中一種其他對焦模式。

建立一個新的 [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) 物件，然後將 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608090) 屬性設定為 **Continuous**。 將 [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) 屬性設定為適合您 App 案例的值，或是使用者從您 UI 中選取的值。 將 **FocusSettings** 物件傳遞給 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) 方法，然後呼叫 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) 來起始連續自動對焦。

[!code-cs[CafFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCafFocusRadioButton)]

> [!IMPORTANT]
> 只有當預覽串流處於執行狀態時，才支援自動對焦模式。 開啟連續自動對焦之前，請先檢查以確定預覽串流處於執行狀態。

### <a name="tap-to-focus"></a>點選以對焦

點選以對焦技術使用 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) 和 [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) 來指定擷取裝置應該對焦的擷取框架子區域。 對焦區域是由使用者在顯示預覽串流的螢幕上點選來決定。

這個範例使用選項按鈕來啟用和停用點選以對焦模式。

[!code-xml[TapFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetTapFocusXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 屬性以查看目前的擷取裝置是否支援 **FocusControl**。 必須支援 **RegionsOfInterestControl** 且必須至少支援一個區域，才能使用這項技術。 檢查 [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066) 和 [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069) 屬性以判斷是否要顯示或隱藏點選以對焦選項按鈕。

[!code-cs[TapFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocus)]

在點選以對焦選項按鈕的 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 事件處理常式中，使用 [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) 屬性來取得控制項的執行個體。 呼叫 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) 以將控制項鎖定，以防萬一您的 App 先前已呼叫 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) 來啟用連續自動對焦，接著等待使用者點選螢幕以變更焦點。

[!code-cs[TapFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusRadioButton)]

這個範例會在使用者點選螢幕時對區域對焦，然後在使用者再次點選時從該區域移除焦點，就像是一種切換。 使用布林值變數來追蹤目前的切換狀態。

[!code-cs[IsFocused](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsFocused)]

下一步是透過處理目前正在顯示擷取預覽串流之 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 的 [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) 事件，在使用者點選螢幕時接聽事件。 如果相機目前沒有進行預覽，或是點選以對焦模式已停用，請從處理常式返回而不進行任何動作。

如果將追蹤變數 *_isFocused* 切換為 false，且如果相機目前並未進行對焦 (由 **FocusControl** 的 [**FocusState**](https://msdn.microsoft.com/library/windows/apps/dn608074) 屬性判斷)，請開始點選以對焦程序。 從傳遞給處理常式的事件引數取得使用者的點選位置。 這個範例也會利用這個機會挑選將對焦的區域大小。 在此案例中，大小為擷取元素最小尺寸的 1/4。 將點選位置和區域大小傳遞給下一節中定義的 **TapToFocus** 協助程式方法。

如果將 *\_isFocused* 切換設定為 true，使用者點選時，應該會從先前的區域清除焦點。 這是在下面所示的 **TapUnfocus** 協助程式方法中進行。

[!code-cs[TapFocusPreviewControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusPreviewControl)]

在 **TapToFocus** 協助程式方法中，請先將 *\_isFocused* 切換設定為 true，以便在下次點選螢幕時將焦點從點選的區域釋出。

這個協助程式方法的下一個工作，是要決定位於將被指派給對焦控制項之預覽串流內的矩形。 這需要兩個步驟。 第一個步驟是決定預覽串流在 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 控制項內佔用的矩形。 這取決於預覽串流的尺寸和裝置的方向。 本節結尾顯示的協助程式方法 **GetPreviewStreamRectInControl** 會執行此工作，並傳回包含預覽串流的矩形。

**TapToFocus** 的下一個工作是將點選位置和所需的焦點矩形大小 (在 **CaptureElement.Tapped** 事件處理常式內判斷) 轉換成擷取串流內的座標。 本節稍後顯示的 **ConvertUiTapToPreviewRect** 協助程式方法會執行這項轉換，然後以擷取串流座標傳回將被要求對焦的矩形。

既然已取得目標矩形，請建立一個新 [**RegionOfInterest**](https://msdn.microsoft.com/library/windows/apps/dn279058) 物件，將 [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn279062) 屬性設定為在先前步驟中取得的目標矩形。

取得擷取裝置的 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788)。 建立新的 [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) 物件，並檢查以確定 **FocusControl** 支援 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608076) 和 [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086)，然後將它們設定成您所需的值。 呼叫 **FocusControl** 上的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) 以讓您的設定生效，並向裝置發出訊號來開始對指定的區域進行對焦。

接著，取得擷取裝置的 [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) 並呼叫 [**SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070) 來設定使用中區域。 如果裝置支援多個感興趣的區域，便可在裝置上設定多個區域，但此範例只設定單一區域。

最後，在 **FocusControl** 上呼叫 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) 來起始對焦。

> [!IMPORTANT]
> 實作點選以對焦時，作業的程序非常重要。 您應該依照以下順序呼叫這些 API：
>
> 1. [**FocusControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070)
> 3. [**FocusControl.FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)

[!code-cs[TapToFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapToFocus)]

在 **TapUnfocus** 協助程式方法中，取得 **RegionsOfInterestControl** 並呼叫 [**ClearRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279068) 以清除已向 **TapToFocus** 協助程式方法內的控制項註冊的區域。 然後，取得 **FocusControl** 並呼叫 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)，讓裝置在不對特定區域對焦的情況下重新對焦。

[!code-cs[TapUnfocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapUnfocus)]

**GetPreviewStreamRectInControl** 協助程式方法會使用預覽串流的解析度和裝置的方向，來決定包含預覽串流之預覽元素內的矩形，其中會剪除控制項可能提供來維持串流外觀比例的所有上下黑邊的邊框間距。 這個方法也會使用在[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中找到的基本媒體擷取範例程式碼中定義的類別成員變數。

[!code-cs[GetPreviewStreamRectInControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetGetPreviewStreamRectInControl)]

**ConvertUiTapToPreviewRect** 協助程式方法會將點選事件的位置、所需的對焦區域大小，以及包含從 **GetPreviewStreamRectInControl** 協助程式方法取得之預覽串流的矩形，做為引數。 這個方法會使用這些值和裝置的目前方向，來計算包含所需區域的預覽串流內的矩形。 同樣地，這個方法也會使用在[使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)中找到的基本媒體擷取範例程式碼中定義的類別成員變數。

[!code-cs[ConvertUiTapToPreviewRect](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetConvertUiTapToPreviewRect)]

### <a name="manual-focus"></a>手動對焦

手動對焦技術使用 **Slider** 控制項來設定擷取裝置的目前對焦深度。 有一個選項按鈕用來開啟和關閉手動對焦。

[!code-xml[ManualFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetManualFocusXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837) 屬性以查看目前的擷取裝置是否支援 **FocusControl**。 如果支援此控制項，您可以顯示和啟用此功能的 UI。

對焦值必須在裝置所支援的範圍內，並且必須是所支援之分段大小的增量。 檢查 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn297808)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn297802) 及 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn297833) 屬性 (用來設定對應的滑桿控制項屬性) 以取得目前裝置支援的值。

取消註冊 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件處理常式之後，將滑桿控制項的值設定為 **FocusControl** 目前的值，如此才不會在設定值時觸發事件。

[!code-cs[對焦](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocus)]

在手動對焦選項按鈕的 **Checked** 事件處理常式中，取得 **FocusControl** 物件並呼叫[**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075)，以防萬一您的 App 先前已透過呼叫 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) 將焦點解除鎖定。

[!code-cs[ManualFocusChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetManualFocusChecked)]

在手動對焦滑桿的 **ValueChanged** 事件處理常式中，藉由呼叫 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn297828) 來取得控制項目前的值並設定對焦值。

[!code-cs[FocusSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusSlider)]

### <a name="enable-the-focus-light"></a>啟用對焦燈

在支援對焦輔助燈的裝置上，您可以啟用該功能來協助裝置對焦。 這個範例使用核取方塊來啟用或停用對焦輔助燈。

[!code-xml[FocusLightXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFocusLightXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 屬性以查看目前的擷取裝置是否支援 **FlashControl**。 同時也檢查 [**AssistantLightSupported**](https://msdn.microsoft.com/library/windows/apps/dn608066) 以確定是否也支援輔助燈。 如果兩者都支援，您可以顯示和啟用此功能的 UI。

[!code-cs[FocusLight](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLight)]

在 **CheckedChanged** 事件處理常式中，取得擷取裝置 [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) 物件。 設定 [**AssistantLightEnabled**](https://msdn.microsoft.com/library/windows/apps/dn608065) 屬性以啟用或停用對焦燈。

[!code-cs[FocusLightCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLightCheckBox)]

## <a name="iso-speed"></a>ISO 速度

[**IsoSpeedControl**](https://msdn.microsoft.com/library/windows/apps/dn297850) 可讓您設定擷取相片或視訊時使用的 ISO 速度。

這個範例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控制項來調整目前的曝光補償值，以及使用核取方塊來切換自動 ISO 速度調整。

[!code-xml[IsoXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetIsoXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297869) 屬性以查看目前的擷取裝置是否支援 **IsoSpeedControl**。 如果支援此控制項，您可以顯示和啟用此功能的 UI。 將用來指出目前是否使用自動 ISO 速度調整的核取方塊核取狀態設定為 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn608093) 屬性的值。

ISO 速度值必須在裝置所支援的範圍內，並且必須是所支援之分段大小的增量。 檢查 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn608095)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn608094) 及 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn608129) 屬性 (用來設定對應的滑桿控制項屬性) 以取得目前裝置支援的值。

取消註冊 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件處理常式之後，將滑桿控制項的值設定為 **IsoSpeedControl** 目前的值，如此才不會在設定值時觸發事件。

[!code-cs[IsoControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoControl)]

在 **ValueChanged** 事件處理常式中，藉由呼叫 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) 來取得控制項目前的值並設定 ISO 速度值。

[!code-cs[IsoSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoSlider)]

在自動 ISO 速度核取方塊的 **CheckedChanged** 事件處理常式中，藉由呼叫 [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn608127) 來開啟自動 ISO 速度調整。 呼叫 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) 並傳入滑桿控制項目前的值，以關閉自動 ISO 速度調整。

[!code-cs[IsoCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoCheckBox)]

## <a name="optical-image-stabilization"></a>光學防手震

光學防手震 (OIS) 透過機械方式操作硬體擷取裝置，可提供優於數位防手震的結果，進而穩定所擷取的視訊資料流。 在不支援 OIS 的裝置上，您可以使用 VideoStabilizationEffect 對所擷取的視訊執行數位防手震。 如需詳細資訊，請參閱[視訊擷取的效果](effects-for-video-capture.md)。

檢查 [**OpticalImageStabilizationControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926689) 屬性，以判斷目前的裝置是否支援 OIS。

OIS 控制項支援開啟、關閉和自動三種模式，這表示裝置會動態判斷 OIS 是否可改進媒體擷取，若是可以，則會啟用 OIS。 若要判斷裝置是否支援特定的模式，請查看 [**OpticalImageStabilizationControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926690) 集合是否包含所需的模式。

將 [**OpticalImageStabilizationControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926691) 設為所需的模式，以啟用或停用 OIS。

[!code-cs[SetOpticalImageStabilizationMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetOpticalImageStabilizationMode)]

## <a name="powerline-frequency"></a>電源頻率
有些相機裝置支援抗閃爍處理，而這取決於了解目前環境中電源的 AC 頻率。 有些裝置支援自動決定電源頻率，有些則需要手動設定頻率。 下列程式碼範例顯示如何判斷裝置上的電源頻率支援，以及必要時如何手動設定頻率。 

首先，呼叫 **VideoDeviceController** 方法 [**TryGetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206898)，傳入一個 [**PowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.PowerlineFrequency) 類型的輸出參數，如果此呼叫失敗，表示目前的裝置不支援電源頻率控制項。 如果支援此功能，您可以試著設定自動模式來判斷裝置上是否可使用自動模式。 透過呼叫 [**TrySetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206899) 並傳入 **Auto** 值以執行此操作。 如果呼叫成功，表示裝置支援您的自動電源頻率。 如果裝置支援電源頻率控制器，但不支援自動頻率偵測，您仍然可以使用 **TrySetPowerlineFrequency** 以手動方式設定頻率。 在這個範例中，**MyCustomFrequencyLookup** 是您為了判斷裝置目前位置的正確頻率而實作的自訂方法。 

[!code-cs[PowerlineFrequency](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetPowerlineFrequency)]

## <a name="white-balance"></a>白平衡

[**WhiteBalanceControl**](https://msdn.microsoft.com/library/windows/apps/dn279104) 可讓您設定擷取相片或視訊時使用的白平衡。

這個範例使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) 控制項以從內建的色溫預設進行選取，以及使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控制項來進行手動白平衡調整。

[!code-xml[WhiteBalanceXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetWhiteBalanceXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279120) 屬性以查看目前的擷取裝置是否支援 **WhiteBalanceControl**。 如果支援此控制項，您可以顯示和啟用此功能的 UI。 將下拉式方塊的項目設定為 [**ColorTemperaturePreset**](https://msdn.microsoft.com/library/windows/apps/dn278894) 列舉的值。 並將選取的項目設定為 [**Preset**](https://msdn.microsoft.com/library/windows/apps/dn279110) 屬性目前的值。

就手動控制而言，白平衡值必須在裝置所支援的範圍內，並且必須是所支援之分段大小的增量。 檢查 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn279109)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn279107) 及 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn279119) 屬性 (用來設定對應的滑桿控制項屬性) 以取得目前裝置支援的值。 啟用手動控制之前，請檢查以確定最小和最大支援值之間的範圍大於分段大小。 如果不是，則表示目前的裝置不支援手動控制。

取消註冊 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件處理常式之後，將滑桿控制項的值設定為 **WhiteBalanceControl** 目前的值，如此才不會在設定值時觸發事件。

[!code-cs[白平衡](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalance)]

在色溫預設下拉式方塊的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件處理常式中，藉由呼叫 [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) 來取得目前選取的預設並設定控制項的值。 如果選取的預設值不是 **Manual**，請停用手動白平衡滑桿。

[!code-cs[WhiteBalanceComboBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceComboBox)]

在 **ValueChanged** 事件處理常式中，藉由呼叫 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927) 來取得控制項目前的值並設定白平衡值。

[!code-cs[WhiteBalanceSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceSlider)]

> [!IMPORTANT]
> 只有當預覽串流處於執行狀態時，才支援調整白平衡。 設定白平衡值或預設之前，請先檢查以確定預覽串流處於執行狀態。

> [!IMPORTANT]
> **ColorTemperaturePreset.Auto** 預設值會指示系統自動調整白平衡層級。 針對某些情況 (例如擷取每個畫面的白平衡層級應該都相同的相片序列)，您可能會想要將控制項鎖定在目前的自動值。 若要這樣做，請呼叫 [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) 並指定 **Manual** 預設，而不要使用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn279114) 在控制項上設定值。 這會導致裝置鎖定目前的值。 請勿嘗試讀取目前的控制項值，然後將傳回的值傳遞給 **SetValueAsync**，因為這個值不一定是正確的。

## <a name="zoom"></a>縮放

[**ZoomControl**](https://msdn.microsoft.com/library/windows/apps/dn608149) 可讓您設定擷取相片或視訊時使用的縮放比例。

這個範例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控制項來調整目前的縮放比例。 下節將說明如何根據螢幕上的捏合手勢調整縮放。

[!code-xml[ZoomXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetZoomXAML)]

請檢查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819) 屬性以查看目前的擷取裝置是否支援 **ZoomControl**。 如果支援此控制項，您可以顯示和啟用此功能的 UI。

縮放比例值必須在裝置所支援的範圍內，並且必須是所支援之分段大小的增量。 檢查 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn633817)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) 及 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818) 屬性 (用來設定對應的滑桿控制項屬性) 以取得目前裝置支援的值。

取消註冊 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件處理常式之後，將滑桿控制項的值設定為 **ZoomControl** 目前的值，如此才不會在設定值時觸發事件。

[!code-cs[ZoomControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomControl)]

在 **ValueChanged** 事件處理常式中，建立新的 [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) 類別執行個體，將 [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) 屬性設定為縮放滑桿控制項目前的值。 如果 **ZoomControl** 的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) 屬性包含 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)，即表示裝置支援在縮放比例之間順暢轉換。 由於這個模式提供較佳的使用者經驗，因此您通常會想要使用這個值做為 **ZoomSettings** 物件的 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) 屬性。

最後，將您的 **ZoomSettings** 物件傳送給 **ZoomControl** 物件的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) 方法，以變更目前的縮放設定。

[!code-cs[ZoomSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomSlider)]

### <a name="smooth-zoom-using-pinch-gesture"></a>使用捏合手勢進行平滑變焦

如上一節所述，在支援平滑變焦的裝置上，平滑變焦模式可允許擷取裝置在數位縮放比例之間平滑轉換，讓使用者可以在進行擷取操作時動態調整縮放比例，而不會產生不連貫且突兀的轉換。 本節說明如何調整回應捏合手勢的縮放比例。

首先，檢查 [**ZoomControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819) 屬性，以判斷目前的裝置是否支援數位縮放控制項。 接下來，檢查 [**ZoomControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) 來看看它是否包含值 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)，以判斷平滑變焦模式是否可用。

[!code-cs[IsSmoothZoomSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsSmoothZoomSupported)]

在支援多點觸控功能的裝置上，典型的案例是調整以兩指捏合手勢為基礎的縮放係數。 將 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 控制項的 [**ManipulationMode**](https://msdn.microsoft.com/library/windows/apps/br208948) 屬性設為 [**ManipulationModes.Scale**](https://msdn.microsoft.com/library/windows/apps/br227934)，以啟用捏合手勢。 接著，註冊捏合手勢變更大小時引發的 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件。

[!code-cs[RegisterPinchGestureHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterPinchGestureHandler)]

在 **ManipulationDelta** 事件的處理常式中，更新以使用者的捏合手勢變更為基礎的縮放係數。 [**ManipulationDelta.Scale**](https://msdn.microsoft.com/library/windows/apps/br242016) 值代表捏合手勢的比例變更，讓少量增加的捏合大小是稍微大於 1.0 的數字，而少量減少的捏合大小是稍微小於 1.0 的數字。 在這個範例中，目前的縮放控制項值會乘以縮放差異。

在設定縮放係數之前，您必須確定此值不會小於裝置所支援的最小值 (以 [**ZoomControl.Min**](https://msdn.microsoft.com/library/windows/apps/dn633817) 屬性表示)。 此外，確定此值是小於或等於 [**ZoomControl.Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) 值。 最後，您必須確定縮放係數是裝置所支援的縮放分段大小的倍數 (以 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818) 屬性表示)。 如果縮放係數不符合這些需求，當您嘗試在擷取裝置上設定縮放比例時，將會擲回例外狀況。

建立新的 [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) 物件，以在擷取裝置上設定縮放比例。 將 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) 屬性設為 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)，然後將 [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) 屬性設為所需的縮放係數。 最後，呼叫 [**ZoomControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) 以在裝置上設定新的縮放值。 裝置將會順暢地轉換到新的縮放值。

[!code-cs[ManipulationDelta](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
