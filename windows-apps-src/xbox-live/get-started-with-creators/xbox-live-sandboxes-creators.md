---
title: Xbox Live 的沙箱
description: Xbox Live 沙箱簡介
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.date: 10/30/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2e5496c9aab2629591a121850685e7f5a1b2fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590223"
---
# <a name="xbox-live-sandboxes-introduction"></a>Xbox Live 沙箱簡介

在  [Xbox Live 服務組態](xbox-live-service-configuration-creators.md)文章中，已說明如您必須在程式標題的相關設定資訊[合作夥伴中心](https://partner.microsoft.com/dashboard)。 這項資訊會包含統計資料、 排行榜、 當地語系化等的項目。 需要之前所做的變更會收取的 Xbox Live 的其餘部分，而且可以存取在您的標題從合作夥伴中心發佈至您的開發沙箱至 Xbox Live 服務組態的變更。

開發沙箱可讓您能夠在您的標題隔離的環境中的變更。 沙箱會提供數個優點：

1. 您可以逐一查看變更的更新標題而不會影響即時生產環境中的版本。
2. 基於安全性理由，有些工具只能在開發沙箱中。
3. 無法看到其他 「 發行者 」，您正在使用沒有正在授與存取您的沙箱。

根據預設，Xbox One 主控台和 Windows 10 電腦是零售沙箱中。 您必須在電腦和/或 Xbox One 轉為開發沙箱，以便存取該版本的 Xbox Live 服務組態。 請務必記得要變更回零售沙箱的裝置，如果您要測試的項目在 RETAIL 或想要稍事休息播放您最愛的 Xbox Live 的遊戲。

## <a name="finding-out-about-your-sandbox"></a>找出關於您的沙箱

當您建立一個標題時，會為您建立的沙箱。 您可以開啟您的產品中找到您的沙箱識別碼**合作夥伴中心**並瀏覽至**Services** > **Xbox Live**。 **沙箱識別碼**將列出在頁面頂端。

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>切換您的 PC 開發沙箱
使用 Unity、 Windows Device Portal (WPD)，或透過命令列，您可以將您的電腦切換至開發沙箱中。

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>必要條件
流入和流出開發沙箱 Unity 中，您可以切換之前，進行下列需求：

1. [設定 Xbox Live 中 Unity](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>切換沙箱
內建在 Xbox Live 組態視窗可讓您輕鬆地開發及零售沙箱之間切換。 若要開始，請前往**Xbox Live** > **Configuration**功能表中。 您可以看到目前在沙箱**開發人員模式下設定**一節。

1. 如果**開發人員模式**說**啟用**，則您目前是在您的遊戲與相關聯的開發沙箱中。 您可以按一下**切換回零售模式**按鈕來切換移出。
2. 如果**開發人員模式**說**停用**，則您目前是零售沙箱中。 您可以按一下**切換至 [開發人員模式**切換移入] 按鈕。

![啟用 XBL](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>必要條件
切換您的沙箱中 Windows Device Portal (WPD) 之前，進行下列需求：

1. [在 Windows 桌面上的 設定裝置入口網站](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>切換沙箱

1. 開啟**Windows 開發人員入口網站**中所述，連線至您的 web 瀏覽器中[安裝裝置入口網站，在 Windows 桌面上](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)文章。
2. 按一下  **Xbox Live**。
3. 文字欄位中輸入您的開發沙箱，然後按一下**變更**。

![](../images/getting_started/wdp_switch_sandbox.png)

若要切換回零售版，您可以輸入零售這裡所示。

### <a name="command-line"></a>命令列

#### <a name="prerequisites"></a>必要條件
您可以流入和流出開發沙箱，透過命令列切換之前，進行下列需求：

1. 下載 Xbox Live 工具套件[ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)並將它解壓縮。

#### <a name="switch-sandboxes"></a>切換沙箱
1. 在中執行 SwitchSandbox.cmd 批次檔**系統管理員模式**。

若要切換您的沙箱的系統管理員模式中執行此作業。 第一個引數是沙箱。 例如，如果您嘗試切換至 MJJSQH.58 沙箱，您會使用此命令：

```cmd
SwitchSandbox.cmd MJJSQH.58
```

若要切換回零售版，您只需要提供，做為第二個引數。

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>切換您的 Xbox One 主控台開發沙箱

### <a name="using-xbox-dev-portal"></a>使用 Xbox 開發人員入口網站

您可以使用 Xbox 開發人員入口網站來變更主控台上的沙箱。 若要這樣做，請前往[開發人員首頁](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home)主控台上並[啟用裝置入口網站](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox)。 一旦您有 Xbox 開發人員入口網站開啟：

2. 按一下  **Xbox Live**。
3. 輸入您的開發沙箱中的文字欄位和小雞**變更**。

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>使用 Xbox One 主控台 UI

您可以使用[開發人員首頁](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home)直接變更沙箱主控台上：

1. 按一下 **變更沙箱**下**快速動作**。
2. 輸入沙箱識別碼，然後按一下**儲存並重新啟動**。

### <a name="sign-in-with-the-xbox-app"></a>使用 Xbox 應用程式登入

一旦您切換開發電腦，您會想要確認您要登入，到 Xbox Live 的合格的測試帳戶標題使用適當的沙箱。 這可以透過登入[Xbox Live 應用程式](https://www.xbox.com/en-US/xbox-app)。 一旦您的開發環境可讓您開始使用所需的沙箱 Xbox 應用程式會使用相同的條件約束為任何其他 Xbox Live 的登入使用者服務在沙箱上執行。 這可讓您可確認您要使用沙箱是有效的帳戶。
