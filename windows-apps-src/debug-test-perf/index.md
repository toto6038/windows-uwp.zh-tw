---
ms.assetid: 16976d00-1564-49fe-81ad-2568e25e9e41
title: 偵錯、測試及效能
description: 使用 Microsoft Visual Studio 和其他工具來偵錯和測試您的應用程式，並準備進行 Microsoft Store 認證程序。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 757de9201d1cb7f753419024271f2be5c1aa67f4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582309"
---
# <a name="debugging-testing-and-performance"></a>偵錯、測試及效能


本節說明如何使用 Microsoft Visual Studio 來偵錯、測試及最佳化您的應用程式。 它也包含 Windows 裝置入口網站 (適用於裝置監視和設定) 和 Windows 應用程式認證套件 (用以針對 Microsoft Store 準備您的應用程式) 等工具。

| 主題 | 描述 |
|-------|-------------|
| [部署和偵錯 UWP 應用程式](deploying-and-debugging-uwp-apps.md) | 本文會引導您完成以各種部署和偵錯目標為目標的步驟。 |
| [處理程序生命週期管理 (PLM) 的測試與偵錯工具](testing-debugging-plm.md) | 用來偵錯和測試您 App 如何使用處理程序生命週期管理的工具及技術。 |
| [使用適用於 Windows 10 行動裝置版的 Microsoft 模擬器進行測試](test-with-the-emulator.md) | 使用「適用於 Windows 10 行動裝置版的 Microsoft 模擬器」隨附的工具來模擬與裝置的實際互動，以及測試應用程式的功能。 模擬器是模擬執行 Windows 10 之行動裝置的傳統型 app。 它提供虛擬化環境，讓您可以在其中針對 Windows app 進行偵錯與測試，而不需要擁有實體裝置。 它也提供隔離的環境供您的應用程式原型使用。 |
| [使用 Visual Studio 測試 Surface Hub應用程式](test-surface-hub-apps-using-visual-studio.md) | Visual Studio 模擬器提供了可讓您設計、開發、偵錯和測試通用 Windows 平台 (UWP) app (包括針對 Microsoft Surface Hub 建置的應用程式) 的環境。 此模擬器不會使用與 Surface Hub 相同的使用者介面，但很適合以 Surface Hub 的畫面大小與解析度測試您應用程式的外觀和行為。 |
| [透過鬆散檔案註冊部署應用程式](loose-file-registration.md) | 本指南說明如何使用鬆散檔案配置來驗證及共用 Windows 10 應用程式，而不需加以封裝。 |
| [搶鮮版 (Beta) 測試](beta-testing.md) | **搶鮮版 (Beta) 測試**可讓您有機會根據您 App 開發小組以外的個人所提供的意見反應，改進您的 App，這些個人會在他們自己的裝置上試用您尚未發行的 App。 |
| [Windows 裝置入口網站](device-portal.md) | Windows Device Portal 能讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。 |
| [Windows 應用程式認證套件](windows-app-certification-kit.md) | 為了讓您的應用程式能順利在 Microsoft Store 上發行或成為 Windows 認證，請在送出以進行認證之前，先在本機進行驗證和測試。 本主題示範如何安裝和執行 Windows 應用程式認證套件。 |
| [效能](performance-and-xaml-ui.md) | 使用者會期望其 app 保持回應性，並可自在地使用，而不會耗盡電池。 在技術上來說，效能是非功能的需求，但是將效能視為功能可協助您滿足使用者的期望。 指定目標和測量是主要因素。 決定您的效能關鍵案例是什麼；定義良好效能代表什麼意義。 然後在整個專案週期中及早並經常進行測量，以確保您能夠達成目標。 |
| [版本調適型應用程式](version-adaptive-apps.md) | 運用最新 API 及功能，而同時又盡可能深入最廣大的客戶群。 使用執行階段 API 檢查，以在執行階段配合應用程式執行所在 Windows 10 版本可用的功能，自動調整您的程式碼和 XAML。 |
