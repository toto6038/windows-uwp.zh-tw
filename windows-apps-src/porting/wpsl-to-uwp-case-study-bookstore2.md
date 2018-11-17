---
author: stevewhims
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 這個案例研究，根據 Bookstore 中提供的資訊，顯示分組資料在 LongListSelector 中的 WindowsPhone Silverlight 應用程式的開頭。
title: WindowsPhone Silverlight 至 UWP 案例研究： Bookstore2
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8e518439ddd4e131c2d045f4467670b42a392fca
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "7146547"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore2"></a>WindowsPhone Silverlight 至 UWP 案例研究： Bookstore2


這個案例研究 — 根據[Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)中的資訊來建置 — 是從開始顯示分組資料在**LongListSelector**中的 WindowsPhone Silverlight 應用程式。 在檢視模型中，每個 **Author** 類別執行個體都代表該作者所著之書籍的群組，而在 **LongListSelector** 中，我們可以檢視依作者分組的書籍清單，或是縮小來查看作者的捷徑清單。 與捲動書籍清單相比，捷徑清單可提供更快速的瀏覽。 我們逐步解說將 app 移植到 Windows10Universal Windows 平台 (UWP) 應用程式的步驟。

**注意：** 時在 Visual Studio 中開啟 Bookstore2Universal\_10，如果您看見 「 需要 Visual Studio 更新 」，則請依照目標平台版本設定[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)中的步驟。

## <a name="downloads"></a>下載

