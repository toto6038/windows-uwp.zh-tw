---
author: muhsinking
Description: This tutorial walks through how to create a basic application user interface. It explains and demonstrates the use of Grid and StackPanel, two of the most common XAML elements.
title: 使用 Grid 和 StackPanel 建立簡單的天氣應用程式。
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10、uwp
ms.assetid: 9794a04d-e67f-472c-8ba8-8ebe442f6ef2
ms.localizationpriority: medium
ms.openlocfilehash: 0327437c809455cf191dcfc572e4a5145b73eb49
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7289658"
---
# <a name="tutorial-use-grid-and-stackpanel-to-create-a-simple-weather-app"></a>教學課程：使用 Grid 和 StackPanel 建立簡單的天氣應用程式

利用 **Grid** 和 **StackPanel** 元素，使用 XAML 來建立簡單的天氣應用程式。 有了這些工具，您便可製作外觀極佳的應用程式，且其可在任何執行 Windows 10 的裝置上運作。 完成本教學課程可能需要 10-20 分鐘。

> **重要 API**：[Grid 類別](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid)、[StackPanel 類別](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.stackpanel)

## <a name="prerequisites"></a>必要條件
- Windows 10 和 Microsoft Visual Studio 2015 或更新版本。 （最新的 Visual Studio 建議使用最新的開發和安全性更新）[按一下此處以了解如何使用 Visual Studio 開始設定](../../get-started/get-set-up.md)。
- 如何使用 XAML 和 C# 建立基本 "Hello World" 應用程式的知識。 如果您未具備此知識，[按一下這裡，以了解如何建立 "Hello World" app](https://msdn.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。

