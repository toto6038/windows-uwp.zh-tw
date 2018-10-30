---
author: Mtoepke
title: 常見問題集
description: Xbox 上 UWP 的常見問題集。
ms.author: mstahl
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 4b2ea47f819d3a187621615ee3a85be5af895d3b
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5765425"
---
# <a name="frequently-asked-questions"></a>常見問題集

項目未以您預期的方式運作？ 請閱讀這一頁的常見問題集。 也請參閱[已知問題](known-issues.md)主題和[開發通用 Windows 應用程式](https://go.microsoft.com/fwlink/?linkid=839446)論壇。 

### <a name="why-arent-my-games-and-apps-working"></a>我的遊戲和應用程式為何無法運作？

如果您的遊戲和應用程式無法運作，或您無法存取存放區或 Live 服務，則可能是以開發人員模式執行。 若要找出您目前在哪一種模式，請按控制器上的 **\[首頁\]** 按鈕。 如果這顯示開發人員首頁，而不是零售首頁體驗，您就是在開發人員模式。 如果您想要玩遊戲，可以開啟 [開發首頁]，然後使用 [**離開開發人員模式**] 按鈕切換回 [零售模式]。

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>為什麼我無法使用 Visual Studio 連線到我的 Xbox One？

請從確認您是以開發人員模式執行開始，而不是零售模式。 處於零售模式時，您無法連線到您的 Xbox One。 若要找出您目前在哪一種模式，請按控制器上的 **\[首頁\]** 按鈕。 如果您看到 Gold/Live 內容，而不是開發人員首頁，您正在零售模式，以及您需要執行啟用開發人員模式 app 以切換至開發人員模式。

> [!NOTE]
> 您必須在已有使用者已登入的情況下才能部署 App。

如需詳細資訊，請參閱此頁面稍後的[修正部署失敗](#fixing-deployment-failures)。

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>如何切換零售模式與開發人員模式?

請依照[啟用 Xbox One 開發人員模式](devkit-activation.md)指示進行，來深入了解這些狀態。

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>如何知道我是處於零售模式還是開發人員模式?

請依照[啟用 Xbox One 開發人員模式](devkit-activation.md)指示進行，來深入了解這些狀態。 

若要找出您目前在哪一種模式，請按控制器上的 **\[首頁\]** 按鈕。 
- 如果這顯示開發人員首頁，您就是在開發人員模式。
- 如果您看到 Gold/Live 內容，您正在零售模式。

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>如果我啟用開發人員模式，我的遊戲和應用程式仍然可以運作嗎？

是，您可以從開發人員模式切換至零售模式 (您可以在此玩遊戲)。 如需詳細資訊，請參閱[啟用 Xbox One 開發人員模式](devkit-activation.md)。 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>我可以開發及發行適用於 Xbox 的 x86 應用程式嗎？
Xbox 不再支援 x86 應用程式開發或 x86 應用程式提交至Microsoft Store。 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>我是否會遺失我的遊戲和應用程式，或是已儲存的變更？

如果您決定離開開發人員計畫，您將不會遺失已安裝的遊戲和 App。 此外，只要您在執行它們時是在線上，您所有的遊戲存檔都會儲存在您的 Live 帳戶雲端設定檔上，因此將不會遺失。

### <a name="how-do-i-leave-the-developer-program"></a>如何離開開發人員計畫？

如需如何離開開發人員計畫的詳細資料，請參閱[停用 Xbox One 開發人員模式](devkit-deactivation.md)主題。

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>我將處於開發人員模式的 Xbox One 賣掉了。 如何停用開發人員模式？

如果您已無法存取 Xbox One，可以在 Windows 開發人員中心停用它。 如需詳細資料，請參閱[停用 Xbox One 開發人員模式](devkit-deactivation.md#deactivate-your-console-using-windows-dev-center)主題的**使用 Windows 開發人員中心停用您的主機**一節。 

### <a name="i-left-the-developer-program-using-windows-dev-center-but-im-in-still-developer-mode-what-do-i-do"></a>我使用 Windows 開發人員中心離開開發人員計畫，但我仍然處於開發人員模式。 我該怎麼做？

啟動 [開發首頁]，然後選取 [**離開開發人員模式**] 按鈕。 這會將您的主機重新啟動為零售模式。 

### <a name="can-i-publish-my-app"></a>我是否可以發佈我的 App？

如果您擁有[開發人員帳戶](https://developer.microsoft.com/store/register)，便可以透過開發人員中心[發佈 App](../publish/index.md)。 在零售 Xbox One 主機上建立和測試的 UWP App，必須接受 Windows 目前所執行的擷取、檢閱和發佈程序，並需要接受額外的檢閱以符合目前的 Xbox One 標準。

### <a name="can-i-publish-my-game"></a>我是否可以發佈我的遊戲？

您可以使用 UWP 和開發人員模式的 Xbox One，以在 Xbox One 上建置並測試您的遊戲。 若要發行 UWP 遊戲，您必須向 [ID@XBOX](http://www.xbox.com/Developers/id) 註冊，或是加入 [Xbox Live Creators 計劃](https://developer.microsoft.com/games/xbox/xboxlive/creator)。 如需詳細資訊，請參閱[開發人員計劃概觀](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html)。

### <a name="will-the-standard-game-engines-work"></a>標準遊戲引擎是否可以運作？

請參閱針對此版本的[已知問題](known-issues.md)頁面。

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Xbox One 上的 UWP 遊戲可以使用哪些功能和系統資源？ 

如需詳細資訊，請參閱[適用於 Xbox One 上 UWP App 和遊戲的系統資源](system-resource-allocation.md)。

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>如果我建立 DirectX 12 UWP 遊戲，是否可以在 Xbox One 上的開發人員模式中執行該遊戲？

如需詳細資訊，請參閱[適用於 Xbox One 上 UWP App 和遊戲的系統資源](system-resource-allocation.md)。

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>是否可以在 Xbox 上使用完整的 UWP API 表面？

請參閱針對此版本的[已知問題](known-issues.md)頁面。

### <a name="fixing-deployment-failures"></a>修正部署失敗

如果您無法從 Visual Studio 部署您的應用程式，這些步驟可協助您修正問題。 如果您遇到困難，請在論壇上尋求協助。

> [!NOTE]
> 您必須在已有使用者已登入的情況下才能部署 App。 如果您收到 0x87e10008 錯誤訊息，請確定已經有使用者登入，然後再試一次。

如果 Visual Studio 無法連線到 Xbox One：

1. 請確定您處於開發人員模式 (先前已在此頁面討論過)。
2. 請確定您已經正確地設定開發電腦。 您是否已依照[開始使用 Xbox One 上的 UWP 應用程式開發](getting-started.md)中的*所有*指示進行？ 

3. 如果您還沒有這麼做，請閱讀[開發環境設定](development-environment-setup.md)主題和 [Xbox One 工具簡介](introduction-to-xbox-tools.md)主題。

4. 請確定您可以從開發電腦 "ping" 到主機 IP 位址。
  > [!NOTE]
  > 為了取得最佳的部署效能，建議您使用有線連線方式連線主機。

5. 請確定您是使用 **\[偵錯\]** 索引標籤的 [驗證] 下拉式清單中的 [通用 (未加密的通訊協定)]。如需詳細資訊，請參閱[開發環境設定](development-environment-setup.md)。


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>如果我是使用 HTML/JavaScript 建置 App，該如何啟用控制器瀏覽？

TVHelpers 是一組 JavaScript 和 XAML/C# 範例和程式庫，可協助您以 JavaScript 和 C# 建置絕佳的 Xbox One 與電視體驗。 TVJS 是可協助您針對 Xbox One 建置優質 UWP app 的程式庫。 TVJS 包含自動控制器瀏覽、多媒體播放、搜尋等支援。 您可以使用 TVJS 搭配託管的 Web 應用程式，就如同搭配具備 Windows 執行階段 API 完整存取權之封裝的 Web UWP app 一樣簡單。

如需詳細資訊，請參閱 [TVHelpers](https://github.com/Microsoft/TVHelpers) 專案和專案 [wiki](https://github.com/Microsoft/TVHelpers/wiki)。

## <a name="see-also"></a>另請參閱
- [Xbox One 上的 UWP 已知問題](known-issues.md)
- [Xbox One 上的 UWP](index.md)
- [Xbox One 上的 UWP](index.md)
