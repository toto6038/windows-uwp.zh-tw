---
ms.assetid: 3848cd72-eccd-400e-93ff-13649cd81b6c
description: 本文提供針對使用舊版背景媒體模型進行播放之 app 的支援，以及移轉至新的模型的指導方針。
title: 舊版背景媒體播放
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ea8d387becaef171175fd5e91bfc3a1402e79faa
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7965511"
---
# <a name="legacy-background-media-playback"></a>舊版背景媒體播放


本文說明用來新增 UWP app 背景音訊支援的舊版雙處理序模型。 從 Windows 10 版本 1607 開始，背景音訊的單一處理程序模型變得更容易實作。 如需有關背景音訊目前建議的詳細資訊，請參閱[在背景播放媒體](background-audio.md)。 本文用來提供已使用舊版雙處理序模型開發之 app 的支援。

> [!NOTE]
> 開始使用 Windows，版本 1703 中， **BackgroundMediaPlayer**已過時，並且可能無法在未來的 Windows 版本中使用。

## <a name="background-audio-architecture"></a>背景音訊架構

執行背景播放的應用程式包含兩個處理程序。 第一個處理程序是在前景執行的主應用程式，其包含應用程式 UI 和用戶端邏輯。 第二個處理程序是背景播放工作，可如同所有的 UWP app 背景工作一樣實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)。 背景工作包含音訊播放邏輯和背景服務。 背景工作會透過系統媒體傳輸控制項與系統進行通訊。

下圖是系統設計方式的概觀。

![Windows 10 背景音訊架構](images/backround-audio-architecture-win10.png)
## <a name="mediaplayer"></a>MediaPlayer

[**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 命名空間包含用來在背景播放音訊的 API。 每個 app 都有一個可發生播放的 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) 執行個體。 您的背景音訊 app 會呼叫方法並設定 **MediaPlayer** 類別的屬性，以設定目前的曲目、開始播放、暫停、向前快轉、倒轉等。 媒體播放程式物件執行個體一律透過 [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528) 屬性存取。

## <a name="mediaplayer-proxy-and-stub"></a>MediaPlayer Proxy 和虛設常式

從 app 的背景處理程序存取 **BackgroundMediaPlayer.Current** 時，**MediaPlayer** 執行個體會在背景工作主機中啟動並可直接進行操作。

從前景 app 存取 **BackgroundMediaPlayer.Current** 時，傳回的 **MediaPlayer** 執行個體實際上是與背景處理程序中的虛設常式進行通訊的 Proxy。 這個虛設常式會與實際 **MediaPlayer** 執行個體進行通訊，該執行個體也裝載在背景處理程序中。

