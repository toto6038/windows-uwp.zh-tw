---
title: 疑難排解 x86 傳統型應用程式
description: 在 ARM 上執行 x86 應用程式的一般問題，以及修正這些問題。
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 s, 永遠連線, ARM 上的 x86 模擬, 疑難排解
ms.localizationpriority: medium
ms.openlocfilehash: 55737790db00adb3104d12ae44df0ea07679d699
ms.sourcegitcommit: 14c37e23b8966468e8c28a3b34d21aaa4e6b571b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
ms.locfileid: "1614204"
---
# <a name="troubleshooting-x86-desktop-apps"></a>疑難排解 x86 傳統型應用程式
如果 x86 傳統型應用程式無法像在 x86 電腦上一樣運作，以下是一些可協助您疑難排解的指引。

|問題|解決方案|
|-----|--------|
| 您的應用程式依賴不是針對 ARM 所設計的驅動程式。 | 將 x86 驅動程式編譯為 ARM64。 查看[使用 WDK 建置 ARM64 驅動程式](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)。 |
| 您的應用程式僅適用於 x64。 | 如果您針對 Microsoft Store 開發，請提交應用程式的 ARM 版本。 如需詳細資訊，請參閱[應用程式套件架構](../packaging/device-architecture.md)。 如果您是 Win32 開發人員，請散發應用程式的 x86 版本。 |
| 您的應用程式使用比 OpenGL 1.1 更新的版本或需要硬體加速 OpenGL。 | 如果有的話，請使用應用程式的 DirectX 模式。 使用 DirectX 9、DirectX 10、DirectX 11 和 DirectX 12 的 x86 應用程式可在 ARM 上正常運作。 如需詳細資訊，請參閱[DirectX 圖形與遊戲](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx)。 |
| 您的 x86 應用程式無法如預期運作。 | 請依照[ARM 上的程式相容性疑難排解員](apps-on-arm-program-compat-troubleshooter.md)中的指引，嘗試使用相容性疑難排解員。 如需一些其他疑難排解步驟，請查看[疑難排解 ARM 上的 x86 應用程式](apps-on-arm-troubleshooting-x86.md)文章。 |

## <a name="best-practices-for-wow"></a>WOW 最佳做法
一個常見的問題發生於 app 發現它在 WOW 中執行，然後假設它是在 x64 系統上。 做此假設，應用程式可能會執行下列動作：

- 嘗試安裝應用程式本身的 x64 版本，這在 ARM 上不受支援。
- 在原生登錄檢視中檢查有無其他的軟體。
- 假設 64 位元的 .NET framework 可用。

一般而言，當已決定應用程式在 WOW 中執行時，應用程式不應該對主機系統做相關假設。 盡可能避免與作業系統的原生元件互動。

應用程式可能會在原生登錄檢視中放置登錄機碼，或根據有無 WOW 執行功能。 原始**IsWow64Process**只表示應用程式是否正在 x64 電腦上執行。 App 現在應該使用[IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx)判斷它們是否正在支援 WOW 的系統上執行。 

## <a name="drivers"></a>驅動程式 
所有核心模式驅動程式、[使用者模式驅動程式架構 (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf)驅動程式和印表機驅動程式必須編譯以符合作業系統的架構。 如果 x86 應用程式有驅動程式，則必須針對 ARM64 重新編譯該驅動程式。 x86 應用程式在模擬下可能會正常執行，不過它的驅動程式會需要針對 ARM64 重新編譯，並將不提供任何依賴驅動程式的應用程式體驗。 針對 ARM64 編譯驅動程式的詳細資訊，請查看[使用 WDK 建置 ARM64 驅動程式](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)。

## <a name="shell-extensions"></a>殼層延伸 
嘗試將 Windows 元件連結至 Windows 處理程序或載入其 DLL 處理程序的應用程式，會需要重新編譯這些 DLL 以符合系統的架構，即 ARM64。 一般而言，輸入法 (IME)、輔助技術，以及殼層延伸應用程式會使用這些（例如，在 [檔案總管] 中顯示雲端儲存圖示或按右鍵操作功能表）。 

ARM64 Win32 SDK 支援即將推出。 另請參閱[使用 WDK 建置 ARM64 驅動程式](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)。

## <a name="debugging"></a>偵錯
若要深入調查應用程式的行為，請查看[ARM 上的偵錯](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)以深入了解 ARM 上偵錯的工具和策略。

## <a name="virtual-machines"></a>虛擬機器
Qualcomm Snapdragon 835 行動裝置版電腦平台上不支援 Windows Hypervisor 平台。 因此使用 Hyper-V 執行虛擬電腦將會無法運作。 我們會持續投資在未來 Qualcomm 晶片組的這些技術。 