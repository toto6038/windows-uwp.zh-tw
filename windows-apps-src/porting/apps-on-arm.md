---
title: Windows 10 on ARM
description: 這篇文章概述體驗和 app 如何在 ARM 上執行、有哪些限制，以及可以前往哪裡了解更多。
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 永遠連線, ARM, ARM64, x86 模擬
ms.localizationpriority: medium
ms.openlocfilehash: 1a72fbaf4982f2a053298f10279eacba6a46d05d
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726001"
---
# <a name="windows-10-on-arm"></a>Windows 10 on ARM
原本 Windows 10 (與 Windows 10 行動裝置版有所區別) 只能在搭載 x86 與 x64 處理器的電腦上執行。 現在，Windows 10 desktop 可以在具有秋季建立者更新或更新版本的 ARM64 處理器支援的機器上執行。 ARM CPU 架構的省電本質可讓這些電腦擁有全天候的電池使用時間及行動數據網路支援。 這些電腦會提供極佳的應用程式相容性，可讓您執行現有未修改狀態的 x86 win32 應用程式。 如需詳細資訊或示範，請查看[適用于 Always CONNECTED 電腦的 Channel 9 影片](https://channel9.msdn.com/Events/Build/2017/P4171)。

我們在這裡使用*ARM*一詞代表在 ARM64 (也被稱為*AArch64*) 處理器上執行 Windows 10 桌上型電腦版本的電腦。  我們使用*ARM32*一詞代表 32 位元 ARM 架構 (在其他文件中通常稱為*ARM*)。

## <a name="apps-and-experiences-on-arm"></a>ARM 上的 App 和體驗

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>內建 Windows 10 體驗、app 和驅動程式
內建的 Windows 10 體驗（例如 Edge、Cortana、[開始] 功能表和 Explorer）都是原生的，並以 ARM64 的形式執行。 這也包括所有設備磁碟機，例如圖形、網路或硬碟。 這可確保您可以取得裝置的最佳使用者體驗和電池壽命，以 Qualcomm Snapdragon 處理器的完整原生速度執行。

### <a name="universal-windows-platform-uwp-apps"></a>通用 Windows 平台 (UWP) 應用程式
ARM 上的 Windows 10 會從 Microsoft Store 執行所有 x86、ARM32 和 ARM64 [UWP 應用程式](../get-started/universal-application-platform-guide.md)。 ARM32 和 ARM64 應用程式會以原生方式執行，而不需要任何模擬，而 x86 應用程式則是在模擬 如果您是 UWP 開發人員，請務必為您的應用程式提交 ARM 套件，這將會為裝置提供最佳的使用者體驗。 如需詳細資訊，請參閱[應用程式套件架構](/windows/msix/package/device-architecture)。

>[!NOTE]
> 若要建立您的 UWP 應用程式以原生方式將目標設為 ARM64 平臺，您必須具有 Visual Studio 2017 15.9 版或更新版本，或 Visual Studio 2019。 如需詳細資訊，請參閱[這篇 blog 文章](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)。


>[!IMPORTANT]
> ARM 上的 Windows 10 支援 x86、ARM32 和 ARM64 UWP 應用程式，其位於 ARM64 裝置上的存放區。 當使用者在 ARM64 裝置上下載您的 UWP 應用程式時，作業系統會自動安裝可供使用的最佳應用程式版本。 如果您將 x86、ARM32 和 ARM64 版本的應用程式提交至存放區，則 OS 會自動安裝應用程式的 ARM64 版本。 如果您只提交 x86 和 ARM32 版本的應用程式，作業系統將會安裝 ARM32 版本。 如果您只提交 x86 版的應用程式，作業系統將會安裝該版本，並在模擬下執行。 如需架構的詳細資訊，請參閱[應用程式套件架構](/windows/msix/package/device-architecture)。

### <a name="win32-apps"></a>Win32 應用程式
除了 UWP 應用程式之外，Windows 10 on ARM 也可以在未修改的情況下執行您的 x86 Win32 應用程式，並提供良好的效能和順暢的使用者體驗，就像任何電腦一樣。 這些 x86 Win32 應用程式不需要針對 ARM 重新編譯，甚至不知道它們是在 ARM 處理器上執行。 請注意，64位 x64 Win32 應用程式不受支援，但大部分的應用程式都有 x86 版本可供使用。  當您選擇應用程式架構時，只要選擇32位 x86 版本，即可在 Windows 10 ARM 電腦上執行應用程式。

## <a name="in-this-section"></a>本節內容
|主題 | 描述 |
|-----|-----|
|[x86 模擬在 ARM 上的運作方式](apps-on-arm-x86-emulation.md)|概觀詳述在 ARM 上如何模擬 x86 應用程式。|
|[對 ARM 上的 x86 應用程式進行疑難排解](apps-on-arm-troubleshooting-x86.md)|在 ARM 上執行 x86 應用程式的一般問題，以及修正這些問題。 |
|[針對 ARM 上的 ARM 應用程式進行疑難排解](apps-on-arm-troubleshooting-arm32.md)|在 ARM 上執行時，ARM32 和 ARM64 應用程式的常見問題，以及如何修正這些問題。 |
|[ARM 上的程式相容性疑難排解員](apps-on-arm-program-compat-troubleshooter.md)|如果您的應用程式在 ARM 上無法正常運作時調整相容性設定的指引。 |

## <a name="related-topics"></a>相關主題
|主題 | 描述 |
|-----|-----|
|[使用 WDK 建置 ARM64 驅動程式](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)|建置 ARM64 驅動程式的指示。 |
| [在 ARM 上調試 x86 應用程式](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) | 用於偵錯 ARM 上的 x86 應用程式的指引。 |
