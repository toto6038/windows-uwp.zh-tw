---
author: c-don
title: 部署應用程式通過鬆散檔案註冊
description: 本指南會示範如何使用鬆散檔案配置來驗證和共用 Windows 10 應用程式，而不需要進行封裝。
ms.author: cdon
ms.date: 6/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 裝置入口網站、 應用程式管理員、 部署、 sdk
ms.localizationpriority: medium
ms.openlocfilehash: a6a96a78cf03ce4994ddee1c929997b12a2d028f
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "4622159"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>部署應用程式通過鬆散檔案註冊 

本指南會示範如何使用鬆散檔案配置來驗證和共用 Windows 10 應用程式，而不需要進行封裝。 登錄鬆散檔案版面配置可讓開發人員快速驗證其應用程式，而不需要以封裝及安裝應用程式。 

## <a name="what-is-a-loose-file-layout"></a>什麼是鬆散檔案配置？

鬆散檔案版面配置是放置應用程式內容，而不是進行封裝程序的資料夾中的動作。 套件內容是 「 鬆散 「 適用於資料夾及不封裝。 

> [!WARNING]
> 鬆散檔案配置註冊為開發人員和快速地在作用中的開發期間驗證其應用程式。 這種方式不應該用來 「 事 」 或飛行應用程式。 我們建議，以受信任的憑證簽署的已封裝應用程式上執行最終的驗證。 

## <a name="advantages-of-loose-file-registration"></a>鬆散檔案註冊的優點

- **快速驗證**-已解壓縮的應用程式檔案，因為使用者可以快速登錄鬆散檔案配置並啟動應用程式。 如同一般的應用程式中，使用者都能使用應用程式，因為這設計。 
- 使用**簡單的網路中發佈**-如果鬆散檔案都位於網路共用，而不是本機磁碟機，開發人員可以傳送網路共用位置給其他使用者擁有的存取權的網路，發行前小眾，而且也可以登錄鬆散檔案配置和執行應用程式。 這可讓多個使用者同時驗證應用程式。 
- **共同作業**鬆散檔案註冊可讓開發人員和視覺資產上繼續運作，而應用程式已登錄。 啟動應用程式時，使用者會看到這些變更。 請注意，您可以只會變更以這種方式的靜態資產。 如果您需要修改任何程式碼或動態建立的內容，您必須重新編譯應用程式。

## <a name="how-to-register-a-loose-file-layout"></a>如何登錄鬆散檔案配置

Windows 提供多個登錄鬆散檔案版面配置，在本機和遠端裝置上的開發人員工具。 您可以選擇從`WinDeployAppCmd`（Windows SDK 工具），Windows 裝置入口網站、 PowerShell，以及[Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)。 以下我們將會介紹如何登錄鬆散檔案使用這些工具。 但是首先，請確定您有下列安裝程式：

- 您的裝置必須是在 Windows 10 Creators Update (組建 14965) 或更新版本。
- 您將需要啟用[開發人員模式](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)和在所有裝置上的[裝置探索](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery)。

> [!IMPORTANT]
> 鬆散檔案註冊就只適用於支援的網路共用 (SMB) 通訊協定的裝置上： 傳統型應用程式和 Xbox。 

### <a name="register-with-windeployappcmd"></a>使用 WinDeployAppCmd 註冊

如果您使用對應到 Windows 10 Creators Update (組建 14965) 或更新版本的 SDK 工具，您可以使用`WinDeployAppCmd`命令在命令提示字元。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**網路路徑**– 應用程式的鬆散檔案的路徑。

**IP 位址**-目標電腦的 IP 位址。

**目標電腦 PIN** – A PIN，如果需要，才能與目標裝置建立連線。 將會提示您使用重試`-pin`如果需要驗證選項。 請參閱 [[裝置探索](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)\] 來了解如何取得 PIN。

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal 是適用於所有 Windows 10 裝置上，由開發人員用來測試及驗證工作。 它 caters 的所有對象的開發人員社群，與 UX 其瀏覽器與 REST 端點。 如需 Device Portal 的詳細資訊，請參閱[Windows Device Portal 概觀](device-portal.md)。

若要註冊裝置入口網站中的鬆散檔案配置，請依照下列步驟。

1. 下列的[Windows Device Portal 概觀](device-portal.md)的 [**安裝**] 區段中的步驟來連線到 Device Portal。
1. 在 [應用程式管理員] 索引標籤中，選取 [**從網路共用註冊**。
1. 輸入網路共用路徑的鬆散檔案配置。 
1. 如果主機裝置沒有存取權的網路共用，會有的命令提示字元中輸入必要的認證。
1. 完成註冊之後，您可以啟動應用程式。

裝置入口網站的應用程式管理員 \] 頁面，您也可以登錄選用的鬆散檔案版面配置的主要的應用程式選取**我想要指定選用套件**的核取方塊，然後指定 [選用應用程式的網路共用路徑。 

### <a name="powershell"></a>PowerShell 

Windows PowerShell 也可讓您登錄鬆散檔案版面配置，但僅限在本機裝置上。 如果您需要註冊到遠端裝置的版面配置，您將需要使用其中一個其他方法。 

若要登錄鬆散檔案配置、 啟動 PowerShell 並輸入下列內容。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>疑難排解

### <a name="mapped-network-drives"></a>對應的網路磁碟機
目前，對應的網路磁碟機不支援鬆散檔案註冊。 對應的磁碟機，以完整的網路共用路徑參考。

### <a name="registration-failure"></a>註冊失敗
在其註冊正在舉行的裝置必須能夠存取檔案配置。 如果網路共用上裝載檔案配置，請確定裝置有存取權。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>修改視覺資產不是在應用程式中載入 
App 會在啟動時載入其視覺資產。 如果修改無視於 \ 視覺資產在啟動應用程式之後，您必須重新啟動 「 應用程式檢視的最新的變更。