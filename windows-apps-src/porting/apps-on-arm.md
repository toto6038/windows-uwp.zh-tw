---
title: Windows 10 on ARM
description: 這篇文章概述體驗和 app 如何在 ARM 上執行、有哪些限制，以及可以前往哪裡了解更多。
ms.date: 05/22/2020
ms.topic: article
keywords: windows 10 s, 永遠連線, ARM, ARM64, x86 模擬
ms.localizationpriority: medium
ms.openlocfilehash: 39ff5b2aa6c72feaeaea0a7a61100196c109257c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162292"
---
# <a name="windows-10-on-arm"></a>Windows 10 on ARM
原本 Windows 10 (與 Windows 10 行動裝置版有所區別) 只能在搭載 x86 與 x64 處理器的電腦上執行。 現在，Windows 10 desktop 可以在具有秋季建立者更新或更新版本的 ARM64 處理器所支援的機器上執行。 ARM CPU 架構的省電本質可讓這些電腦擁有全天候的電池使用時間及行動數據網路支援。 這些電腦會提供極佳的應用程式相容性，可讓您執行現有未修改狀態的 x86 win32 應用程式。 如需詳細資訊或示範，請參閱 [Channel 9 影片以取得永遠連接的電腦](https://channel9.msdn.com/Events/Build/2017/P4171)。

我們在這裡使用*ARM*一詞代表在 ARM64 (也被稱為*AArch64*) 處理器上執行 Windows 10 桌上型電腦版本的電腦。  我們使用*ARM32*一詞代表 32 位元 ARM 架構 (在其他文件中通常稱為*ARM*)。

## <a name="apps-and-experiences-on-arm"></a>ARM 上的 App 和體驗

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>內建 Windows 10 體驗、app 和驅動程式
內建的 Windows 10 體驗（例如 Edge、Cortana、[開始] 功能表和 Explorer）全都是原生的，而且會以 ARM64 的形式執行。 這也包括所有設備磁碟機，例如圖形、網路或硬碟。 這可確保您能以 Qualcomm Snapdragon 處理器的完整原生速度，取得裝置的最佳使用者體驗和電池壽命。

### <a name="universal-windows-platform-uwp-apps"></a>通用 Windows 平台 (UWP) 應用程式
ARM 上的 Windows 10 會從 Microsoft Store 執行所有 x86、ARM32 和 ARM64 [UWP 應用程式](../get-started/universal-application-platform-guide.md) 。 ARM32 和 ARM64 應用程式以原生方式執行，不需要任何模擬，而 x86 應用程式則是在模擬下執行 如果您是 UWP 開發人員，請務必為您的應用程式提交 ARM 套件，這將會為裝置提供最佳的使用者體驗。 如需詳細資訊，請參閱[應用程式套件架構](/windows/msix/package/device-architecture)。

>[!NOTE]
> 若要建立您的 UWP 應用程式，以原生的 ARM64 平臺為目標，您必須有 Visual Studio 2017 15.9 版或更新版本，或 Visual Studio 2019。 如需詳細資訊，請參閱[這篇部落格文章](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)。


>[!IMPORTANT]
> ARM 上的 Windows 10 支援從 ARM64 裝置上的存放區進行 x86、ARM32 和 ARM64 UWP 應用程式。 當使用者在 ARM64 裝置上下載 UWP 應用程式時，作業系統會自動安裝可用應用程式的最佳版本。 如果您將應用程式的 x86、ARM32 和 ARM64 版本提交至存放區，作業系統會自動安裝應用程式的 ARM64 版本。 如果您只提交應用程式的 x86 和 ARM32 版本，作業系統將會安裝 ARM32 版本。 如果您只提交應用程式的 x86 版本，作業系統會安裝該版本，並在模擬下執行它。 如需架構的詳細資訊，請參閱[應用程式套件架構](/windows/msix/package/device-architecture)。

### <a name="win32-apps"></a>Win32 應用程式
除了 UWP 應用程式以外，ARM 上的 Windows 10 也可以在未經修改的情況下執行 x86 Win32 應用程式，並提供良好的效能和順暢的使用者體驗，就像任何電腦一樣。 這些 x86 Win32 應用程式不需要針對 ARM 重新編譯，也不會發現它們正在 ARM 處理器上執行。 請注意，不支援64位 x64 Win32 應用程式，但大部分的應用程式都有 x86 版本可供使用。  當提供應用程式架構的選擇時，請選擇32位 x86 版本，以在 ARM 電腦上的 Windows 10 上執行應用程式。

## <a name="downloads"></a>下載

Visual Studio 2019 針對 ARM 上的 Windows 10 提供數個工具下載。 使用 Visual Studio 2017 還是的使用者可以使用安裝程式來尋找並安裝可比較的工具和套件。 請注意，若要遵循這些步驟，您必須使用 Visual Studio 2019。

### <a name="visual-c-redistributable"></a>Visual C++ 可轉散發套件

Visual C++ 可轉散發套件可供 ARM 應用程式使用。 請流覽 [Visual Studio 下載] 頁面](https://visualstudio.microsoft.com/downloads/) 向下移至 **所有下載**、開啟 **其他工具和**架構，然後流覽至 Microsoft Visual C++ 可轉散發套件以 **進行 Visual Studio 2019** 專案。 選取 [ **ARM64** ] 選項按鈕，然後 **下載**。

### <a name="remote-tools"></a>遠端工具

Visual Studio 遠端工具適用于 ARM 應用程式。 請流覽 [Visual Studio 下載] 頁面](https://visualstudio.microsoft.com/downloads/) 向下滾動至 **所有下載**，開啟 **Visual Studio 2019 的工具**，然後流覽至 **Visual Studio 遠端工具 2019** 專案。 選取 [*ARM64* ] 選項按鈕，然後 **下載**。


## <a name="in-this-section"></a>本節內容
|主題 | 說明 |
|-----|-----|
|[x86 模擬在 ARM 上的運作方式](apps-on-arm-x86-emulation.md)|概觀詳述在 ARM 上如何模擬 x86 應用程式。|
|[對 ARM 上的 x86 應用程式進行疑難排解](apps-on-arm-troubleshooting-x86.md)|在 ARM 上執行 x86 應用程式的一般問題，以及修正這些問題。 |
|[針對 ARM 上的 ARM 應用程式進行疑難排解](apps-on-arm-troubleshooting-arm32.md)|在 ARM 上執行時，ARM32 和 ARM64 應用程式的常見問題，以及如何修正這些問題。 |
|[ARM 上的程式相容性疑難排解員](apps-on-arm-program-compat-troubleshooter.md)|如果您的應用程式在 ARM 上無法正常運作時調整相容性設定的指引。 |

## <a name="related-topics"></a>相關主題
|主題 | 說明 |
|-----|-----|
|[使用 WDK 建置 ARM64 驅動程式](/windows-hardware/drivers/develop/building-arm64-drivers)|建置 ARM64 驅動程式的指示。 |
| [偵錯 ARM 上的 x86 應用程式](/windows-hardware/drivers/debugger/debugging-arm64) | 用於偵錯 ARM 上的 x86 應用程式的指引。 |