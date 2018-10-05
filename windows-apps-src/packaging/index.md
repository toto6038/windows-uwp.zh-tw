---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: 封裝 app
description: 本節包含或連結至關於封裝通用 Windows 平台 (UWP) 應用程式的文章。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 封裝
ms.localizationpriority: medium
ms.openlocfilehash: ce77391fc189ef33aba3002685b0662d7cab1953
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390016"
---
# <a name="packaging-apps"></a>封裝應用程式


## <a name="purpose"></a>用途

本節包含或連結至關於封裝通用 Windows 平台 (UWP) app 的文章。

| 主題 | 描述 |
|-------|-------------|
| [使用 Visual studio 封裝 UWP app](packaging-uwp-apps.md) | 若要發佈或銷售您的通用 Windows 平台 (UWP) 應用程式，您必須為其建立應用程式套件。 |
| [手動封裝應用程式](manual-packaging-root.md) | 如果想要建立並簽署應用程式套件，但是沒有使用 Visual Studio 來開發 App 時，您必須使用手動 App 封裝工具。 |
| [應用程式套件架構](device-architecture.md) | 深入了解在建置 UWP app 套件時您應使用哪種處理器架構。 |
| [UWP app 串流安裝](streaming-install.md) | 通用 Windows 平台 (UWP) 應用程式串流安裝，讓您可以指定您應用程式中 Microsoft Store 需要優先下載的部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。 |
| [選用套件及相關集合的製作](optional-packages.md) | 選用套件包含了可與主要套件整合的內容。 這些選用套件相當適合用於可下載內容 (DLC)、將大型應用程式根據尺寸限制進行分割，或是傳送與您原始應用程式分離的額外內容。 |
| [具可執行程式碼的選用套件](optional-packages-with-executable-code.md) | 了解如何使用 Visual Studio 建立具可執行程式碼的選用套件。 |
| [使用應用程式安裝程式安裝 UWP app](appinstaller-root.md) | 應用程式安裝程式可讓您以按兩下應用程式套件的方式來安裝 UWP app。 |
| [使用 WinAppDeployCmd.exe 工具安裝 App](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 應用程式部署 (WinAppDeployCmd.exe) 是可以用來從 Windows10 電腦將 UWP app 部署到任何 Windows10 行動裝置版裝置的命令列工具。 您可以使用此工具來部署應用程式套件，Windows 10 行動裝置版裝置是透過 USB 連接或可在相同的子網路上使用而不需要該 app 適用的 Microsoft Visual Studio 或解決方案時。 本文章說明如何使用此工具安裝 UWP app。 |
| [為您的 UWP app 設定自動化組建](auto-build-package-uwp-apps.md) | 如果您要在自動化建置程序中封裝您的 App，本主題示範如何使用 Visual Studio Team Services (VSTS) 來完成。 |
| [App 功能宣告](app-capability-declarations.md) | 功能必須在您的 UWP app 的[套件資訊清單](https://msdn.microsoft.com/library/windows/apps/BR211474)中進行宣告，才能存取特定的 API 或資源 (如圖片、音樂)，或是相機或麥克風等裝置。 |
| [從 Microsoft Store 下載與安裝套件更新](self-install-package-updates.md) | 您的 UWP app 可以程式設計方式檢查套件更新並安裝更新。 您的 app 也可以在 Windows 開發人員中心儀表板上查詢已標示為強制性的套件，並在安裝強制更新之前停用功能。  |
