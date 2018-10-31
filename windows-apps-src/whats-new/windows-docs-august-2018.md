---
author: QuinnRadich
title: 2018 年 8 月 Windows 文件的最新動向-開發 UWP app
description: 新功能、 影片、 範例及開發人員指引已加入 2018 年 8 月的 Windows 10 開發人員文件。
keywords: 新動向，更新，功能，開發人員指引，Windows 10，august
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 76de6c5c1e8925dd8b166a8d99c39116bf66141d
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5829716"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>在 2018 年 8 月 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、 開發人員指引和影片已可供 8 月月份中。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>設計

下列功能已新增到 Windows Insider Preview 組建，可透過[Windows 測試人員](https://insider.windows.com/)計畫。

* [Windows UI 文件庫](https://aka.ms/winui-docs)是一組提供控制項與其他使用者 interfact 元素用於 UWP 應用程式的 NuGet 套件。 這些套件也是使用較舊版本的 Windows 10 相容，因此您的應用程式運作方式，即使您的使用者沒有最新的 OS 版本。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、 [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)，以及[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)提供按鈕控制項具有特殊功能來增強您的應用程式使用者介面。

![分割按鈕來選取前景色彩](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 現已支援[頂端瀏覽](../design/controls-and-patterns/navigationview.md)，對於您的應用程式有數目較少的瀏覽選項且需要更多空間，您的應用程式內容的案例。

* 樹狀檢視已增強對支援[資料繫結，項目範本，以及拖。](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>套件支援的架構

套件支援架構是開放原始碼套件，可協助您將套用修正程式中對 win32 應用程式時您不需要的存取權的原始程式碼，讓它可以 MSIX 容器中執行。

若要了解更多資訊，請參閱[套用執行階段修正程式，以使用套件支援架構 MSIX 封裝](../porting/package-support-framework.md)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="web-api-extensions"></a>Web API 擴充功能

已新增的[舊版 Microsoft API 延伸](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)清單跨瀏覽器 web 開發的 Mozilla Developer Network 文件。 這些 API 的擴充功能都是唯一的 Internet Explorer 或 Microsoft Edge，並補充現有 MDN web 文件中的相容性及 broswer 支援的相關資訊。舊版 Microsoft [CSS 擴充功能](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)與[JavaScript 延伸模組](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也使用，且您可以找到豐富 web API 的資訊從 MDN 直接在呈現[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + /winrt 程式碼範例

我們已經新增 250 [C + + WinRT](../cpp-and-winrt-apis/index.md)程式碼清單主題中我們的文件，伴隨的現有的 C + + /CX 程式碼範例。

### <a name="project-rome"></a>Project Rome

[Project Rome 文件](https://docs.microsoft.com/windows/project-rome/)網站已重新整理到功能第一種方法。 這應該讓開發人員尋找他們所尋找的並且跨多個平台實作他們選擇的功能更容易。

## <a name="videos"></a>影片

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 外掛程式

Unity 的 Xbox Live 外掛程式包含加入 Xbox Live 簽署、 統計資料、 朋友清單、 雲端儲存空間及排行榜加入您的遊戲的支援。 [觀看影片](https://youtu.be/fVQZ-YgwNpY)以了解更多]，然後[下載 GitHub 套件](https://aka.ms/UnityPlugin)來開始。

### <a name="one-dev-question"></a>一個開發人員問題

在開發人員問題影片系列，longtime Microsoft 開發人員會討論一系列的 Windows 開發、 小組文化特性和歷程記錄的相關問題。 以下是我們已經回答的最新問題 ！

Raymond Chen:

* [如何核心知道何時要重新啟動視訊驅動程式？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [什麼是 Windows 中的 Burgermaster 物件後方故事？](https://youtu.be/0TDSbyAIvX0)