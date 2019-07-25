---
description: 本主題將逐步引導您完成在 C++/WinRT 專案內新增 WinUI 簡單支援的程序。
title: C++/WinRT Windows UI 程式庫簡單範例
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, Windows UI 程式庫, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 082e7ca0684495e1f67c2fa79b448866f68a059c
ms.sourcegitcommit: cba3ba9b9a9f96037cfd0e07d05bd4502753c809
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/14/2019
ms.locfileid: "67870343"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>C++/WinRT Windows UI 程式庫簡單範例

本主題將逐步引導您完成將 Windows UI (WinUI) 程式庫的簡單支援新增至 C++/WinRT 專案的程序。

> [!NOTE]
> Windows UI (WinUI) 程式庫工具組是以 NuGet 套件的形式提供，您可以使用 Visual Studio 將這些套件新增至任何現有或新的專案 (如我們將在本主題中所見)。 如需更多背景、安裝和支援資訊，請參閱[開始使用 Windows UI 程式庫](/uwp/toolkits/winui/getting-started)。

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>建立空白應用程式 (HelloWinUICppWinRT)

在 Visual Studio 中，使用 [空白應用程式 (C++/WinRT)]  專案範本建立新的專案，並將其命名為 *HelloWinUICppWinRT*。

## <a name="install-the-microsoftuixaml-nuget-package"></a>安裝 Microsoft.UI.Xaml NuGet 套件

按一下 [專案]  \> [管理 NuGet 套件...]  \>[瀏覽]  、在搜尋方塊中輸入或貼上 **Microsoft.UI.Xaml**、在搜尋結果中選取此項目，然後按一下 [安裝]  將此套件安裝到您的專案中 (您也將看見授權合約提示)。 請小心只要安裝 **Microsoft.UI.Xaml** 套件，不要安裝 **Microsoft.UI.Xaml.Core.Direct**。

## <a name="declare-winui-application-resources"></a>宣告 WinUI 應用程式資源

開啟 `App.xaml`，並在現有開頭和結尾 **Application** 標記之間貼上下列標記。

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>將 WinUI 控制項新增至 MainPage

接下來，開啟 `MainPage.xaml`。 現有的開頭 **Application**標記中有一些 xml 命名空間宣告。 新增 xml 命名空間宣告 `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`。 然後，在現有開頭與結尾 **Page**標記之間貼上下列標記，並覆寫現有的 **StackPanel**元素。

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpageh-and-cpp-as-necessary"></a>視需要編輯 MainPage.h 和 .cpp

在 `MainPage.h` 中，編輯您的 include 元素，使其看起來像這樣。

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

最後，在 `MainPage.cpp` 中，刪除您的 **MainPage::ClickHandler**實作內的程式碼，因為 *myButton*不再位於 XAML 標記中。

您現在可以建置及執行專案。

![簡單 C++/WinRT Windows UI 程式庫螢幕擷取畫面](images/winui.png)

## <a name="related-topics"></a>相關主題
* [開始使用 Windows UI 程式庫](/uwp/toolkits/winui/getting-started)