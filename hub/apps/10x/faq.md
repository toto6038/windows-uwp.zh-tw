---
Description: 瞭解一些關於 Windows 10 的基本開發人員問題的答案。
title: Windows 10 開發人員常見問題
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 3ba14e33c098d3515522a9a5907065751fafba87
ms.sourcegitcommit: 13bda6040988461a61b1b5561fde2f7a54835ccd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/03/2020
ms.locfileid: "84318234"
---
# <a name="windows-10x-developer-faq"></a>Windows 10 開發人員常見問題

> [!IMPORTANT]
> 我們最近宣佈了 Windows 10 和 Windows 10 的優先順序變更。
> 這些公告包括對 Windows 10 外型規格優先順序的變更。 [如需詳細資訊，請參閱這裡。](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/)

Windows 10 是 Windows 系列中的產品線，最適合用於雙螢幕裝置。 身為開發人員，您可以將 Windows 10 的應用程式優化，利用行動裝置和雙畫面觀眾特有的新功能，同時享有相同範圍的 Windows 10 功能和豐富的桌面支援，來觸及更多物件。 [我們已于2019年底宣佈 Windows 10](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97)，我們正期待在晚期2020中發行。

![執行 Windows 10 的裝置](images/windows-10x-devices.png)
 
*[顯示發行前版本的產品、模擬的螢幕並可能變更]*

如需有關建立雙畫面體驗和 Windows 10 倍的詳細資訊，請參閱[Microsoft 365 Dev Day](https://developer.microsoft.com/microsoft-365/virtual-events)的虛擬研討會，或[雙畫面開發人員](https://docs.microsoft.com/dual-screen/)檔。如需概覽資訊，以下是您可能會遇到的一些問題答案。

### <a name="how-is-this-different-from-developing-for-windows-10"></a>這與針對 Windows 10 進行開發有何不同？

對於大部分的應用程式而言，完全不同。 撰寫 Windows 10 的應用程式是透過 Windows 10 SDK 來支援。 Windows 10 是 Windows 10 的運算式，可支援 Windows 執行階段（WinRT） Api，並透過原生容器執行 Win32 應用程式。 接著，您可以從新的或現有的 UWP 或 Win32 應用程式呼叫專為雙畫面裝置設計的新 Api，讓他們能夠存取此新平臺的功能和優點。

### <a name="does-this-replace-desktop-windows-10"></a>這會取代桌上型電腦 Windows 10 嗎？

不會。 Windows 10 將會平行發行至 Windows 10 的桌上出版本。 桌上出版的 Windows 10 將繼續提供新式桌面應用程式案例的增強功能和改進。 Windows 10 是另一個針對支援雙畫面平臺而優化的平臺。

### <a name="when-will-windows-10x-be-released"></a>何時會釋放 Windows 10 倍？

Windows 10 將會隨著 Surface Neo 和其他協力廠商雙畫面裝置在2020晚期發行。

### <a name="when-can-i-start-development-for-windows-10x"></a>何時可以開始開發 Windows 10？

您現在可以下載[Microsoft 模擬器和 Windows 10 倍模擬器映射](https://docs.microsoft.com/dual-screen/windows/get-dev-tools)。 我們將繼續改善此模擬器，並對其他支援 Windows 10 的裝置提供更好的協助工具。 這些模擬器與 Windows SDK 的搶鮮版結合，可讓您在第一個雙畫面裝置公開發行之前，先針對 Windows 10 進行開發。

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>我的通用 Windows 平臺（UWP）應用程式會在 Windows 10 上執行嗎？

大部分的 UWP 應用程式在 Windows 10 上完全受到支援，並在執行 Windows 10 的裝置上運作，而不需要任何變更。 所有的 WinRT Api 都受到支援，就像 UWP 應用程式可以存取的其他功能一樣。 當發行前版本開發繼續進行時，我們將發行詳述其他不支援功能的檔。

### <a name="will-my-win32-apps-run-on-windows-10x"></a>我的 Win32 應用程式會在 Windows 10 上執行嗎？

Windows 10 提供原生支援，可在包含的環境中執行 Win32 應用程式。 大部分的 Win32 應用程式都可以在 Windows 10 裝置上執行及進行調試，而不需要事件，而且您也可以使用新的 Win32 Api 將雙畫面支援新增至您的應用程式。

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>我的應用程式有任何功能無法在 Windows 10 上使用嗎？

隨著 Windows 10 繼續進行發行前版本的開發，我們將發行檔來強調其特定限制。 不過，用來執行 Win32 應用程式的內含環境並不包含 Windows Shell，因此不支援 Shell 延伸模組和類似的功能。 同樣地，Windows 10 的裝置不支援與特定系統設定相關的 Api，例如電源使用選項。

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>如果我使用 Windows 10 的功能來增強我的應用程式，它還是會在執行 desktop Windows 10 的裝置上執行？

針對 Windows 10 所設計的應用程式仍可在執行桌上出版本 Windows 10 的裝置上運作，不過，在下一個主要版本更新之前，這些新的 Windows Api 將不會新增至 Windows 10 的桌上出版本。 就像您在多個桌面 Windows 10 版本上開發的應用程式一樣，請遵循調適型[編碼最佳作法](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)，以確保您的應用程式可在10倍和桌面 windows 10 上正常運作。 
