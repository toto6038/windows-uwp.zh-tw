---
title: WinUI 2.5 版本資訊
description: WinUI 2.5 的版本資訊，包括新功能和錯誤修正。
ms.date: 12/01/2020
ms.topic: reference
ms.openlocfilehash: d1b7873e874ff38037fbf766ef16b1e924edec16
ms.sourcegitcommit: 03308873eafd0f768e1c518f4d1cc4e4fe0b70b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2020
ms.locfileid: "96606025"
---
# <a name="windows-ui-library-25"></a>Windows UI 程式庫 2.5

WinUI 2.5 是 Windows UI 程式庫 (WinUI) 的最新官方版本。

WinUI 是開放原始碼專案，裝載於 GitHub 上的 [Windows UI 程式庫存放庫](https://aka.ms/winui)。 請在此存放庫中註冊所有錯誤報告、功能要求和社群程式碼參與。

WinUI 版本：[GitHub 版本頁面](https://github.com/microsoft/microsoft-ui-xaml/releases)

您可以透過 NuGet 套件管理員，將 WinUI 套件新增至 Visual Studio 專案。 如需詳細資訊，請參閱[開始使用 Windows UI 程式庫](../getting-started.md)。

NuGet 套件下載：[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新功能

### <a name="infobar"></a>資訊列

[資訊列](/uwp/design/controls-and-patterns/infobar)控制項是用來將全應用程式狀態訊息顯示給高度可見且非侵入的使用者。 控制項包含嚴重性屬性，可指出所顯示的訊息類型，以及指定您自己的動作或超連結按鈕的選項。 當資訊列與其他 UI 內容內嵌時，您也可以指定控制項是否一律可見，或使用者是否可加以關閉。

這個範例會顯示具有關閉按鈕和訊息的預設狀態資訊列。

:::image type="content" source="../images/infobar-default-title-message.png" alt-text="具有關閉按鈕和訊息的預設狀態資訊列範例。":::

這個動畫範例會顯示具有各種嚴重性狀態和自訂訊息的資訊列。

:::image type="content" source="../images/infobar-severity-animated.gif" alt-text="資訊列嚴重性狀態和自訂訊息的動畫範例。":::

[使用方針](/windows/uwp/design/controls-and-patterns/infobar)

[API 參考](/windows/winui/api/microsoft.ui.xaml.controls.infobar)

### <a name="determinate-progressring"></a>確定 ProgressRing

[ProgressRing](/uwp/design/controls-and-patterns/progress-controls) 的「確定」狀態會顯示工作已完成的百分比。 此控制項應用於已知持續時間的作業，以及用於作業進度不應封鎖使用者與 App 的互動。

下列動畫影像示範確定 ProgressRing 控制項。

:::image type="content" source="../images/progressring-determinate-mode-animated.gif" alt-text="確定 ProgressRing 控制項的動畫範例。":::<br>

[使用方針](/windows/uwp/design/controls-and-patterns/progress-controls#progress-controls-best-practices)

[API 參考](/windows/winui/api/microsoft.ui.xaml.controls.progressring)


### <a name="navigationview-footermenuitems"></a>NavigationView FooterMenuItems

使用 NavigationView 控制項的 FooterMenuItems 屬性，將瀏覽項目放在瀏覽窗格的結尾 (相較於 MenuItems 屬性，則是將項目放在窗格的開頭)。

下圖顯示在頁尾功能表中具有「帳戶」、「您的購物車」以及「說明」瀏覽項目的 NavigationView。

:::image type="content" source="../images/navigationview-footermenuitems.png" alt-text="在頁尾功能表中具有「帳戶」、「您的購物車」以及「說明」瀏覽項目的 NavigationView 範例。":::

[使用方針](/windows/uwp/design/controls-and-patterns/navigationview?#footer-menu-items)

[API 參考](/windows/winui/api/microsoft.ui.xaml.controls.navigationview.footermenuitems)

## <a name="samples"></a>範例

**XAML 控制項庫** 範例應用程式包含每個 WinUI 功能和控制項的範例。

如果您已安裝 **XAML 控制項庫** 應用程式，並將其更新為最新版本：

- 請參閱[資訊列](xamlcontrolsgallery:/item/InfoBar)運作方式。
- 請參閱 [ProgressRing](xamlcontrolsgallery:/item/ProgressRing) 運作方式。
- 請參閱 [NavigationView](xamlcontrolsgallery:/item/NavigationView) 運作方式。

如果您尚未安裝 XAML 控制項庫應用程式，請從 [Microsoft Store](https://aka.ms/xamlgalleryapp) 取得。

您也可以在 [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery) 中檢視、複製及建立 XAML 控制項庫原始程式碼。

## <a name="other-updates"></a>其他更新

如需此版本中所解決的許多 GitHub 問題，請參閱我們的[重大變更](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.5.0)清單。
