---
title: ARM 上應用程式和體驗的限制
description: 適用於 ARM 裝置上不正確運作的應用程式的疑難排解步驟。
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 永遠連線, 限制, ARM 上的 windows 10
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: e9bbef6f9b714b99148cf4ac082f98b4422f23b2
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683961"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>ARM 上應用程式和體驗的限制
ARM 上的 Windows 10 有下列必要限制：

- **僅支援 ARM64 驅動程式**。 如同所有架構，核心模式驅動程式、[使用者模式驅動程式架構 (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf)驅動程式和印表機驅動程式必須編譯以符合作業系統的架構。 雖然 ARM 作業系統有模擬 x86 使用者模式 app 的功能，但是目前不會模擬為其他架構（例如 x64 或 x86）實作的驅動程式，因此在此平台上不受支援。 有專屬自訂驅動程式的任何應用程式都需要移植到 ARM64。 在有限案例中，應用程式可在模擬下執行為 x86，但是應用程式的驅動程式部分必須移植到 ARM64。 針對 ARM64 編譯驅動程式的詳細資訊，請查看[使用 WDK 建置 ARM64 驅動程式](/windows-hardware/drivers/develop/building-arm64-drivers)。

- **不支援 x64 應用程式**。 ARM 上的 Windows 10 不支援模擬 x64 應用程式。

- **某些遊戲無法運作**。 使用比 OpenGL 1.1 更新的版本或需要硬體加速 OpenGL 的遊戲及應用程式無法運作。 此外，此平台上不支援依賴「反作弊」驅動程式的遊戲。

- **自訂 Windows 體驗的應用程式可能無法正常運作**。 原生 OS 元件無法載入非原生元件。 通常會執行此動作的應用程式範例，包含一些輸入法 (IME)、輔助技術，以及雲端儲存應用程式。 IME 和輔助技術通常會針對大部分的 app 功能連結至輸入堆疊。 雲端儲存應用程式通常會使用殼層延伸（例如檔案總管中的圖示和按右鍵功能表上的新增項目）；它們的殼層延伸可能會失敗，而且如果未適當處理失敗，app 本身可能無法運作。

- **假設所有 ARM 裝置都是執行 Windows 行動裝置版的應用程式可能無法正常運作**。 做此假設的應用程式可能會出現錯誤的方向、顯示未預期的 UI 配置或呈現，或在未先測試合約可用性的情況下，嘗試叫用僅限行動裝置版 API 時完全無法啟動。

- **ARM 不支援 Windows Hypervisor 平台上**。 在 ARM 裝置上使用 Hyper-V 執行任何虛擬電腦將會無法運作。

下表列出一些常見問題，並提供如何修正問題的相關建議。

|問題|解決方案|
|-----|--------|
| 您的應用程式依賴不是針對 ARM 所設計的驅動程式。 | 將 x86 驅動程式編譯為 ARM64。 查看[使用 WDK 建置 ARM64 驅動程式](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)。 |
| 您的應用程式僅適用於 x64。 | 如果您針對 Microsoft Store 開發，請提交應用程式的 ARM 版本。 如需詳細資訊，請參閱[應用程式套件架構](/windows/msix/package/device-architecture)。 如果您是 Win32 開發人員，請散發應用程式的 x86 版本。 |
| 您的應用程式使用比 OpenGL 1.1 更新的版本或需要硬體加速 OpenGL。 | 使用 DirectX 9、DirectX 10、DirectX 11 和 DirectX 12 的 x86 應用程式可在 ARM 上正常運作。 如需詳細資訊，請參閱[DirectX 圖形與遊戲](https://docs.microsoft.com/windows/desktop/directx)。 |
| 您的 x86 應用程式無法如預期運作。 | 請依照[ARM 上的程式相容性疑難排解員](apps-on-arm-program-compat-troubleshooter.md)中的指引，嘗試使用相容性疑難排解員。 如需一些其他疑難排解步驟，請查看[疑難排解 ARM 上的 x86 應用程式](apps-on-arm-troubleshooting-x86.md)文章。 |
| 您的 x86 應用程式不會偵測正在 ARM 上執行。 | 使用[IsWow64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)以判斷您的應用程式是否正在 ARM 上執行。 |
| 您的 UWP ARM32 應用程式無法如預期運作。 | 查看[疑難排解 ARM 上的 ARM32 應用程式](apps-on-arm-troubleshooting-arm32.md)以了解如何讓應用程式在 ARM 上正常運作。 |
