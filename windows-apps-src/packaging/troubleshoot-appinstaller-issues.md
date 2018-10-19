---
author: ridomin
title: 疑難排解應用程式安裝程式檔案的安裝問題
description: 使用應用程式安裝程式檔案側載應用程式時的常見問題。
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 應用程式安裝程式, AppInstaller, 側載
ms.localizationpriority: medium
ms.openlocfilehash: e94eb0e819796dda456899bb877057e4532f5ce9
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "4956799"
---
# <a name="troubleshoot-installation-issues-with-the-app-installer-file"></a>疑難排解應用程式安裝程式檔案的安裝問題

如果您從應用程式安裝程式檔案安裝應用程式時發現任何問題，本主題會提供一些可能有助益的疑難排解指引。

## <a name="prerequisites"></a>必要條件

若要能夠在 Windows 10 中側載應用程式，使用者裝置必須符合以下需求：

- 必須啟用裝置以使用開發人員模式或是側載應用程式。 若要深入了解，請參閱[啟用您的裝置以用於開發](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。
- 用來簽署套件的憑證，必須受到裝置信任。 如需詳細資訊，請參閱以下的**＜驗證憑證＞**一節。
- Windows 10 版本必須支援 `.appinstaller` 檔案結構描述和散發通訊協定。

## <a name="common-issues"></a>常見問題

第一次在使用者電腦中側載應用程式時有一些常見問題。 接下來的幾個章節描述最常見的問題和其解決方案。

### <a name="windows-version"></a>Windows 版本

每個 Windows 10 版本改善側載體驗，在下表您會找到每個主要版本中可用的功能。 如果您嘗試使用 Windows 10 版本中不支援的方法側載應用程式，會發生部署錯誤。

| 版本 | 側載附註 |
|---------|----------------|
| 組建 17134 (2018 年 4 月更新，版本 1804)    | 可以透過 UNC/共用資料夾存取 `.appinstaller` 檔案。 也會提供可設定的更新檢查。 |
| 組建 16299 (Fall Creators Update，版本 1709) | 導入了`.appinstaller`檔案以提供應用程式的自動更新。 此版本僅支援 HTTP 端點。 更新檢查不可以設定，而且每隔 24 小時發生一次。 |
| 組建 15063 (Creators Update，版本 1703)      | 應用程式安裝程式應用程式可以從 Microsoft Store 下載 app 相依性（只能在發行模式）。 |
| 組建 14393 (年度更新版，版本 1607)   | 導入了應用程式安裝程式應用程式以安裝.appx 和.appxbundle 檔案，.appinstaller 檔案不受支援。 |
| 組建 10586 (11 月更新，版本 1511)      | 透過 PowerShell 使用[Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps)命令，才可以側載。 |
| 組建 10240 (Windows 10 版本 1507)           | 透過 PowerShell 使用[Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps)命令，才可以側載。 |

### <a name="trusted-certificates"></a>受信任的憑證

應用程式套件必須使用裝置信任的憑證進行簽署。 常見的憑證授權單位提供的憑證，在 Windows 作業系統中預設為受信任的憑證，但如果是不受信任的憑證，必須在安裝應用程式**之前**已安裝在裝置中。 若要信任憑證，憑證必須出現在裝置上的下列其中一個本機電腦憑證存放區：

- 受信任的發行者
- 受信任的人
- 受信任的根憑證授權單位（不建議）

 >[!IMPORTANT]
 > 在本機電腦存放區安裝憑證需要系統管理存取權。

### <a name="dependencies-not-installed"></a>未安裝相依性 

根據產生應用程式使用的應用程式平台，UWP 應用程式可能會有架構相依性。 如果您使用 C# 或 VB，app 會需要 .NET 執行階段和 .NET Framework 套件。 C++ 應用程式需要 VCLibs。

>[!IMPORTANT] 
> 如果在發行模式設定中建置應用程式套件，將會從 Microsoft Store 取得架構相依性。 不過，如果在偵錯模式設定中建置應用程式，將會從`.appinstaller`檔案中指定的位置取得相依性。

### <a name="files-not-accessible"></a>不可存取檔案

從 HTTP 端點安裝時，請務必確認所有檔案都是正確的 MIME 類型、可供存取。 確認這些檔案最簡單的方法是追蹤 Visual Studio 所產生 HTML 網頁中的連結。 您必須檢查這些檔案︰

- `.appinstaller` 檔案，類型為 `application/xml`
- `.appx` 和`.appxbundle`檔案，類型為 `application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>找出應用程式安裝程式應用程式問題

如果應用程式安裝程式應用程式無法安裝應用程式，下列步驟將會協助找出安裝問題。

### <a name="verify-app-package-file-installation"></a>確認應用程式套件檔案安裝

- 下載應用程式套件檔案到本機資料夾，並嘗試使用[Add-appxpackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) PowerShell 命令來安裝它。

- 下載`.appinstaller`檔案到本機資料夾，然後使用`Add-AppxPackage -Appinstaller` PowerShell 命令嘗試安裝它。

## <a name="related-logs"></a>相關記錄檔

應用程式部署基礎結構於 Windows 事件檢視器中提供用於偵錯的記錄檔。 可在這裡找到這些記錄檔： `Application and Services Logs->Microsoft->Windows->AppxDeployment-Server`



