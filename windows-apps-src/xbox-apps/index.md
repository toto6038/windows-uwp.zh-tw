---
title: Xbox One 上的 UWP
description: 如何在 Xbox One 上建置通用 Windows 平台 (UWP) 的應用程式。
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: 1fe3e4e23bc2be46d3ddb09d9ebf83b5fda5ce9d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173712"
---
# <a name="uwp-on-xbox-one"></a>Xbox One 上的 UWP

開始在 Xbox One 上建置通用 Windows 平台 (UWP) 的應用程式。

Xbox One 上的 UWP 支援開發應用程式和遊戲。 您要實驗、建立和測試 Xbox 上的遊戲或應用程式，並不需要是開發人員計畫的一份子。 您只要有[合作夥伴中心](https://developer.microsoft.com/store/register)的[開發人員帳戶](https://partner.microsoft.com/dashboard)即可。 當您準備好要在 Xbox One 上發行和銷售遊戲，或是在 Windows 10 上運用 Xbox Live 時，您必須加入 [Xbox Live 創作者計畫](https://developer.microsoft.com/games/xbox/xboxlive/creator) 或成為 [ID@Xbox](https://www.xbox.com/Developers/id) 開發人員。 如果您想要成為 ID@Xbox 開發人員，建議您先申請參加計劃，再註冊開發人員帳戶。 如需詳細資訊，請參閱[開發人員計畫概觀](/gaming/xbox-live/developer-program-overview)。

本節包括設定步驟，透過驗證程序的指南、安裝所需版本的 Visual Studio 和 Windows 10 工具的相關資訊，以及建置、執行和偵錯您第一個簡單應用程式的步驟。 

| 主題      | 描述 |
|------------|-------------|
|[快速入門](getting-started.md)| Xbox One 上的 UWP 開發入門指南。 |
|[新功能](whats-new.md)| 針對 Xbox One 上的 UWP 新功能進行重點摘要。 |
|[啟用 Xbox One 開發人員模式](devkit-activation.md)| 說明如何在 Xbox One 上啟用開發人員模式。 |
|[在 Xbox One 上停用開發人員模式](devkit-deactivation.md)| 說明如何在 Xbox One 上停用開發人員模式。 |
|[在 Xbox 開發環境上設定 UWP](development-environment-setup.md)| 描述設定並測試 Xbox One 開發環境的步驟。 |
|[範例](samples.md)| GitHub 位置指標 (TVHelpers)，您將在其中找到有用的 XAML 和 JavaScript 範例以開始針對 Xbox 進行開發。 範例包括完整的 XAML 媒體應用程式範本，以及針對網路技術的自動控制器瀏覽、多媒體播放及搜尋。 |
|[已知問題](known-issues.md)| Xbox One 上的 UWP 已知問題。 |
|[常見問題集](frequently-asked-questions.md)| 與 Xbox One 上的 UWP 相關的常見問題集。 |
|[工具](introduction-to-xbox-tools.md)| 說明 Xbox One 專屬的工具「開發人員首頁」__、如何使用 Windows Device Portal，和如何針對開發設定 Visual Studio。 本節也會引導新的開發人員完成第一個 Xbox UWP 應用程式範例，並說明 Fiddler 工具的使用方式以檢視網路流量。 |
| [Xbox 事件上的應用程式開發](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | Xbox 事件上的應用程式開發是開發人員在 Xbox 上建置應用程式的絕佳起點。 觀賞錄製工作階段，閱讀事件部落格貼文。 |
|[針對 Xbox 和電視進行設計](../design/devices/designing-for-tv.md)| 描述將在電視上觀看並使用控制器進行輸入的應用程式設計的最佳做法。 |
|[Xbox 最佳做法](tailoring-for-xbox.md)| 如何關閉滑鼠模式、繪製到螢幕邊緣，以及停用縮放。 |
|[使用語音來叫用 UI 元素](ves-on-xbox.md)| 說明在 Xbox 上支援 UWP 應用程式中啟用語音的命令介面的最佳做法。 |
|[適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源](system-resource-allocation.md)| 描述您的應用程式在 Xbox One 上執行時可以使用的資源。 |
|[多使用者應用程式的簡介](multi-user-applications.md)| 描述 Xbox One 上的多使用者應用程式 (MUA)。 |
| [自動化 Xbox One 開發工作](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | GitHub 上的 WindowsDevicePortalWrapper 專案提供程式庫，可讓您將部署或啟動應用程式等常見的開發工作自動化。 專案包括範例 XboxWdpDriver.exe，示範如何使用 API 進行一般工作。 |
|[將現有的遊戲移到 Xbox](development-lanes-landing.md)|根據您建置遊戲時所使用的技術，我們可以引導您使用逐步指示，以加快使用 UWP 將遊戲移到 Xbox 的程序。|
|[Xbox One 上尚未支援的 UWP 功能](/uwp/extension-sdks/uwp-limitations-on-xbox)|  描述 Xbox One 上尚未完全運作的 UWP 功能區域。|

## <a name="videos"></a>影片

下列在 Channel 9 上的討論，是在 Xbox 上建置優秀應用程式的絕佳資訊來源：

* [建置適用於 Xbox 的絕佳通用 Windows 平台 (UWP) 應用程式](https://channel9.msdn.com/Events/Build/2016/B883)
* [針對 Xbox One 與電視調整您的應用程式](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [UWP 開發 1：建置調適型 UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [瀏覽器以外的 Web 應用程式：跨平台與跨裝置](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>另請參閱

- [自動化啟動 Windows 10 UWP 應用程式](automate-launching-uwp-apps.md)
- [CPUSets 遊戲開發](cpusets-games.md)
- [適用於 Xbox One 的漸進式 Web 應用程式](/microsoft-edge/progressive-web-apps/xbox-considerations)