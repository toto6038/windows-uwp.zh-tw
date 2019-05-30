---
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: 本主題提供可移植到 Windows 10 通用 Windows 平台 (UWP) 應用程式非常簡單的 Windows Phone Silverlight 應用程式的個案研究。
title: Windows Phone Silverlight UWP 案例研究，Bookstore1
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 02337d02472b7215f0fb9be47419caf52420e0f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372411"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore1"></a>Windows Phone Silverlight UWP 案例研究：Bookstore1


本主題提供可移植到 Windows 10 通用 Windows 平台 (UWP) 應用程式非常簡單的 Windows Phone Silverlight 應用程式的個案研究。 使用 Windows 10，您可以建立單一應用程式封裝，您的客戶可以將安裝到各種不同的裝置，以及這是我們要在此案例研究。 請參閱 [UWP app 指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

我們將移植的 app 包含繫結到檢視模型的 **ListBox**。 此檢視模型有一個顯示書名、作者及封面的書籍清單。 書籍封面影像的 [建置動作] 是設定為 [內容]，而 [複製到輸出目錄] 是設定為 [不要複製]。    

本節之前的主題說明平台之間的差異，並針對 app 各個方面 (從 XAML 標記、經過繫結到檢視模型，再到存取資料) 的移植程序，提供深入的詳細資料和指導方針。 案例研究旨在藉由真實範例中的運作示範，來為該指導方針提供補充。 這些案例研究是假設您已看過指導方針，因此不會重複其內容。

**附註**  時開啟 Bookstore1Universal\_10，以 Visual Studio 中，如果您看到訊息 「 Visual Studio 需要更新 」，然後遵循步驟來選取中的目標平台版本控制[TargetPlatformVersion](wpsl-to-uwp-troubleshooting.md)。

## <a name="downloads"></a>下載

[下載 Bookstore1WPSL8 Windows Phone Silverlight 應用程式](https://go.microsoft.com/fwlink/?linkid=517053)。

[下載 Bookstore1Universal\_10 的 Windows 10 應用程式](https://go.microsoft.com/fwlink/?linkid=532950)。

## <a name="the-windowsphone-silverlight-app"></a>Windows Phone Silverlight 應用程式

以下是我們即將移植的 Bookstore1WPSL8 app 的外觀。 它是一個垂直捲動的書籍清單方塊，位於 app 名稱的標題和網頁標題下方：

![bookstore1wpsl8 的外觀](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 專案

這是一項非常快速的工作，可在 Visual Studio 中建立新專案、從 Bookstore1WPSL8 將檔案複製到其中，以及在新專案中包含複製的檔案。 一開始先建立新的空白應用程式 (Windows 通用) 專案。 它命名為 Bookstore1Universal\_10。 這些都是透過從 Bookstore1WPSL8 複製到 Bookstore1Universal 檔案\_10。

-   複製包含書封面影像 PNG 檔案的資料夾 (資料夾\\資產\\CoverImages)。 在複製資料夾之後，請在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 在您複製的資料夾上按一下滑鼠右鍵，然後按一下 [加入至專案]。  該命令就是我們所謂的在專案中「包含」檔案或資料夾。 每次當您複製檔案或資料夾時，請按一下 [方案總管] 中的 [重新整理]，然後在專案中加入檔案或資料夾。   不需要對目的地中您正在取代的檔案執行此動作。
-   複製包含檢視模型的原始程式檔的資料夾 (資料夾是\\ViewModel)。
-   複製 MainPage.xaml 並取代目的地中的檔案。

我們可以保留 App.xaml 和 Visual Studio 為我們產生在 Windows 10 專案中的 App.xaml.cs。

編輯您剛才複製的來源的程式碼和標記檔案，並將 Bookstore1WPSL8 命名空間的任何參考變更為 Bookstore1Universal\_10。 執行此作業的快速方法是使用 [**檔案中取代**] 功能。 在檢視模型原始程式檔的命令式程式碼中，需要進行下列移植變更：

-   將 `System.ComponentModel.DesignerProperties` 變更為 `DesignMode`，然後對其使用 [解析] 命令。  刪除 `IsInDesignTool` 屬性，然後使用 IntelliSense 來新增正確的屬性名稱：`DesignModeEnabled`。
-   對 `ImageSource` 使用 [解析] 命令。 
-   對 `BitmapImage` 使用 [解析] 命令。 
-   刪除 using `System.Windows.Media;` 和 `using System.Windows.Media.Imaging;`。
-   變更所傳回的值**Bookstore1Universal\_10.BookstoreViewModel.AppName**屬性從"BOOKSTORE1WPSL8"為"BOOKSTORE1UNIVERSAL 」。

在 MainPage.xaml 中，需要進行下列移植變更：

-   將 `phone:PhoneApplicationPage` 變更為 `Page` (別忘了變更出現在屬性元素語法中的部分)。
-   刪除 `phone` 和 `shell` 命名空間前置字元宣告。
-   將其餘命名空間前置字元宣告中的 "clr-namespace" 變更為 "using"。

如果我們想要以最快的方式看到結果，我們可以選擇非常不費力的方式來更正標記編譯錯誤，即使這意謂著暫時移除標記。 但是讓我們記下因為這麼做所導致的負債。 以下是此案例的做法：

1.  在 **MainPage.xaml** 的根 **Page** 元素中，刪除 `SupportedOrientations="Portrait"`。
2.  在 **MainPage.xaml** 的根 **Page** 元素中，刪除 `Orientation="Portrait"`。
3.  在 **MainPage.xaml** 的根 **Page** 元素中，刪除 `shell:SystemTray.IsVisible="True"`。
4.  在 `BookTemplate` 資料範本中，刪除了對 `PhoneTextExtraLargeStyle` 和 `PhoneTextSubtleStyle` **TextBlock** 樣式的參考。
5.  在 `TitlePanel` **StackPanel** 中，刪除了對 `PhoneTextNormalStyle` 和 `PhoneTextTitle1Style` **TextBlock** 樣式的參考。

首先讓我們處理行動裝置系列的 UI，我們可以在之後考量其他尺寸。 您現在可以建置與執行 app。 以下是它在行動裝置模擬器上的外觀。

![行動裝置上包含初始原始程式碼變更的 UWP app](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

檢視與檢視模型一起正常運作，而 **ListBox** 也功能正常。 我們通常只需要修正樣式並讓影像顯示即可。

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>清償負債項目，以及一些初始樣式

預設支援所有方向。 Windows Phone Silverlight 應用程式明確限制本身以直向僅限，不過，因此債務項目\#1 及\#2 移至應用程式封裝資訊清單，在新的專案並檢查付費**直向**底下**支援的方向**。

此應用程式中，項目\#3 不是債務因為預設會顯示 [狀態] 列 （先前稱為系統匣）。 項目的\#4 並\#5，我們需要找出四個通用 Windows 平台 (UWP) **TextBlock**對應至我們使用的 Windows Phone Silverlight 樣式的樣式。 您可以在模擬器中執行 Windows Phone Silverlight 應用程式，並比較它與此圖中並存[文字](wpsl-to-uwp-porting-xaml-and-ui.md)一節。 從這樣做，以及查看 Windows Phone Silverlight 系統樣式的屬性，我們可以讓此資料表。

| Windows Phone Silverlight 樣式索引鍵 | UWP 樣式索引鍵          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |
 
若要設定這些樣式，您可以直接在標記編輯器中輸入它們，或是使用 Visual Studio XAML 工具並設定它們，而不需要輸入任何內容。 若要這樣做，您以滑鼠右鍵按一下**TextBlock**然後按一下**編輯樣式** &gt; **套用資源**。 若要這樣做，使用**TextBlock**中的項目範本，以滑鼠右鍵按一下**ListBox** ，按一下 **編輯其他樣板** &gt; **編輯產生的項目 (ItemTemplate)** 。

項目後方有 80% 不透明的白色背景，因為 **ListBox** 控制項的預設樣式將其背景設定為 `ListBoxBackgroundThemeBrush` 系統資源。 在 **ListBox** 上設定 `Background="Transparent"` 以清除該背景。 若要將項目範本中的 **TextBlock** 靠左對齊，請以上述方式再次編輯它，然後將兩個 **TextBlock** 上的 **Margin** 都設定為 `"9.6,0"`。

完成後，因為[變更與檢視像素相關](wpsl-to-uwp-porting-xaml-and-ui.md)，所以我們需要將我們尚未變更的所有固定大小維度 (邊界、寬度、高度等等) 逐一乘以 0.8 。 例如，影像應該從 70x70px 變更為 56x56px。

但是讓我們在顯示樣式結果之前，先取得這些影像。

## <a name="binding-an-image-to-a-view-model"></a>將影像繫結到檢視模型

在 Bookstore1WPSL8 中，我們是這麼做：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

在 Bookstore1Universal，我們將使用 ms-appx [URI 配置](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10))。 如此，我們便可以讓程式碼的其餘部分保持不變，我們可以使用不同的 **System.Uri** 建構函式多載將 ms-appx URI 配置放在基底 URI 中，然後將路徑的其餘部分附加至其上。 就像這樣：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>通用樣式

現在我們只需要進行一些最終樣式調整，並確認應用程式的外觀在桌面 (和其他) 尺寸和行動裝置尺寸上一樣適當。 步驟如下。 而且您可以使用此主題頂端的連結來下載專案，並查看這裡和案例研究結束時所有變更的結果。

-   若要盡可能縮短項目之間的間距，請找出 MainPage.xaml 中的 `BookTemplate` 資料範本，並將 `Margin` 屬性從根目錄 **Grid** 中刪除。
-   如果您想要讓頁面標題有更多的空間，您可以將頁面標題 **TextBlock** 上 `-5.6` 的下邊界重設為 `0`。
-   現在我們需要將 `LayoutRoot` 的背景設為正確的預設值，如此一來，無論使用哪一種佈景主題，app 在所有裝置上執行時都會具有適當的外觀。 請將它從 `"Transparent"` 變更為 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。

若是更複雜的 app，這會是我們使用[尺寸和使用者經驗的移植](wpsl-to-uwp-form-factors-and-ux.md)中指導方針的時機，且在可以執行 app 的許多裝置上，真正選擇性地使用尺寸。 但是針對這個簡單的應用程式，我們可以稍微在這裡停下，並查看應用程式在最後一個樣式設定操作順序後的外觀。 它實際在行動裝置與桌面裝置上看起來相同，雖然它並未妥善使用寬尺寸的空間 (但我們將在稍後的案例研究中深入討論)。

請參閱[佈景主題變更](wpsl-to-uwp-porting-xaml-and-ui.md)，了解如何控制您 app 的佈景主題。

![移植完成的 Windows 10 app](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

移轉的行動裝置上執行的 Windows 10 應用程式

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>選擇性調整行動裝置的清單方塊

當應用程式在行動裝置上執行時，兩個佈景主題中清單方塊的背景顏色都是預設為淺色。 這可能會是您喜歡的樣式，如果是這樣的話，就不需要再執行其他動作。 但是控制項的設計是可以讓您自訂它們的外觀，卻不會影響它們的行為。 因此，如果您希望清單方塊套用深色佈景主題 (原始 app 外觀)，請依照＜選擇性調整＞下方的[這些指示](w8x-to-uwp-case-study-bookstore1.md)執行。

## <a name="conclusion"></a>結論

這個案例研究示範移植一個非常簡單之 app 的程序—可以說是相當不實際的簡單 app。 例如，清單控制項可用來進行選取或用來建立要瀏覽的內容；app 會瀏覽到含有所點選之項目的更多相關詳細資料的頁面。 這個特定的應用程式既不處理使用者的選項，也沒有任何瀏覽。 即使如此，提供此案例研究是為了介紹移植程序，並示範您能用於實際 UWP app 的重要程式碼共用技術。

下一個案例研究是 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md)，我們將在其中探討如何存取和顯示分組資料。
