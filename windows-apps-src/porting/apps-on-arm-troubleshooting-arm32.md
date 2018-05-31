---
title: 針對 ARM32 UWP app 問題進行疑難排解
author: msatranjr
description: 在 ARM 上執行 ARM32 應用程式的一般問題，以及修正這些問題。
ms.author: misatran
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 s, 永遠連線, ARM 上的 ARM32 應用程式, ARM 上的 windows 10, 疑難排解
ms.localizationpriority: medium
ms.openlocfilehash: 71d92ec26311514e0eebdfa4a1dab39e86ce72fc
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595112"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>針對 ARM32 UWP app 問題進行疑難排解
如果您的 ARM32 UWP app 在 ARM 上無法正常運作，以下指引可能有幫助。 

## <a name="common-issues"></a>常見問題
以下提供一些您在疑難排解 ARM32 應用程式時應記住的常見問題。

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>在 ARM 處理器上使用僅限 Windows 10 行動裝置版 API 
ARM32 應用程式在使用僅限行動裝置版 API (例如，**HardwareButtons**) 時可能會發生問題。 若要減輕此問題，您可以在呼叫這些 API 之前動態偵測您的應用程式是否正在 Windows 10 行動裝置版上執行。 請依照部落格文章[利用 API 協定動態偵測功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)中的指引。

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>包括 UWP 應用程式不支援的相依性
未使用 Visual Studio 和 UWP SDK 正確建置的通用 Windows 平台(UWP) 應用程式，可能有 OS 元件的相依性 (在 ARM64 系統上執行的 ARM32 應用程式無法使用這些元件)。 這些相依性的範例包括：

- 預期 .NET Framework 組件可供使用。
- 參考與 UWP 不相容的第三方.NET 元件。

這些問題可以透過下列方式解決：移除無法使用的相依性以及重建 app，透過使用最新 Microsoft Visual Studio 與 UWP SDK 版本；或最後一種解決方式是從 Microsoft Store 移除 ARM32 應用程式，讓應用程式的 x86 版本（如果有的話）下載到使用者的電腦。 

如需適用於 UWP 應用程式的 .NET API 的詳細資訊，請查看[適用於 UWP 應用程式的 .NET](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>使用舊版 Visual Studio 和 SDK 編譯應用程式
如果發生問題，請務必使用 Microsoft Visual Studio 和 Windows SDK 的最新版本編譯應用程式。 使用舊版 Visual Studio 和 SDK 編譯的應用程式，可能會有在較新版本中已修正的問題。

## <a name="debugging"></a>偵錯
您可以使用現有的工具，開發適用於 ARM 平台的 ARM32 應用程式。 以下是一些實用的資源。

- Visual Studio 15.5 Preview 1 及更新版本支援使用通用驗證模式執行 ARM32 應用程式。 這會自動啟動必要遠端偵錯工具。
- 若要深入了解 ARM 上偵錯的工具和策略，請參閱[ARM64 上的偵錯](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)。