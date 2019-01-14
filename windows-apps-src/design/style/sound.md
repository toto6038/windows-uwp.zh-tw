---
Description: Sound helps complete an application's user experience, and gives them that extra audio edge they need to match the feel of Windows across all platforms.
label: Sound
title: 音效
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5060012c90ec9cfef093021f44b39321f452e01c
ms.sourcegitcommit: 59f874b6667c3f639d8b0c7eeca886e71bf95614
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2019
ms.locfileid: "9004594"
---
# <a name="sound"></a>音效

![主角圖像](images/header-sound.svg)

有許多方式可以使用音效來增強您的 App。 您可以使用音效來補充其他 UI 元素，讓使用者能透過音效辨識事件。 針對視覺殘障人士而言，音效可以是有效的使用者介面元素。 您可以使用音效來建立一個讓使用者身歷其境的氛圍；例如，您可以在拼圖遊戲的背景中播放詭譎的音樂，或針對恐怖/求生遊戲使用具威脅性的音效。

## <a name="sound-global-api"></a>音效全域 API

UWP 提供一個可輕鬆存取的音效系統，您只要「撥動開關」，即可在整個應用程式體驗沈浸式音訊。

[**ElementSoundPlayer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer) 是 XAML 中的整合式音效系統，當開啟所有預設控制項時，就會自動播放音效。
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
**ElementSoundPlayer** 有三種不同的狀態︰**\[On\]**、**\[Off\]** 和 **\[Auto\]**。

如果設定為 **\[Off\]**，不論您的 App 在何處執行，一律不會播放音效。 如果設定為 **\[On\]**，您的應用程式的音效將會在每個平台上播放。

啟用 ElementSoundPlayer 將會自動啟用空間音訊 (3D 音效)。 若要停用 3D 音效 (但保持音效開啟)，請停用 ElementSoundPlayer 的 **SpatialAudioMode**： 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

**SpatialAudioMode** 屬性可以具備下列值： 
- **Auto**：當音效開啟時將會開啟空間音訊。 
- **Off**：空間音訊一律關閉 (即使音訊開啟)。
- **On**：一律播放空間音訊。

若要深入了解空間音訊，以及 XAML 如何處理此功能，請參閱 [AudioGraph - 空間音訊](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio)。

### <a name="sound-for-tv-and-xbox"></a>電視和 Xbox 的音效

音效是 10 英呎體驗的重要部分，而 **ElementSoundPlayer** 的狀態會預設為 **\[Auto\]**，這表示您的應用程式在 Xbox 上執行時，您才會聽到音效。
若要深入了解電視和 Xbox 的音效運作方式，請參閱 [Xbox 和電視設計](http://go.microsoft.com/fwlink/?LinkId=760736)文章。

## <a name="sound-volume-override"></a>音效音量覆寫

應用程式內的所有音效都可以透過 **Volume** 控制項停用。 不過，應用程式內的音效不能「比系統音效大聲」**。

若要設定應用程式的音效層級，請呼叫︰
```C#
ElementSoundPlayer.Volume = 0.5;
```
其中最大音量 (相對於系統音量) 是 1.0，而最小音量是 0.0 (基本上是靜音)。

## <a name="control-level-state"></a>控制項層級狀態

如果不需要控制項的預設音效，可予以停用。 這可透過控制項上的 **ElementSoundMode** 來達成。

**ElementSoundMode** 有兩種狀態︰**\[Off\]** 和 **\[Default\]**。 未設定時，則為 **\[Default\]**。 如果設定為 **\[Off\]**，則控制項所播放的每個音效都會是靜音，但 *「焦點除外」*。

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>這是正確的音效嗎？

當建立自訂控制項，或變更現有控制項的音效時，請務必了解系統提供的所有音效的用法。

每個音效會與某個基本使用者互動相關，雖然可以將音效自訂成在任何互動時播放，但本節主要說明應使用音效而讓所有 UWP 應用程式維持一致體驗的案例。

### <a name="invoking-an-element"></a>叫用元素

我們的系統中目前最常見的控制項觸發音效為 **Invoke** 音效。 當使用者透過點選/按一下/Enter/空格或按遊戲台的 'A' 按鈕叫用控制項時，就會播放這個音效。

通常，只有在使用者透過[輸入裝置](../input/index.md)明確地以簡單控制項或控制項部分為目標時，才會播放這個音效。

&lt;SelectButtonClick.mp3 音效剪輯在此&gt;

若要從任何控制項事件播放這個音效，只需從 **ElementSoundPlayer** 呼叫 Play 方法，然後傳入 **ElementSound.Invoke**：
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>顯示及隱藏內容

XAML 中有許多飛出視窗、對話方塊及可解除的 UI，而任何觸發其中一個重疊的動作應該呼叫 **Show** 或 **Hide** 音效。

當重疊內容視窗出現時，應該呼叫 **Show** 音效︰

&lt;OverlayIn.mp3 音效剪輯在此&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
相反地，當重疊內容視窗關閉 (或消失關閉) 時，應該呼叫 **Hide** 音效︰

&lt;OverlayOut.mp3 音效剪輯在此&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>頁面內的瀏覽

面板或檢視內應用程式的頁面之間瀏覽時 （請參閱[索引標籤和樞紐](../controls-and-patterns/tabs-pivot.md)），通常是雙向移動。 這表示您可以移至下一個或上一個檢視/面板，而不需離開您目前所在的應用程式頁面。

**MovePrevious** 和 **MoveNext** 音效可達成以此瀏覽概念為主的音訊體驗。

移至清單中被視為「下一個項目」** 的檢視/面板時，呼叫︰

&lt;PageTransitionRight.mp3 音效剪輯在此&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
而移至清單中被視為「上一個項目」** 的上一個檢視/面板時，呼叫︰

&lt;PageTransitionLeft.mp3 音效剪輯在此&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>向後巡覽

從目前頁面瀏覽到應用程式中前一個頁面時，應呼叫 **GoBack** 音效︰

&lt;BackButtonClick.mp3 音效剪輯在此&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>將焦點放在某個元素

**Focus** 音效我們的系統中唯一隱含的音效。 這表示使用者並未直接與任何項目互動，但仍會聽到音效。

使用者瀏覽應用程式時會發生對焦，這可利用遊戲台/鍵盤/遙控器或 Kinect 進行。 **Focus** 音效通常不會在 PointerEntered 或滑鼠暫留事件** 播放。

若要設定控制項以在控制項取得焦點時播放 **Focus** 音效，請呼叫︰

&lt;ElementFocus1.mp3 音效剪輯在此&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>循環播放焦點音效

音效系統預設會在每個瀏覽觸發程序循環播放 4 個不同的音效，這是呼叫 **ElementSound.Focus** 的新增功能。 這表示任意兩個焦點音效不會彼此接連播放。

此循環功能背後的目的是為了避免焦點音效變單調，以及避免無法引起使用者的注意；焦點音效將會最常播放，因此應該最為精緻。

## <a name="related-articles"></a>相關文章

* [針對 Xbox 和電視進行設計](http://go.microsoft.com/fwlink/?LinkId=760736)
* [ElementSoundPlayer 類別文件](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer)
