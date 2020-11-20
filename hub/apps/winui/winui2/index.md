---
title: Windows UI 程式庫
description: 提供有關 WinUI 2.x 和 Windows 應用程式開發的資訊。
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, 工具組 sdk, winui, Windows UI 程式庫
ms.custom: RS5
ms.openlocfilehash: c7baa8fe74a45d1f7ba7f829f6d9f1228c70d44f
ms.sourcegitcommit: 75e1f49be211e8b4b3e825978d67625776f992f5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94691556"
---
# <a name="windows-ui-library-2x"></a>Windows UI 程式庫 2.x

![WinUI 控制項](images/winUI-library-767.png)

Windows UI 程式庫可為 Windows 應用程式提供官方原生 Windows UI 控制項和其他使用者介面元素。

其保持舊版 Windows 10 的向下相容性，因此即便使用者沒有最新的作業系統，您的應用程式仍可運作。

> [!NOTE]
> 請查看 [Windows UI 程式庫 3 預覽版 3 (2020 年 11 月)](../winui3/index.md)，這是 Windows 10 UI 平台的重大更新。

## <a name="features"></a>功能

* **新控制項**：Windows UI 程式庫包含未隨附於預設 Windows 平台的新控制項。

* **現有控制項的更新版本**：此程式庫也包含現有 Windows 平台控制項的更新版本，可搭配舊版 Windows 10 使用。

* **舊版 Windows 10 的支援**：Windows UI 程式庫 API 可在較舊版本的 Windows 10 上運作，因此您並不需要加入版本檢查或條件式 XAML 來支援沒有執行最新作業系統的使用者。

* **XamlDirect 支援**：Xaml Direct API 是針對中介軟體開發人員所設計，可讓您存取較低層級的 Xaml 功能，以提供更佳的 CPU 和工作集效能。 XamlDirect 可讓您在舊版 Windows 10 上使用 XamlDirect API，而不需要撰寫特殊程式碼來處理多個目標 Windows 10 版本。

## <a name="examples"></a>範例

Xaml 控制項庫範例應用程式包含 WinUI 控制項用法的互動式示範和範例程式碼。

* 從 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安裝 XAML 控制項庫應用程式

* Xaml 控制項庫也是 [GitHub 上的開放原始碼項目](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文件

Windows UI 程式庫控制項的操作說明文章隨附於[通用 Windows 平台控制項文件](/windows/uwp/design/controls-and-patterns/)。

API 參照文件位於此處：[Windows UI 程式庫 API](/uwp/api/overview/winui/)。

## <a name="install-and-use-the-windows-ui-library"></a>安裝及使用 Windows UI 程式庫

如需指示，請參閱[開始使用 Windows UI 程式庫](getting-started.md)。

## <a name="open-source-and-developer-roadmap"></a>開放原始碼與開發人員藍圖

WinUI 是裝載於 GitHub 上的開放原始碼專案。 我們歡迎您在 [Windows UI 程式庫存放庫](https://aka.ms/winui)中提出錯誤回報、功能要求和社群程式碼。

我們會繼續開發及發展 WinUI，以支援更多開發人員案例。 如需 WinUI 計畫的最新詳細資料，請參閱 Windows UI 程式庫存放庫中的[藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

## <a name="nuget-package-list"></a>NuGet 套件清單

Windows UI 程式庫包含多個 NuGet 套件：[Windows UI 程式庫 NuGet 套件清單](nuget-packages.md)。

## <a name="see-also"></a>另請參閱

[Windows UI 程式庫 2.x 版本資訊](release-notes/index.md)
