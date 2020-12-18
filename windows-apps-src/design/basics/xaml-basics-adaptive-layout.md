---
title: 建立調適型配置的教學課程
description: 了解如何在 XAML 中使用調適型配置功能，以建立外觀適合任何視窗大小的應用程式。
keywords: XAML, UWP, 開始使用
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6919a2dc15aac7c5e589f2c0355f9c1cc13bfc7a
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598809"
---
# <a name="tutorial-create-adaptive-layouts"></a>教學課程：建立調適型配置

本教學課程涵蓋使用 XAML 調適型配置功能的基本知識，可讓您建立在任何規模上看起來都很適宜的應用程式。 您將了解如何新增視窗中斷點、建立新的 DataTemplate，以及如何使用 VisualStateManager 類別來量身打造應用程式的版面配置。 您將使用這些工具，針對較小型的視窗大小最佳化影像編輯程式。

影像編輯程式有兩個頁面。 「主頁面」  會顯示影像中心檢視，以及一些關於每個影像檔案的資訊。

![Photolab 主頁面的螢幕擷取畫面。](../basics/images/xaml-basics/mainpage.png)

「詳細資料頁面」  會在選取單一相片之後，顯示該相片。 飛出視窗編輯功能表可用來變更、重新命名和儲存相片。

![Photolab 詳細頁面的螢幕擷取畫面。](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>必要條件

+ Visual Studio 2019：[下載 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (此為免費的社群版本)。
+ Windows 10 SDK (10.0.17763.0 或更新版本)：[下載最新 Windows SDK (免費)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10，版本 1809 或更新版本

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：從 github 取得起始程式碼

針對此教學課程，您將從簡化版的 PhotoLab 範例開始著手。

1. 移至 GitHub 範例頁面：[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。
2. 下一步，您將需要複製或下載範例。 按一下 [複製或下載]  按鈕。 子功能表會出現。
    ![PhotoLab 範例的 GitHub 頁面上的複製或下載功能表](images/xaml-basics/clone-repo.png)

    **如果您不熟悉 GitHub：**

    a. 按一下 [下載 ZIP]  ，然後在本機儲存此檔案。 這個下載的 .zip 檔案，包含您所需要的所有專案檔案。

    b. 將檔案解壓縮。 使用 [檔案總管] 瀏覽至您下載的 .zip 檔案，以滑鼠右鍵按一下檔案，然後選取 [解壓縮全部...]。

    c. 瀏覽至您範例的本機副本，並移至 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout` 目錄。

    **如果您熟悉 GitHub：**

    a. 在本機複製存放庫中的主要分支。

    b. 瀏覽至 `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout` 目錄。

3. 按兩下 `Photolab.sln`，在 Visual Studio 中開啟解決方案。

## <a name="part-1-define-window-breakpoints"></a>第 1 部分：定義視窗中斷點

執行應用程式。 這在全螢幕上看起來很不錯，但當您將視窗縮得太小時，使用者介面 (UI) 就不是那麼理想了。 您可以使用 [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) 類別，讓 UI 隨著不同視窗大小進行調整，以確保在外觀和風格上始終有良好的使用者體驗。

![小型視窗：之前](../basics/images/xaml-basics/adaptive-layout-small-before.png)

如需有關應用程式版面配置的詳細資訊，請參閱文件的＜[版面配置](../layout/index.md)＞一節。

### <a name="add-window-breakpoints"></a>新增視窗中斷點

第一個步驟是定義套用不同視覺狀態的「中斷點」。 如需小型、中型和大型螢幕中斷點的詳細資訊，請參閱[螢幕大小和中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

從方案總管開啟 App.xaml，然後將下列程式碼加到 `MergedDictionaries` 後面，但在結尾的 `</ResourceDictionary>` 標記之前。

```xaml
    <!--  Window width adaptive breakpoints.  -->
    <x:Double x:Key="MinWindowBreakpoint">0</x:Double>
    <x:Double x:Key="MediumWindowBreakpoint">641</x:Double>
    <x:Double x:Key="LargeWindowBreakpoint">1008</x:Double>
```

這會建立 3 個中斷點，可讓您建立 3 個視窗大小範圍的新視覺狀態：

+ 小型 (0 - 640 像素寬度)
+ 中型 (641 - 1007 像素寬度)
+ 大型 (> 1007 像素寬度)

在此範例中，您只會針對小型視窗大小建立新的外觀。 中型和大型的大小會使用相同的外觀。

## <a name="part-2-add-a-data-template-for-small-window-sizes"></a>第 2 部分：新增小型視窗大小的資料範本

若要讓此應用程式即使在小型視窗中顯示時，仍有不錯的外觀，您可以建立新的資料範本，以最佳化使用者縮小視窗時，影像庫檢視中的影像顯示方式。

### <a name="create-a-new-datatemplate"></a>建立新的 DataTemplate

 從 [方案總管] 開啟 MainPage.xaml，然後在 `Page.Resources` 標籤之內新增下列程式碼。

```xaml
<DataTemplate x:Key="ImageGridView_SmallItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">

        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImageSource, Mode=OneWay}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

這個圖庫範本藉由消除影像周圍框線，以及移除每個縮圖下面的影像中繼資料 (檔案名稱、評分等等)，節省螢幕實際可用空間。 但您不這麼做，而是將每個縮圖顯示為簡單的正方塊。

### <a name="add-metadata-to-a-tooltip"></a>將中繼資料新增至工具提示

您仍然希望使用者能夠存取每個影像的中繼資料，因此將工具提示新增至每個影像項目。 在您剛建立的 DataTemplate 的`Image` 標籤內新增下列程式碼。

```xaml
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
```

這樣就會在您將滑鼠游標暫留在縮圖 (或按住觸控螢幕) 時，顯示影像的標題、檔案類型及尺寸。

## <a name="part-3-define-visual-states"></a>第 3 部分：定義視覺狀態

現已為您的資料建立新的版面配置，但應用程式目前並不知道何時要透過預設樣式使用這個版面配置。 若要修正此問題，您必須加入[VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) 和 [VisualState](/uwp/api/windows.ui.xaml.visualstate) 定義。

### <a name="add-a-visualstatemanager"></a>加入 VisualStateManager

將下列程式碼新增至頁面的根元素 `RelativePanel`。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="create-statetriggers-to-apply-the-visual-state"></a>建立 StateTriggers 以套用視覺狀態

接下來，建立對應至每個貼齊點的 `StateTriggers`。 在 MainPage.xaml 中，將下列程式碼加入每個 `VisualState`。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

觸發每個視覺狀態時，應用程式就會使用指派給作用中 `VisualState` 的任何版面配置屬性。

### <a name="set-properties-for-each-visual-state"></a>設定每個視覺狀態的屬性

最後，設定每個視覺狀態的屬性，以告知 `VisualStateManager` 狀態觸發時要套用哪些屬性。 每個 setter 都會以特定 XAML 元素的其中一個屬性為目標，並將該屬性設為指定值。 將此程式碼新增至您剛才建立的 `SmallWindow` 視覺狀態 (在 `StateTriggers` 之後)。

```xaml
    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply small template and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_SmallItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_SmallItemContainerStyle}" />

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
```

這些 setter 會將影像庫的 `ItemTemplate` 設定為您在上一節中建立的新 `DataTemplate`。 此外，也會調整縮放滑桿，使之更適合小型螢幕。

### <a name="run-the-app"></a>執行應用程式

執行應用程式。 當應用程式載入時，嘗試變更視窗的大小。 將視窗縮減成小型大小時，您應該會看到應用程式切換至您在第 2 部分建立的小型版面配置。

![小型視窗：之後](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>更進一步

現在完成了這個實習課程，您已具備充足的調整型配置知識，可以進一步自行實驗。 若要進行更大的挑戰，請嘗試最佳化較大螢幕大小的版面配置，例如 Surface Hub。 如果您想要測試 Surface Hub 版面配置，請參閱[使用 Visual Studio 測試 Surface Hub 應用程式](../../debug-test-perf/test-surface-hub-apps-using-visual-studio.md)。

如果遇到困難，您可以在[使用 XAML 回應式版面配置](../layout/layouts-with-xaml.md)的下列各節中找到更多指引。

+ [視覺狀態與狀態觸發程序](../layout/layouts-with-xaml.md#adaptive-layouts-with-visual-states-and-state-triggers)
+ [量身訂做的版面配置](../layout/layouts-with-xaml.md#tailored-layouts)

或者，如果您想要深入了解最初的編輯相片應用程式是如何建置，請參閱 XAML [使用者介面](../basics/xaml-basics-ui.md)及[資料繫結](../../data-binding/xaml-basics-data-binding.md)的那些教學課程。

## <a name="get-the-final-version-of-the-photolab-sample"></a>取得最終版本 PhotoLab 範例

本教學課程不會建置到完整的編輯相片應用程式，因此請務必查看[最終版本](https://github.com/Microsoft/Windows-appsample-photo-lab)來了解各項功能，例如自訂動畫和樣式。