[下載 Bookstore2WPSL8 WindowsPhone Silverlight 應用程式](http://go.microsoft.com/fwlink/p/?linkid=522601)。

[下載 Bookstore2Universal\_10 windows 10 應用程式](http://go.microsoft.com/fwlink/?linkid=532952)。

##  <a name="the-windowsphone-silverlight-app"></a>WindowsPhone Silverlight 應用程式

下圖示範 Bookstore2WPSL8 (我們即將移植的 app 看起來的樣子)。 它是依作者分組的書籍垂直捲動 **LongListSelector**。 您可以縮小至捷徑清單，然後從該處往回瀏覽至任何群組。 此應用程式有兩個主要部分：提供分組資料來源的檢視模型，以及繫結至該檢視模型的使用者介面。 如同我們將看到的這兩個部分都可輕鬆從 WindowsPhone Silverlight 技術的移植到通用 Windows 平台 (UWP)。

![bookstore2wpsl8 的外觀](images/wpsl-to-uwp-case-studies/c02-01-wpsl-how-the-app-looks.png)

##  <a name="porting-to-a-windows10-project"></a>移植到 windows 10 專案

這是一項快速的工作，可在 Visual Studio 中建立新專案、從 Bookstore2WPSL8 將檔案複製到其中，以及在新專案中包含複製的檔案。 一開始先建立新的空白應用程式 (Windows 通用) 專案。 將它命名為 Bookstore2Universal\_10。 這些是從 Bookstore2WPSL8 複製到 Bookstore2Universal\_10 的檔案。

-   複製包含書籍封面影像的 PNG 檔案 (此資料夾為 \\Assets\\CoverImages)。 在複製資料夾之後，請在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 在您複製的資料夾上按一下滑鼠右鍵，然後按一下 **\[加入至專案\]**。 該命令就是我們所謂的在專案中「包含」檔案或資料夾。 每次當您複製檔案或資料夾時，請按一下 **\[方案總管\]** 中的 **\[重新整理\]**，然後在專案中加入檔案或資料夾。 不需要對目的地中您正在取代的檔案執行此動作。
-   複製包含檢視模型來源檔案的資料夾 (此資料夾是 \\ViewModel)。
-   複製 MainPage.xaml 並取代目的地中的檔案。

我們可以保留 App.xaml，以及 Visual Studio 為我們產生的 windows 10 專案中的 App.xaml.cs。

編輯您剛才複製的原始程式碼與標記檔案，並將對 Bookstore2WPSL8 命名空間的任何參考變更為參考 Bookstore2Universal\_10。 執行此作業的快速方法是使用 **\[檔案中取代\]** 功能。 在檢視模型原始程式檔的命令式程式碼中，需要進行下列移植變更。

-   將 `System.ComponentModel.DesignerProperties` 變更為 `DesignMode`，然後對其使用 **\[解析\]** 命令。 刪除 `IsInDesignTool` 屬性，然後使用 IntelliSense 來新增正確的屬性名稱：`DesignModeEnabled`。
-   對 `ImageSource` 使用 **\[解析\]** 命令。
-   對 `BitmapImage` 使用 **\[解析\]** 命令。
-   刪除 `using System.Windows.Media;` 和 `using System.Windows.Media.Imaging;`。
-   將 **Bookstore2Universal\_10.BookstoreViewModel.AppName** 屬性傳回的值從 "BOOKSTORE2WPSL8" 變更為 "BOOKSTORE2UNIVERSAL"。
-   正如同我們對 [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) 的做法，更新 **BookSku.CoverImage** 屬性的實作 (請參閱[將映像繫結至檢視模型](wpsl-to-uwp-case-study-bookstore1.md))。

在 MainPage.xaml 中，需要進行下列初始移植變更：

-   將 `phone:PhoneApplicationPage` 變更為 `Page` (包括出現在屬性元素語法中的部分)。
-   刪除 `phone` 和 `shell` 命名空間前置字元宣告。
-   將其餘命名空間前置字元宣告中的 "clr-namespace" 變更為 "using"。
-   刪除 `SupportedOrientations="Portrait"` 和 `Orientation="Portrait"`，並在新專案之 app 套件資訊清單中設定 [**縱向**]。
-   刪除 `shell:SystemTray.IsVisible="True"`。
-   捷徑清單項目轉換器 (在標記中是以資源形式存在) 的類型已移至 [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) 命名空間中。 因此，請新增命名空間前置字元宣告 Windows\_UI\_Xaml\_Controls\_Primitives，並將其對應至 **Windows.UI.Xaml.Controls.Primitives**。 在捷徑清單項目轉換器資源上，將前置字元從 `phone:` 變更為 `Windows_UI_Xaml_Controls_Primitives:`。
-   正如同我們對 [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) 的做法，以 `SubtitleTextBlockStyle` 的參考取代 `PhoneTextExtraLargeStyle` **TextBlock** 樣式的所有參考、以 `SubtitleTextBlockStyle` 取代 `PhoneTextSubtleStyle`、以 `CaptionTextBlockStyle` 取代 `PhoneTextNormalStyle`，並以 `HeaderTextBlockStyle` 取代 `PhoneTextTitle1Style`。
-   `BookTemplate` 中的一個例外狀況。 第二個 **TextBlock** 的樣式應該參考 `CaptionTextBlockStyle`。
-   從 `AuthorGroupHeaderTemplate` 內的 **TextBlock** 移除 FontFamily 屬性，並將 **Border** 的背景設定為參考 `SystemControlBackgroundAccentBrush` 而非 `PhoneAccentBrush`。
-   由於[變更與檢視像素相關](wpsl-to-uwp-porting-xaml-and-ui.md)，因此請細查標記，並將所有固定大小維度 (邊界、寬度、高度等等) 乘以 0.8。

## <a name="replacing-the-longlistselector"></a>取代 LongListSelector


以 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 控制項取代 **LongListSelector** 將需要數個步驟，因此讓我們開始進行這項操作。 **LongListSelector** 會直接繫結至分組資料來源，但是 **SemanticZoom** 包含 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 或 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 控制項，這些控制項會透過 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833) 配接器間接繫結至資料。 **CollectionViewSource** 必須在標記中做為資源，因此讓我們從將它新增至 MainPage.xaml 之 `<Page.Resources>` 內的標記中開始著手。

```xml
    <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{Binding Authors}"
        IsSourceGrouped="true"/>
```

請注意，**LongListSelector.ItemsSource** 上的繫結會變成 **CollectionViewSource.Source** 的值，而 **LongListSelector.IsGroupingEnabled** 會變成 **CollectionViewSource.IsSourceGrouped**。 **CollectionViewSource** 有名稱 (注意：不是如您可能預期的索引鍵)，因此我們可以與它繫結。

接著，以此標記取代 `phone:LongListSelector`，這會提供一個初步的 **SemanticZoom** 供我們使用：

```xml
    <SemanticZoom>
        <SemanticZoom.ZoomedInView>
            <ListView
                ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource BookTemplate}">
                <ListView.GroupStyle>
                    <GroupStyle
                        HeaderTemplate="{StaticResource AuthorGroupHeaderTemplate}"
                        HidesIfEmpty="True"/>
                </ListView.GroupStyle>
            </ListView>
        </SemanticZoom.ZoomedInView>
        <SemanticZoom.ZoomedOutView>
            <ListView
                ItemsSource="{Binding CollectionGroups, Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource ZoomedOutAuthorTemplate}"/>
        </SemanticZoom.ZoomedOutView>
    </SemanticZoom>
```

一般清單和捷徑清單模式的 **LongListSelector** 概念分別在放大和縮小檢視的 **SemanticZoom** 概念中做了答覆。 放大檢視是一項屬性，且您會將該屬性設定到 **ListView** 的執行個體。 在此案例中，縮小檢視也會設定到 **ListView**，且兩個 **ListView** 控制項都會繫結到我們的 **CollectionViewSource**。 放大檢視使用的項目範本、群組標頭範本及 **HideEmptyGroups** 設定 (現在稱為 **HidesIfEmpty**) 與 **LongListSelector** 的一般清單相同。 而縮小檢視使用的項目範本則與 **LongListSelector** 之捷徑清單樣式 (`AuthorNameJumpListStyle`) 內的非常類似。 另外，也請注意，縮小檢視會繫結至 **CollectionViewSource** 的一個特殊屬性 (名為 **CollectionGroups**)，這是一個包含群組 (而不是項目) 的集合。

我們不再需要 `AuthorNameJumpListStyle`，至少不再需要它的全部。 在縮小檢視中，我們只需要群組 (在此應用程式中是作者) 的資料範本。 因此我們刪除 `AuthorNameJumpListStyle` 樣式，並以下列資料範本取代它：

```xml
   <DataTemplate x:Key="ZoomedOutAuthorTemplate">
        <Border Margin="9.6,0.8" Background="{Binding Converter={StaticResource JumpListItemBackgroundConverter}}">
            <TextBlock Margin="9.6,0,9.6,4.8" Text="{Binding Group.Name}" Style="{StaticResource SubtitleTextBlockStyle}"
            Foreground="{Binding Converter={StaticResource JumpListItemForegroundConverter}}" VerticalAlignment="Bottom"/>
        </Border>
    </DataTemplate>
```

請注意，因為此資料範本的資料內容是群組而不是項目，所以我們會繫結至名為 **Group** 的特殊屬性。

您現在可以建置與執行 app。 以下是它在行動裝置模擬器上的外觀。

![行動裝置上包含初始原始程式碼變更的 UWP app](images/wpsl-to-uwp-case-studies/c02-02-mob10-initial-source-code-changes.png)

雖然檢視模型和放大和縮小檢視正常運作，我們還需要處理一些樣式與範本方面的工作。 例如，正確的樣式和筆刷尚未用不，因此看不見您可以按一下來放大的群組標頭上的文字。如果您在桌面裝置上執行應用程式，您會看到第二個問題，也就是應用程式還不會調整它的使用者介面以提供最佳體驗和較大的裝置上的空間視窗可能會比行動裝置的螢幕還要大的使用。 因此在接下來的幾個小節 ([初始設定樣式和範本](#initial-styling-and-templating)、[彈性 UI](#adaptive-ui) 與[最終的樣式](#final-styling))，我們將會解決這些問題。

## <a name="initial-styling-and-templating"></a>初始設定樣式和範本

若要恰當地保留群組標頭的空間，請在 **Border** 上編輯 `AuthorGroupHeaderTemplate` 並將 **Margin** 設定為 `"0,0,0,9.6"`。

若要恰當地保留書籍項目的空間，請同時在兩個 **TextBlock** 上編輯 `BookTemplate` 並將 **Margin** 設定為 `"9.6,0"`。

若要將 app 名稱與頁面標題配置的更好，請透過將值設定為 `"7.2,0,0,0"`，在 `TitlePanel` 內移除第二個 **TextBlock** 上頂端的 **Margin**。 並且在 `TitlePanel` 本身上，將邊界設定為 `0` (或者您看起來滿意的任何值)

將 `LayoutRoot` 的背景變更為 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。

## <a name="adaptive-ui"></a>彈性 UI

因為我們是以手機應用程式開始，在程序的此階段中，我們所移植之應用程式的 UI 配置只適用於小型裝置與縮小視窗，也就不讓人驚訝。 但是我們真的希望當應用程式執行於較寬的視窗中 (這只有在具備大螢幕的裝置上才有可能)，以及只能在應用程式縮小視窗使用我們目前所擁有的 UI (這會發生在小型裝置上，也有可能發生在大型裝置上) 時，UI 配置能夠自行調整並充分運用空間。

我們可以使用彈性的 Visual State Manager 功能來達到這個目的。 我們將在視覺元素上設定屬性，讓 UI 預設為使用我們目前所使用的範本，以縮小狀態進行配置。 接著，將偵測 app 的視窗寬於或等於特定大小的時機 (以[有效像素](wpsl-to-uwp-porting-xaml-and-ui.md)單位來測量)，並變更視覺元素的屬性，來取得更大且更寬的配置做為回應。 我們會將這些屬性變更放置於某個視覺狀態中，並使用彈性觸發程序持續監視，且根據視窗寬度 (以有效像素為單位) 決定是否要套用該視覺狀態。 在此案例中我們觸發了視窗寬度，但是也可能觸發視窗高度。

視窗的最小寬度為 548 epx，這是最適合這個使用案例的大小，因為這是我們想要顯示寬型配置的最小型裝置的大小。 手機通常會低於 548 epx，因此在這類小型裝置上，我們會保留預設的窄型配置。 在電腦上，視窗預設會以足夠寬度啟動以觸發寬度狀態的切換，這會顯示 250x250 大小的項目。 您將能從該處拖曳視窗窄邊，使其足以顯示至少兩個包含大小為 250x250 項目的欄。 比這個還窄一點，且觸發程序將停用、將移除寬型視覺狀態，而預設的窄型配置將會生效。

在解決彈性 Visual State Manager 片段之前，首先我們必須設計寬度狀態，這代表將新的視覺元素和範本新增到我們的標記中。 這些步驟說明如何執行這個動作。 透過視覺元素和範本的命名慣例，我們會將文字「寬度」包含在任何用於寬度狀態的元素與範本名稱中。 如果元素或範本不包含「寬度」這個字，則您可以假設元素或範本適用於縮小狀態，這會是預設狀態且其屬性值會設定為頁面中視覺元素的本機值。 只有寬度狀態的屬性值是透過標記中實際的「視覺狀態」進行設定。

-   製作標記中 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 控制項的複本，並在複本上設定 `x:Name="narrowSeZo"`。 在原始檔案上設定 `x:Name="wideSeZo"` 與 `Visibility="Collapsed"`，如此預設才不會顯示包含文字「寬度」的元素或範本。
-   在 `wideSeZo` 中，同時在放大檢視和縮小檢視中將 **ListView** 變更為 **GridView**。
-   製作 `AuthorGroupHeaderTemplate`、`ZoomedOutAuthorTemplate` 和 `BookTemplate` 這三個資源的複本，並將文字 `Wide` 附加到這些複本的索引鍵。 也將 `wideSeZo` 更新為參考這些新資源的索引鍵。
-   然後將 `AuthorGroupHeaderTemplateWide` 的內容取代成 `<TextBlock Style="{StaticResource SubheaderTextBlockStyle}" Text="{Binding Name}"/>`。
-   然後將 `ZoomedOutAuthorTemplateWide` 的內容取代成：

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250" >
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
          <TextBlock Foreground="{StaticResource ListViewItemOverlayForegroundThemeBrush}"
              Style="{StaticResource SubtitleTextBlockStyle}"
            Height="80" Margin="15,0" Text="{Binding Group.Name}"/>
        </StackPanel>
    </Grid>
```

-   然後將 `BookTemplateWide` 的內容取代成：

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250">
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <Image Source="{Binding CoverImage}" Stretch="UniformToFill"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}"
                TextWrapping="NoWrap" TextTrimming="CharacterEllipsis"
                Margin="12,0,24,0" Text="{Binding Title}"/>
            <TextBlock Style="{StaticResource CaptionTextBlockStyle}" Text="{Binding Author.Name}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}" TextWrapping="NoWrap"
                TextTrimming="CharacterEllipsis" Margin="12,0,12,12"/>
        </StackPanel>
    </Grid>
```

-   針對寬度狀態，放大檢視中的群組周圍將需要更多垂直空間。 建立和參考一些項目面板範本將可讓我們獲得想要的結果。 以下是標記看起來的樣子。

```xml
   <ItemsPanelTemplate x:Key="ZoomedInItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" GroupPadding="0,0,0,20"/>
    </ItemsPanelTemplate>
    ...

    <SemanticZoom x:Name="wideSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <GridView
            ...
            ItemsPanel="{StaticResource ZoomedInItemsPanelTemplate}">
            ...
```

-   最後，將適當的 Visual State Manager 標記新增為 `LayoutRoot` 的第一個子項。

```xml
    <Grid x:Name="LayoutRoot" ... >
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
```

## <a name="final-styling"></a>最終的樣式

接下來就是一些要調整的最終的樣式

-   在 `AuthorGroupHeaderTemplate` 中，於 **TextBlock** 上設定 `Foreground="White"`，讓它在行動裝置系列上執行時的外觀正確。
-   將 `FontWeight="SemiBold"` 同時新增到 `AuthorGroupHeaderTemplate` 與 `ZoomedOutAuthorTemplate` 中的 **TextBlock**。
-   在 `narrowSeZo` 中，群組標頭和縮小檢視中的作者是靠左對齊，而不是自動縮放，讓我們來處理這個問題。 為放大檢視建立 [**HeaderContainerStyle**](https://msdn.microsoft.com/library/windows/apps/dn251841)，其中將 [**HorizontalContentAlignment**](https://msdn.microsoft.com/library/windows/apps/br209417) 設定為 `Stretch`。 然後為縮小檢視建立包含該相同 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 的 [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/br242817)。 以下是看起來的樣子。

```xml
   <Style x:Key="AuthorGroupHeaderContainerStyle" TargetType="ListViewHeaderItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    <Style x:Key="ZoomedOutAuthorItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    ...

    <SemanticZoom x:Name="narrowSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <ListView
            ...
                <ListView.GroupStyle>
                    <GroupStyle
                    ...
                    HeaderContainerStyle="{StaticResource AuthorGroupHeaderContainerStyle}"
                    ...
        <SemanticZoom.ZoomedOutView>
            <ListView
                ...
                ItemContainerStyle="{StaticResource ZoomedOutAuthorItemContainerStyle}"
                ...
```

樣式作業的最後一個步驟會讓 app 看起來像這樣。

![放大檢視已移植完成且正在傳統型裝置上執行的 Windows 10 app，具備兩種視窗大小](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

在傳統型裝置、 放大檢視、 兩種視窗大小上執行的已移植 windows 10 app 
![在傳統型裝置、 縮小檢視、 兩種視窗大小上執行的已移植的 windows 10 app](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

在傳統型裝置、 縮小檢視、 兩種視窗大小上執行的已移植 windows 10 app

![放大檢視已移植完成且正在行動裝置上執行的 Windows 10 app](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

執行行動裝置上，放大檢視已移植 windows 10 應用程式

![縮小檢視已移植完成且正在行動裝置上執行的 Windows 10 app](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

執行行動裝置上，縮小檢視已移植 windows 10 應用程式

## <a name="making-the-view-model-more-flexible"></a>讓檢視模型更具彈性

本節包含因將 app 移轉成使用 UWP 而得以利用之功能的範例。 在這裡，我們將說明在透過 **CollectionViewSource** 存取檢視模型的情況下，您可依循以使檢視模型更具彈性的選擇性步驟。 檢視模型 （原始程式檔位於 ViewModel\\BookstoreViewModel.cs） 我們從 WindowsPhone Silverlight app Bookstore2WPSL8 移植包含名為 Author 衍生自類別**清單&lt;T&gt;**，其中**T**為 BookSku。 這表示 Author 類別*是一個* BookSku 群組。

當我們將 **CollectionViewSource.Source** 繫結至 Authors 時，我們唯一要傳達的就是 Authors 中的每個 Author 都是一個 *「某種東西」* 的群組。 我們將它留給 **CollectionViewSource** 去判斷，而在此案例中，Author 是一個 BookSku 群組。 這樣行得通：但是不具彈性。 如果我們希望 Author 能夠 *「既是」* 一個 BookSku 群組 *「也是」* 該作者居住過之地址的群組，該怎麼辦？ Author 無法同時 *「是」* 這兩個群組。 但是 Author 可以 *「有」* 任何數目的群組。 而這就是方案：使用 *「有一個群組」* 模式來取代或補充我們目前使用的 *「是一個群組」* 模式。 方法如下：

-   變更 Author，讓它不再衍生自 **List&lt;T&gt;**。
-   將下列欄位新增至 Author：`private ObservableCollection<BookSku> bookSkus = new ObservableCollection<BookSku>();`。
-   將下列屬性新增至 Author：`public ObservableCollection<BookSku> BookSkus { get { return this.bookSkus; } }`。
-   當然，我們可以重複上述兩個步驟，依所需的數目將多個群組新增至 Author。
-   將 AddBookSku 方法的實作變更為 `this.BookSkus.Add(bookSku);`。
-   既然 Author 已至少 *「有」* 一個群組，我們需要向 **CollectionViewSource** 傳達它應該使用這些群組當中的哪一個群組。 若要這樣做，請將這個屬性新增至 **CollectionViewSource**： `ItemsPath="BookSkus"`

這些變更會讓這個 app 在功能上保持不變，但您現在已了解可如何延伸 Author 以及 **CollectionViewSource** (如果需要的話)。 讓我們對 Author 進行最後一個變更，以便在使用它但 *「不」* 指定 **CollectionViewSource.ItemsPath** 的情況下，會使用我們所選擇的預設群組：

```csharp
    public class Author : IEnumerable<BookSku>
    {
        ...

        public IEnumerator<BookSku> GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
    }
```

現在，如果我們想要，我們可以選擇移除 `ItemsPath="BookSkus"`，而 app 將仍以相同的方式運作。

## <a name="conclusion"></a>總結

與前一個案例研究相比，這個案例研究涉及更酷炫的使用者介面。 所有的設備和 WindowsPhone Silverlight **LongListSelector**概念 — 及其他 — 找不到可供 UWP app 中的**SemanticZoom**、 **ListView**、 **GridView**及**CollectionViewSource**形式。 我們示範了如何同時重複使用 (或複製和編輯) UWP app 中的命令式程式碼和標記，以完成自訂符合最窄到最寬的 Windows 裝置尺寸和之間所有大小的功能、UI 和互動。
