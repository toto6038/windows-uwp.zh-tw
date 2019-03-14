---
Description: 使用 Cortana 語音命令、語音辨識以及語音合成，將語音加入您的應用程式。
title: Surface Dial 互動
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial、Windows 滾輪、RadialController、Radial 控制器、使用者互動、輸入
ms.date: 02/08/2017
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: bcdb8ca6843d126bc245e48f0b50209890740819
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639613"
---
# <a name="surface-dial-interactions"></a>Surface Dial 互動

![使用介面的 Studio 介面撥號的映像](images/windows-wheel/dial-pen-studio-600px.png)  
*Surface Dial、Surface Studio 和手寫筆* (可在 [Microsoft 網上商店](https://aka.ms/purchasesurfacedial)購買)。

## <a name="overview"></a>概觀

Windows 滾輪裝置，例如 Surface Dial，是一種新的輸入裝置，可以針對 Windows 和 Windows 應用程式提供極具吸引力且獨特的使用者互動體驗。 

> [!IMPORTANT]
> 在本主題中，我們特別說明 Surface Dial 互動，但此資訊適用於所有 Windows 滾輪裝置。 

| 影片 |   |
| --- | --- |
| <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> |
| *介面的撥號應用程式合作夥伴* | *開發人員介面撥號* |

Surface Dial 使用根據「旋轉」動作 (或手勢) 的形狀規格，做為次要、多重強制回應的輸入裝置，補充主要裝置的輸入。 在大部分情況下，使用者是以慣用手執行工作 (例如以手寫筆寫字)，同時以非慣用手操作這類裝置。 其設計的目的並不是為了精確的指標輸入 (例如觸控、手寫筆或滑鼠)。 

Surface Dial 也支援「長按」以及「按一下」兩個動作。 長按有單一的功能︰顯示命令的功能表。 如果功能表使用中，旋轉並按一下輸入是由功能表處理。 否則，輸入就會傳遞到您的應用程式進行處理。 

**所有 Windows 輸入裝置，您可以自訂及調整以符合您的應用程式中的功能的 Surface Dial 互動體驗。**

> [!TIP]
> Surface Dial 與新的 Surface Studio 一起使用，可以提供更獨特的使用者體驗。  
>
>除了上述預設的長按功能表體驗之外，Surface Dial 也可以直接放置在 Surface Studio 的螢幕上。 這會產生特殊的「螢幕上」功能表。 
>
>系統會偵測 Surface Dial 的接觸位置和界限，使用此資訊處理裝置的遮蔽範圍，並沿著 Dial 外圍迴旋顯示較大版本的功能表。 您的應用程式也可以使用這個相同的資訊，針對裝置是否存在及其預期的使用方式 (例如使用者的手與手臂的放置位置) 打造 UI。

| Surface Dial 移開螢幕時的功能表 | | Surface Dial 位於螢幕上的功能表 |
| --- | --- | --- |
| ![Surface Dial 移開螢幕時的功能表](images/windows-wheel/surface-dial-menu-offscreen.png) | | ![Surface Dial 位於螢幕上的功能表](images/windows-wheel/surface-dial-menu-onscreen.png) |

## <a name="system-integration"></a>系統整合

Surface Dial 與 Windows 緊密整合，而且支援功能表上的一組內建工具︰系統音量、捲動、放大/縮小，以及復原/重做。

這一組內建工具可配合目前的系統內容而包含︰
- 使用者在 Windows 桌面時的系統亮度工具
- 播放媒體時的上一首/下一首工具

除了這個一般的平台支援，Surface Dial 也能與 Windows Ink 平台控制項 ([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) 與 [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar)) 緊密整合。

![使用 Surface 手寫筆的介面撥號](images/windows-wheel/dial-and-pen-400px.png)  
*使用 Surface 手寫筆的介面撥號*

與 Surface Dial 搭配使用時，這些控制項可以有其他功能來修改筆跡屬性，以及控制筆跡工具列的尺規樣板。

當您在使用筆跡工具列的筆跡應用程式中開啟 Surface Dial 功能表時，功能表現在所含的工具可以控制手寫筆類型與筆刷粗細。 尺規啟用時，功能表中就會新增對應的工具，讓裝置控制尺規的位置與角度。

![使用畫筆 Windows 筆跡 工具列中選取工具的介面撥功能表](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*使用畫筆 Windows 筆跡 工具列中選取工具的介面撥功能表*

![使用 Windows 筆跡 工具列的筆劃大小工具的介面撥功能表](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*使用 Windows 筆跡 工具列的筆劃大小工具的介面撥功能表*

![使用 Windows 筆跡 工具列的尺規工具的介面撥功能表](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*使用 Windows 筆跡 工具列的尺規工具的介面撥功能表*

## <a name="user-customization"></a>使用者自訂項目

使用者可以透過 \[Windows 設定\] -&gt; \[裝置\] -&gt; \[滾輪\] 頁面自訂自己的 Dial 體驗的某些層面，包含預設工具、震動 (或觸覺回饋) 以及寫字的手 (或慣用手)。 

自訂 Surface Dial 使用者體驗時，一定要確定使用者可以使用並啟用特定的功能或行為。

## <a name="custom-tools"></a>自訂工具

我們將在下面討論在 Surface Dial 功能表上自訂顯示工具的 UX 與開發人員指導方針。

### <a name="ux-guidance"></a>UX 指導方針

**確定您的工具對應到目前的情境** 如果工具的功能以及 Surface Dial 互動的運作方式清楚而直覺，就可以協助使用者快速了解並聚焦在自己的工作上。

**最大的應用程式工具數目降至最低**  
Surface Dial 功能表的空間可容納七個項目。 如果有八個以上的項目，使用者就必須旋轉 Dial 才能在溢出的飛出視窗中查看有哪些工具可用，如此一來功能表就很難瀏覽，工具就很難探索和選取。

我們建議為您的應用程式或應用程式情境提供單一的自訂工具。 這樣您就可以根據使用者正在進行的動作設定該工具，使用者不需要啟用 Surface Dial 功能表並選取工具。 

**動態更新工具的集合**  
因為 Surface Dial 功能表項目不支援停用的狀態，所以您應該根據使用者的情境 (目前的檢視或具有焦點的視窗) 動態新增和移除工具 (包括內建的預設工具)。 如果某個工具與目前的活動不相關或重複，就移除該工具。

> [!IMPORTANT]
> 當您在功能表中新增項目時，請確定當中還沒有該項目。

**請勿移除內建的系統磁碟區設定工具**  
使用者通常都會需要音量控制。 使用者在使用您的應用程式時可能會聆聽音樂，因此必須一律可從 Surface Dial 功能表存取音量和下一首工具。 (媒體播放時，下一首工具會自動新增到功能表中)。

**與功能表組織保持一致**  
這可以協助使用者在使用您的應用程式時探索和了解可以使用哪些工具，並且在切換工具時協助提昇效率。

**提供的內建的圖示與一致的高品質圖示**  
圖示可以傳達專業及優點，並且獲得使用者的信任。
- 提供高品質 64 x 64 像素的 PNG 影像 (最小支援 44 x 44)
- 確保背景是透明的
- 圖示應該填滿大部分的影像
- 白色圖示要有一個黑色外框可在高對比模式中顯示

|   |   |   |
| --- | --- | --- |
| ![使用 alpha 背景的圖示](images/windows-wheel/surface-dial-menu-icon1.png) | ![滾輪功能表上使用預設佈景主題圖示所顯示的圖示](images/windows-wheel/surface-dial-menu-icon2.png) | ![Surface Dial 位於螢幕上的功能表](images/windows-wheel/surface-dial-menu-icon3.png) |
| *使用 alpha 色背景的圖示* | *使用預設佈景主題的滾輪 功能表上顯示的圖示* | *具有白色高對比佈景主題的滾輪 功能表上顯示的圖示* |

**使用精簡的、 描述性的名稱**  
工具名稱會與工具圖示一起顯示在工具功能表中，螢幕助讀程式也會使用此名稱。 
- 名稱應該要簡短到可以放入滾輪功能表的中央圓形內
- 名稱應該要清楚地識別主要動作 (可以暗示補充動作)︰
  - 捲軸指出兩個旋轉方向的效果
  - 復原指定主要動作，但是使用者可以推斷和輕鬆探索重做 (補充動作)

### <a name="developer-guidance"></a>開發人員指導方針

您可以透過一組完整的 [Windows 執行階段 API](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 自訂 Surface Dial 體驗，補充您 App 的功能。 

如先前所述，預設的 Surface Dial 功能表會預先填入一組內建工具，涵蓋廣泛的基本系統功能 (系統音量、系統亮度、捲動、縮放、復原，以及當系統偵測到播放音訊或視訊時的媒體控制項)。 不過，這些預設工具可能無法提供您的應用程式所需的功能。 

在下列各節中，我們將說明如何在 Surface Dial 功能表中新增自訂工具，並指定要顯示哪些內建工具。

**加入自訂工具**

在此範例中，我們要新增一個基本的自訂工具，將旋轉和按一下事件的輸入資料傳遞到一些 XAML UI 控制項。

1. 首先，我們在 XAML 中宣告我們的 UI (只有一個滑桿和切換按鈕)。

   ![範例應用程式 UI 的影像](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *範例應用程式 UI*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. 然後，在程式碼後置中，我們將自訂工具新增到 Surface Dial 功能表，並宣告 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 輸入處理常式。 

   我們呼叫 [**CreateForCurrentView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 取得 Surface Dial (myController) 的 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.CreateForCurrentView) 物件的參考。

   接著我們呼叫 [**RadialControllerMenuItem.CreateFromIcon**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 建立 [**RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/mt759255) (myItem) 的執行個體。 

   接下來，我們將該項目附加到功能表項目的集合。

   我們宣告 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 物件的輸入事件處理常式 ([**ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged) 和 [**RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController))。

   最後，我們定義事件處理常式。

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

當我們執行應用程式時，我們使用 Surface Dial 與其進行互動。 首先，我們長按以開啟功能表，然後選取我們自訂的工具。 自訂工具啟用之後，可以旋轉 Dial 調整滑桿控制項，而且可以按一下 Dial 切換開關。

![範例應用程式 UI 啟動使用 Surface Dial 的自訂工具的影像](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*範例應用程式 UI 啟動使用 Surface Dial 的自訂工具*

**指定內建的工具**

您可以使用 [**RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 類別來自訂您應用程式的內建功能表項目集合。

例如，如果您的應用程式不會有任何捲動或縮放區域，而且不需要復原/重做功能，就可以從功能表移除這些工具。 這樣就能在功能表上騰出空間，新增您應用程式的自訂工具。 

> [!IMPORTANT] 
> Surface Dial 功能表必須至少要有一個功能表項目。 如果您在新增自訂工具之前移除了所有預設工具，預設工具會還原，您的工具則會附加到預設集合。

根據設計指導方針，我們不建議移除媒體控制項工具 (音量與上一首/下一首)，因為使用者經常會播放背景音樂，同時執行其他工作。

以下，我們將說明如何設定 Surface Dial 功能表，只放入音量和下一首/上一首的媒體控制項。

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>自訂互動

如先前所述，Surface Dial 支援三種手勢 (長按、旋轉、按一下)，分別有對應的預設互動。 

請確定根據這些手勢的任何自訂互動，對選取的動作或工具是有意義的。 

> [!NOTE]
> 互動體驗取決於 Surface Dial 功能表的狀態。 如果功能表使用中，就會處理輸入，否則您的應用程式會接手。

### <a name="press-and-hold"></a>長按

這個手勢會啟用並顯示 Surface Dial 功能表，沒有任何App 功能這個手勢相關聯。 

此功能表預設會顯示在使用者的螢幕中央。 不過，使用者可以抓取它，然後移動到任何位置。

> [!NOTE]
> 如果將 Surface Dial 放在 Surface Studio 的螢幕上，就會在 Surface Dial 放在螢幕上的位置中央顯示功能表。

### <a name="rotate"></a>Rotate

Surface Dial 主要是設計來支援涉及順暢、增量調整類比值或控制項的互動旋轉。

裝置可以順時針方向和逆時針方向旋轉，也可以提供觸覺回饋以指出不連續的距離。

> [!NOTE]
> 使用者可以在 \[Windows 設定\] -&gt; \[裝置\] -&gt; \[滾輪\] 頁面中停用觸覺回饋。

#### <a name="ux-guidance"></a>UX 指導方針

**連續或高旋轉的區分大小寫的工具應該停用 haptic 意見反應**

觸覺回饋要符合使用中工具的旋轉靈敏度。 我們建議具連續或高旋轉靈敏度的工具停用觸覺回饋，因為使用者可能會感到不舒服。 

**主控項的手應該不會影響旋轉式互動**

Surface Dial 無法偵測到您正在使用哪隻手，但是使用者可以在 \[Windows 設定\] -&gt; \[裝置\] -&gt; \[手寫筆與 Windows Ink\] 中設定寫字的手 (或慣用手)。

**地區設定應該視為旋轉的所有互動**

讓您的互動提供並可配合地區設定以及由右至左配置，提高客戶的滿意度。

Dial 功能表上的內建工具和命令遵循下列指導方針進行以旋轉為基礎的互動︰

|   |   |   |
| --- | --- | --- |
| Left<br/>Up<br/>向外 | ![Surface Dial 的影像](images/windows-wheel/surface-dial-rotate.png) | Right<br/>Down<br/>向內 |
|   |   |   |

| 概念方向 | 對應到 Surface Dial | 順時針方向旋轉 | 逆時針方向旋轉 |
| --- | --- | --- | --- |
| 水平 | 以 Surface Dial 的頂端為基準對應向左和向右 | Right | Left |
| 垂直 | 以 Surface Dial 的左側為基準對應向上和向下 | Down | Up |
| Z 軸 | 向內 (或接近) 對應到向上/向右<br/>向外 (或離開) 對應到向左/向下 | 向內 | 向外 |

#### <a name="developer-guidance"></a>開發人員指導方針

當使用者旋轉裝置，會根據相對於旋轉方向的差異 ([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged)) 引發 [**RadialController.RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees) 事件。 使用 [**RadialController.RotationResolutionInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationResolutionInDegrees) 屬性可以設定資料的靈敏度 (或解析度)。

> [!NOTE]
> 根據預設，裝置最少要旋轉 10 度，旋轉的輸入事件才會傳遞至 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 物件。 每個輸入事件都會導致裝置震動。

一般而言，我們建議當旋轉解析度設定為少於 5 度時停用觸覺回饋。 這可以為連續的互動提供更順暢的體驗。 

您可以設定 [**RadialController.UseAutomaticHapticFeedback**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.UseAutomaticHapticFeedback) 屬性，啟用和停用自訂工具的觸覺回饋。

> [!NOTE]
> 您不能覆寫系統工具 (例如音量控制) 的觸覺回饋行為。 這些工具的觸覺回饋，只能由使用者從滾輪設定頁面停用。

以下是如何自訂旋轉資料解析度，以及啟用或停用觸覺回饋的範例。

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>按一下

按一下 Surface Dial 類似按一下滑鼠左鍵 (裝置的旋轉狀態不會影響此動作)。

#### <a name="ux-guidance"></a>UX 指導方針

**未對應的動作或命令到這個筆勢如果使用者無法輕易復原的結果**

您的應用程式根據使用者按一下 Surface Dial 所採取的任何動作都必須是可以還原的。 請務必讓使用者可以輕鬆地將應用程式周遊回堆疊，並還原先前的應用程式狀態。

二進位操作 (例如靜音/取消靜音或顯示/隱藏) 使用按一下手勢可以提供良好的使用者體驗。

**強制回應工具不應啟用或停用依序按一下 Surface Dial**

有些應用程式/工具模式可能會與依賴旋轉的互動發生衝突，或停用這類互動。 Windows Ink 工具列中尺規之類的工具，應該透過其他 UI 能供性開啟或關閉 (Ink 工具列提供內建的 [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.ToggleButton) 控制項)。

對於強制回應工具，請將使用中的 Surface Dial 功能表項目對應到目標工具或先前選取的功能表項目。

#### <a name="developer-guidance"></a>開發人員指導方針

按一下 Surface Dial 時，會引發 [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 事件。 [  **RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs) 包括 [**Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact) 屬性，其中包含 Surface Dial 在 Surface Studio 螢幕上接觸的位置以及週框區域。 如果 Surface Dial 沒有接觸螢幕，這個屬性為 null。 

### <a name="on-screen"></a>螢幕上

如先前所述，Surface Dial 可以搭配 Surface Studio 使用，在特殊的螢幕上模式顯示 Surface Dial 功能表。 

處於這個模式時，可以與您的應用程式更進一步整合並自訂您的 Dial 互動體驗。 同時使用 Surface Dial 與 Surface Studio 才能獲得的獨特體驗包括下列範例︰
- 根據 Surface Dial 的位置顯示與內容相關的工具 (例如調色盤)，讓這些工具更容易尋找及使用
- 根據 Surface Dial 放置的 UI 設定可使用的工具
- 根據 Surface Dial 的位置放大螢幕區域
- 根據螢幕位置進行獨特的遊戲互動

#### <a name="ux-guidance"></a>UX 指導方針

**Surface Dial 偵測到螢幕上時，應用程式應該會回應**

視覺化回饋有助於告訴使用者您的應用程式已偵測到裝置放在 Surface Studio 的螢幕上。

**調整 Surface Dial 相關的 UI，根據裝置位置**

取決於使用者放置裝置的位置，裝置 (和使用者的身體) 可能會遮蓋重要的 UI。

**調整 Surface Dial 相關的 UI，根據使用者互動**

除了硬體遮蔽，使用者在使用裝置時，手和手臂可能會遮蓋到部分螢幕。 

被遮蔽的區域取決於正用哪隻手使用裝置。 由於裝置主要是搭配非慣用手使用而設計，所以應該針對使用者指定的相反手調整與 Surface Dial 相關的 UI (\[Windows 設定\] &gt; \[裝置\] &gt; \[手寫筆與 Windows Ink\] &gt; \[選擇您用來寫字的那隻手\] 設定)。

**Surface Dial 位置，而不是移動回應互動**

裝置是設計來固定在螢幕上而不是滑動，因為它不是精確的指標裝置。 因此，我們希望使用者較常提起和放置 Surface Dial 而不是在螢幕上拖曳。

**使用畫面位置，以判斷使用者的想法**

根據 UI 內容設定可使用的工具 (例如接近控制項、畫布或視窗)，減少執行工作所需的步驟可以增進使用者體驗。

#### <a name="developer-guidance"></a>開發人員指導方針

將 Surface Dial 放在 Surface Studio 的數位板表面時，會引發 [**RadialController.ScreenContactStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ScreenContactStarted) 事件並將接觸資訊 ([**RadialControllerScreenContactStartedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs.Contact)) 提供給您的應用程式。

同樣地，如果在接觸 Surface Studio 的數位板表面時按一下 Surface Dial，會引發 [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 事件並將接觸資訊 ([**RadialControllerButtonClickedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact)) 提供給您的應用程式。 

接觸資訊 ([**RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact)) 包含 Surface Dial 中心在應用程式座標空間中的 X/Y 座標 ([**RadialControllerScreenContact.Position**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Position))，以及週框 ([**RadialControllerScreenContact.Bounds**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Bounds))，單位為裝置獨立畫素 (DIP)。 這項資訊對於提供情境給可使用工具，以及提供與裝置相關的視覺化回饋給使用者時很有用。

在下列範例中，我們建立了一個有四個不同區段的基本應用程式，每一個區段都包含一個滑桿和一個切換開關。 我們接著會使用 Surface Dial 在螢幕上的位置指定 Surface Dial 要控制哪一組滑桿和切換開關。

1. 首先，我們在 XAML 中宣告我們的 UI (四個區段，每個區段都有一個滑桿和切換按鈕)。

   ![範例應用程式 UI 的影像](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *範例應用程式 UI*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. 以下程式碼後置包含針對 Surface Dial 螢幕位置定義的處理常式。

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

當我們執行應用程式時，我們使用 Surface Dial 與其進行互動。 首先，我們將裝置放在 Surface Studio 螢幕上，應用程式會偵測到並與右下角的區段關聯 (參閱影像)。 然後，我們長按 Surface Dial 以開啟功能表，然後選取我們自訂的工具。 自訂工具啟用之後，可以旋轉 Surface Dial 調整滑桿控制項，而且可以按一下 Surface Dial 切換開關。

![範例應用程式 UI 啟動使用 Surface Dial 的自訂工具的影像](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*範例應用程式 UI 啟動使用 Surface Dial 的自訂工具*

## <a name="summary"></a>摘要

本主題提供 Surface Dial 輸入裝置的概觀，以及搭配 Surface Studio 使用時，如何針對移開螢幕時的案例和放上螢幕時的案例自訂使用者體驗的 UX 與開發人員指導方針。

## <a name="feedback"></a>意見反應

請將您的問題、建議和意見反應傳送給 [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com)。

## <a name="related-articles"></a>相關文章

### <a name="api-reference"></a>API 參考資料

- [**RadialController** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu**類別](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem**類別](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon**列舉](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind** enum](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>範例

[通用 Windows 平台的範例 (C#和 c + +)](https://go.microsoft.com/fwlink/?linkid=832713)

[Windows 傳統桌面範例](https://aka.ms/radialcontrollerclassicsample)