## <a name="step-1-create-a-blank-app"></a>步驟 1：建立空白的應用程式
1. 在 Visual Studio 功能表中，選取 **\[檔案\]** > **\[新增專案\]**。
2. 在 **\[新增專案\]** 對話方塊的左窗格中，選取 **\[Visual C#\]** > **\[Windows\]** > **\[通用\]** 或 **\[Visual C++\]** > **\[Windows\]** > **\[通用\]**。
3. 在中央窗格中，選取 **\[空白應用程式\]**。
4. 在 **\[名稱\]** 方塊中，輸入 **WeatherPanel**，然後選取 **\[確定\]**。
5. 若要執行程式，從功能表中選取 **\[偵錯\]** > **\[開始偵錯\]**，或選取 F5。

## <a name="step-2-define-a-grid"></a>步驟 2︰定義 Grid
在 XAML 中，**Grid** 是由一系列的列和欄所組成。 透過在 **Grid** 內指定元素的列與欄，您便可在使用者介面內放置及隔開其他元素。 列與欄是使用 **RowDefinition** 和 **ColumnDefinition** 元素所定義。

若要開始建立版面配置，請使用 **\[方案總管\]** 來開啟 **MainPage.xaml**，然後使用此程式碼來取代自動產生的 **Grid** 元素。

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="3*"/>
        <ColumnDefinition Width="5*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="2*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
</Grid>
```

新的 **Grid** 會建立一個兩列和兩欄的組合，其會定義應用程式介面的版面配置。 第一欄的 **Width** 為 "3\*"，而第二欄為 "5\*"，並以 3:5 的比例在兩欄之間劃分出水平空間。 透過相同的方式，這兩列的 **Height** 分別為 "2\*" 和 "\*"，因此，**Grid** 為第一列配置的空間為第二列的兩倍 ("\*" 相當於 "1\*")。 即使重新調整視窗大小或變更裝置，都會保留這些比例。

若要了解調整列與欄的其他方法，請參閱[使用 XAML 定義版面配置](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml#layout-properties)。

如果您立即執行應用程式，就只會看見空白頁面，因為 **Grid** 區域中沒有任何內容。 為了顯示 **Grid**，我們將為它提供一些色彩。

## <a name="step-3-color-the-grid"></a>步驟 3︰為 Grid 上色
為了為 **Grid** 上色，我們新增了三個 **Border** 元素，每一個都有不同的背景色彩。 此外，也會使用 **Grid.Row** 和 **Grid.Column** 屬性，將每一個元素指派給父項 **Grid** 中的列與欄。 這些屬性的值均預設為 0，如此您就不需將它們指派給第一個 **Border**。 將下列程式碼新增到 **Grid** 元素的列與欄定義之後。

```xml
<Border Background="#2f5cb6"/>
<Border Grid.Column ="1" Background="#1f3d7a"/>
<Border Grid.Row="1" Grid.ColumnSpan="2" Background="#152951"/>
```

請注意，我們針對第三個 **Border** 使用額外的屬性 **Grid.ColumnSpan**，這導致此 **Border** 會在較低的列中橫跨兩欄。 您可以透過相同方式來使用 **Grid.RowSpan**，而且一起使用這些屬性，可讓您在任意數目的列和欄上橫跨某個元素。 這類橫跨的左上角永遠是元素屬性中所指定的 **Grid.Column** 和 **Grid.Row**。

如果您執行此應用程式，結果看起來就像這樣。

![為格線上色](images/grid-weather-1.png)

## <a name="step-4-organize-content-by-using-stackpanel-elements"></a>步驟 4︰使用 StackPanel 元素整理內容
**StackPanel** 是我們將用來建立天氣應用程式的第二個 UI 元素。  **StackPanel** 是許多基本應用程式配置的基礎部分，可讓您以垂直或水平方式堆疊元素。

在下列程式碼中，我們會建立兩個 **StackPanel** 元素，並使用三個 **TextBlocks** 來填滿每一個元素。 將這些 **StackPanel** 元素新增至 **Grid** 的 **Border** 元素 (來自步驟 3) 下方。 這導致 **TextBlock** 元素會呈現在我們稍早建立的彩色 **Grid** 上方。

```xml
<StackPanel Grid.Column="1" Margin="40,0,0,0" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="Today - 64° F"/>
    <TextBlock Foreground="White" FontSize="25" Text="Partially Cloudy"/>
    <TextBlock Foreground="White" FontSize="25" Text="Precipitation: 25%"/>
</StackPanel>
<StackPanel Grid.Row="1" Grid.ColumnSpan="2" Orientation="Horizontal"
            HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="High: 66°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Low: 43°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Feels like: 63°"/>
</StackPanel>
```

在第一個 **Stackpanel** 中，每個 **TextBlock** 都會垂直堆疊在下一個的下方。 這是 StackPanel 的預設行為，因此我們不需要設定 **Orientation** 屬性。 在第二個 StackPanel 中，我們希望子元素以水平方向由左到右堆疊，因此我們將 **Orientation** 屬性設為 "Horizontal"。 我們也必須將 **Grid.ColumnSpan** 屬性設為 "2"，讓文字會在較低的 **Border** 上置中。

如果您立即執行此應用程式，將會看見如下的結果。

![新增 StackPanel](images/grid-weather-2.png)

## <a name="step-5-add-an-image-icon"></a>步驟 5︰新增影像圖示

最後，讓我們將代表今日天氣的影像填入 **Grid** 中的空白區段，而這類影像會包含「有時多雲」之類的內容。

下載下列影像，並以 "partially-cloudy" 名稱將它儲存為 PNG 檔。

![有時多雲](images/partially-cloudy.PNG)

在 **\[方案總管\]** 中，以滑鼠右鍵按一下 **\[資產\]** 資料夾，然後選取 **\[新增\]** -> **\[現有項目...\]**。在快顯的瀏覽器中尋找 partially-cloudy.png、選取它，然後按一下 **[新增]**。

接著，在 **MainPage.xaml** 中，將下列 **Image** 元素新增到步驟 4 的 StackPanel 下方。

```xml
<Image Margin="20" Source="Assets/partially-cloudy.png"/>
```

由於我們希望影像出現在第一列和欄中，因此不需要設定它的 **Grid.Row** 或 **Grid.Column** 屬性，讓它們的預設值為 "0"。

這樣就大功告成了！ 您已成功建立簡單天氣應用程式的版面配置。 如果您按 **F5** 來執行應用程式，您應該會看到類似這樣的畫面︰

![天氣窗格範例](images/grid-weather-3.PNG)

您可以視需要試著使用上述的版面配置進行試驗，並探索您可用來代表天氣資料的各種方式。

## <a name="related-articles"></a>相關文章
如需如何設計 UWP 應用程式版面配置的簡介，請參閱 [UWP 應用程式設計簡介](https://msdn.microsoft.com/windows/uwp/layout/design-and-ui-intro)

若要了解如何建立可適應不同螢幕大小的回應式版面配置，請參閱[使用 XAML 定義頁面版面配置](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml)
