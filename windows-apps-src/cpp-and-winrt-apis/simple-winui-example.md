---
description: 本主題將逐步引導您完成在 C++/WinRT 專案內新增 WinUI 簡單支援的程序。
title: C++/WinRT Windows UI 程式庫簡單範例
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, Windows UI 程式庫, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: aadf177bc4a44f67550dba1f6f706525b8460857
ms.sourcegitcommit: c9bab19599c0eb2906725fd86d0696468bb919fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256171"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>C++/WinRT Windows UI 程式庫簡單範例

本主題將逐步引導您完成將 [Windows UI (WinUI)](https://github.com/Microsoft/microsoft-ui-xaml) 程式庫的簡單支援新增至 C++/WinRT 專案的程序。 順便一提，Windows UI 程式庫本身是以 C++/WinRT 撰寫。

> [!NOTE]
> Windows UI (WinUI) 程式庫工具組是以 NuGet 套件的形式提供，您可以使用 Visual Studio 將這些套件新增至任何現有或新的專案 (如我們將在本主題中所見)。 如需更多背景、安裝和支援資訊，請參閱[開始使用 Windows UI 程式庫](/uwp/toolkits/winui/getting-started)。

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>建立空白應用程式 (HelloWinUICppWinRT)

在 Visual Studio 中，使用 [空白應用程式 (C++/WinRT)]  專案範本建立新的專案。 請確定您正在使用 (C++/WinRT)  範本，而不是 (通用 Windows)  範本。

將新專案的名稱設定為 *HelloWinUICppWinRT*，並 (如此您的資料夾結構會符合此逐步解說) 取消核取 [將解決方案和專案放置於同一個目錄]  。

## <a name="install-the-microsoftuixaml-nuget-package"></a>安裝 Microsoft.UI.Xaml NuGet 套件

按一下 [專案]  \> [管理 NuGet 套件...]  \> [瀏覽]  ，在搜尋方塊中輸入或貼上 **Microsoft.UI.Xaml**、在搜尋結果中選取此項目，然後按一下 [安裝]  將此套件安裝到您的專案中 (您也將看見授權合約提示)。 請小心只要安裝 **Microsoft.UI.Xaml** 套件，不要安裝 **Microsoft.UI.Xaml.Core.Direct**。

## <a name="declare-winui-application-resources"></a>宣告 WinUI 應用程式資源

開啟 `App.xaml`，並在現有開頭和結尾 **Application** 標記之間貼上下列標記。

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>將 WinUI 控制項新增至 MainPage

接下來，開啟 `MainPage.xaml`。 現有的開頭 **Page** 標記中有一些 xml 命名空間宣告。 新增 xml 命名空間宣告 `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`。 然後，在現有開頭與結尾 **Page**標記之間貼上下列標記，並覆寫現有的 **StackPanel**元素。

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpagecpp-and-h-as-necessary"></a>視需要編輯 MainPage.cpp 和 .h

在 `MainPage.cpp` 中，刪除您的 **MainPage::ClickHandler** 實作內的程式碼，因為 *myButton* 不再位於 XAML 標記中。

在 `MainPage.h` 中，編輯您的 include 元素，使其看起來像下面清單中的項目。 如果您要從一個以上的 XAML 頁面使用 WinUI，則可以移至預先編譯的標頭檔 (通常為 `pch.h`) 並改為將其包含在其中。

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

立即建置專案。

當您將 NuGet 套件新增至 C++/WinRT 專案 (例如您稍早新增的 **Microsoft.UI.Xaml** 套件)，並建立該專案時，工具會在專案的 `\Generated Files\winrt` 資料夾中產生一組投影標頭檔。 如果您已遵循逐步解說的指示，現在會有一個 `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt` 資料夾。 您對上述 `MainPage.h` 所做的變更，會使這些投影標頭檔包含在專案中。 這是必要的，以便解析 NuGet 套件中的類型參考。

您現在可以執行專案。

![簡單 C++/WinRT Windows UI 程式庫螢幕擷取畫面](images/winui.png)

## <a name="related-topics"></a>相關主題
* [開始使用 Windows UI 程式庫](/uwp/toolkits/winui/getting-started)