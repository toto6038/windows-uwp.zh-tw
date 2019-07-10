---
title: 透過鬆散檔案註冊部署應用程式
description: 本指南說明如何使用鬆散檔案配置來驗證及共用 Windows 10 應用程式，而不需加以封裝。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10、 uwp、 裝置入口網站、 應用程式管理員、 部署、 sdk
ms.localizationpriority: medium
ms.openlocfilehash: 3369f3a982efec258fb5ac2358b2962e84e6cefb
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713764"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>透過鬆散檔案註冊部署應用程式 

本指南說明如何使用鬆散檔案配置來驗證及共用 Windows 10 應用程式，而不需加以封裝。 註冊鬆散檔案配置，可讓開發人員快速驗證他們的應用程式，而不需要封裝並安裝應用程式。 

## <a name="what-is-a-loose-file-layout"></a>什麼是鬆散式檔案的版面配置？

鬆散式檔案版面配置是將應用程式內容放在一個資料夾，而不會搜查封裝程序的動作。 套件內容會是 「 鬆散 」 可用的資料夾中和未封裝。 

> [!WARNING]
> 鬆散式檔案配置註冊是為了開發人員和設計工具，快速地在開發期間驗證其應用程式。 這個方法不應該用來 「 內部 」 或飛行應用程式。 我們建議使用受信任的憑證簽署的已封裝應用程式上執行最終驗證。 

## <a name="advantages-of-loose-file-registration"></a>鬆散式檔案註冊的優點

- **快速驗證**-已解除封裝的應用程式檔案，因為使用者可以快速註冊鬆散檔案配置並啟動應用程式。 就像一般的應用程式中，使用者將能夠使用應用程式，因為它的設計。 
- **輕鬆在網路發佈**-如果鬆散式檔案位於網路共用，而不是本機的磁碟機中，開發人員可以傳送給其他使用者可存取網路的網路共用位置，而且他們可以註冊的鬆散式檔案配置，並執行應用程式。 這可讓多位使用者同時驗證應用程式。 
- **共同作業**-鬆散檔案註冊可讓開發人員和設計工具，來登錄應用程式時，視覺效果資產上繼續運作。 在啟動應用程式時，使用者會看到這些變更。 請注意，您可以只變更靜態資產，以這種方式。 如果您需要修改任何程式碼或動態建立的內容，您必須重新編譯應用程式。

## <a name="how-to-register-a-loose-file-layout"></a>如何註冊鬆散檔案版面配置

Windows 提供多個開發人員工具，以註冊在本機和遠端裝置上的鬆散式檔案配置。 您可以從中`WinDeployAppCmd`（Windows SDK 工具），Windows Device Portal，PowerShell，並[Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)。 下面我們將介紹如何註冊使用這些工具的鬆散式檔案。 但首先，請確定您有下列設定：

- 您的裝置必須在 Windows 10 Creators Update (建置 14965) 或更新版本。
- 您必須啟用[開發人員模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)並[裝置探索](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery)在所有裝置上。

> [!IMPORTANT]
> 鬆散式檔案註冊才可以使用支援的網路共用 (SMB) 通訊協定的裝置：桌面和 Xbox。 

### <a name="register-with-windeployappcmd"></a>向 WinDeployAppCmd

如果您使用的對應至 Windows 10 Creators Update (建置 14965) 或更新版本的 SDK 工具，您可以使用`WinDeployAppCmd`命令在命令提示字元。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**網路路徑**– 應用程式的鬆散式檔案的路徑。

**IP 位址**– 目標機器的 IP 位址。

**目標電腦的 PIN** – pin 碼，如有需要，建立與目標裝置的連線。 系統會提示您再試一次`-pin`選項需要驗證時。 請參閱[裝置探索](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)以了解如何取得 pin 碼。

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal 並用於所有 Windows 10 裝置，並為開發人員用來測試及驗證他們的工作。 它為所有開發人員社群使用其瀏覽器 UX 的對象與 REST 端點所提供。 如需有關裝置入口網站的詳細資訊，請參閱[Windows Device Portal 概觀](device-portal.md)。

若要在裝置入口網站中註冊的鬆散式檔案配置，請遵循下列步驟。

1. 連接到裝置入口網站中的步驟**安裝程式**一節[Windows Device Portal 概觀](device-portal.md)。
1. 在 [應用程式管理員] 索引標籤中，選取**從網路共用註冊**。
1. 輸入網路共用路徑以鬆散檔案配置。 
1. 如果主機裝置無法存取網路共用，則會提示輸入必要的認證。
1. 註冊完成之後，您可以啟動應用程式。

在裝置入口網站的應用程式管理員] 頁面上，您也可以註冊選用的鬆散式檔案配置的主要應用程式選取**我要指定選擇性套件**核取方塊，然後指定 [網路共用的選擇性應用程式的路徑. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell 也可讓您註冊鬆散檔案配置，但只能在本機裝置上。 如果您需要註冊到遠端裝置的版面配置，您必須使用其中一種其他方法。 

若要註冊的鬆散式檔案配置，請啟動 PowerShell 並輸入下列項目。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>疑難排解

### <a name="mapped-network-drives"></a>連線的網路磁碟機
目前，鬆散檔案註冊不支援對應的網路磁碟機。 是指對應的磁碟機，無論是完整的網路共用路徑。

### <a name="registration-failure"></a>註冊失敗
在其註冊正在進行裝置都必須能夠存取的檔案配置。 如果檔案配置裝載在網路共用，請確定裝置可以存取。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>不修改視覺效果資產載入應用程式 
應用程式會在啟動時載入其視覺效果的資產。 如果已修改視覺效果資產啟動應用程式之後，您必須重新啟動此應用程式檢視最新的變更。
