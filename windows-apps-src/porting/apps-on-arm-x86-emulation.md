---
title: x86 和 ARM32 模擬在 ARM 上的運作方式
description: 瞭解 x86 應用程式的模擬如何讓現有 Win32 應用程式的豐富生態系統可在 ARM 裝置上使用。
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 永遠連線, ARM 上的 x86 模擬
ms.localizationpriority: medium
ms.openlocfilehash: 61d994a4a022da671b4141ded80c6235ae38bae6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162322"
---
# <a name="how-x86-emulation-works-on-arm"></a>x86 模擬在 ARM 上的運作方式
x86 應用程式模擬讓 Win32 應用程式的豐富生態系統也適用於 ARM。 這提供使用者執行現有 x86 win32 應用程式的神奇體驗，而不需要應用程式的任何變更。 應用程式甚至不知道它正在 Windows-on-ARM 電腦上執行，除非呼叫特定 API ([IsWoW64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2))。

[WOW64](/windows/desktop/WinProg64/running-32-bit-applications)層級的 Windows 10 可讓 x86 程式碼在 ARM64 版本的 Windows 10 上執行。 x86 模擬的運作方式是將 x86 指令區塊編譯到 ARM64 指令，並啟用最佳化以改善效能。 服務會快取程式碼的轉譯區塊來減少指令翻譯的負荷，並於程式碼再次執行時允許最佳化。 為每個模組產生快取，因此其他應用程式可以在第一次啟動時使用它們。 

如需有關這些技術的詳細資訊，請觀看[ARM 上的 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171) Channel9 影片。