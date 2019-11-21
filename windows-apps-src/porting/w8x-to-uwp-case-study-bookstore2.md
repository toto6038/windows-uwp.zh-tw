---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 這個案例研究根據 Bookstore1 中所提供的資訊來建置，是從在 SemanticZoom 控制項中顯示分組資料的通用 8.1 app 開始著手。
title: Windows Runtime 8.x 至 UWP 的案例研究：Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fb3fdd324caa54c6965ae63485b5b4f264980d7e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259123"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows Runtime 8.x 至 UWP 的案例研究：Bookstore2


這個案例研究—根據 [Bookstore1](w8x-to-uwp-case-study-bookstore1.md) 中所提供的資訊來建置—是從在 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控制項中顯示分組資料的通用 8.1 app 開始著手。 在檢視模型中，每個 **Author** 類別執行個體都代表該作者所著之書籍的群組，而在 **SemanticZoom** 中，我們可以檢視依作者分組的書籍清單，或是縮小來查看作者的捷徑清單。 與捲動書籍清單相比，捷徑清單可提供更快速的瀏覽。 我們會逐步解說將應用程式移植到 Windows 10 通用 Windows 平臺（UWP）應用程式的步驟。

**注意**   在 Visual Studio 中開啟 Bookstore2Universal\_10 時，如果您看到「需要更新 Visual Studio」訊息，請遵循[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)中的步驟。

## <a name="downloads"></a>下載

[下載 Bookstore2\_81 通用8.1 應用程式](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81)。

[下載 Bookstore2Universal\_10 Windows 10 應用程式](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10)。

## <a name="the-universal-81-app"></a>通用 8.1 應用程式

