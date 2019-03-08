---
title: 逐步解說指南適用於 Xbox Live 的合作夥伴
description: 提供步驟，將受管理的協力廠商的整合 Xbox Live 的指導方針。
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9a2d0b9e7d97adfb02281e7bae34ed51afd44f7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660843"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>逐步解說指南適用於受管理的合作夥伴整合 Xbox Live 和ID@Xbox成員

本節將協助您快速啟動並執行使用 Xbox Live:

## <a name="1-choose-a-platform"></a>1.選擇平台
決定讓 Xbox Development Kit (XDK)、 通用 Windows 平台 (UWP) 或交互式的遊戲。

- XDK 基礎只能在 Xbox One 主控台上執行的遊戲
- UWP 遊戲可以針對任何的 Windows 平台，例如 Windows PC、 Windows Phone 或 Xbox One
  - Xbox one，請參閱[Xbox One 上的 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) ，特別是[UWP 應用程式和遊戲 Xbox One 上的系統資源](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- 跨玩遊戲通常是以 Xbox One 和 Windows 電腦使用的 XDK 和 UWP 的路徑為目標的遊戲。

## <a name="2-ensure-you-have-a-title-created-in-partner-center-or-xdp"></a>2.請確定您已在合作夥伴中心或 XDP 中建立的標題
必須先在合作夥伴中心或 Xbox 開發人員入口網站 (XDP) 中定義每個 Xbox Live 的標題，您將能夠登入，並進行 Xbox Live 服務呼叫。  [建立新的標題](create-a-new-title.md)示範如何執行這項操作。

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3.依照下列適當的指南，來設定您的 IDE 或遊戲引擎
您可以依照下列適當使用者入門指南，您的平台和引擎，並了解的 Xbox Live 基本概念，您進行。

* [開始使用 Visual Studio for UWP 遊戲](get-started-with-visual-studio-and-uwp.md)將為您示範如何連結您的 Xbox Live 設定在合作夥伴中心內的 Visual Studio 專案。

* [開始使用適用於 UWP 遊戲使用 Unity](partner-add-xbox-live-to-unity-uwp.md)會顯示您如何建立新的 Xbox Live 啟用 Unity 標題、 排名等功能加入您的標題，並產生原生的 Visual Studio 專案。

* [開始使用 Visual Studio for XDK 架構的遊戲](xdk-developers.md)將為您示範如何取得您的 Visual Studio 專案設定，如果您正在使用 XDK 的 Xbox One 標題。

* [開始使用快速入門進行跨遊戲](get-started-with-cross-play-games.md)說明如何讓一個產品的 Xbox One UWP 型的 XDK 型遊戲，遊戲的 Windows 10 電腦。

## <a name="4-xbox-live-concepts"></a>4.Xbox Live 的概念
建立遊戲之後，您應該深入了解會影響您開發遊戲體驗的 Xbox Live 概念。

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

ID@Xbox 標題和發行的遊戲發行商的標題是 Microsoft 合作夥伴必須通過認證程序之前發行。  這是因為這些項目都可存取額外的功能，例如多人遊戲、 成積、 以及豐富的組態選項。  一旦您已準備好發行您的標題，請連絡您的 Microsoft 代表，如需詳細資訊，在提交和發行程序。
