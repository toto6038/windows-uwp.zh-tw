---
author: msatranjr
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: "封裝 app"
description: "本節包含或連結至關於封裝通用 Windows 平台 (UWP) app 的文章。"
translationtype: Human Translation
ms.sourcegitcommit: f7eec4d8df2b62143c198fe507dffd936e2e325c
ms.openlocfilehash: 230c77cbe60631d643bebe905a92cdbeb76af4d2

---
# 封裝 app

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## 用途

本節包含或連結至關於封裝通用 Windows 平台 (UWP) app 的文章。

| 主題 | 說明 |
|-------|-------------|
| [封裝 UWP app](packaging-uwp-apps.md) | 若要銷售您的 UWP app 或將其發佈給其他使用者，您必須建立其 appxupload 套件。 當您建立 appxupload 時，將產生另一個 appx 套件以用於測試和側載。 您可以透過將 appx 套件側載到裝置以直接發佈您的應用程式。 本文描述設定、建立和測試 UWP 應用程式套件的程序。 如需側載的詳細資訊，請參閱[使用 DISM 側載 app](http://go.microsoft.com/fwlink/?LinkID=231020)。 |
| [使用 WinAppDeployCmd.exe 工具安裝 app](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 應用程式部署 (WinAppDeployCmd.exe) 是可以用來從 Windows 10 電腦將 UWP app 部署到任何 Windows 10 行動裝置版裝置的命令列工具。 當 Windows 10 行動裝置版裝置是透過 USB 連接或可在相同的子網路上使用而不需要 Microsoft Visual Studio 或該 app 適用的方案時，您就可以使用此工具來部署 .appx 套件。 本文章說明如何使用此工具安裝 UWP app。 |
| [app 功能宣告](app-capability-declarations.md) | 功能必須在您的 UWP app 的[套件資訊清單](https://msdn.microsoft.com/library/windows/apps/BR211474)中進行宣告，才能存取特定的 API 或資源 (如圖片、音樂)，或是相機或麥克風等裝置。 |
| [下載與安裝 App 的套件更新](self-install-package-updates.md) | 您的 UWP app 可以程式設計方式檢查套件更新並安裝更新。 您的 app 也可以在 Windows 開發人員中心儀表板上查詢已標示為強制性的套件，並在安裝強制更新之前停用功能。 本文說明如何執行這些工作。 |
 



<!--HONumber=Aug16_HO5-->


