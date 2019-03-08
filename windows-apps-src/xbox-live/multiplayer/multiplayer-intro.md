---
title: Xbox Live 的多人遊戲平台
description: 深入了解所支援的 Xbox Live 的多人遊戲平台。
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 多人遊戲
ms.localizationpriority: medium
ms.openlocfilehash: c71fc28a9bb8668d0253834c1a2c9c6ca7736fd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651153"
---
# <a name="xbox-live-multiplayer-platform"></a>Xbox Live 的多人遊戲平台

Xbox Live 多人遊戲平台可讓您將透過網際網路結合在一起的 Xbox Live 的玩家的遊戲，並可以大幅擴充的生命週期和使用量超過一般的單人遊戲的標題。

藉由建置絕佳的多人遊戲體驗的目標對象，您可以利用 Xbox Live 的玩家來增加您的遊戲群，並將升級的專用的愛用者一起播放持續的社群的大型社交網路。


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>Xbox Live 多人遊戲平台是什麼？

Xbox Live 多人遊戲平台是一組用戶端 Api，可用來實作即時多人連線遊戲。 主要的子系統中的 API 套件︰

-   **Xbox Live 多人遊戲工作階段目錄 (MPSD)** 服務。 MPSD 服務搭配整合式的 UI 體驗，以協助使用者尋找及播放邀請彼此。 使用 Xbox Live 的服務整合可讓客戶使用 Xbox Live 合作對象對談來組合。
-   **簡單與進階配對設備。** Xbox Live 提供傳統的快速配對功能，但也是工作階段瀏覽並支援高度自訂的配對案例。 Xbox Live 尋找群組 (LFG) 時，也可讓玩家能找到彼此，集結在合作對象交談，然後在玩這個遊戲。
-   **若要對等與用戶端-伺服器對等網路功能 Api**提供利用現代化的網際網路標準的即時通訊安全並隨時監控 Xbox Live。 標準化和疑難排解體驗的 Xbox Live 網路整合可讓使用者快速修復連線問題。  
-   **整合式的語音和文字對談解決方案**，促進安全利用 Xbox Live 的社交圖形、 媒體服務和特定編碼的硬體，在 Xbox One 裝置上的遊戲中通訊。

如需一些最常見的多人遊戲案例中，和哪些 Xbox Live 功能可協助實作這些案例的概觀，請參閱 <<c0> [ 多人遊戲的層級](multiplayer-scenarios.md)。

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>如何實作在遊戲中的 Xbox Live 多人遊戲？
根據您的案例，Xbox Live 多人遊戲平台會提供數種方法來實作您的遊戲中的 Xbox Live 多人遊戲。

### <a name="xbox-integrated-multiplayer-xim"></a>Xbox 整合式多人遊戲 (XIM)
XIM 是周全的解決方案，將即時多人連線的網路功能與通訊新增至您的遊戲，透過 Xbox Live 服務的強大功能。 XIM 的目標是讓它比以往若要建置高品質的對等 (P2P) 多人遊戲 Xbox One 和 Windows 10 上更輕鬆。

XIM 提供下列功能：
- 支援遊戲的邀請和簡單的配對。
- 簡單且安全即時網路以透明的方式，來擴充 Xbox Live 的多人轉送服務。
- 簡單的語音和文字，與交談謄寫和 narrating 通訊更容易存取的使用者體驗的功能。
- 用於偵測及管理網路壅塞，以及遊戲狀態移轉繁多。

XIM 是最簡單的多人遊戲解決方案可透過 Xbox Live 多人遊戲平台，但也至少可自訂，並只適用於 P2P 遊戲。 如需 XIM 的詳細資訊，請參閱[Xbox 整合式多人遊戲](xbox-integrated-multiplayer.md)。

### <a name="xbox-multiplayer-manager"></a>Xbox 多人遊戲管理員
Xbox 多人遊戲管理員 (MPM) 是一個用戶端 API，可提供彈性存取 Xbox Live 的多人連線的工作階段目錄、 邀請和配對的服務。

它可以實作許多常見的多人遊戲案例以有效率的方式，遵循最佳做法，並也會處理您的遊戲必須實作才能通過認證的 Xbox 需求 (XRs)。

Xbox 多人遊戲管理員未實作網路或交談層。 MPM 被設計成有彈性，但簡化和合併多人遊戲管理 API 與透過 Windows.Networking.XboxLive 實作安全的網路層的配對遊戲。 可以新增遊戲中的通訊，而[遊戲的對談 2](chat/game-chat-2-overview.md) API 或透過 XIM 聊天保留項目。 網路和遊戲的聊天 2 Api 會記錄在 Xbox One XDK 及 Xbox Live 平台擴充功能 SDK。

如果您為多人連線遊戲裝載專用的伺服器，MPM 會是最佳選擇。 也適用於進階案例，例如 Xbox Live 的比賽與整合 MPM。 如需有關 MPM 的詳細資訊，請參閱[多人遊戲 Manager 簡介](multiplayer-manager/multiplayer-manager-api-overview.md)。

若要使用多人連線管理員，您必須設定您的多人遊戲情節的 Xbox Live 服務。 如需有關此設定的詳細資訊，請參閱 <<c0> [ 設定多人遊戲服務](service-configuration/configure-the-multiplayer-service.md)。

>多人遊戲管理員目前不支援工作階段瀏覽。 如需資訊[多人遊戲工作階段瀏覽](session-browse.md)。

### <a name="api-capabilites"></a>API 功能

功能 | Xbox 整合多人遊戲| 多人遊戲管理員
--  | -- | --
可見度 |  XIM 提供已編譯的程式庫，沒有來源。  | MPM 提供與來源，讓您可以直接互動與 Xbox Live 服務或 XSAPI 以自訂行為。
工作階段和配對 | XIM 提供簡單的預先設定的配對規則，並不需要多人遊戲設定。 | MPM[需要設定多人遊戲服務](service-configuration/configure-the-multiplayer-service.md)，可讓複雜的配對和工作階段行為的規格。
網路功能 | XIM 提供簡單且安全 player 來播放程式網路，支援時所需的 Xbox Live 的轉送服務。 | MPM 旨在讓您可以插入您自己使用 Windows.Networking.XboxLive 的安全網路解決方案。
遊戲的對談 | XIM 提供整合式的語音和文字的對談。 | 遊戲中的通訊可以使用遊戲的聊天 2 API 來實作，或使用 XIM MPM 啟用對談的保留項目外的頻外管理名冊。
