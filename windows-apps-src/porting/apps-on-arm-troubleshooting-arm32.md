---
title: 針對 ARM32 UWP app 問題進行疑難排解
description: 在 ARM 上執行 ARM32 應用程式的一般問題，以及修正這些問題。
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, 永遠連線, ARM 上的 ARM32 應用程式, ARM 上的 windows 10, 疑難排解
ms.localizationpriority: medium
ms.openlocfilehash: 60c7a129844d69d18287ea7885a0acaf01f1f625
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171222"
---
# <a name="troubleshooting-arm-uwp-apps"></a>針對 ARM UWP 應用程式進行疑難排解

如果您的 ARM32 或 ARM64 UWP 應用程式在 ARM 上無法正常運作，以下是一些可能有所説明的指引。

>[!NOTE]
> 若要建立您的 UWP 應用程式，以原生的 ARM64 平臺為目標，您必須有 Visual Studio 2017 15.9 版或更新版本，或 Visual Studio 2019。 如需詳細資訊，請參閱[這篇部落格文章](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)。


## <a name="common-issues"></a>常見問題
以下是針對 ARM32 和 ARM64 應用程式進行疑難排解時，應謹記在心的一些常見問題。

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>在 ARM 處理器上使用僅限 Windows 10 行動裝置版 API
使用僅限行動裝置的 Api 時，ARM 應用程式可能會遇到問題 (例如， **HardwareButtons**) 。 若要減輕此問題，您可以在呼叫這些 API 之前動態偵測您的應用程式是否正在 Windows 10 行動裝置版上執行。 請依照部落格文章[利用 API 協定動態偵測功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)中的指引。

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>包括 UWP 應用程式不支援的相依性
通用 Windows 平臺 (UWP) 未以 Visual Studio 正確建立的應用程式，而 UWP SDK 可能相依于 ARM64 系統上執行的 ARM 應用程式無法使用的作業系統元件。 這些相依性的範例包括：

- 預期 .NET Framework 組件可供使用。
- 參考與 UWP 不相容的第三方.NET 元件。

這些問題可透過下列方式解決：移除無法使用的相依性，並使用最新的 Microsoft Visual Studio 和 UWP SDK 版本重建應用程式;或者，作為最後的手段，從 Microsoft Store 移除 ARM 應用程式，以便將應用程式的 x86 版本 (（如果有) 的話）下載到使用者的電腦。

如需適用於 UWP 應用程式的 .NET API 的詳細資訊，請查看[適用於 UWP 應用程式的 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>使用舊版 Visual Studio 和 SDK 編譯應用程式
如果發生問題，請務必使用 Microsoft Visual Studio 和 Windows SDK 的最新版本編譯應用程式。 使用舊版 Visual Studio 和 SDK 編譯的應用程式，可能會有在較新版本中已修正的問題。

## <a name="debugging"></a>偵錯
您可以使用現有的工具來開發 ARM 平臺的應用程式。 以下是一些實用的資源。

- Visual Studio 15.5 Preview 1 及更新版本支援使用通用驗證模式執行 ARM32 應用程式。 這會自動啟動必要遠端偵錯工具。
- 若要深入了解 ARM 上偵錯的工具和策略，請參閱[ARM64 上的偵錯](/windows-hardware/drivers/debugger/debugging-arm64)。