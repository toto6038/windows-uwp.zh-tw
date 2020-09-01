---
Description: 瞭解 Windows 10X 的一些基本開發人員相關問題的答案。
title: Windows 10X 開發人員常見問題
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: f321815658a1b59d941f8b2c0e1fa5aa0142b4f7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157672"
---
# <a name="windows-10x-developer-faq"></a>Windows 10X 開發人員常見問題

> [!IMPORTANT]
> 我們最近宣佈了 Windows 10 和 Windows 10X 中優先順序的變更。
> 這些公告包括對 Windows 10X 外形規格優先順序的變更。 [請於此處深入了解。](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/)

Windows 10X 是 Windows 系列中最適合用於雙螢幕裝置的產品線。 身為開發人員，您可以藉由將應用程式優化以進行 Windows 10X，利用行動裝置和雙螢幕觀眾專屬的新功能，同時享有相同的 Windows 10 功能及豐富的桌面支援，來觸及更多物件。 [我們已在2019年底宣佈 Windows 10X](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97)，我們期待在晚期2020中發行。

![執行 Windows 10 倍的裝置](images/windows-10x-devices.png)
 
*[顯示的發行前產品，模擬的畫面並有可能變更]*

如需有關建立雙螢幕體驗和 Windows 10X 的詳細資訊，請參閱 [Microsoft 365 Dev Day](https://developer.microsoft.com/microsoft-365/virtual-events)的虛擬研討會，或 [雙螢幕開發人員](/dual-screen/)檔。以下是一些您可能會遇到的問題的解答。

### <a name="how-is-this-different-from-developing-for-windows-10"></a>這與開發 Windows 10 有何不同？

大部分的應用程式都不會有太大的差異。 您可以透過 Windows 10 SDK 來支援撰寫 Windows 10X 的應用程式。 Windows 10 的運算式中，Windows 10X 支援 Windows 執行階段 (WinRT) Api，並透過原生容器執行 Win32 應用程式。 然後，您可以從新的或現有的 UWP 或 Win32 應用程式，呼叫專為雙螢幕裝置設計的新 Api，讓他們能夠存取這個新平臺的功能和優點。

### <a name="does-this-replace-desktop-windows-10"></a>這會取代桌面 Windows 10 嗎？

否。 Windows 10X 將會與 Windows 10 的桌上出版本平行發行。 桌上出版的 Windows 10 將繼續提供新式桌面應用程式案例的增強功能和增強功能。 Windows 10X 是另一個優化的平臺，可支援雙螢幕平臺。

### <a name="when-will-windows-10x-be-released"></a>何時會 Windows 10X 發行？

Windows 10X 將在2020年的協力廠商 Neo 和其他協力廠商雙螢幕裝置上發行。

### <a name="when-can-i-start-development-for-windows-10x"></a>何時可以開始 Windows 10X 的開發？

您現在可以下載 [Microsoft Emulator 和 Windows 10X Emulator Image](/dual-screen/windows/get-dev-tools) 。 我們將繼續改善此模擬器，並透過支援其他已啟用 Windows 10X 的裝置來補充此模擬器。 這些模擬器結合了 Windows SDK 的發行前版本，可讓您在第一個雙螢幕裝置公開發行之前，進行 Windows 10X 的開發。

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>我的通用 Windows 平臺 (UWP) 應用程式是否會在 Windows 10X 上執行？

大部分 UWP 應用程式在 Windows 10X 上都受到完整支援，且在執行 Windows 10X 的裝置上運作，而不需要任何變更。 所有的 WinRT Api 都受到支援，如同 UWP 應用程式可存取的其他大部分功能。 當發行前版本開發繼續進行時，我們將發行詳述其他不支援功能的檔。

### <a name="will-my-win32-apps-run-on-windows-10x"></a>我的 Win32 應用程式是否會在 Windows 10X 上執行？

Windows 10X 提供原生支援，以在包含的環境中執行 Win32 應用程式。 大部分的 Win32 應用程式都可以在沒有事件的 Windows 10X 裝置上執行和調試，您也可以使用新的 Win32 Api 將雙螢幕支援新增至您的應用程式。

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>我的應用程式是否有任何無法在 Windows 10X 上運作的功能？

隨著 Windows 10X 繼續進行發行前版本的開發，我們將版本資訊其特定限制的檔。 不過，用來執行 Win32 應用程式的包含環境不包含 Windows Shell，因此不支援 Shell 擴充功能和類似的功能。 同樣地，Windows 10X 裝置不支援與特定系統設定相關的 Api，例如電源使用選項。

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>如果我使用 Windows 10X 功能來增強我的應用程式，它仍會在執行 desktop Windows 10 的裝置上執行嗎？

針對 Windows 10X 設計的應用程式仍可在執行桌上出版 Windows 10 的裝置上運作，不過這些新的 Windows Api 在下一個主要版本更新之前，將不會新增至桌上出版的 Windows 10。 就像您開發的應用程式在多個版本的桌上型電腦上所支援的一樣 Windows 10，請遵循 [適應性編碼的最佳作法](/windows/uwp/debug-test-perf/version-adaptive-code) ，以確保您的應用程式在10倍和 Windows 10 桌上型電腦上都能正常運作。