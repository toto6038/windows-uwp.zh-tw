---
title: 開始使用
description: 遊戲開發學習指南
ms.assetid: 40490837-6c7f-4f82-96b5-14f6858982b3
ms.date: 01/25/2018
ms.topic: article
keywords: windows 10、 uwp、 遊戲，開始使用
localizationpriority: medium
ms.openlocfilehash: f818837a6f8703721520a8be8c0ed9b062cf797a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653553"
---
# <a name="getting-started"></a>開始使用

這篇文章是入門指南，建立者想要開發 Windows 或 Xbox 的遊戲。 

以下是一些問題，可協助您尋找您所需的資訊：
* 您有經驗的遊戲開發人員且想要所有詳細資料嗎？ 請參閱[Windows 10 遊戲開發指南](e2e.md)。
* 第一次接觸撰寫程式碼嗎？ 例如一些有趣[Minecraft 小時的程式碼的教學課程](https://code.org/minecraft)也許會感興趣。
* 只尋找播放精彩的遊戲嗎？ 請參閱[Microsoft Store](https://www.microsoft.com/store)。
* 準備好開始進行 Windows 或 Xbox 開發精彩的遊戲嗎？  您已在正確的位置 ！

## <a name="quick-start-guide"></a>快速入門指南

進入立即開發遊戲的步驟。

### <a name="step-1-get-the-software-and-tools"></a>步驟 1：取得軟體和工具

請確定您已安裝在您的裝置上的 Windows 10，且有最新安裝的更新。

安裝適當的 IDE，例如 Visual Studio。 Visual Studio Community 2017 可供免費下載。 如需詳細資訊，請參閱 < [Visual Studio 下載](https://www.visualstudio.com/downloads/)。

如果您打算使用的遊戲引擎和其他中介軟體，請參閱[橋接器、 遊戲引擎和中介軟體](e2e.md#bridges-game-engines-and-middleware)一節[Windows 10 遊戲開發指南](e2e.md)。 如需開發使用特定的遊戲引擎的 Windows 和 Xbox 遊戲，您必須移至遊戲引擎的文件。

### <a name="step-2-prepare-your-hardware-for-development"></a>步驟 2：準備您的硬體進行開發

如果您要進行開發，第一次，您必須在您的裝置上啟用開發人員模式。 如需詳細資訊，請參閱 <<c0> [ 啟用您的裝置進行開發](../get-started/enable-your-device-for-development.md)。

對於使用者而言正規劃開發 Xbox 遊戲，您的 xbox Xbox 360，您也必須啟用，並在其上啟用開發人員模式。 如需詳細資訊，請參閱 < [Xbox 一個開發人員模式下啟動](../xbox-apps/devkit-activation.md)並[Xbox 上的 UWP 應用程式開發入門](../xbox-apps/getting-started.md)。 

> [!Note]
> 您必須註冊[合作夥伴中心](https://partner.microsoft.com/dashboard)帳戶才能啟用在 xbox 開發人員模式。 如需有關登入合作夥伴中心帳戶的詳細資訊，請參閱[步驟 5](#step-5-sign-up-for-a-partner-center-account)如下。

### <a name="step-3-run-a-sample-and-see-how-it-works"></a>步驟 3：執行範例，並查看其運作方式

若要開始使用 UWP DirectX 開發，請參閱[建立簡單的 UWP 遊戲搭配 DirectX](tutorial--create-your-first-uwp-directx-game.md)。 如果您只想要閱讀和 DirectX 的概念，例如哪些緩衝區是要了解，請參閱[Direct3D 圖形概念](../graphics-concepts/index.md)。

如需更多範例，請參閱[遊戲範例](e2e.md#game-samples)。

### <a name="step-4-consider-joining-a-program"></a>步驟 4：請考慮加入方案

如果您想要開發 Xbox 遊戲，或在您的遊戲中使用 Xbox Live 功能，將其中一個[Xbox Live 創作者計劃](https://developer.microsoft.com/games/xbox/xboxlive/creator)或是[ ID@Xbox ](https://www.xbox.com/Developers/id)程式。 

若要深入了解可用於每個程式的 Xbox Live 功能，請參閱[特徵資料表](../xbox-live/developer-program-overview.md#feature-table)。 如需詳細資訊，請參閱 <<c0> [ 開發人員計劃](e2e.md#developer-programs)。

> [!Note]
> Xbox Live 創作者計劃是所有開發人員可使用。 **任何人**可以發行 Xbox 遊戲。 若要讓您標題的一部分 Xbox Live 創作者計劃，您只需要啟用此選項，從合作夥伴中心。 如需有關登入合作夥伴中心帳戶的詳細資訊，請參閱[步驟 5](#step-5-sign-up-for-a-partner-center-account)如下。

### <a name="step-5-sign-up-for-a-partner-center-account"></a>步驟 5：登入合作夥伴中心帳戶

合作夥伴中心帳戶可讓您存取[合作夥伴中心](https://partner.microsoft.com/dashboard)，可讓您管理和提交所有的應用程式和 Windows 裝置，在同一個地方的遊戲。

適用於 Windows 的遊戲開發，您可以選擇等候直到您想要存取合作夥伴中心，或當您想要在您的遊戲中使用 Xbox Live 的功能。

為 Xbox 遊戲開發，您應該註冊的合作夥伴中心帳戶來設定您的零售 Xbox 開發所需。 請參閱[步驟 2](#step-2-prepare-your-hardware-for-development)如需詳細資訊。

如需詳細資訊，請參閱 <<c0> [ 發行的 Windows 應用程式和遊戲](../publish/index.md)。

## <a name="useful-links"></a>實用的連結

* [Windows 10 遊戲開發指南](e2e.md)
* [什麼是 UWP app？](../get-started/universal-application-platform-guide.md)
* [使用適用於 UWP 遊戲的雲端服務](cloud-for-games.md)
* [讓遊戲更容易存取](accessibility-for-games.md)