以下是 Bookstore2\_81 （我們要移植的應用程式）的樣子。 這會以水平捲動方式 (在 Windows Phone 上為垂直捲動) [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 來顯示依作者分組的書籍。 您可以縮小範圍至捷徑清單，然後從該處往回瀏覽至任何群組。 此應用程式有兩個主要部分：提供分組資料來源的檢視模型，以及繫結至該檢視模型的使用者介面。 如我們所見，這兩個部分都是從 WinRT 8.1 技術輕鬆地移植到 Windows 10。

![windows 上的 bookstore2\-81，放大的視圖](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Windows 上的 Bookstore2\_81，放大的視圖
 

![windows 上的 bookstore2\-81，放大視圖](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Windows 上的 Bookstore2\_81，放大視圖

![windows phone 上的 bookstore2\-81，放大的視圖](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Windows Phone 上的 Bookstore2\_81，放大的視圖

![windows phone 上的 bookstore2\-81，放大視圖](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Windows Phone 上的 Bookstore2\_81，放大視圖

##  <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 專案

Bookstore2\_81 解決方案是8.1 通用應用程式專案。 Bookstore2\_81. Windows 專案會建立 Windows 8.1 的應用程式套件，而 Bookstore2\_81. .Windows 及 .windowsphone 專案會建立 Windows Phone 8.1 的應用程式套件。 Bookstore2\_81. Shared 是包含原始程式碼、標記檔案以及其他兩個專案所使用之其他資產和資源的專案。

就像上一個案例研究一樣，我們將採用的選項（[如有通用8.1 應用程式](w8x-to-uwp-root.md)中所述），是將共用專案的內容移植至以通用裝置系列為目標的 Windows 10。

一開始先建立新的空白應用程式 (Windows 通用) 專案。 將它命名為 Bookstore2Universal\_10。 這些是要從 Bookstore2\_81 複製到 Bookstore2Universal\_10 的檔案。

**從共用的專案**

-   複製包含書籍封面影像 PNG 檔案的資料夾（此資料夾是 \\資產\\CoverImages）。 在複製資料夾之後，請在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 在您複製的資料夾上按一下滑鼠右鍵，然後按一下 **\[加入至專案\]** 。 該命令就是我們所謂的在專案中「包含」檔案或資料夾。 每次您複製檔案或資料夾時，請在每次複製時，按一下 **\[方案總管\]** 中的 **\[重新整理\]** ，然後在專案中加入檔案或資料夾。 不需要對目的地中您正在取代的檔案執行此動作。
-   複製包含視圖模型來源檔案的資料夾（資料夾是 \\ViewModel）。
-   複製 MainPage.xaml 並取代目的地中的檔案。

**從 Windows 專案**

-   複製 BookstoreStyles.xaml。 我們會使用此檔案作為良好的起點，因為此檔案中的所有資源金鑰都會在 Windows 10 應用程式中解析;對等 .Windows 及 .windowsphone 檔案中的某些部分將不會。
-   複製 SeZoUC.xaml 和 SeZoUC.xaml.cs。 我們將從這個檢視的 Windows 版本開始 (其適用於寬型視窗)，然後將它調整為適用於較小型視窗，接下來則是更小型裝置。

編輯您剛複製的原始程式碼和標記檔案，然後將 Bookstore2\_81 命名空間的任何參考變更為 Bookstore2Universal\_10。 執行此作業的快速方法是使用 [**檔案中取代**] 功能。 在檢視模型中，或在任何其他非常重要的程式碼中，都不需要變更任何程式碼。 但是，為了讓您更輕鬆地查看應用程式的哪個版本正在執行，請將**Bookstore2Universal\_BookstoreViewModel**的值從 "Bookstore2\_81" 變更為 "Bookstore2Universal\_10"。

您可以立即建置並執行。 以下是新的 UWP 應用程式在尚未完成任何工作並將其移植到 Windows 10 之後的外觀。

![放大檢視已變更初始原始程式碼且正在傳統型裝置上執行的 Windows 10 應用程式](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

Windows 10 應用程式的初始原始程式碼變更在桌面裝置上執行，放大的視圖

![縮小檢視已變更初始原始程式碼且正在傳統型裝置上執行的 Windows 10 應用程式](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

Windows 10 應用程式，並在桌面裝置上執行初始原始程式碼變更，相應放大視圖

儘管會產生難以察覺的問題，但是檢視模型與放大和縮小檢視還是可以正確地一同運作。 有一個問題是 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 不會捲動。 這是因為在 Windows 10 中， [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)的預設樣式會使其以垂直方式配置（而且 Windows 10 設計指導方針建議在新的和已移植的應用程式中使用它）。 但是，我們從 Bookstore2\_81 專案（專為8.1 應用程式所設計）複製的 [自訂專案面板] 範本中，水準滾動設定與 Windows 10 預設樣式中的垂直捲動設定相衝突，這是因為我們已移植到 Windows 10 應用程式的結果。 第二點是 app 還不會調整它的使用者介面，以便在不同大小的視窗中及小型裝置上提供最佳體驗。 而第三點是還無法使用正確的樣式和筆刷，因而導致許多文字無法顯示 (包括您可以按一下來放大的群組標題)。 因此，在接下來的三個小節 ([SemanticZoom 和 GridView 設計變更](#semanticzoom-and-gridview-design-changes)、[彈性 UI](#adaptive-ui) 及[通用樣式](#universal-styling)) 中，我們將會解決這三個問題。

## <a name="semanticzoom-and-gridview-design-changes"></a>SemanticZoom 和 GridView 設計變更

[SemanticZoom 變更](w8x-to-uwp-porting-xaml-and-ui.md)一節會說明 Windows 10 中的設計變更與[**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)控制項。 我們不需在本節中執行任何動作來回應這些變更。

對於 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 所做的變更，請參閱 [GridView/ListView 變更](w8x-to-uwp-porting-xaml-and-ui.md)中的說明。 我們必須進行一些非常細微的調整以適應這些變更，如下所述。

-   在 SeZoUC.xaml 的 `ZoomedInItemsPanelTemplate` 中，設定 `Orientation="Horizontal"` 和 `GroupPadding="0,0,0,20"`。
-   在 SeZoUC.xaml 中，刪除 `ZoomedOutItemsPanelTemplate`，並從縮小檢視中移除 `ItemsPanel` 屬性。

這樣就完成了！

## <a name="adaptive-ui"></a>彈性 UI

進行該變更之後，SeZoUC.xaml 提供的 UI 配置會優於應用程式在寬型視窗中執行的樣子 (這只適用於配有大型螢幕的裝置)。 儘管應用程式的視窗較窄 (這會發生於小型裝置上，也會發生於大型裝置上)，我們在 Windows Phone 市集應用程式中所擁有的 UI 可說是最適用的。

我們可以使用彈性的 Visual State Manager 功能來達到這個目的。 我們將設定視覺元素上的屬性，因此，預設會使用我們在 Windows Phone 市集應用程式中使用的較小型範本，以窄型狀態來配置 UI。 接著，將偵測應用程式的視窗寬於或等於特定大小的時機 (以[有效像素](w8x-to-uwp-porting-xaml-and-ui.md)單位來測量)，並變更視覺元素的屬性，來取得更大且更寬的配置做為回應。 我們會將這些屬性變更放置於某個視覺狀態中，並使用彈性觸發程序持續監視，且根據視窗寬度 (以有效像素為單位) 決定是否要套用該視覺狀態。 在此案例中我們觸發了視窗寬度，但是也可能觸發視窗高度。

視窗的最小寬度為 548 epx，這是最適合這個使用案例的大小，因為這是我們想要顯示寬型配置的最小型裝置的大小。 手機通常會低於 548 epx，因此在這類小型裝置上，我們會保留預設的窄型配置。 在電腦上，視窗將使用足以觸發切換為寬型狀態的預設寬度來啟動。 您將能從該處拖曳視窗窄邊，使其足以顯示兩個包含大小為 250x250 項目的欄。 比這個還窄一點，且觸發程序將停用、將移除寬型視覺狀態，而預設的窄型配置將會生效。

因此，我們需要使用哪些屬性來設定—及變更—以實現這兩個不同的配置？ 有兩個替代方案，每個都需要不同的方法。

1.  我們會在標記中放置兩個 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控制項。 其中一個是我們在 Windows 執行階段8.x 應用程式中使用的標記複本（在其中使用[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)控制項），預設會折迭。 另一個則是在 Windows Phone 市集應用程式中使用的標記複本 (在其內部使用 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 控制項)，且預設會顯示。 視覺狀態會切換兩個 **SemanticZoom** 控制項的可見度屬性。 這不需要花費太多心力就能達成，但一般而言，這並非高效能技術。 因此，如果您使用它，就應該分析您的應用程式，並確認它仍然符合您的效能目標。
2.  我們可以使用包含 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控制項的單一 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)。 為了實現這兩個配置，我們會在寬型視覺狀態中，變更 **ListView** 控制項的屬性 (包括套用到它們的範本)，讓它們能夠以 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 所做的相同方式進行配置。 這可能會執行地更好，但是在 **GridView** 和 **ListView** 的各種樣式和範本間，以及在其各種項目類型 (這是更難達成的方案) 之間，有很多細微差異。 這個方案也會與目前設計預設樣式和範本的方式緊密結合，提供精細且敏感的解決方案，以便未來對預設值進行任何變更。

在這個案例研究中，我們即將使用第一個替代方案。 但是，您可以視需要嘗試第二個替代方案，並查看它的運作方式是否更適合您。 以下是要實作的第一個替代方案所採取的步驟。

-   在新專案中，於標記中的 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 上設定 `x:Name="wideSeZo"` 和 `Visibility="Collapsed"`。
-   回到 Bookstore2\_81. .Windows 及 .windowsphone 專案，然後開啟 SeZoUC。 複製該檔案的 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 元素標記，並將它貼到新專案中緊接在 `wideSeZo` 之後。 在您剛貼上的元素上設定 `x:Name="narrowSeZo"`。
-   但是 `narrowSeZo` 需要幾個我們尚未複製的樣式。 同樣地，在 Bookstore2\_81. .Windows 及 .windowsphone 中，將兩個樣式（`AuthorGroupHeaderContainerStyle` 和 `ZoomedOutAuthorItemContainerStyle`）複製到 SeZoUC，並將它們貼入新專案中的 BookstoreStyles。
-   您目前在新的 SeZoUC.xaml 中有兩個 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 元素。 在 **Grid** 中將這兩個元素自動換行。
-   在新專案的 BookstoreStyles.xaml 中，將文字 `Wide` 附加到下列三個資源索引鍵 (以及它們在 SeZoUC.xaml 中的參考，但僅限 `wideSeZo` 內部的參考)： `AuthorGroupHeaderTemplate`、`ZoomedOutAuthorTemplate` 及 `BookTemplate`。
-   在 Bookstore2\_81. .Windows 及 .windowsphone 專案中，開啟 BookstoreStyles。 在這個檔案中，複製這三個資源（如上所述）和兩個跳躍清單專案轉換器，而命名空間前置詞宣告 Windows\_UI\_Xaml\_控制\_的基本專案，並將它們全部貼入 BookstoreStyles 中的新專案。
-   最後，在新專案的 SeZoUC.xaml 中，將適當的 Visual State Manager 標記新增到您在上方新增的 **Grid**。

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>通用樣式

現在，讓我們來修正某些樣式問題，包括上方所述之從舊專案複製時所發生的問題。

-   在 MainPage.xaml 中，將 `LayoutRoot` 的背景變更為 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。
-   在 BookstoreStyles.xaml 中，將資源 `TitlePanelMargin` 的值設為 `0` (或任何您喜歡的值)。
-   在 SeZoUC.xaml 中，將 `wideSeZo` 的邊界設為 `0` (或任何您喜歡的值)。
-   在 BookstoreStyles.xaml 中，從 `AuthorGroupHeaderTemplateWide` 移除邊界屬性。
-   從 `AuthorGroupHeaderTemplate` 和 `ZoomedOutAuthorTemplate` 移除 FontFamily 屬性。
-   Bookstore2\_81 使用 `BookTemplateTitleTextBlockStyle`、`BookTemplateAuthorTextBlockStyle`和 `PageTitleTextBlockStyle` 資源金鑰做為間接取值，讓單一索引鍵在兩個應用程式中具有不同的執行方式。 我們已經不再需要該間接取值，我們可以直接參照系統樣式。 因此，請個別使用 `TitleTextBlockStyle`、`CaptionTextBlockStyle` 及 `HeaderTextBlockStyle` 來取代那些參考。 您可以使用 Visual Studio 的 [**檔案中取代**] 功能，快速且準確地執行此動作。 然後，就可以刪除這三個未使用的資源。
-   在 `AuthorGroupHeaderTemplate` 中，使用 `PhoneAccentBrush` 來取代 `SystemControlBackgroundAccentBrush`，並在 `Foreground="White"`TextBlock**上設定**，讓它在行動裝置系列上執行時看起來是正確的。
-   在 `BookTemplateWide` 中，將前景屬性從第二個 **TextBlock** 複製到第一個。
-   在 `ZoomedOutAuthorTemplateWide` 中，將對 `SubheaderTextBlockStyle` (現在已經有一點太大) 的參照變更成參照 `SubtitleTextBlockStyle`。
-   新平台中的縮小檢視 (捷徑清單) 不再與放大檢視重疊，如此一來，就可以從 `Background` 的縮小檢視移除 `narrowSeZo` 屬性 。
-   因此，所有的樣式與範本都會在同一個檔案中，請將 `ZoomedInItemsPanelTemplate` 移出 SeZoUC.xaml 並移入 BookstoreStyles.xaml。

樣式作業的最後一個步驟會讓 app 看起來像這樣。

![放大檢視已移植完成且正在傳統型裝置上執行的 Windows 10 app，具備兩種視窗大小](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

在桌面裝置上執行的已移植 Windows 10 應用程式，放大模式，兩個視窗大小

![縮小檢視已移植完成且正在傳統型裝置上執行的 Windows 10 app，具備兩種視窗大小](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

在桌面裝置上執行的已移植 Windows 10 應用程式，相應放大的視圖，兩種大小的視窗

![放大檢視已移植完成且正在行動裝置上執行的 Windows 10 app](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

在行動裝置上執行的已移植 Windows 10 應用程式，放大的視圖

![縮小檢視已移植完成且正在行動裝置上執行的 Windows 10 app](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

在行動裝置上執行的已移植 Windows 10 應用程式，放大的視圖

## <a name="conclusion"></a>結論

與前一個案例研究相比，這個案例研究涉及更酷炫的使用者介面。 如同前一個案例研究，這個特定的檢視模型完全不需進行任何工作，而且我們的努力重點主要是重構使用者介面。 某些變更是將兩個專案結合成一個，同時仍支援許多不同尺寸規格所產生的必要結果 (事實上，比之前多很多)。 有一些變更是利用已對平台所做的變更來進行。

下一個案例研究是 [QuizGame](w8x-to-uwp-case-study-quizgame.md)，我們將在其中探討如何存取和顯示分組資料。
