---
title: WinUI 2.3 版本資訊
description: WinUI 2.3 的版本資訊，包括新功能和錯誤修正。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: e3f6aca344fb86725e6addc7758110aca9eaf01b
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603845"
---
# <a name="windows-ui-library-23"></a>Windows UI 程式庫 2.3

WinUI 2.3 是 Windows UI 程式庫 (WinUI) 2020 年 1 月的發行版本。

WinUI 是開放原始碼專案，裝載於 GitHub 上的 [Windows UI 程式庫存放庫](https://aka.ms/winui)。 請在此存放庫中註冊所有錯誤報告、功能要求和社群程式碼參與。

WinUI 版本：[GitHub 版本頁面](https://github.com/microsoft/microsoft-ui-xaml/releases)

您可以透過 NuGet 套件管理員，將 WinUI 套件新增至 Visual Studio 專案。 如需詳細資訊，請參閱[開始使用 Windows UI 程式庫](../getting-started.md)。

NuGet 套件下載：[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新功能

### <a name="progress-bar-visual-refresh"></a>進度列視覺效果重新整理

**ProgressBar** 有兩個不同的視覺效果表示法。

#### <a name="indeterminate-progress-bar"></a>不確定的進度列

顯示正在進行中，但不會封鎖使用者互動的工作。

![不確定的進度列](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>確定的進度列

顯示已知工作量的完成進度。 

![確定的進度列](../images/DeterminateProgressBar.gif)

[文件連結](/windows/uwp/design/controls-and-patterns/progress-controls)

[範例連結](/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

**NumberBox** 代表可用來顯示和編輯數字的控制項。 這可支援基本方程式的驗證、遞增逐步執行及計算內嵌計算，例如乘法、除法、加法和減法。

![NumberBox](../images/NumberBoxGif.gif)

[文件和範例連結](/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**RadioButtons** 是新的容器控制項，可讓您輕鬆建立 RadioButton 元素的相關群組，同時又能正確地支援鍵盤輸入和朗讀程式/螢幕助讀程式功能

![已選取第三個選項按鈕 (共三個) 的螢幕擷取畫面。](../images/RadioButtons.png)

[文件和範例連結](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>範例

Xaml 控制項庫範例應用程式包含 WinUI 控制項用法的互動式示範和範例程式碼。

* 從 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安裝 XAML 控制項庫應用程式

* Xaml 控制項庫也是 [GitHub 上的開放原始碼項目](https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文件

Windows UI 程式庫控制項的操作說明文章隨附於[通用 Windows 平台控制項文件](/windows/uwp/design/controls-and-patterns/)。

API 參照文件位於此處：[Windows UI 程式庫 API](/windows/winui/api/)。