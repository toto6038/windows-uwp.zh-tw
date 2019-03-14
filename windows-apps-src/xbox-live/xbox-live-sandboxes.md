---
title: Xbox Live 的沙箱
description: 深入了解適用於 Xbox Live 開發的沙箱。
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee284550a9b508a8d46556bf0353bd75d55014f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599473"
---
# <a name="xbox-live-sandboxes-intro"></a>Xbox Live 的沙箱簡介

在  [Xbox Live 服務組態](xbox-live-service-configuration.md)，它所述，您必須設定通常在的 線上，您的標題的相關資訊[合作夥伴中心](https://partner.microsoft.com/dashboard)。  這項資訊包括像是排行榜想要顯示，播放程式可以解除鎖定的成就的標題、 配對組態等。

當您變更您的服務組態時，這些需要之前所做的變更會收取的 Xbox Live 的其餘部分，而且可以看到依您的標題，從合作夥伴中心發佈。

您發行至所謂的開發沙箱。  這些可讓您能夠在您的標題隔離的環境中的變更。  這些解決方案提供了幾項中所述的優點如下一節。

根據預設，一個 Xbox 和 Windows 10 電腦是零售沙箱中。

## <a name="benefits"></a>優點

開發沙箱提供了幾個優點：


1. 您可以逐一查看變更的更新標題而不會影響目前可用的版本。
2. 有些工具只能在開發沙箱，基於安全性考量。
3. 在您的小組上有些開發人員可能想要 「 分支 」 和 「 測試服務組態變更，而不會影響您的主要開發中的服務組態。
4. 無法看到其他 「 發行者 」，您正在使用沒有正在授與存取您的沙箱。

您也可以**選擇性地**建立測試帳戶。  您可以使用這些如果您不想要用於測試您的標題，一般的 Xbox Live 帳戶，或您需要數個帳戶，來測試案例，例如社交互動 (例如： 檢視某位朋友的統計資料) 或多人連線遊戲。

測試帳戶可以只有登入到開發沙箱，並將在下一節中說明。

## <a name="finding-out-about-your-sandbox"></a>找出關於您的沙箱

大部分的開發人員必須只有一個沙箱。  幸運的是在沙箱會為您建立時建立的標題。

1. 您了解您的沙箱這裡前往合作夥伴中心： ![](images/getting_started/first_xbltitle_dashboard.png)

1. 然後按一下您的標題： ![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. 最後，按一下 [服務]-> Xbox Live 左側功能表中 ![](images/getting_started/first_xbltitle_leftnav.png)

1. 您現在可以看到您的沙箱所列，如下所示 ![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>您的沙箱會如何影響您的工作流程

通常您搭配沙箱功能如下：

1. （單次）切換至您的開發沙箱的 PC 或 Xbox One。
2. （多倍）當您變更您的服務組態，您將發行變更至您的開發沙箱。  服務設定變更是定義成積、 新增排行榜，或修改多人遊戲工作階段範本等項目。
3. （幾次）如果您正在與其他小組成員，您可以提供存取您的沙箱
4. （單次）如果您要測試的項目在零售，或想要休息播放您最愛的 Xbox 遊戲，您必須切換回 零售版的您的沙箱。

這些案例將會在下面詳細說明。  程序會有電腦和主控台中，有些差異，因此會有不同的區段，每個。

## <a name="switch-your-pcs-development-sandbox"></a>切換您的 PC 開發沙箱

如果您想要切換您的 PC 開發沙箱，若要這樣做的建議的方式使用 Windows Device Portal (WDP)。  您也可以執行操作，透過命令列。  我們將說明這兩種方式。

### <a name="windows-device-portal"></a>Windows Device Portal

如果您未在您的電腦上已啟用 WDP，請依照下列指示來執行這項操作。 [在 Windows 桌面上的 設定裝置入口網站](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

一旦您這麼做，開啟 Windows 開發人員入口網站連接至它在網頁瀏覽器中，上述文件所述。

然後您按一下 「 Xbox Live 「 前往適當的區段，如下所示。

![](images/getting_started/wdp_switch_sandbox.png)

您可以輸入您的沙箱所得到的步驟中透過*尋找出您的沙箱*並按一下 [變更]。

若要切換回零售版，您可以輸入零售這裡所示。

### <a name="powershell-module"></a>PowerShell 模組

[Xbox Live 的 PowerShell 模組](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md)XboxlivePSModule 包含各種公用程式，來協助 Xbox Live 的開發，包括變更主控台的電腦上的沙箱。

* 若要使用它從[PowerShell 資源庫](https://www.powershellgallery.com/packages/XboxlivePSModule)，開啟 PowerShell 視窗：
    1. 下載並安裝模組： `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. 開始使用執行 `Import-Module XboxlivePSModule`
    3. 執行 cmdlet，亦即組 XblSandbox XDKS.1 或 Get XblSandbox

* 從 zip 檔案，在取用它[ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)，開啟 PowerShell 視窗中，
    1. 執行 `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`
    2. 執行 cmdlet，亦即組 XblSandbox XDKS.1 或 Get XblSandbox

### <a name="command-prompt-script"></a>命令提示字元指令碼

下載 Xbox Live 工具套件[ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)並將它解壓縮。  您會發現 SwitchSandbox.cmd 批次檔內。

若要切換您的沙箱的系統管理員模式中執行此作業。  第一個引數是沙箱。  例如若您嘗試切換至 XDKS.1 沙箱，您必須請：

```
SwitchSandbox.cmd XDKS.1
```

若要切換回零售版，您只需要提供，做為第二個引數。

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>切換您的 Xbox One 主控台開發沙箱

### <a name="using-windows-dev-portal"></a>使用 Windows 開發人員入口網站

您可以使用 Windows 開發人員入口網站來變更主控台上的沙箱。  若要這樣做，請移至 [開發人員首頁] 主控台上並加以啟用。

之後，您可以在您的電腦連線到您的主控台上輸入網頁瀏覽器上的 IP 位址。  然後，您可以按一下"Xbox Live"，並那里的文字方塊中輸入沙箱。

### <a name="using-xbox-one-manager"></a>使用 Xbox One Manager

Xbox One Manager 可讓您從您的電腦管理主控台的特定層面。  這包括重新開機、 管理已安裝的應用程式，以及變更您的沙箱。

以滑鼠右鍵按一下您想要變更的沙箱，請移至 [設定] 的主控台

然後，您可以輸入的沙箱。

### <a name="using-xbox-one-console-ui"></a>使用 Xbox One 主控台 UI

如果您想要變更您的開發沙箱，直接從您的主控台，您可以移至 [設定]。  然後移至 [開發人員設定]，您會看到一個選項來變更您的沙箱。

## <a name="sandbox-uses"></a>沙箱會使用

### <a name="data-that-is-sandboxed"></a>已沙箱化的資料
您可以使用沙箱功能來管理您的小組在開發程序的開發人員之間的存取。  例如，您可能要隔離您的開發團隊與測試人員之間的資料。

沙箱化的資料包括：
- 成就、 排行榜和的使用者統計資料。  成就累積一個沙箱中的使用者不會轉譯為另一個沙箱。
- 多人遊戲和配對。  使用者無法進行多人與人的遊戲是不同的沙箱。
- 服務組態。  如果您在一個沙箱中的標題中新增新的成就，它不顯示在不同的沙箱。  這適用於所有服務組態資料。

非沙箱化的資料是以社交資訊。  讓使用者如果遵循另一位使用者，關聯性是沙箱無從驗證的範例。

### <a name="examples"></a>範例
下面將提供一些範例說明一些您可能想要使用多個沙箱的優點。

> **注意**：如果您是在 Xbox 創作者計劃中，您只能有一個沙箱。  如果您有需要建立多個沙箱，請將套用至ID@Xbox程式。

#### <a name="service-config-isolation"></a>服務設定隔離
如先前所述，服務組態會是特定的沙箱。  因此您可能必須*開發*沙箱並*測試*沙箱。  當您為您的標題，以您的測試人員組建時，您會發行您[服務組態](xbox-live-service-configuration.md)要*測試*沙箱。

然後在此同時，您可以新增成積、 或不同的多人遊戲工作階段類型以您*開發*沙箱，而不會影響您的測試人員發現服務組態。

#### <a name="multiplayer"></a>多人遊戲
需要上述範例中的使用*Development*並*測試*沙箱。  或許您的服務組態之間是一樣的沙箱，但您的開發人員會建立多人遊戲的功能，而且想要測試與彼此的配對。  您的測試人員也會測試多人連線遊戲。

在這類情況下，您的開發人員可能不想要在 Xbox Live 配對讓它們符合與測試人員，因為它們會分別偵錯問題。  若要避免這個問題的好方法是為要在開發人員*Development*沙箱和要在個別的測試人員*測試*沙箱。  如此可讓隔離這兩個群組。

## <a name="advanced"></a>進階

為了簡化開發程序，開始您的預設沙箱並新增新的沙箱請謹慎。

一旦您找到需要增加您的存取控制和資料隔離，您可以看到[進階 Xbox Live 沙箱](advanced-xbox-live-sandboxes.md)文章。  
