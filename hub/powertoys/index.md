---
title: Microsoft PowerToys
description: Microsoft PowerToys 是一組自訂 Windows 10 的公用程式。 公用程式包括 [ColorPicker] (按一下任何位置以抓取色彩值)、[FancyZones] (將視窗置於格線配置內的快捷鍵)、[檔案總管附加元件] (預覽 SVG 或 Markdown 檔案)、[圖像大小調整器] (簡單按一下滑鼠右鍵以調整一或多個影像的大小)、[鍵盤管理器] (重新對應按鍵或建立您自己的快捷鍵)、[PowerRename] (使用搜尋和取代進行大量重新命名)、[PowerToys Run] (Alt + 空格鍵以啟動應用程式)、[快捷鍵指南] 等等。
ms.date: 12/02/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 1f441c7ef9fc4268b35c041f1100cb7f116318a5
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97611828"
---
# <a name="microsoft-powertoys-utilities-to-customize-windows-10"></a>Microsoft PowerToys：自訂 Windows 10 的公用程式

Microsoft PowerToys 是一組公用程式，可協助大量使用者調整及簡化其 Windows 10 體驗，從而提升生產力。

> [!div class="nextstepaction"]
> [安裝 PowerToys](install.md)

## <a name="processor-support"></a>處理器支援

