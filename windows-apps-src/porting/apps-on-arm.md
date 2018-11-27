---
title: ARM 上的 Windows 10
description: 這篇文章概述體驗和 app 如何在 ARM 上執行、有哪些限制，以及可以前往哪裡了解更多。
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 永遠連線, ARM, ARM64, x86 模擬
ms.localizationpriority: medium
ms.openlocfilehash: bdbd0e4f3ab2d060cdb0b2519117e4f14725347f
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7706290"
---
# <a name="windows-10-on-arm"></a>ARM 上的 Windows 10
原本 Windows 10 (與 Windows 10 行動裝置版有所區別) 只能在搭載 x86 與 x64 處理器的電腦上執行。 現在，Windows 10 Desktop（專業版和 S 版）可以在搭載 ARM64 處理器並已安裝 Fall Creators Update 的電腦上執行。 ARM CPU 架構的省電本質可讓這些電腦擁有全天候的電池使用時間及行動數據網路支援。 這些電腦會提供極佳的應用程式相容性，可讓您執行現有未修改狀態的 x86 win32 應用程式。 例如 Adobe Reader。 更多的資訊或示範，請查看[永遠連線電腦的 Channel 9 影片](https://channel9.msdn.com/Events/Build/2017/P4171)。 

我們在這裡使用*ARM*一詞代表在 ARM64 (也被稱為*AArch64*) 處理器上執行 Windows 10 桌上型電腦版本的電腦。  我們使用*ARM32*一詞代表 32 位元 ARM 架構 (在其他文件中通常稱為*ARM*)。

## <a name="apps-and-experiences-on-arm"></a>ARM 上的 App 和體驗

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>內建 Windows 10 體驗、app 和驅動程式
內建 Windows 10 體驗，例如 Edge、Cortana、[開始] 功能表和檔案總管都是原生而且執行為 ARM64（或 ARM32）。 這也會包含所有的裝置驅動程式，例如圖形、網路功能或硬碟。 這樣可確保裝置以 Qualcomm Snapdragon 處理器完整原生速度執行，獲得最佳的使用者體驗和電池使用時間

### <a name="universal-windows-platform-uwp-apps"></a>通用 Windows 平台 (UWP) 應用程式
ARM 上的 Windows 10 執行來自 Microsoft Store 的所有 x86 和 ARM32 [UWP 應用程式](../get-started/universal-application-platform-guide.md)。 ARM32 應用程式原生執行，而不需要任何模擬，而 x86 應用程式在模擬下執行。 如果您是 UWP 開發人員，請務必為您的應用程式提交 ARM 套件，這將會為裝置提供最佳的使用者體驗。 如需詳細資訊，請參閱[應用程式套件架構](../packaging/device-architecture.md)。

>[!IMPORTANT] 
> 當使用者從 Microsoft Store 下載 UWP app 時，ARM32 版本將會安裝到 ARM64 裝置，除非只提供 x86 版本。 如需架構的詳細資訊，請參閱[應用程式套件架構](../packaging/device-architecture.md)。

### <a name="win32-apps"></a>Win32 應用程式
除了 UWP 應用程式，ARM 上的 Windows 10 也可以執行未修改狀態的 x86 Win32 應用程式 (例如 Adobe Reader)，提供很好的效能與順暢的使用者體驗，就像任何電腦。 這些 x86 win32 應用程式不需要為 ARM 重新編譯，甚至不知道它們正在 ARM 處理器上執行。 請注意，不支援 64 位元 x64 Win32 應用程式，但大部分的所有應用程式都有 x86 應用程式版本，所以從使用者觀點，只要選擇 x86 32 位元安裝程式就能在 Windows-on-ARM 電腦上執行。

## <a name="in-this-section"></a>本節內容
|主題 | 描述 |
|-----|-----|
|[x86 模擬在 ARM 上的運作方式](apps-on-arm-x86-emulation.md)|概觀詳述在 ARM 上如何模擬 x86 應用程式。|
|[疑難排解 ARM 上的 x86 應用程式](apps-on-arm-troubleshooting-x86.md)|在 ARM 上執行 x86 應用程式的一般問題，以及修正這些問題。 |
|[疑難排解 ARM 上的 ARM32 應用程式](apps-on-arm-troubleshooting-arm32.md)|在 ARM 上執行 ARM32 應用程式的一般問題，以及修正這些問題。 |
|[ARM 上的程式相容性疑難排解員](apps-on-arm-program-compat-troubleshooter.md)|如果您的應用程式在 ARM 上無法正常運作時調整相容性設定的指引。 |

## <a name="related-topics"></a>相關主題
|主題 | 描述 |
|-----|-----|
|[使用 WDK 建置 ARM64 驅動程式](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|建置 ARM64 驅動程式的指示。 |
| [偵錯 ARM 上的 x86 應用程式](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | 用於偵錯 ARM 上的 x86 應用程式的指引。 |
