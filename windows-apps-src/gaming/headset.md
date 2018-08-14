---
author: mithom
title: 耳機
description: 使用 Windows.Gaming.Input 耳機 API，偵測耳機、擷取播放程式語音和播放音訊。
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 遊戲, 耳機
ms.openlocfilehash: 04baee2a3011cee63933fe1fdab759d1b6d29c89
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "209078"
---
# <a name="headset"></a>耳機

此頁面說明使用 [Windows.Gaming.Input.Headset][headset] 進行耳機程式設計的基本知識，以及通用 Windows 平台 (UWP) 的相關 API。

閱讀此頁面，即可了解：
* 如何存取連接至輸入或瀏覽裝置的耳機
* 如何偵測已連接或中斷連接的耳機


## <a name="headset-overview"></a>耳機概觀

耳機是音訊擷取和播放裝置，最常用於在線上遊戲中與其他玩家通訊，但也在遊戲中使用或者用於其他創意用途。 在 Windows 10 和 Xbox UWP 應用程式中，耳機受到 [Windows.Gaming.Input][] 命名空間所支援。


## <a name="detect-and-track-headsets"></a>偵測和追蹤耳機

耳機是由系統管理，因此您不需要對其進行建立或初始化動作。 系統透過所連接的輸入裝置提供耳機的存取權，以及在連接或中斷連接耳機時通知您的事件。

### <a name="igamecontrollerheadset"></a>IGameController.Headset

[Windows.Gaming.Input][] 命名空間中的所有輸入裝置會實作 [IGameController][] 介面，該介面定義 [Headset][igamecontroller.headset] 屬性為目前連接至裝置的耳機。

### <a name="connecting-and-disconnecting-headsets"></a>連接和中斷連接耳機。

當連接或中斷連接耳機時，就會引發 [HeadsetConnected][igamecontroller.headsetconnected] 與 [HeadsetDisconnected][igamecontroller.headsetdisconnected] 事件。 您可以登錄這些事件的處理常式來追蹤輸入裝置目前是否有連接耳機。

下列範例顯示如何登錄 `HeadsetConnected` 事件的處理常式。

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

下列範例顯示如何登錄 `HeadsetDisconnected` 事件的處理常式。

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>使用耳機

[Headset][] 類別是由兩個代表 XAudio 端點識別碼的字串組成，一個用於音訊擷取 (從耳麥式麥克風錄製)，另一個用於音訊轉譯 (透過耳機播放)。

使用 XAudio 的詳細資料不在此做討論，如需詳細資訊，請參閱 [XAudio2 程式設計指南](https://msdn.microsoft.com/library/windows/desktop/ee415737.aspx) (英文) 和 [XAudio2 API 參考](https://msdn.microsoft.com/library/windows/desktop/ee415899.aspx) (英文)。


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headset.aspx
[igamecontroller.headsetconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetconnected.aspx
[igamecontroller.headsetdisconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetdisconnected.aspx
[耳機]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.headset.aspx