- **x64**:支援
- **x86**:開發中 (請參閱[問題 #602](https://github.com/microsoft/PowerToys/issues/602))
- **ARM**：開發中 (請參閱[問題 #490](https://github.com/microsoft/PowerToys/issues/490))

## <a name="current-powertoy-utilities"></a>目前的 PowerToy 公用程式

目前可用的公用程式包括：

### <a name="color-picker"></a>色彩選擇器

:::row:::
    :::column:::
        [![ColorPicker 螢幕擷取畫面](../images/pt-color-picker.png)](color-picker.md)
    :::column-end:::
    :::column span="2":::
        [ColorPicker](color-picker.md) 是一種全系統的色彩挑選公用程式，可透過 <kbd>Win</kbd>+<kbd>Shift</kbd>+<kbd>C</kbd> 啟動。 從目前執行中的任何應用程式挑選色彩，選擇器會以可設定的格式自動將色彩複製到剪貼簿。 色彩選擇器也包含編輯器，其中會顯示先前挑選色彩的歷程記錄，可讓您微調選取的色彩，並複製不同的字串表示法。 這段程式碼是以 [Martin Chrzan 的色彩選擇器](https://github.com/martinchrzan/ColorPicker)為基礎。
    :::column-end:::
:::row-end:::

### <a name="fancy-zones"></a>美觀的區域

:::row:::
    :::column:::
        [![FancyZones 螢幕擷取畫面](../images/pt-fancy-zones.png)](fancyzones.md)
    :::column-end:::
    :::column span="2":::
        [FancyZones](fancyzones.md) 是一個視窗管理器，可讓您輕鬆地建立複雜的視窗版面配置，並快速將視窗定位到這些配置中。
    :::column-end:::
:::row-end:::

### <a name="file-explorer-add-ons"></a>檔案總管附加元件

:::row:::
    :::column:::
        [![檔案總管螢幕擷取畫面](../images/pt-file-explorer.png)](file-explorer.md)
    :::column-end:::
    :::column span="2":::
        [檔案總管](file-explorer.md)附加元件會在 [檔案總管] 中啟用預覽窗格轉譯來顯示 SVG 圖示 (.svg) 和 Markdown (.md) 檔案預覽。 若要啟用預覽窗格，請選取 [檔案總管] 中的 [檢視] 索引標籤，然後選取 [預覽窗格]。
    :::column-end:::
:::row-end:::

### <a name="image-resizer"></a>圖像大小調整器

:::row:::
    :::column:::
        [![圖像大小調整器螢幕擷取畫面](../images/pt-image-resizer.png)](image-resizer.md)
    :::column-end:::
    :::column span="2":::
        [圖像大小調整器](image-resizer.md)是 Windows Shell 擴充功能，可快速調整影像大小。  只要以滑鼠右鍵按一下 [檔案總管]，就能立即調整一或多個影像的大小。 這段程式碼是以 [Brice Lambson 的圖像大小調整器](https://github.com/bricelam/ImageResizer)為基礎。
    :::column-end:::
:::row-end:::

### <a name="keyboard-manager"></a>鍵盤管理器

:::row:::
    :::column:::
        [![鍵盤管理器螢幕擷取畫面](../images/pt-keyboard-manager.png)](keyboard-manager.md)
    :::column-end:::
    :::column span="2":::
        [鍵盤管理器](keyboard-manager.md)可讓您藉由重新對應按鍵及建立您自己的鍵盤快速鍵，來自訂鍵盤以提高生產力。 此 PowerToy 需要 Windows 10 1903 (組建18362) 或更新版本。
    :::column-end:::
:::row-end:::

### <a name="powerrename"></a>PowerRename

:::row:::
    :::column:::
        [![PowerRename 螢幕擷取畫面](../images/pt-rename.png)](powerrename.md)
    :::column-end:::
    :::column span="2":::
        [PowerRename](powerrename.md) 可讓您執行大量重新命名、搜尋和取代檔檔案名稱。 其中包含先進的功能，例如使用規則運算式、以特定檔案類型為目標、預覽預期的結果，以及復原變更的功能。 這段程式碼是以 [Chris Davis 的 SmartRename](https://github.com/chrdavis/SmartRename) 為基礎。
    :::column-end:::
:::row-end:::

### <a name="powertoys-run"></a>PowerToys Run

:::row:::
    :::column:::
        [![PowerToys Run 螢幕擷取畫面](../images/pt-run.png)](run.md)
    :::column-end:::
    :::column span="2":::
        [PowerToys Run](run.md) 可協助您立即搜尋及啟動應用程式 - 只要輸入 <kbd>Alt</kbd>+<kbd>空格鍵</kbd>這個快捷鍵並開始輸入。 其為開放原始碼，並可針對其他外掛程式進行模組化。 現在也包含 [視窗切換器]。 此 PowerToy 需要 Windows 10 1903 (組建18362) 或更新版本。
    :::column-end:::
:::row-end:::

### <a name="shortcut-guide"></a>快捷鍵指南

:::row:::
    :::column:::
        [![快捷鍵指南螢幕擷取畫面](../images/pt-shortcut-guide.png)](shortcut-guide.md)
    :::column-end:::
    :::column span="2":::
        當使用者按住 Windows 鍵一秒以上時，隨即出現 [Windows 按鍵快捷鍵指南](shortcut-guide.md)，並且會顯示桌面目前狀態的可用快捷鍵。
    :::column-end:::
:::row-end:::

## <a name="powertoys-video-walk-through"></a>PowerToys 影片逐步解說

在這段影片中，Clint Rutkas (PowerToys 的 PM) 除了會分享一些秘訣、參與方式的資訊等等，還會逐步解說如何安裝及使用各種可用的公用程式。

> [!VIDEO https://channel9.msdn.com/Shows/Tabs-vs-Spaces/PowerToys-Utilities-to-customize-Windows-10/player?format=ny]

## <a name="future-powertoy-utilities"></a>未來的 PowerToy 公用程式

### <a name="experimental-powertoys"></a>Experimental PowerToys

安裝 PowerToys 的發行前實驗性版本，以試用最新的實驗性公用程式，包括：

#### <a name="video-conference-mute-experimental"></a>視訊會議靜音 (實驗性)

:::row:::
    :::column:::
        [![視訊會議靜音螢幕擷取畫面](../images/pt-video-conference-mute.png)](video-conference-mute.md)
    :::column-end:::
    :::column span="2":::
        [視訊會議靜音](video-conference-mute.md)是一種在電話會議進行時，使用 <kbd>⊞ Win</kbd>+<kbd>N</kbd> 將麥克風和攝影機全域「靜音」，而不論目前焦點的應用程式為何的快速方法。 這僅包含在 [PowerToys 的發行前/實驗性版本](https://github.com/microsoft/PowerToys/releases/)中，且需要 Windows 10 1903 (組建 18362) 或更新版本。
    :::column-end:::
:::row-end:::

## <a name="known-issues"></a>已知問題

在 GitHub 上 PowerToys 存放庫的 [[問題](https://github.com/microsoft/PowerToys/issues)] 索引標籤中，搜尋已知問題或提出新問題。

## <a name="contribute-to-powertoys-open-source"></a>參與 PowerToys (開放原始碼)

PowerToys 歡迎您的參與！ PowerToys 開發團隊很高興能夠與進階使用者團體合作，打造可協助使用者充分運用 Windows 的工具。 有多種方式可以參與：

- 撰寫[技術規格](https://codeburst.io/on-writing-tech-specs-6404c9791159)
- 提交[設計概念或建議](https://www.microsoft.com/design/inclusive/)
- [參與編輯文件](https://docs.microsoft.com/contribute/)
- 識別並修正[原始程式碼](https://github.com/microsoft/PowerToys/tree/master/src)中的 Bug
- [撰寫新功能和 PowerToy 公用程式的程式碼](https://github.com/microsoft/PowerToys/tree/master/doc/devdocs)

開始處理您想要參與的功能之前，請 **閱讀[參與者指南](https://github.com/microsoft/PowerToys/blob/master/CONTRIBUTING.md)** 。 PowerToys 團隊很樂意與您合作找出最佳的方法、提供整個功能開發的指引和導師計劃，並協助避免任何浪費或重複的工作。

## <a name="powertoys-release-notes"></a>PowerToys 版本資訊

PowerToys [版本資訊](https://github.com/microsoft/PowerToys/releases/)會列在 GitHub 存放庫的安裝頁面上。 如需參考，您也可以在 PowerToys Wiki 上找到[版本檢查清單](https://github.com/microsoft/PowerToys/wiki/Release-check-list)。

## <a name="powertoys-history"></a>PowerToys 歷程記錄

受到 [Windows 95 年代的 PowerToys 專案](https://en.wikipedia.org/wiki/Microsoft_PowerToys)所啟發，這次重新啟動讓進階使用者能夠從 Windows 10 命令介面中萃取到更高的效率，並針對個別的工作流程進行自訂。  您可以在[這裡](https://socket3.wordpress.com/2016/10/22/using-windows-95-powertoys/)找到 Windows 95 PowerToys 的絕佳概觀。

## <a name="powertoys-roadmap"></a>PowerToys 藍圖

PowerToys 是一項快速策畫，開放原始碼團隊旨在讓進階使用者能夠從 Windows 10 命令介面中萃取到更高的效率，並針對個別的工作流程進行自訂。 工作優先順序會一致地進行檢查、重新評估及調整，旨在提升使用者的生產力。

- [可能的 PowerToys 新規格](https://github.com/microsoft/PowerToys/wiki/Specs)
- [待辦項目優先順序清單](https://github.com/microsoft/PowerToys/wiki/Roadmap#backlog-priority-list-in-order)
- [1.0 版策略規格](https://github.com/microsoft/PowerToys/wiki/Version-1.0-Strategy)，2020 年 2 月
