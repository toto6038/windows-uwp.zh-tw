---
title: ARM 上的程式相容性疑難排解員
author: msatranjr
description: 如果您的應用程式在 ARM 上無法正常運作時調整相容性設定的指引
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 永遠連線, 相容性疑難排解員, ARM 上的 windows
ms.localizationpriority: medium
ms.openlocfilehash: 4765ad324e90167c7279c9245bccd840bce1163d
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5875582"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>ARM 上的程式相容性疑難排解員
模擬支援 x86 應用程式，是為 ARM64 上的 Windows 10 建立的新功能。 有時候模擬執行的最佳化不會造成最佳體驗。 您可以使用程式相容性疑難排解員，為您的 x86 應用程式切換模擬設定，減少預設最佳化並可能增加相容性。

## <a name="start-the-program-compatibility-troubleshooter"></a>啟動程式相容性疑難排解員
在任何 Windows 10 電腦上以相同的方式手動啟動[程式相容性疑難排解員](https://support.microsoft.com/en-us/help/15078/windows-make-older-programs-compatible)：在可執行檔案 (.exe) 上按一下滑鼠右鍵，然後選取 **\[疑難排解相容性\]**。 這個畫面隨即顯示。

![疑難排解相容性選項的螢幕擷取畫面](images/arm/Capture4.png)

如果您按一下 **\[疑難排解程式\]**，以下列選項將會出現。

![疑難排解相容性選項的螢幕擷取畫面](images/arm/Capture5.png)

所有選項都會啟用所有 Windows 10 桌上型電腦上套用的適用設定。 此外，第一個、第二個和第四個選項會套用[停用應用程式快取](#disable-app-cache)和[停用混合執行模式](#disable-hybrid-exec-mode)模擬設定。

## <a name="toggling-emulation-settings"></a>切換模擬設定
> [!WARNING]
> 變更模擬設定可能會造成應用程式未預期當機或根本無法啟動。

您可以在可執行檔上按一下滑鼠右鍵，然後選取**\[內容\]**，切換模擬設定。

在 ARM 上，名為**\[ARM 上的 Windows 10\]** 的區段將會出現在**\[相容性\]** 索引標籤。按一下**\[變更模擬設定\]** 以啟動第二個視窗，如此處所示。

![變更模擬設定螢幕擷取畫面](images/arm/Capture.png)

這個視窗提供兩種修改模擬設定的方式。 您可以選取預先定義的模擬設定群組，也可以按一下**\[使用進階設定\]** 選項，讓您選擇個別設定。

群組的模擬設定會為了品質而降低效能最佳化。 以下是一些您可以選取的群組設定。

![變更模擬設定螢幕擷取畫面2](images/arm/Capture2.png)

選取 **\[使用進階設定\]** 來選擇個別設定，如本表格中所述。

| 模擬設定 | 結果 |
| ----------------- | ----------- |
| <p id="disable-app-cache">停用應用程式快取</p> | 作業系統會快取已編譯的程式碼區塊，以減少後續執行的模擬負荷。 此設定需要模擬器在執行階段中重新編譯所有應用程式程式碼。 |
| <p id="disable-hybrid-exec-mode">停用混合執行模式</p> | 已編譯的混合可攜式執行檔 (CHPE) 二進位檔是 x86 相容二進位檔，包括原生 ARM64 程式碼以提升效能，但可能與某些應用程式不相容。 此設定會強制使用僅限 x86 二進位檔案。 |
| 嚴格自我修改程式碼支援 | 啟用這個選項可確保模擬正確地支援任何自我修改程式碼。 最常見的自我修改程式碼案例是由預設模擬器行為所涵蓋。 啟用這個選項會大幅降低執行期間自我修改程式碼的效能。 |
| 停用 RWX 頁面效能最佳化 | 這項最佳化可提升可讀取且可執行 (RWX) 頁面上的程式碼效能，但可能會與某些 app 不相容。 |

您也可以選取多核心設定，如下所示。

![多核心設定螢幕擷取畫面](images/arm/Capture3.png)

這些設定變更記憶體障礙數目，記憶體障礙用於模擬期間同步處理應用程式的核心記憶體存取。 **\[快速\]** 是預設模式，但**\[嚴格\]** 和**\[非常嚴格\]** 選項將會增加障礙數目。 這會讓應用程式速度變慢，但降低應用程式錯誤的風險。 **\[單核心\]** 選項會移除所有障礙，但強制所有應用程式執行緒在單一核心上執行。

如果變更特定的設定解決您的問題，請將包含詳細資料的電子郵件傳送至 *woafeedback@microsoft.com*，以便我們納入您的意見反應。