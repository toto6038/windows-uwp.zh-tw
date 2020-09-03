---
title: 透過鬆散檔案註冊部署應用程式
description: 本指南說明如何使用鬆散檔案配置來驗證及共用 Windows 10 應用程式，而不需加以封裝。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, 裝置入口網站, 應用程式管理員, 部署, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 0fd5bf6be691974d956de0c71f4a1d11aa1a229f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166092"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>透過鬆散檔案註冊部署應用程式 

本指南說明如何使用鬆散檔案配置來驗證及共用 Windows 10 應用程式，而不需加以封裝。 註冊鬆散檔案配置可讓開發人員快速驗證其應用程式，而不需要封裝和安裝應用程式。 

## <a name="what-is-a-loose-file-layout"></a>什麼是鬆散檔案配置？

鬆散檔案配置只是單純將應用程式內容放在資料夾中的動作，而不是將程式封裝。 這些內容在資料夾中是「鬆散」的，未經過封裝。 

> [!WARNING]
> 鬆散檔案配置註冊是用於讓開發人員和設計人員在開發期間快速驗證其應用程式。 此方法不應用於應用程式的搶先測試版或正式發行前小眾測試版。 我們建議您對使用受信任憑證簽署過的已封裝應用程式執行最後的驗證。 

## <a name="advantages-of-loose-file-registration"></a>鬆散檔案註冊的優點

- **快速驗證** - 因為應用程式檔案已經解壓縮，使用者可以快速註冊鬆散檔案配置並啟動應用程式。 就像一般應用程式一樣，使用者也可以依設計原意使用應用程式。 
- **簡單的網路內散發** - 如果鬆散檔案位於網路共用中，而不是本機磁碟機中，開發人員可以將網路共用位置傳送給具有該網路存取權的其他使用者，這些使用者便可以註冊鬆散檔案配置並執行應用程式。 這可讓多個使用者同時驗證應用程式。 
- **共同作業** - 鬆散檔案註冊可讓開發人員和設計人員在應用程式註冊時繼續處理視覺資產。 使用者會在啟動應用程式時看到這些變更。 請注意，您只能以這種方式變更靜態資產。 如果您需要修改任何程式碼或動態建立的內容，必須重新編譯應用程式。

## <a name="how-to-register-a-loose-file-layout"></a>如何註冊鬆散檔案配置

Windows 提供多個開發人員工具，可在本機和遠端裝置上註冊鬆散檔案配置。 您可以選擇 `WinDeployAppCmd` (Windows SDK 工具)、Windows 裝置入口網站、PowerShell 及 [Visual Studio](./deploying-and-debugging-uwp-apps.md#register-layout-from-network)。 以下我們將討論如何使用這些工具來註冊鬆散檔案。 但首先，請確定您已遵循下列設定：

- 您的裝置必須是在 Windows 10 Creators Update (組建 14965) 或更新版本之上。
- 您將需要在所有裝置上啟用[開發人員模式](../get-started/enable-your-device-for-development.md)和[裝置探索](../get-started/enable-your-device-for-development.md#device-discovery)。

> [!IMPORTANT]
> 鬆散檔案註冊僅適用於支援網路共用 (SMB) 通訊協定的裝置：桌上型電腦和 Xbox。 

### <a name="register-with-windeployappcmd"></a>使用 WinDeployAppCmd 進行註冊

如果您使用 Windows 10 Creators Update (組建14965) 或更新版本相對應的 SDK 工具，就可以在命令提示字元中使用 `WinDeployAppCmd` 命令。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**網路路徑** – 應用程式的鬆散檔案路徑。

**IP 位址** – 目標電腦的 IP 位址。

**目標電腦** - 用於與目標裝置建立連線的 PIN (如為必要)。 如果需要驗證，系統會提示您使用 -`-pin` 選項重試。 如需瞭解如何取得 PIN，請參閱[裝置探索](../get-started/enable-your-device-for-development.md#device-discovery)。

### <a name="windows-device-portal"></a>Windows 裝置入口網站

Windows 裝置入口網站能用於所有 Windows 10 裝置，供開發人員用來測試和驗證其工作。 它已經考慮開發人員社群的所有對象及其瀏覽器 UX 和 REST 端點。 如需裝置入口網站的詳細資訊，請參閱 [Windows 裝置入口網站概觀](device-portal.md)。

若要在裝置入口網站中註冊鬆散檔案配置，請遵循下列步驟。

1. 依照 [Windows 裝置入口網站概觀](device-portal.md)的**設定**中所述步驟，連線到裝置入口網站。
1. 在 [應用程式管理員] 索引標籤中，選取 [從網路共用註冊]  。
1. 輸入鬆散檔案配置的網路共用路徑。 
1. 如果主機裝置沒有網路共用的存取權，系統會提示您輸入必要的認證。
1. 註冊完成後，您就可以啟動應用程式。

在裝置入口網站的 [應用程式管理員] 頁面上，您也可以選取 [我要指定選用套件]  核取方塊，然後指定選用應用程式的網路共用路徑，為您的主要應用程式註冊選用的鬆散檔案配置。 

### <a name="powershell"></a>PowerShell 

您也可用 Windows PowerShell 註冊鬆散檔案配置，但只能在本機裝置上註冊。 如果您需要在遠端裝置上註冊配置，必須使用其他方法的其中一種。 

若要註冊鬆散檔案配置，請啟動 PowerShell 並輸入如下。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>疑難排解

### <a name="mapped-network-drives"></a>連線的網路磁碟機
目前，不支援將對應的網路磁碟機用於鬆散檔案註冊。 請使用完整網路共用路徑指向對應的磁碟機。

### <a name="registration-failure"></a>註冊失敗
進行註冊的裝置必須具有檔案配置的存取權。 如果檔案配置是存放在網路共用上，請確定該裝置具有存取權。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>視覺資產的修改未在應用程式中載入 
應用程式會在啟動時載入其視覺資產。 如果是啟動應用程式之後修改視覺資產，您必須重新啟動應用程式，才能查看最新的變更。