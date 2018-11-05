---
author: muhsinking
title: 建立調適型配置的教學課程
description: 此文章說明 XAML 調適型配置的基本知識
keywords: XAML, UWP, 開始使用
ms.author: mukin
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 000aa2d8f3684aa813b85076d9124a87a71b6a8c
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6024266"
---
# <a name="tutorial-create-adaptive-layouts"></a>教學課程：建立調適型配置

本教學課程涵蓋使用 XAML 量身打造的調適型配置功能基本知識，可讓您建立在任何裝置上看起來都很適宜的應用程式。 您將會了解如何使用 VisualStateManager 和 AdaptiveTrigger 元素，建立新的 DataTemplate、新增視窗貼齊點，以及量身打造應用程式的版面配置。 我們會使用這些工具，針對較小型裝置螢幕最佳化影像編輯程式。 

您將使用的影像編輯程式有兩個頁面/螢幕：

**main page** 顯示影像中心檢視，以及一些關於每個影像檔案的資訊。

![MainPage](../basics/images/xaml-basics/mainpage.png)

**details page** 在選取單一相片之後，顯示該相片。 飛出視窗編輯功能表可用來變更、重新命名和儲存相片。

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>必要條件

* Visual Studio 2017:[下載 Visual Studio 2017 社群 (免費) ](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10SDK (10.0.15063.468 或更新版本)：[下載最新的 Windows SDK (免費) ](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Windows 行動裝置模擬器：[下載 Windows 10 行動裝置模擬器 (免費) ](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：從 github 取得起始程式碼

針對此教學課程，您將從簡化版的 PhotoLab 範例開始著手。 

1. 移至[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。 這會將您帶到 GitHub 頁面以取得範例。 
2. 下一步，您將需要複製或下載範例。 按一下 **\[複製或下載\]** 按鈕。 子功能表會出現。
    <figure>
        <img src="../basics/images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Photolab 範例的 GitHub 頁面上的 <b>複製或下載</b>功能表。</figcaption>
    </figure>

    **如果您不熟悉 GitHub：**
    
    a. 按一下**下載 ZIP**，然後在本機儲存此檔案。 這個下載的 .zip 檔案，包含您所需要的所有專案檔案。
    b. 將檔案解壓縮。 使用 [檔案總管] 瀏覽至您下載的 .zip 檔案，滑鼠右鍵按一下它，然後選取**解壓縮全部...**。c. 瀏覽至您範例的本機複製，並前往 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout` 目錄。    

    **如果您熟悉 GitHub：**

    a. 在本機複製存放庫中的主要分支。
    b. 瀏覽至 `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout` 目錄。

3. 按一下 `Photolab.sln` 開啟專案。

## <a name="part-1-run-the-mobile-emulator"></a>第 1 部分：執行行動模擬器

在 Visual Studio 工具列中，確認 [方案平台] 已設定為 x86 或 x64 (而非 ARM)，然後將目標裝置從本機電腦變更為您已安裝的其中一個行動擬器 (例如 Mobile Emulator 10.0.15063 WVGA 5 吋 1GB)。 按下 **F5**，嘗試在您所選取的行動模擬器中執行 [影像中心] 應用程式。

應用程式啟動時，您可能會立即注意到，當應用程式運作時，顯示在這樣小的檢視區效果不佳。 可變式方格元素會嘗試減少顯示的欄數來配合有限的螢幕實際可用空間做調整，但得到的結果會是版面配置看起來平淡無奇，並且在如此小的檢視區中顯得格格不入。

![行動版面配置：之後](../basics/images/xaml-basics/adaptive-layout-mobile-before.png)

## <a name="part-2-build-a-tailored-mobile-layout"></a>第 2 部分：建置量身打造的行動版面配置
為了讓這個應用程式在較小型裝置上變得美觀，我們將會 XAML 頁面中建立不同的一組只會在偵測到行動裝置時使用的樣式。

### <a name="create-a-new-datatemplate"></a>建立新的 DataTemplate
我們將會為新的影像建立 DataTemplate，以量身打造應用程式的圖庫檢視。 從 [方案總管] 開啟 MainPage.xaml，然後在 **Page.Resources** 標籤之內新增下列程式碼。

```XAML
<DataTemplate x:Key="ImageGridView_MobileItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">
        
        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImagePreview}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

這個圖庫範本藉由消除影像周圍框線，以及移除每個縮圖下面的影像中繼資料 (檔案名稱、評分等等)，節省螢幕實際可用空間。 我們不這麼做，而是將每個縮圖顯示為簡單的正方塊。

### <a name="add-metadata-to-a-tooltip"></a>將中繼資料新增至工具提示
我們仍然希望使用者能夠存取每個影像的中繼資料，因此將工具提示新增至每個影像項目。 在您剛建立的 DataTemplate 的**Image** 標籤內新增下列程式碼。

```XAML
<Image ...>

    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
</Image>
```

這樣就會在您將滑鼠游標暫留在縮圖 (或按住觸控螢幕) 時，顯示影像的標題、檔案類型及尺寸。

### <a name="add-a-visualstatemanager-and-statetrigger"></a>新增 VisualStateManager 和 StateTrigger

現已為我們的資料建立新的版面配置，但應用程式目前並不知道何時要透過預設樣式使用這個版面配置。 為了修正這點，我們需要新增 **VisualStateManager**。 將下列程式碼新增至頁面的根元素 **RelativePanel**。

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <!-- Add a new VisualState for mobile devices -->
        <VisualState x:Key="Mobile">

            <!-- Trigger visualstate when a mobile device is detected -->
            <VisualState.StateTriggers>
                <local:MobileScreenTrigger InteractionMode="Touch" />
            </VisualState.StateTriggers>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

這樣將會加入新的 **VisualState** 和 **StateTrigger**，當應用程式偵測到本身是在行動裝置上執行時，將會觸發這兩個新物件 (偵測作業的邏輯可在為您提供的 PhotoLab 目錄內的 MobileScreenTrigger.cs 中找到)。 **StateTrigger** 啟動時，應用程式會使用任何指派給這個 **VisualState** 的版面配置屬性。

### <a name="add-visualstate-setters"></a>新增 VisualState setter
接下來，使用 **VisualState** setter 指示 **VisualStateManager** 要在狀態受觸發時套用哪些屬性。 每個 setter 都會以特定 XAML 元素的其中一個屬性為目標，並將該屬性設為指定值。 將這個程式碼新增至您剛建立的行動裝置版 **VisualState** 的 **VisualState.StateTriggers** 元素下方。 

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <VisualState x:Key="Mobile">
            ...

            <!-- Add setters for mobile visualstate -->
            <VisualState.Setters>

                <!-- Move GridView about the command bar -->
                <Setter Target="ImageGridView.(RelativePanel.Above)"
                        Value="MainCommandBar" />

                <!-- Switch to mobile layout -->
                <Setter Target="ImageGridView.ItemTemplate"
                        Value="{StaticResource ImageGridView_MobileItemTemplate}" />

                <!-- Switch to mobile container styles -->
                <Setter Target="ImageGridView.ItemContainerStyle"
                        Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

                <!-- Move command bar to bottom of the screen -->
                <Setter Target="MainCommandBar.(RelativePanel.AlignBottomWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignLeftWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignRightWithPanel)"
                        Value="True" />

                <!-- Adjust the zoom slider to fit mobile screens -->
                <Setter Target="ZoomSlider.Minimum"
                        Value="80" />
                <Setter Target="ZoomSlider.Maximum"
                        Value="180" />
                <Setter Target="ZoomSlider.TickFrequency"
                        Value="20" />
                <Setter Target="ZoomSlider.Value"
                        Value="100" />
            </VisualState.Setters>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

```

這些 setter 會將影像圖庫的 **ItemTemplate** 設定為我們最先建立的新的 **DataTemplate**，然後將命令列及縮放滑桿與螢幕底部對齊，讓您的拇指更容易在行動電話螢幕上觸及這些控制項。

### <a name="run-the-app"></a>執行應用程式
現在請嘗試使用行動模擬器執行應用程式。 新的版面配置是否成功顯示？ 您應該會看到小型縮圖方格，如下所示。 如果仍然看到舊的版面配置，您的 **VisualStateManager** 程式碼可能有錯字。

![行動版面配置：之後](../basics/images/xaml-basics/adaptive-layout-mobile-after.png)

## <a name="part-3-adapt-to-multiple-window-sizes-on-a-single-device"></a>第 3 部分：適應單一裝置上的多個視窗大小做調整。
建立新的量身打造的版面配置解決了行動裝置的回應式設計難題，但在桌上型電腦和平板電腦上又會如何？ 應用程式可能顯示在全螢幕上的效果還不錯，但要是使用者縮小視窗，得到的結果或許是很難看的介面。 我們可以使用 **VisualStateManager** 配合單一裝置上的多個視窗大小進行調整，確保始終都能在外觀和風格上提供良好的使用者體驗。

![小型視窗：之前](../basics/images/xaml-basics/adaptive-layout-small-before.png)

### <a name="add-window-snap-points"></a>新增視窗貼齊點
第一個步驟是定義將會用來觸發 **VisualStates** 的「貼齊點」。 從 [方案總管] 開啟 App.xaml，然後在 **Application** 標籤之間新增下列程式碼。

```XAML
<Application.Resources>
    <!--  window width adaptive snap points  -->
    <x:Double x:Key="MinWindowSnapPoint">0</x:Double>
    <x:Double x:Key="MediumWindowSnapPoint">641</x:Double>
    <x:Double x:Key="LargeWindowSnapPoint">1008</x:Double>
</Application.Resources>
```

這會提供三個貼齊點，可讓我們用來為三個範圍的視窗大小建立新的 **VisualStates**：
+ 小型 (0 - 640 像素寬度)
+ 中型 (641 - 1007 像素寬度)
+ 大型 (> 1007 像素寬度)

### <a name="create-new-visualstates-and-statetriggers"></a>建立新的 VisualStates 和 StateTriggers
接下來，建立對應至每個貼齊點的 **VisualStates** 及 **StateTriggers**。 在 MainPage.xaml 中，將下列程式碼新增至您於第 2 部分建立的 **VisualStateManager**。

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>
     
        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>
        
        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="add-setters"></a>新增 setter
最後，將這些 setter 新增至 **SmallWindow** 狀態。

```XAML

<VisualState x:Key="SmallWindow">
    ...

    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply mobile itemtemplate and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_MobileItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>

</VisualState>

```

只要檢視區寬度小於 641 像素，這些 setter 就會將行動裝置版 **DataTemplate** 及樣式套用至傳統型應用程式。 此外，也會調整縮放滑桿，使之更適合小型螢幕。

### <a name="run-the-app"></a>執行應用程式

在 Visual Studio 工具列中，將目標裝置設定為 **\[本機電腦\]** 並執行應用程式。 當應用程式載入時，嘗試變更視窗的大小。 將視窗縮減成小型大小時，您應該會看到應用程式切換至您在第 2 部分建立的行動版面配置。

![小型視窗：之後](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>更進一步

現在完成了這個實習課程，您已具備充足的調整型配置知識，可以進一步自行實驗。 嘗試將評分控制項新增至您稍早加入的僅限行動裝置版工具提示。 或者，嘗試更大的挑戰，將較大型螢幕大小 (考慮一下電視螢幕或 Surface Studio) 的版面配置最佳化

如果遇到困難，您可以在[使用 XAML 定義頁面版面配置](../layout/layouts-with-xaml.md)的下列各節中找到更多指引。

+ [視覺狀態與狀態觸發程序](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [量身訂做的版面配置](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#tailored-layouts)

或者，如果您想要深入了解最初的編輯相片應用程式是如何建置，請參閱 XAML [使用者介面](../basics/xaml-basics-ui.md)及[資料繫結](../../data-binding/xaml-basics-data-binding.md)的那些教學課程。

## <a name="get-the-final-version-of-the-photolab-sample"></a>取得最終版本 PhotoLab 範例

本教學課程不會建置到完整的編輯相片應用程式，因此請務必查看[最終版本](https://github.com/Microsoft/Windows-appsample-photo-lab)來了解各項功能，例如自訂動畫和電話支援。