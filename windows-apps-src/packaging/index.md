---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: 封裝應用程式
description: 本節包含或連結至關於封裝通用 Windows 平台 (UWP) 應用程式的文章。
ms.date: 07/22/2019
ms.topic: article
keywords: windows 10, uwp, 封裝
ms.localizationpriority: medium
ms.openlocfilehash: 35adf8db66bbfaa1be11c0b389efea88b5c2437b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164361"
---
# <a name="packaging-apps"></a>封裝應用程式

本節包含或連結至關於在 MSIX 和 .appx 應用程式套件中封裝通用 Windows 平台 (UWP) 應用程式的文章，以進行部署和安裝。 其中一些連結會移至 [MSIX 文件](/windows/msix/)中的相關文章。

> [!NOTE]
> Windows 10 中 UWP 應用程式的原始應用程式封裝格式為 .appx。 從 Windows 10 版本 1809 開始，此封裝格式已重新命名為 .msix 並已延伸為支援所有類型的Windows 應用程式，包含 .NET 和 C++/Win32 桌面應用程式。 MSIX 的支援也會延伸到舊版的 Windows。 如需詳細資訊，請參閱 [MSIX 文件](/windows/msix/)。

| 主題 | 描述 |
|-------|-------------|
| [使用 Visual Studio 封裝 UWP 應用程式](/windows/msix/package/packaging-uwp-apps) | 若要發佈或銷售您的通用 Windows 平台 (UWP) 應用程式，您必須為其建立應用程式套件。 |
| [手動應用程式封裝](/windows/msix/package/manual-packaging-root) | 如果想要建立並簽署應用程式套件，但是沒有使用 Visual Studio 來開發應用程式時，您必須使用手動應用程式封裝工具。 |
| [應用程式套件架構](/windows/msix/package/device-architecture) | 深入了解在建置應用程式套件時，應使用哪種處理器架構。 |
| [UWP 應用程式串流安裝](/windows/msix/package/streaming-install) | 您可以透過應用程式串流安裝，指定 Microsoft Store 優先下載的應用程式部分。 當應用程式的必要檔案獲得優先下載時，使用者可以直接啟動並與應用程式進行互動，同時讓剩餘的檔案在背景完成下載。 |
| [選用套件及相關集合的製作](/windows/msix/package/optional-packages) | 選用套件包含了可與主要套件整合的內容。 這些選用套件相當適合用於可下載內容 (DLC)、將大型應用程式根據大小限制進行分割，或是傳送與您原始應用程式分離的額外內容。 |
| [具可執行程式碼的選用套件](/windows/msix/package/optional-packages-with-executable-code) | 了解如何使用 Visual Studio 建立具可執行程式碼的選用套件。 |
| [使用應用程式安裝程式安裝 Windows 10 應用程式](/windows/msix/app-installer/app-installer-root) | 應用程式安裝程式可讓您按兩下應用程式套件，安裝 Windows 10 應用程式。 |
| [使用 WinAppDeployCmd.exe 工具安裝應用程式](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 應用程式部署 (WinAppDeployCmd.exe) 是可以用來從 Windows 10 電腦將 UWP 應用程式部署到任何 Windows 10 行動裝置版裝置的命令列工具。 如果 Windows 10 行動裝置版裝置是透過 USB 連接，或可在相同的子網路上使用而不需要 Microsoft Visual Studio 或該應用程式適用的解決方案時，您就可以使用此工具來部署 .appx 套件。 本文章說明如何使用此工具安裝 UWP App。 |
| [設定您的 UWP 應用程式的自動化組建](auto-build-package-uwp-apps.md) | 如果您要在自動化建置程序中封裝您的 App，本主題示範如何使用 Visual Studio Team Services (VSTS) 來完成。 |
| [應用程式功能宣告](app-capability-declarations.md) | 必須在您的應用程式[套件資訊清單](/uwp/schemas/appxpackage/appx-package-manifest)中宣告功能，才能存取特定的 API 或資源 (如圖片、音樂)，或是相機或麥克風等裝置。 |
| [從 Microsoft Store 下載與安裝套件更新](self-install-package-updates.md) | 您的 UWP app 可以程式設計方式檢查套件更新並安裝更新。 您的應用程式也可以在合作夥伴中心上查詢已標示為強制性的套件，並在安裝強制更新之前停用功能。  |