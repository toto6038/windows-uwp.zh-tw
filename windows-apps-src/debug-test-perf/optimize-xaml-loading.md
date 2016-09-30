---
author: mcleblanc
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: "最佳化您的 XAML 標記"
description: "剖析 XAML 標記以在記憶體建構物件，對複雜 UI 而言很耗費時間。 以下是一些您可以執行的動作，以針對您的 app 改善 XAML 標記剖析和載入時間及記憶體效率。"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c2131b084d8bb989f1f7767f54db697e1cdd8dcf

---
# 最佳化您的 XAML 標記

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

剖析 XAML 標記以在記憶體建構物件，對複雜 UI 而言很耗費時間。 以下是一些您可以執行的動作，以針對您的 app 改善 XAML 標記剖析和載入時間及記憶體效率。

在 app 啟動時，限制 XAML 標記僅針對您需要的初始 UI 載入。 檢查初始頁面中的標記，並且確認它不包含任何不需要的項目。 如果頁面參照不同檔案中定義的使用者控制項或資源，則架構也會剖析該檔案。

在這個範例中，因為 InitialPage.xaml 使用來自 ExampleResourceDictionary.xaml 的某個資源，所以必須在啟動時剖析整個 ExampleResourceDictionary.xaml。

**InitialPage.xaml。**

```xml
<Page x:Class="ExampleNamespace.InitialPage" ...> 
    <UserControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </UserControl.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml。**

```xml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
    used in the app, but not during startup.-->
</ResourceDictionary>
```

如果您在您的 app 的許多頁面上使用資源，則將它儲存在 App.xaml 是很好的作法，並且可以避免重複。 但是 App.xaml 會在 app 啟動時進行剖析，所以僅在單一頁面中使用的任何資源 (除非該頁面是初始頁面) 應該放在頁面的本機資源。 這個反例顯示 App.xaml 包含僅由單一頁面 (不是初始頁面) 使用的資源。 這會無端地增加 app 啟動時間。

**InitialPage.xaml。**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->   
    <StackPanel>  
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**SecondPage.xaml。**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel> 
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" /> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**App.xaml**

```xml
<Application ...> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
     <Application.Resources>  
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/> 
    </Application.Resources> 
</Application> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

讓上述反例更有效率的方法是將 `SecondPageTextBrush` 移至 SecondPage.xaml，以及將 `ThirdPageTextBrush` 移至 ThirdPage.xaml。 `InitialPageTextBrush` 可以保留在 App.xaml 中，因為在任何情況下，應用程式資源必須在 app 啟動時進行剖析。

## 最小化元素計數

雖然 XAML 平台能夠顯示大量的元素，您可以讓您的 app 配置和轉譯速度更快，方法是使用最少量的元素來達成您想要的視覺效果。

