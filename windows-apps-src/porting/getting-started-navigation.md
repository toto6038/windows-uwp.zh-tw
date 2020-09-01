---
title: 瀏覽入門
description: 瞭解如何使用通用 Windows 平臺 (UWP) Frame 類別，將頁面流覽新增至具有多個視圖的 Windows 10 應用程式。
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ec5073664c911133b769ed9079a597459381a670
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174862"
---
# <a name="getting-started-navigation"></a>開始使用：瀏覽


## <a name="adding-navigation"></a>新增瀏覽

iOS 提供 **UINavigationController** 類別來協助 app 內瀏覽：您可以推入及彈出檢視控制項來建立 **UIViewControllers** 的階層以定義您的 app。

相反地，包含多個檢視的 Windows 10 app 會採取類似網站的方式進行瀏覽。 您可以想像您的使用者透過不斷點按控制項，從一個頁面跳至不同的頁面來使用 app。 如需詳細資訊，請參閱[瀏覽設計基本知識](../design/basics/navigation-basics.md)。

在 Windows 10 app 中管理這類瀏覽的其中一種方式是使用 [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) 類別。 下列逐步解說會說明如何嘗試使用這種方式。

讓我們繼續之前開始的方案，開啟 **MainPage.xaml** 檔案，然後在 \[**設計**\] 檢視中新增一個按鈕。 將按鈕的 \[**Content**\] 屬性從「Button」變更為「Go To Page」。 然後為按鈕的 **Click** 事件建立一個處理常式，如下圖中所示。 如果您不記得怎麼做，請檢閱上一節中的逐步解說 (提示：按兩下 \[**設計**\] 檢視中的按鈕)。

![在 Visual Studio 中新增按鈕及其 Click 事件](images/ios-to-uwp/vs-go-to-page.png)

讓我們開始新增頁面。 在 \[**方案**\] 檢視中，依序點選 \[**專案**\]功能表和 \[**加入新項目**\]。 點選 **空白頁** \(如下圖所示\)，然後點選 **新增**。

![在 Visual Studio 中新增頁面](images/ios-to-uwp/vs-add-new-page.png)

接下來，將按鈕新增到 BlankPage.xaml 檔案。 讓我們使用 AppBarButton 控制項，並為它提供一個返回箭頭影像：在 \[**XAML**\] 檢視中，於 `<Grid> </Grid>` 元素之間新增 ` <AppBarButton Icon="Back"/>`。

現在，讓我們將事件處理常式新增至按鈕：在 **設計** 視圖中按兩下控制項，然後 Microsoft Visual Studio 將文字 "icon \_ click" 加入至 **按一下** 方塊（如下圖所示），然後在 BlankPage.xaml.cs 檔案中加入並顯示對應的事件處理常式。

![在 Visual Studio 中新增上一頁按鈕及其 Click 事件](images/ios-to-uwp/vs-add-back-button.png)

如果您返回檔案的 \[**XAML**\] 檢視，`<AppBarButton>` 元素的 Extensible Application Markup Language (XAML) 程式碼現在應該看起來像這樣：

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

返回 BlankPage.xaml.cs 檔案並新增這個程式碼，讓使用者點選按鈕之後返回到上一個頁面。

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

最後，開啟 MainPage.xaml.cs 檔案並新增下列程式碼。 它會在使用者點選按鈕之後開啟 BlankPage。

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

現在執行程式。 點選「Go To Page」按鈕移至其他頁面，然後點選上一頁箭頭按鈕返回上一個頁面。

頁面瀏覽是由 [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) 類別來管理的。 當 iOS 中的 **UINavigationController** 類別使用 **pushViewController** 和 **popViewController** 方法時，UWP 應用程式的 **Frame** 類別會提供 [**導覽**](/uwp/api/windows.ui.xaml.controls.frame.navigate) 和 [**GoBack**](/uwp/api/windows.ui.xaml.controls.frame.goback) 方法。 **Frame** 類別也有名為 [**GoForward**](/uwp/api/windows.ui.xaml.controls.frame.goforward) 的方法，可執行您期待的動作。

這個逐步解說會在您每次瀏覽到 BlankPage 時，建立新的 BlankPage 執行個體。 (系統會自動釋出或*釋放*之前的執行個體)。 如果您不想要每次都建立新的執行個體，請將下列程式碼新增到 BlankPage.xaml.cs 檔案中的 BlankPage 類別建構函式。 這會啟用 [**NavigationCacheMode**](/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) 行為。

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

您也可以取得或設定 **Frame** 類別的 [**CacheSize**](/uwp/api/windows.ui.xaml.controls.frame.cachesize) 屬性，以管理瀏覽記錄可以快取的頁面數。

如需瀏覽的詳細資訊，請參閱[瀏覽](../design/basics/navigation-basics.md)和 [XAML 個人特質動畫範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20personality%20animations%20sample%20(Windows%208))。

**注意**   如需使用 JavaScript 和 HTML 流覽 UWP 應用程式的詳細資訊，請參閱[快速入門：使用單頁流覽](/previous-versions/windows/apps/hh452768(v=win.10))。
 
### <a name="next-step"></a>後續步驟

[開始使用：動畫](getting-started-animation.md)