前景和背景處理程序兩者都可以存取 **MediaPlayer** 執行個體的大部分屬性，但 [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 和 [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) 除外，它們只能從背景處理程序存取。 前景 app 和背景處理程序兩者都可以收到媒體特定事件的通知，如 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/dn652609)、[**MediaEnded**](https://msdn.microsoft.com/library/windows/apps/dn652603) 及 [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/dn652606)。

## <a name="playback-lists"></a>播放清單

背景音訊 App 的常見案例就是連續播放多個項目。 此案例最容易在您的背景處理程序中使用 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 物件完成，而將該物件指派給 [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 屬性，即可在 **MediaPlayer** 上將它設為來源。

不可能從在背景處理程序中設定的前景處理程序存取 **MediaPlaybackList**。

## <a name="system-media-transport-controls"></a>系統媒體傳輸控制項

使用者不需直接使用 App 的 UI，即可透過藍牙裝置、SmartGlass 和系統媒體傳輸控制項來控制音訊播放。 您的背景工作會使用 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 類別，訂閱這些使用者起始的系統事件。

若要在背景處理程序中取得 **SystemMediaTransportControls** 執行個體，請使用 [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) 屬性。 前景 app 可藉由呼叫 [**SystemMediaTransportControls.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) 來取得此類別的執行個體，但傳回的執行個體是與背景工作無關的僅限前景執行個體。

## <a name="sending-messages-between-tasks"></a>在工作間傳送訊息

有時候，您會想在背景音訊 app 的兩個處理程序間進行通訊。 例如，開始播放新曲目時，您可能希望背景工作通知前景工作，然後將新的歌曲標題傳送到前景工作以顯示在畫面上。

簡單的通訊機制可同時在前景和背景處理程序中引發事件。 [**SendMessageToForeground**](https://msdn.microsoft.com/library/windows/apps/dn652533) 和 [**SendMessageToBackground**](https://msdn.microsoft.com/library/windows/apps/dn652532) 方法會個別叫用對應處理程序中的事件。 訂閱 [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) 和 [**MessageReceivedFromForeground**](https://msdn.microsoft.com/library/windows/apps/dn652531) 事件，可以接收訊息。

資料可以做為引數傳遞至傳送訊息方法，然後再傳入收到訊息事件處理常式。 使用 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 類別傳遞資料。 此類別是一個字典，其中包含的字串為索引鍵，而其他值類型則是值。 您可以傳送簡易的值類型，例如整數、字串和布林值。

## <a name="background-task-life-cycle"></a>背景工作週期

背景工作的存留期與 App 的目前播放狀態有很緊密的關係。 例如，當使用者暫停音訊播放時，系統可能會根據情況終止或取消您的 App。 在一段沒有音訊播放的時間之後，系統可能會自動關閉背景工作。

[**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法會在 app 從在前景 app 中執行的程式碼存取 [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528) 時，或在您登錄 [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) 事件的處理常式時首次呼叫，以較早發生者為準。 建議您在首次呼叫 **BackgroundMediaPlayer.Current** 之前，先註冊收到訊息的處理常式，如此前景 app 就不會錯過任何從背景處理程序傳送的訊息。

若要讓背景工作持續執行，您的 app 必須在 **Run** 方法中要求 [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499)，並在工作執行個體收到 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 或 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) 事件時呼叫 [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)。 不要在 **Run** 方法中執行迴圈或等待，因為這樣會消耗資源，而且可能導致系統終止 app 的背景工作。

當 **Run** 方法完成且沒有要求延遲時，您的背景工作會取得 **Completed** 事件。 在某些情況下，當您的 app 取得 **Canceled** 事件時，稍後也可能會收到 **Completed** 事件。 執行 **Run** 時，您的工作可能會收到 **Canceled** 事件，所以請務必管理此潛在的同時發生情形。

可能會取消背景工作的情況包含：

-   具備音訊播放功能的新 App 會在強制執行專屬性子原則的系統上啟動。 請參閱下面[背景音訊工作存留期的系統原則](#system-policies-for-background-audio-task-lifetime)一節。

-   背景工作已啟動但尚未播放音樂，然後前景應用程式就暫停。

-   其他媒體中斷，例如來電或 VoIP 通話。

可能未經通知即終止背景工作的情況包含：

-   VoIP 通話到來，而系統上沒有足夠的可用記憶體可讓背景工作持續執行。

-   違反資源原則。

-   工作取消或完成沒有正常結束。

## <a name="system-policies-for-background-audio-task-lifetime"></a>背景音訊工作存留期的系統原則

下列原則有助於判斷系統如何管理背景音訊工作的存留期。

### <a name="exclusivity"></a>專屬性

若已啟用，這個子原則會限制任何指定時間的背景音訊工作數目最多為 1。 它已在行動裝置和其他非桌上型電腦 SKU 上啟用。

### <a name="inactivity-timeout"></a>閒置逾時

由於資源限制，系統可能會在一段閒置時間之後終止您的背景工作。

如果符合下列兩個條件，背景工作會被視為「非使用中」：

-   看不見前景處理程序 (已暫停或已終止)。

-   背景媒體播放程式不在播放狀態中。

如果這些條件都已符合，則背景媒體系統原則會啟動計時器。 如果在計時器到期時兩個條件均未變更，則背景媒體系統原則會終止背景工作。

### <a name="shared-lifetime"></a>共用的存留期

若已啟用，這個子原則會強制背景工作相依於前景工作的存留期。 如果使用者或系統已關閉前景工作，則背景工作也會關閉。

不過，請注意這不表示前景相依於背景。 如果背景工作已關閉，這不會強制關閉前景工作。

下表列出各裝置類型上會強制執行的原則。

| 子原則             | 桌上型電腦  | 行動裝置版   | 其他    |
|------------------------|----------|----------|----------|
| **專屬性**        | 已停用 | 已啟用  | 已啟用  |
| **閒置逾時** | 已停用 | 已啟用  | 已停用 |
| **共用的存留期**    | 已啟用  | 已停用 | 已停用 |


 

 




