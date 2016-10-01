---
author: TylerMSFT
title: "已連線的 app 與裝置 (專案 &quot;Rome&quot;)"
description: "本節說明如何使用專案 &quot;Rome&quot; 來探索連線的裝置、啟動另一部裝置上的 app，以及與遠端裝置上的 app 通訊。"
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: 4f49acfd7efcb10d99f9d23884d20c0fc51e5a4a

---

# 已連線的 app 與裝置 (專案 "Rome")

本節說明如何使用專案 "Rome" 跨裝置與平台來和 app 連線。 了解如何探索連線的裝置、啟動另一部裝置上的 app，以及與遠端裝置上的 app 通訊。

大部分人都擁有多部裝置，而且經常是在一部裝置上開始活動，卻在另一部上完成。 若要做到這一點，app 需要跨越裝置與平台。

Windows 10 (版本 1607) 中引進的[遠端系統 API](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)，可讓您撰寫 app，允許使用者在一部裝置上啟動工作，並在另一部上完成。 工作仍是中心焦點，而且使用者能在使用最方便的裝置上執行工作。 例如，您可能會在車內聆聽手機上的收音機，但當您到家時，可能會想要換到和家庭立體聲系統連結的 Xbox One 上播放。

您也可以將專案 "Rome" 用於配對裝置或遠端控制案例。 使用 app 訊息傳送 API，在兩部裝置之間建立 app 通道，來傳送和接收自訂訊息。 例如，您可以為手機撰寫可控制電視播放的 app，或撰寫配對 app 來為您在另一個 app 上觀看的電視節目角色提供相關資訊。  

近端可以透過藍牙和無線來與裝置連線，或使用裝置使用者的 Microsoft 帳戶透過雲端從遠端連線。

請參閱[遠端系統範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )，以取得如何探索遠端系統、啟動遠端系統上的 app，以及使用應用程式服務在兩個系統上執行的應用程式之間傳送訊息的範例。

| 遠端活動 | 說明                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [探索遠端裝置](discover-remote-devices.md)  | 了解如何探索能夠連線的裝置。 |
| [啟動遠端裝置上的 app](launch-a-remote-app.md) | 了解如何啟動遠端裝置上的 app。  |
| [與遠端應用程式服務通訊](communicate-with-a-remote-app-service.md) | 了解如何與遠端裝置上的 app 互動。 |



<!--HONumber=Aug16_HO5-->


