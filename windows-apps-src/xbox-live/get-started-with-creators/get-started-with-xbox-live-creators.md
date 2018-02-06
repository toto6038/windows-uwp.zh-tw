---
title: "開始使用 Xbox Live 創作者計畫"
author: KevinAsgari
description: "提供連結，協助您開始使用 Xbox Live 創作者計畫。"
ms.assetid: 2a744405-7ee4-42b4-8f36-9916e8c3a530
ms.author: kevinasg
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 遊戲, uwp, windows 10, xbox one"
ms.localizationpriority: high
ms.openlocfilehash: ba7f974ec6157e8f1d94232a64099fcae766eba8
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-the-xbox-live-creators-program"></a>開始使用 Xbox Live 創作者計畫
 
Xbox Live 創作者計畫提供簡化的認證流程，不需要進行任何概念核准，能讓您快速、直接地將您的遊戲發佈至 Xbox One 和 Windows 10。 如果您的遊戲整合 Xbox Live 並遵循我們的[標準 Store 原則](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)，您已準備好可以發佈。 本文將列出透過 Xbox Live 整合讓您的遊戲啟動和持續運作的所需步驟。 

Xbox Live 創作者計畫遊戲必須是通用 Windows 平台 (UWP) 應用程式。 若是 Xbox One，請參閱 [Xbox One 上的 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index)，尤其是[適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)。 透過 Xbox Live 創作者計畫發行的遊戲不能存取成就或線上多人服務。 如需支援服務的完整清單，請參閱[開發人員計畫概觀功能表格](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview#feature-table)。

## <a name="1-ensure-you-have-a-title-created-on-dev-center"></a>1. 確保您已在開發人員中心建立遊戲
每個 Xbox Live 遊戲都必須先在開發人員中心定義，您才能登入並打電話給 Xbox Live 服務。  [建立新的創作者遊戲](create-and-test-a-new-creators-title.md)會說明做法。

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. 依照適當指南來設定您的 IDE 或遊戲引擎
您可以依照適用於您平台和引擎的「入門指南」進行，順便了解 Xbox Live 的基本知識：

* [使用 Visual Studio 開發創作者遊戲](develop-creators-title-with-visual-studio.md)會說明如何將 Visual Studio 專案連結至開發人員中心上的 Xbox Live 設定。
* [使用 Unity 開發創作者遊戲](develop-creators-title-with-unity.md)會說明如何建立新的 Xbox Live 支援 Unity 遊戲、處理單一使用者和多使用者登入、新增排行榜和統計資料等功能，以及產生原生 Visual Studio 專案。

雖然 Unity 是我們為其提供說明文件的唯一一個第三方遊戲引擎，但遊戲引擎 [Construct (2 & 3)](https://www.scirra.com/construct2) 和 [Game Maker Studio](https://www.yoyogames.com/gamemaker) 也提供文件來協助您分別將 Xbox Live 整合至 Construct 或 Game Maker Studio 遊戲。

* [Game Maker Studio 2 UWP 現在支援 Xbox Live 創作者計畫](https://www.yoyogames.com/gamemaker/xblc)將說明如何匯出 Game Maker Studio 專案以在 Xbox One 和 Windows 10 電腦上進行。
* [在 UWP app 使用 Xbox Live - Construct](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps) 將說明如何在 Construct 2 和 3 遊戲中使用 Xbox Live。

至於其他未記載 Xbox Live 整合的遊戲開發引擎，您仍然可以使用 Xbox Live API 來將 Xbox Live 加入您的遊戲。 若要從您的專案使用 Xbox Live API，您可以使用 NuGet 套件新增二進位檔的參考，或是新增 API 來源。 新增 NuGet 套件可以加快編譯過程，而新增來源可以讓偵錯變容易。

如需使用 Xbox Live 服務搭配非 Unity 的第三方遊戲引擎的相關支援，請洽詢適當的遊戲引擎人員來回答您的問題。

## <a name="3-xbox-live-concepts--testing"></a>3. Xbox Live 概念與測試
建立遊戲之後，您應該深入了解會影響您開發遊戲體驗的 Xbox Live 概念。 請務必在遊戲支援的所有平台上測試您的遊戲，以確保其行為如預期般運作。

- [創作者計畫的 Xbox Live 服務設定](xbox-live-service-configuration-creators.md)
- [Xbox Live 測試環境](../xbox-live-sandboxes.md)
- [授權 Xbox Live 帳戶](authorize-xbox-live-accounts.md)

## <a name="4-enable-xbox-live-sign-in"></a>4. 啟用 Xbox Live 登入
所有 Xbox Live 創作者計畫遊戲都必須整合 Xbox Live 登入，並顯示使用者身分識別 (玩家代號、玩家圖示等)。 您可以選擇讓使用者自動登入，或讓使用者按按鈕來啟動。 登入後就必須顯示玩家代號，好讓玩家確認他們使用的是正確的設定檔。

- [Xbox Live 社交平台 - 設定檔、好友、線上狀態](../social-platform/social-platform.md)

## <a name="5-add-optional-xbox-live-features"></a>5. 加入選用 Xbox Live 功能

Xbox Live 創作者計畫提供一系列功能，專門設計來協助推銷遊戲並持續吸引玩家：

- [Xbox Live 資料平台 - 統計資料、排行榜](../data-platform/data-platform.md)透過讓玩家和好友互相競爭以提高排名，來協助提高遊戲吸引力。
- [Xbox Live 儲存平台 - 連接的儲存空間、遊戲儲存空間](../storage-platform/storage-platform.md)提供裝置間的免費存檔遊戲漫遊，讓玩家可以輕鬆地在 Xbox One 及 Windows 電腦之間繼續玩遊戲。
- [Xbox Live 社交平台 - 設定檔、好友、線上狀態](../social-platform/social-platform.md)能讓玩家與朋友保持聯繫，討論您的遊戲。

請務必注意，Xbox Live 創作者計畫不支援線上多人遊戲、成就或玩家分數。

## <a name="6-release-your-game"></a>6. 發行您的遊戲

如果您使用 Xbox Live 創作者計畫，則發行程序與發行其他 UWP 應用程式完全相同：

- [Microsoft Store 原則](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)包括[遊戲和 Xbox 原則](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_13)
- [發佈 Windows 應用程式](https://developer.microsoft.com/en-us/store/publish-apps)