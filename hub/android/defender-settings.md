---
title: 藉由更新 Defender 設定來改善效能速度
description: 藉由更新 Windows Defender 設定以排除檢查指定的檔案類型，來改善整體效能速度和組建時間的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、windows defender、設定、設定、排除專案、% USERPROFILE%、devenv.exe、效能、速度、組建、gradle
ms.date: 04/28/2020
ms.openlocfilehash: e762e595bf2b8aa9b8420d41f68d2890e7d5dfe0
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823202"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>更新 Windows Defender 設定以來改善效能

本指南說明如何在您的 Windows Defender 安全性設定中設定排除專案，以改善您的組建時間以及 Windows 電腦的整體效能速度。

## <a name="windows-defender-overview"></a>Windows Defender 總覽

在 Windows 10 1703 版和更新版本中， [Windows Defender 防毒軟體](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus) 應用程式是 windows 安全性的一部分。 Windows Defender 的目標是要讓您的電腦保持安全，以抵禦病毒、勒索軟體、間諜軟體和其他安全性威脅的內建保護。

**不過**，在開發 Android 應用程式時，Windows Defender 的即時保護也會大幅減緩檔案系統存取和建立速度。

在 Android 組建程式期間，會在您的電腦上建立許多檔案。 啟用防毒軟體即時掃描時，在防毒程式掃描該檔案時，每次建立新檔案時，就會終止組建程式。

幸運的是，Windows Defender 可以排除檔案、專案目錄，或是您知道防毒軟體掃描程式安全的檔案類型。

> [!WARNING]
> 為了確保您的電腦免于惡意軟體的安全，您不應該完全停用即時掃描或 Windows Defender 防毒軟體。

## <a name="add-exclusions-to-windows-defender"></a>將排除專案新增至 Windows Defender

若要改善您的 Android 組建速度，請透過下列方式在 [Windows Defender 安全性中心](windowsdefender://) 新增排除專案：

1. 選取 Windows 功能表的 [ **開始** ] 按鈕
2. 輸入 **Windows 安全性**
3. 選取 [**病毒與威脅防護**]
4. 選取 [**病毒 & 威脅防護設定**] 下的 [**管理設定**]
5. 滾動至 [**排除** 專案] 標題，然後選取 [**新增或移除排除** 專案]
6. 選取 [ **+ 新增排除**]。 接著，您必須選擇要新增的 **排除是檔案、****資料夾**、**檔案類型** 或 **進程**。

![Windows Defender 新增排除螢幕擷取畫面](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>建議的排除專案

下列清單顯示建議從 Windows Defender 即時掃描新增為排除的每個 Android Studio 目錄的預設位置：

- Gradle cache： `%USERPROFILE%\.gradle`
- Android Studio 專案： `%USERPROFILE%\AndroidStudioProjects`
- Android SDK： `%USERPROFILE%\AppData\Local\Android\SDK`
- Android Studio 系統檔案： `%USERPROFILE%\.AndroidStudio<version>\system`

如果您尚未使用 Android Studio 設定的預設位置，或您已從 GitHub 下載專案 (例如) ，則這些目錄位置可能不會套用至您的專案。 請考慮將排除專案新增至您目前的 Android 開發專案目錄中，任何位置都可以找到。

您可能想要考慮的其他排除專案包括：

- Visual Studio 開發環境進程： `devenv.exe`
- Visual Studio build process： `msbuild.exe`
- JetBrains 目錄： `%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

如需新增防毒軟體掃描排除專案的詳細資訊，包括如何自訂群組策略受控制環境的目錄位置，請參閱 Android Studio 檔的 [防毒軟體影響](https://developer.android.com/studio/intro/studio-config#antivirus-impact) 一節。

> [!Note]
> Daniel Knoodle 已使用建議的腳本設定 GitHub 存放庫，以新增 [Visual Studio 2017 的 Windows Defender 排除](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10)專案。

## <a name="additional-resources"></a>其他資源

- [開發適用于 Android 的雙螢幕應用程式，並取得 Surface 雙核裝置 SDK](/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](./defender-settings.md)

- [啟用虛擬化支援以改善模擬器效能](./emulator.md#enable-virtualization-support)