-   配置面板有 [**Background**](https://msdn.microsoft.com/library/windows/apps/BR227512) 屬性，所以不需要只是為了塗上色彩而將 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 放在面板前面。

**沒有效率。**

```xml
<Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Rectangle Fill="Black"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**有效率。**

```xml
<Grid Background="Black"/>
```

-   如果您重複使用相同的向量元素夠多次，改為使用 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素會更有效率。 向量元素需要耗費較多的資源，因為 CPU 必須個別建立每個元素。 影像檔只需要解碼一次。

## 將看起來相同的多個筆刷合併到一個資源

XAML 平台會嘗試快取常用的物件，這樣就可以盡可能地重複使用這些物件。 但是，XAML 無法清楚分辨某個標記中宣告的筆刷與另一個標記中宣告的筆刷是否相同。 此處的範例使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 進行示範，但是 [**GradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210068) 的案例更類似且更重要。

**沒有效率。**

```xml
<Page ... > <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel>  
        <TextBlock>  
            <TextBlock.Foreground>  
                <SolidColorBrush Color="#FFFFA500"/> 
            </TextBlock.Foreground> 
        </TextBox> 
        <Button Content="Submit"> 
            <Button.Foreground> 
                <SolidColorBrush Color="#FFFFA500"/> 
            </Button.Foreground> 
        </Button> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

同時檢查使用預先定義色彩的筆刷：`"Orange"` 和 `"#FFFFA500"` 色彩相同。 若要修正重複，將筆刷定義為資源。 如果其他頁面中的 控制項使用相同的筆刷，則將其移至 App.xaml。

**有效率。**

```xml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>
    
    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## 最小化過度繪製

過度繪製是相同螢幕像素中繪製多個物件的位置。 請注意，在這個指導方針和想要最小化元素計數之間有時需要取捨。

-   如果元素因為是透明的或是隱藏在其他元素後面而看不見，而且它不會參與配置，則刪除它。 如果元素在初始視覺狀態看不見，但是在其他視覺狀態中可以看見，則在元素本身將 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 設為 **Collapsed**，並且在適當的狀態中將值變更為 **Visible**。 這個啟發學習法有例外狀況：一般來說，屬性在主要視覺狀態所具有的值是在本機上對元素的最佳設定。
-   使用複合元素，而不要分層放置多個元素來建立效果。 在這個範例中，結果是雙色調圖形，上半部是黑色 (從 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 的背景)，下半部是灰色 (從半透明白色的 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) alpha 混合 **Grid** 的黑色背景)。 這裡需要 150% 的像素以達成填滿的結果。

**沒有效率。**
    
```xml
    <Grid Background="Black"> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Grid.RowDefinitions> 
            <RowDefinition Height="*"/> 
            <RowDefinition Height="*"/> 
        </Grid.RowDefinitions> 
        <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**有效率。**

```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Rectangle Fill="Black"/>
        <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
    </Grid>
```

-   配置面板可以有兩個目的：為區域上色和配置子元素。 如果進一步以 z 順序排序的元素已為區域上色，則前方的配置面板不需要繪製該區域：而是可以只專注於配置其子系。 這裡提供一個範例。

**沒有效率。**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid Background="Blue"/> 
            </DataTemplate> 
        </GridView.ItemTemplate> 
    </GridView> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**有效率。**

```xml
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid/> 
            </DataTemplate>
        </GridView.ItemTemplate>
    </GridView> 
```

如果 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 可以進行點擊測試，則在其上設定透明背景值。

-   使用 [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209253) 元素以在物件周圍繪製框線。 在這個範例中，[**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 是做為 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 周圍的臨時框線。 但是會過度繪製中心儲存格中的所有像素。

**沒有效率。**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <Grid Background="Blue" Width="300" Height="45"> 
        <Grid.RowDefinitions> 
            <RowDefinition Height="5"/> 
            <RowDefinition/> 
            <RowDefinition Height="5"/> 
        </Grid.RowDefinitions> 
        <Grid.ColumnDefinitions> 
            <ColumnDefinition Width="5"/> 
            <ColumnDefinition/> 
            <ColumnDefinition Width="5"/> 
        </Grid.ColumnDefinitions> 
        <TextBox Grid.Row="1" Grid.Column="1"></TextBox> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**有效率。**

```xml
    <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
        <TextBox/>
    </Border>
```

-   請注意邊界。 如果負數邊界延伸到彼此轉譯的界限內，並且導致過度繪製，則兩個鄰近的元素會重疊 (可能會意外發生)。

使用 [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) 做為視覺診斷。 您會發現您未注意到的繪製物件出現在場景中。

## 快取靜態內容

另一個過度繪製的來源是來自許多重疊元素的圖形。 如果您在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)上將 [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084) 設為 **BitmapCache**，該元素包含組合圖形，則平台會將元素轉譯為點陣圖一次，然後在每個框架使用該點陣圖，而不會過度繪製。

**沒有效率。**

```xml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![三個單色圓圈組成的文氏圖表](images/solidvenn.png)

上面的影像是結果，但以下是過度繪製區域的地圖。 紅色越深代表過度繪製的次數越多。

![顯示重疊區域的文氏圖表](images/translucentvenn.png)

**有效率。**

```xml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

請注意使用 [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084)。 如果有任何子圖形具有動畫效果，請勿使用這項技巧，因為每個框架可能需要重新產生點陣快取，而導致破壞原本的目的。

## ResourceDictionaries

ResourceDictionaries 通常是用來將您的資源儲存在稍微全域的層級。 您應用程式想要在多個位置中參考的資源。 例如，樣式、筆刷、範本等等。 一般而言，我們已將 ResourceDictionaries 最佳化，讓它除非接獲要求，否則不將資源具現化。 但是有少數幾個您需要稍加小心的地方。

**含有 x:Name 的資源**。 任何含有 x:Name 的資源都無法從平台最佳化受益，但取而代之的是，它會在 ResourceDictionary 一建立好就立即具現化。 這是因為 x:Name 會告知平台您的應用程式需要此資源的欄位存取權，因此平台需要建立可供建立參考的內容。

**UserControl 中的 ResourceDictionaries**。 在 UserControl 內定義的 ResourceDictionaries 會帶來不利的後果。 平台會為 UserControl 的每個執行個體建立一個這類 ResourceDictionary 的複本。 如果您有一個使用頻率很高的 UserControl，則請將 ResourceDictionary 從 UserControl 中移出，然後將它放到頁面層級。

## 使用 XBF2

XBF2 是 XAML 標記的二進位表示法，可避免在執行階段產生的所有文字剖析成本。 它也會將您的二進位檔針對載入和樹狀目錄建立進行最佳化，並且允許針對 XAML 類型使用「快速路徑」以改善堆積和物件建立成本，例如 VSM、ResourceDictionary、樣式等等。 它完全採用記憶體對應，因此沒有用來載入和讀取 XAML 頁面的堆積磁碟使用量。 此外，它也可以減少 appx 中儲存的 XAML 頁面的磁碟使用量。 XBF2 是一個較精簡的表示法，與 XAML/XBF1 檔案相比，最多可以減少 50% 的磁碟使用量。 例如，內建的「相片」應用程式在轉換成 XBF2 之後的縮減程度大約為 60%，從大約 ~1 mb 的 XBF1 資產降低成 ~400kb 的 XBF2 資產。 我們也看過應用程式的受益情況是在 CPU 方面的縮減程度大約為 15% 到 20%，而在 Win32 堆積方面為 10% 到 15%。

XAML 內建控制項和架構提供的字典已經完全啟用 XBF2。 針對您自己的應用程式，請確定您專案檔宣告的是 TargetPlatformVersion 8.2 或更新版本。

若要檢查您是否有 XBF2，請在二進位編輯器中開啟您的應用程式；如果您有 XBF2，則第 12 和第 13 個位元組會是 00 02。




<!--HONumber=Jun16_HO4-->


