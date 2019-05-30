---
title: 已連線的應用程式與裝置 (Project Rome)
description: 本節說明如何使用遠端系統平台來探索遠端裝置、啟動遠端裝置上的 app，以及與遠端裝置上的 app 服務通訊。
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10 uwp，連線裝置、 遠端系統、 羅馬、 project rome
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: ae9229378f75adeb215a881bdaf955b010cd7806
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366368"
---
# <a name="connected-apps-and-devices-project-rome"></a>已連線的應用程式與裝置 (Project Rome)

本節說明如何將應用程式跨裝置與平台使用的連接[Project Rome](https://developer.microsoft.com/en-us/windows/project-rome)。 若要了解如何實作 Project Rome 的跨平台案例中，請造訪[Project Rome 的主要的 docs 頁面](https://docs.microsoft.com/en-us/windows/project-rome/)。

大部分的使用者擁有多部裝置，而且經常是在一部裝置上開始活動，卻在另一部上完成。 若要做到這一點，app 需要跨越裝置與平台。 Project Rome 可讓您探索遠端裝置、 啟動遠端裝置上的應用程式，並與遠端裝置上的 app service 進行通訊。

Windows 10 (版本 1607) 中引進的[遠端系統 API](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems)，可讓您撰寫 App，允許使用者在一部裝置上啟動工作，並在另一部上完成。 工作仍是中心焦點，而且使用者能在使用最方便的裝置上執行工作。 例如，使用者可能會在車內聆聽手機上的收音機，但當到家時，可能會想要換到和家庭立體聲系統連結的 Xbox One 上播放。

您也可以將專案 Rome 用於隨附裝置或遠端控制案例。 使用 App 服務訊息傳送 API，在兩部裝置之間建立 App 通道，來傳送和接收自訂訊息。 例如，您可以為手機撰寫可控制電視播放的 App，或撰寫配對 App 來為您透過另一個 App 上觀看的電視節目角色提供相關資訊。  

近端可以透過藍牙和無線來與裝置連線，或使用裝置使用者的 Microsoft 帳戶 (MSA) 透過雲端從遠端連結。

請參閱[遠端系統 UWP 範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )，以取得如何探索遠端系統、啟動遠端系統上的 App，以及使用 App 服務在兩個系統上執行的 App 之間傳送訊息的範例。

如需一般有關專案 Rome 的詳細資訊，包括跨平台整合的資源，請移至 [aka.ms/project-rome](https://aka.ms/project-rome)。

| 主題 | 描述 |
|-------|-------------|
| [啟動遠端裝置上的應用程式](launch-a-remote-app.md) | 了解如何啟動遠端裝置上的應用程式。 本主題涵蓋最簡單的使用案例和初步安裝。  |
| [探索遠端裝置](discover-remote-devices.md)  | 了解如何探索能夠連線的裝置。 |
| [與遠端應用程式服務通訊](communicate-with-a-remote-app-service.md) | 了解如何與遠端裝置上的應用程式互動。 |
| [透過遠端工作階段連接裝置](remote-sessions.md) | 將裝置加入遠端工作階段，以建立跨多部裝置的共用體驗。 |
| [繼續使用者活動，甚至是在各個裝置之間](useractivities.md)| 幫助使用者繼續它們已在做什麼應用程式，甚至可跨多個裝置。|
| [使用者活動的最佳作法](useractivities-best-practices.md)| 了解建議的做法，來建立和更新使用者的活動。|
