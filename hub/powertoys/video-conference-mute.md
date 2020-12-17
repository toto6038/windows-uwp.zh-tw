---
title: Windows 10 的 Powertoy 視訊會議靜音公用程式
description: 一種公用程式，可讓使用者快速地將麥克風靜音 (音訊) ，並在使用單一按鍵的會議電話上關閉相機 (影片) ，不論應用程式在電腦上有何焦點。
ms.date: 12/07/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9a7ec901ffc31e565adb94a1a043fd149064c12
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97611981"
---
# <a name="video-conference-mute-preview"></a> (預覽) 的影片會議靜音

> [!IMPORTANT]
> 這是預覽功能，僅包含在 Powertoy 的發行前版本中。 執行這個發行前版本需要 Windows 10 1903 (組建 18362) 或更新版本。

無論應用程式在電腦上有何焦點，都能快速地將麥克風 (音訊) 並關閉相機 (影片) 在使用單一按鍵的會議上進行。

## <a name="usage"></a>使用量

使用影片會議靜音的預設設定為：

- <kbd>⊞勝利</kbd> +<kbd>N</kbd>同時切換音訊和影片
- <kbd>⊞勝利</kbd> +<kbd>Shift</kbd> +<kbd>A</kbd>切換麥克風
- <kbd>⊞勝利</kbd> +<kbd>Shift</kbd> +切換影片的<kbd>O</kbd>

![音訊和影片靜音通知螢幕擷取畫面](../images/pt-video-audio-mute-notification.png)

使用麥克風和/或攝影機切換快速鍵時，您會看到一個小工具欄視窗顯示，讓您知道您的麥克風和相機是否設定為 [開啟]、[關閉] 或 [未使用]。 您可以在 [Powertoy 設定] 的 [視訊會議靜音] 索引標籤中設定此工具列的位置。

> [!NOTE]
> 請記住，您必須先安裝並執行 [powertoy 的發行前版本/實驗版](https://github.com/microsoft/PowerToys/releases/) ，並在 [powertoy 設定] 中啟用 [視訊會議靜音] 功能，才能使用此公用程式。

## <a name="settings"></a>設定

[Powertoy 設定] 中的 [視訊會議靜音] 索引標籤提供下列選項：

- **快速鍵：** 變更用來將麥克風、相機或兩者結合在一起的快速鍵。
- **選取的麥克風：** 選取您電腦上此公用程式將設為目標的麥克風。
- **選取的相機：** 選取此公用程式將設為目標之電腦上的相機。
- **攝影機重迭影像：** 選取當相機關閉時將用來作為預留位置的影像。  (預設情況下，當您的相機與此公用程式) 關閉時，就會出現黑色畫面。
- **工具列：** 判斷開啟麥克風的位置 *，* 在預設的)  (右上角切換時，會顯示相機工具列。
- **顯示工具列：** 選取您是否偏好只在主要監視器上顯示工具列 (預設) 或所有監視器。
- **Unmuted 攝影機和麥克風時隱藏工具列**：可使用核取方塊來切換此選項。

![Powertoy 設定中的影片會議靜音選項](../images/pt-video-conference-mute-settings.png)

## <a name="how-does-this-work-under-the-hood"></a>這在幕後的運作方式

應用程式會以不同的方式與音訊和影片互動。

如果攝影機停止運作，使用它的應用程式通常不會復原，直到 API 進行完整重設為止。 若要在應用程式中使用相機時開啟和關閉全球隱私權攝影機，通常會損毀且無法復原。

那麼，Powertoy 如何處理此問題，以便您可以繼續進行串流？

- **音訊：** Powertoy 使用 Windows 中的全域麥克風靜音 API。  應用程式應該在切換為開啟和關閉時復原。
- **影片：** Powertoy 具有相機的虛擬驅動程式。 影片會透過驅動程式路由傳送，然後回到應用程式。 選取視訊會議靜音快速鍵會阻止影片進行串流，但應用程式仍會認為它正在接收影片，而影片只會取代為黑色或您儲存在設定中的影像預留位置。

### <a name="debug-the-camera-driver"></a>將相機驅動程式進行偵錯工具

若要檢查攝影機驅動程式，請在您的電腦上尋找此資料夾：

```markdown
C:\Windows\ServiceProfiles\LocalService\AppData\Local\Temp\PowerToysVideoConference.log
```

您也可以在 `PowerToysVideoConferenceVerbose.flag` 相同的目錄中建立空的，以啟用驅動程式中的詳細資訊記錄模式。

## <a name="known-issues"></a>已知問題

若要查看目前在「影片會議靜音」公用程式上開啟的所有已知問題，請參閱 [GitHub 上的 powertoy 追蹤問題 #6246](https://github.com/microsoft/PowerToys/issues/6246)。 Powertoy 開發小組和參與者的團隊正積極努力解決這些問題，並計畫將公用程式保留在發行前，直到解決重要問題為止。

![顯示5個參與者和裝置設定的 Powertoy video 會議螢幕擷取畫面](../images/pt-video-conference.png)
