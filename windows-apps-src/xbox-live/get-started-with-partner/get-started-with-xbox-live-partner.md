---
title: 開始使用 Xbox Live 身為合作夥伴
description: 提供的連結，協助受管理的夥伴或ID@Xbox成員開始使用 Xbox Live 的開發。
ms.assetid: 69ab75d1-c0e7-4bad-bf8f-5df5cfce13d5
ms.date: 06/07/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 夥伴， ID@Xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1a219ee6f4e1dc80ead893c4e230d70e8caa2400
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659823"
---
# <a name="get-started-with-xbox-live-as-a-managed-partner-or-an-idxbox-developer"></a>開始使用 Xbox Live 為受管理的夥伴或ID@Xbox開發人員

此章節將涵蓋為受管理的夥伴或開始使用 Xbox LiveID@Xbox開發人員。 合作夥伴和識別碼的開發人員可以存取的完整範圍的 Xbox Live 的功能，包括成就、 多人遊戲，等等。

受管理夥伴和ID@Xbox開發人員可以針對通用 Windows 平台 (UWP) 或 Xbox Developer Kit (XDK) 平台開發 Xbox Live 的標題。

除了可用的內容之外，另外還有其他的文件所使用的夥伴，並已獲授權的合作夥伴中心帳戶。 您可以存取該內容如下：[Xbox Live 合作夥伴內容](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/partner-content)。

## <a name="why-should-you-use-xbox-live"></a>為什麼您應該使用 Xbox Live？

Xbox Live 提供一系列的設計目的是協助提升您的遊戲，並讓玩家參與的功能：

- Xbox Live 社交平台可讓玩家與朋友連線，並討論您的遊戲。
- Xbox Live 的成就，可協助您取得熱門時玩家解除鎖定的成就，讓您遊戲的免費促銷，到 Xbox Live 的社交圖形的遊戲。
- Xbox Live 的排行榜有助於提升參與度，遊戲的讓玩家競賽訊號朋友和陣序規範向上移動。
- Xbox Live 的多人遊戲可讓玩家玩朋友或 get 與其他玩家競賽或相互合作，在遊戲中比對。
- Xbox Live 連接儲存體供應項目免費儲存遊戲讓玩家可以輕鬆地繼續 Xbox One 和 Windows 電腦之間進行的遊戲的裝置之間漫遊。

## <a name="1-choose-a-platform"></a>1.選擇平台
決定讓 Xbox Development Kit (XDK)、 通用 Windows 平台 (UWP) 或交互式的遊戲：

- XDK 基礎只能在 Xbox One 主控台上執行的遊戲
- UWP 遊戲可以針對任何的 Windows 平台，例如 Windows PC、 Windows Phone 或 Xbox One
  - Xbox one，請參閱[Xbox One 上的 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) ，特別是[UWP 應用程式和遊戲 Xbox One 上的系統資源](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- 跨玩遊戲通常是以 Xbox One 和 Windows 電腦使用的 XDK 和 UWP 的路徑為目標的遊戲。

## <a name="2-ensure-that-you-have-a-title-created-in-partner-center-or-xdp"></a>2.請確定您已在合作夥伴中心或 XDP 中建立的標題
必須先在合作夥伴中心或 Xbox 開發人員入口網站 (XDP) 中定義每個 Xbox Live 的標題，您將能夠登入，並進行 Xbox Live 服務呼叫。  [建立新的標題](create-a-new-title.md)示範如何執行這項操作。

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3.依照下列適當的指南，來設定您的 IDE 或遊戲引擎
您可以依照下列適當使用者入門指南，您的平台和引擎，並了解的 Xbox Live 基本概念，您進行。

* [開始使用 Visual Studio for UWP 遊戲](get-started-with-visual-studio-and-uwp.md)將為您示範如何連結您的 Xbox Live 設定在合作夥伴中心內的 Visual Studio 專案。
* [開始使用適用於 UWP 遊戲使用 Unity](partner-add-xbox-live-to-unity-uwp.md)會顯示您如何建立新的 Xbox Live 啟用 Unity 標題、 排名等功能加入您的標題，並產生原生的 Visual Studio 專案。
* [開始使用 Visual Studio for XDK 架構的遊戲](xdk-developers.md)將為您示範如何取得您的 Visual Studio 專案設定，如果您正在使用 XDK 的 Xbox One 標題。
* [開始使用快速入門進行跨遊戲](get-started-with-cross-play-games.md)說明如何讓一個產品的 Xbox One UWP 型的 XDK 型遊戲，遊戲的 Windows 10 電腦。

## <a name="4-xbox-live-concepts"></a>4.Xbox Live 的概念
一旦您已建立的標題，您應該了解的 Xbox Live 的概念，將會影響您的開發項目使用經驗：

- [Xbox Live 的沙箱](../xbox-live-sandboxes.md)
- [Xbox Live 的測試帳戶](../xbox-live-test-accounts.md)
- [Xbox Live Api 簡介](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5.加入 Xbox Live 功能

Xbox Live 功能加入您的遊戲：

- [Xbox Live 社交平台-設定檔，朋友們，存在](../social-platform/social-platform.md)
- [Xbox Live 資料平台-Stats 排行榜成就](../data-platform/data-platform.md)
- [Xbox Live 的多人遊戲平台-邀請，配對比賽](../multiplayer/multiplayer-intro.md)
- [Xbox Live 的儲存體平台-已連接的儲存體，標題儲存體](../storage-platform/storage-platform.md)
- [內容相關式搜尋](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6.發行您的標題

ID@Xbox 而且受管理的合作夥伴必須透過認證程序之前釋出其標題。  這是因為標題有額外的功能，例如線上多人遊戲、 成積、 以及豐富的目前狀態存取。  一旦您已準備好發行您的標題，如需提交和發行程序的詳細資訊，請連絡您的 Microsoft 代表。
