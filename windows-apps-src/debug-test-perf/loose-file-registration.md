---
title: 透過鬆散檔案註冊部署應用程式
description: 本指南說明如何使用鬆散檔案配置來驗證及共用 Windows 10 應用程式，而不需加以封裝。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10，uwp，裝置入口網站，應用程式管理員，部署，sdk
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681929"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>透過鬆散檔案註冊部署應用程式 

本指南說明如何使用鬆散檔案配置來驗證及共用 Windows 10 應用程式，而不需加以封裝。 註冊鬆散檔案版面配置可讓開發人員快速驗證其應用程式，而不需要封裝和安裝應用程式。 

## <a name="what-is-a-loose-file-layout"></a>什麼是鬆散式檔案版面配置？

鬆散檔案配置只是將應用程式內容放在資料夾中的動作，而不是進行封裝程式。 封裝內容在資料夾中是「鬆散」的，而且不會封裝。 

> [!WARNING]
> 鬆散檔案配置註冊是讓開發人員和設計人員在開發期間快速驗證其應用程式。 此方法不應用來「欵產品」或飛行應用程式。 我們建議您在使用受信任憑證簽署的已封裝應用程式上執行最後的驗證。 

## <a name="advantages-of-loose-file-registration"></a>鬆散檔案註冊的優點

- **快速驗證**-因為應用程式檔已經解壓縮，使用者可以快速註冊鬆散檔案配置並啟動應用程式。 就像一般應用程式一樣，使用者也可以使用設計的應用程式。 
- **簡單的網路散發**-如果鬆散的檔案位於網路共用而不是本機磁片磁碟機，則開發人員可以將網路共用位置傳送給具有網路存取權的其他使用者，他們可以註冊鬆散檔案配置並執行應用程式。 這可讓多個使用者同時驗證應用程式。 
- 共同**作業-鬆散**檔案註冊可讓開發人員和設計人員在註冊應用程式時繼續處理視覺資產。 使用者會在啟動應用程式時看到這些變更。 請注意，您只能以這種方式變更靜態資產。 如果您需要修改任何程式碼或動態建立的內容，您必須重新編譯應用程式。

## <a name="how-to-register-a-loose-file-layout"></a>如何註冊鬆散檔案版面配置

Windows 提供了多個開發人員工具，可在本機和遠端裝置上註冊鬆散檔案版面配置。 您可以從 `WinDeployAppCmd` （Windows SDK 工具）、Windows 裝置入口網站、PowerShell 和[Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)選擇。 下面我們將討論如何使用這些工具來註冊鬆散檔案。 但首先，請確定您已遵循下列設定：

- 您的裝置必須是 Windows 10 建立者更新（組建14965）或更新版本。
- 您將需要在所有裝置上啟用[開發人員模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)和[裝置探索](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)。

> [!IMPORTANT]
> 鬆散檔案註冊僅適用于支援網路共用（SMB）通訊協定的裝置： Desktop 和 Xbox。 

### <a name="register-with-windeployappcmd"></a>向 WinDeployAppCmd 註冊

如果您使用的 SDK 工具對應到 Windows 10 建立者更新（組建14965）或更新版本，您可以在命令提示字元中使用 `WinDeployAppCmd` 命令。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**網路路徑**–應用程式鬆散檔案的路徑。

**Ip 位址**–目的電腦的 ip 位址。

**目的電腦 pin** – pin （如有必要），以建立與目標裝置的連線。 如果需要驗證，系統會提示您使用 [`-pin`] 選項重試。 若要瞭解如何取得 PIN，請參閱[裝置探索](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)。

### <a name="windows-device-portal"></a>Windows Device Portal

Windows 裝置入口網站適用于所有 Windows 10 裝置，供開發人員用來測試和驗證其工作。 它會已經考慮開發人員社區的所有物件及其瀏覽器 UX 和 REST 端點。 如需裝置入口網站的詳細資訊，請參閱[Windows 裝置入口網站總覽](device-portal.md)。

若要在裝置入口網站中註冊鬆散檔案版面配置，請遵循下列步驟。

1. 遵循[Windows 裝置入口網站總覽](device-portal.md)的**安裝**一節中的步驟，連接到裝置入口網站。
1. 在 [應用程式管理員] 索引標籤中，選取 [**從網路共用註冊**]。
1. 輸入鬆散檔案配置的網路共用路徑。 
1. 如果主機裝置沒有網路共用的存取權，系統會提示您輸入必要的認證。
1. 註冊完成後，您就可以啟動應用程式。

在裝置入口網站的 [應用程式管理員] 頁面上，您也可以藉由選取 [**我要指定選擇性套件**] 核取方塊，然後指定選用應用程式的網路共用路徑，為您的主要應用程式註冊選擇性的鬆散檔案配置。 

### <a name="powershell"></a>PowerShell 

Windows PowerShell 也可讓您登錄鬆散檔案配置，但只能在本機裝置上註冊。 如果您需要將配置註冊到遠端裝置，您必須使用其中一種其他方法。 

若要註冊鬆散檔案配置，請啟動 PowerShell 並輸入下列。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>[疑難排解]

### <a name="mapped-network-drives"></a>連線的網路磁碟機
目前，不支援將對應的網路磁碟機機用於鬆散檔案註冊。 請參閱具有完整網路共用路徑的對應磁片磁碟機。

### <a name="registration-failure"></a>註冊失敗
註冊進行所在的裝置必須具有檔案配置的存取權。 如果檔案配置是裝載在網路共用上，請確定該裝置具有存取權。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>未在應用程式中載入視覺資產的修改 
應用程式會在啟動時載入其視覺資產。 如果在啟動應用程式之後對視覺資產進行修改，您必須重新開機應用程式，以查看最新的變更。
