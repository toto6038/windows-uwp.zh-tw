---
author: TylerMSFT
title: "通用 Windows 平台 (UWP) app 指南"
description: "在本指南中，深入了解可以在各種裝置上執行的通用 Windows 平台 (UWP) app。"
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
translationtype: Human Translation
ms.sourcegitcommit: 4ad8dc5883b7edafa2c2579d3733eafba0b9cc1f
ms.openlocfilehash: 8f4e906c9f1c685a5f6aeebd5fe0ebcc96ff9a7c

---

# 通用 Windows 平台 (UWP) app 指南


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在本指南中，您將深入了解：

-   什麼是*裝置系列*，以及如何決定要以哪一個做為目標。
-   新的 UI 控制項和面板可讓您的 UI 適應不同裝置表單係數。
-   如何了解與控制可供您的 app 使用的 API 表面。

Windows 8 引進 Windows 執行階段 (WinRT)，它是由 Windows 應用程式模型演進而來。 做為通用應用程式架構。

Windows Phone 8.1 變成可用時，Windows 執行階段會對齊 Windows Phone 8.1 和 Windows。 如此可讓開發人員建立*通用 Windows 8 應用程式*，使用共用的程式碼基底，同時以 Windows 和 Windows Phone 為目標。

Windows 10 引進了通用 Windows 平台 (UWP)，進一步進化 Windows 執行階段模型，並將它帶到 Windows 10 統一核心。 做為核心的一部分，UWP 現在提供執行 Windows 10 的每個裝置上可用的通用 app 平台。 因為這樣的進化，以 UWP 為目標的 app 不僅可以呼叫所有裝置通用的 WinRT API，還可以呼叫 app 執行所在之裝置系列特定的 API (包括 Win32 和 .NET API)。 UWP 提供跨裝置的保證核心 API 層。 這表示您可以建立能夠安裝到各種裝置上的單一 app 套件。 同時，Windows 市集使用該單一應用程式套件提供統一的通路，可以達到您的 app 可以執行的所有裝置類型。

![通用 Windows 平台 app 在各種裝置上執行、支援彈性的使用者介面、自然的使用者輸入、一個市集、一個開發人員中心和雲端服務](images/universalapps-overview.png)

因為您的 UWP app 在具有不同表單係數和輸入形式的各種裝置上執行，您想要針對每個裝置量身打造，且能夠解除鎖定每個裝置的獨特功能。 裝置將其獨特的 API 新增至保證 API 層。 您可以撰寫程式碼以有條件地存取這些獨特的 API，讓您的 app 可以在呈現其他裝置的不同體驗的同時，開啟某個裝置類型特定的功能。 彈性的 UI 控制項和新的配置面板可協助您針對各種螢幕解析度調整您的 UI。

## 裝置系列

Windows 8.1 和 Windows Phone 8.1 app 的目標作業系統 (OS)：Windows 或 Windows Phone。 使用 Windows 10，您不再是以作業系統為目標，而是以一或多個裝置系列做為您的 app 的目標。 裝置系列會識別 API、系統特性以及您對於裝置系列中的裝置可以預期的行為。 它也會判斷可以從市集安裝您的 app 的一組裝置。 以下是裝置系列階層。

![裝置系列](images/devicefamilytree.png)

裝置系列是一起收集的一組 API，並且給予名稱和版本號碼。 裝置系列是作業系統的基礎。 電腦執行傳統型作業系統，是以傳統型裝置系列為基礎。 手機和平板電腦等等執行行動裝置作業系統，是以行動裝置系列為基礎。 依此類推。

通用裝置系列是特殊的。 它不直接做為任何作業系統的基礎。 相反地，通用裝置系列中的一組 API 是由子裝置系列繼承。 因此通用裝置系列 API 保證會存在於每個 OS 及每個裝置上。

每個子裝置系列會將自己的 API 新增至它繼承的 API。 子裝置系列中產生的 API 聯集保證會存在於以該裝置系列為基礎的作業系統中，以及執行該作業系統的每個裝置上。

裝置系列的一項優點是您的 App 可以在任何 (甚至是所有) 裝置上執行，從手機、平板電腦、桌上型電腦到 Surface Hub 和 Xbox 主機，以及 HoloLens。 您的 App 也可以使用彈性的程式碼，以動態偵測並使用通用裝置系列以外的裝置功能。

關於您的 app 要以哪個 (哪些) 裝置系列為目標的決策由您決定。 該決策會以這些重要的方式影響您的 app。 它會決定：

-   您的 app 在執行時假設可以存在的 API 集合 (因此可以自由地呼叫)。
-   僅在條件陳述式內安全的 API 集合呼叫。
-   可以從市集安裝您的 app 的一組裝置 (以及您後續需要考量的表單係數)。

進行裝置系列選擇有兩個主要的後果：可以由 app 無條件地呼叫 API 表面，以及 app 可連線的裝置數目。 這兩項因素需要做出取捨並且成反比相關。 例如，UWP app 是專門針對通用裝置系列，因此後續適用於所有裝置。 以通用裝置系列為目標的 app 可以假設 API 僅存在於通用裝置系列中 (因為這是它的目標)。 其他 API 必須有條件地呼叫。 此外，這類 app 必須具有高度彈性的 UI 和完整的輸入的功能，因為它可以在各種裝置上執行。 Windows 行動裝置 app 是特別針對行動裝置系列的 app，適用於其作業系統是以行動裝置系列為基礎的裝置 (包括手機、平板電腦和類似的裝置)。 行動裝置系列 app 可以假設所有 API 存在於行動裝置系列中，且其 UI 必須適度調整。 以 IoT 裝置系列為目標的 app 只可以安裝在 IoT 裝置上，並且可以假設所有 API 存在於 IoT 裝置系列中。 因為您知道 app 將只在特定裝置類型上執行，所以該 app 的 UI 和輸入能力可以非常特殊。

以下是一些可協助您決定要以哪個裝置系列為目標的考量：

**最大化您的 app 可及範圍**

若要達到您的 app 對於裝置的最大範圍，並且讓它儘可能在多個類型的裝置上執行，您的 app 將會以通用裝置系列為目標。 這麼做可以讓 app 自動以根據通用的每個裝置系列為目標 (在圖表中，通用的所有子系)。 這表示 app 會在以這些裝置系列為基礎的每個作業系統上執行，以及在執行這些作業系統的所有裝置上執行。 保證可以在所有裝置上使用的唯一 API 是您做為目標之通用裝置系列的特定版本所定義的集合。 (這個版本中，該版本永遠是 10.0.x.0。) 若要了解 app 可以在其目標裝置系列版本以外呼叫 API 的方式，請參閱本主題稍後的＜撰寫程式碼＞。

**將您的 App 限制於單一類型的裝置**

基於某些原因，您可能不希望您的 App 在眾多類型的裝置上執行，例如它可能是特別針對桌上型電腦或 Xbox 主機所設計。 在這種情況下，您可以選擇讓您的 App 以其中一個子裝置系列為目標。 例如，如果您以傳統型裝置系列為目標，API 保證可用於您的 app，包括從通用裝置系列繼承的 API，加上傳統型裝置系列特定的 API。

**將您的 app 限制為所有可能的裝置子集**

不是以通用裝置系列為目標，或是以其中一個子裝置系列為目標，而是可以兩個 (或多個) 子裝置系列為目標。 以傳統型和行動裝置為目標，對於您的 App 可能較為合理。 或是傳統型和 HoloLens。 或是傳統型、Xbox 及 Surface Hub 等等。

**排除特定版本裝置系列的支援**

在少數情況下，您可能想要讓您的 app 可以隨處執行，具有特定裝置系列之特定版本的裝置除外。 例如，假設您的 App 以通用裝置系列的 10.0.x.0 版為目標。 當未來作業系統版本變更時 (假設變更為 10.0.x.2)，屆時您就可以透過讓 App 以通用的 10.0.x.0 及 Xbox 的 10.0.x.2 為目標，來指定您的 App 僅可在 Xbox 的 10.0.x.1 版以外的版本上執行。 您的 App 將會無法適用 Xbox 10.0.x.1 (含) 和更舊版本的裝置系列版本集合。

根據預設，Microsoft Visual Studio 會將應用程式套件資訊清單檔案中的 **Windows.Universal** 指定為目標裝置系列。 若要指定從市集將您的 app 提供給一或多個裝置系列，請在您的 Package.appxmanifest 檔案中手動設定 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 元素。

## UI 和通用輸入

UWP app 可以在具有不同輸入形式、螢幕解析度、DPI 密度以及其他獨特特性的許多不同類型裝置上執行。 Windows 10 提供新的通用控制項、配置面板和工具，可協助您針對您的 app 可能會在其上執行的裝置調整 UI。 例如，當您的 app 在桌上型電腦與行動裝置上執行時，您可以調整 UI 以利用不同的螢幕解析度。

您的 app UI 的某些層面會跨裝置自動調整。 例如按鈕和滑桿的控制項會跨裝置系列和輸入模式自動調整。 不過，您的 app 使用者經驗設計可能需要依據 app 執行所在的裝置進行調整。 例如，在小型、手持裝置上執行相片 app 時應該調整 UI，以確保適合單手操作使用。 在桌上型電腦上執行相片 app 時，應該調整 UI 以充分利用額外的螢幕空間。

Windows 使用下列功能，協助您讓您的 UI 以多個裝置為目標：

-   通用控制項與配置面板可協助您針對裝置的螢幕解析度最佳化您的 UI
-   通用輸入處理可讓您透過觸控、手寫筆、滑鼠或鍵盤，或像是 Microsoft Xbox 控制器的控制器接收輸入
-   工具會協助您設計適應不同螢幕解析度的 UI
-   跨裝置針對解析度和 DPI 差異進行彈性縮放比例調整

### 通用控制項與配置面板

Windows 10 包含新的控制項，例如行事曆和分割檢視。 Pivot 控制項先前僅適用於 Windows Phone，現在也適用於通用裝置系列。

控制項已經更新，可以在較大型的螢幕上正常運作、根據裝置可用的螢幕像素自行調整，並且可以與如鍵盤、滑鼠、觸控、手寫筆以及像是 Xbox 控制器的控制器等多種類型輸入正常配合運作。

您可能會發現您需要根據您的 app 執行所在之裝置的螢幕解析度來調整整體 UI。 例如，在傳統型裝置上執行的通訊 app 可能包含呼叫者的子母畫面和適合滑鼠輸入的控制項：

![傳統型通訊 app UI](images/adaptiveux-desktop.png)

不過，當 app 在手機上執行時，因為可以使用的螢幕實際空間較少，您的 app 可能會消除子母畫面檢視以及放大呼叫按鈕，以利單手操作：

![手機通訊 app UI](images/adaptiveux-phone.png)

為了協助您根據可用螢幕空間量調整您的整體 UI 配置，Windows 10 引進了彈性面板和設計狀態。

### 使用彈性面板設計彈性 UI

配置面板會依據可用空間，將大小和位置給予其子項。 例如，[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) 依序排序其子項 (水平或垂直)。 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 就像是 CSS 格線，將其子項放入儲存格。

新的 [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/dn879546) 會實作配置樣式，該配置依據其子元素之間的關係來定義。 它是用來建立可以適應螢幕解析度變更的 app 配置。 **RelativePanel** 會簡化重新排列元素的程序，方法是定義元素之間的關係，可讓您不需要使用巢狀配置即可建立更動態的 UI。

在下列範例中，**blueButton** 將出現在 **textBox1** 的右邊，無論方向或配置是否變更，**orangeButton** 會立即在 **blueButton** 下方顯示並且對齊，即使 **textBox1** 的寬度在將文字輸入它時變更。 它先前需要 **Grid** 中的列與欄，才可以達到這個效果，但是現在可以使用更少的標記來完成。

![relativepanel 範例](images/relativepane-standalone.png)

```XML
<RelativePanel>
    <TextBox x:Name="textBox1" Text="textbox" Margin="5"/>
    <Button x:Name="blueButton" Margin="5" Background="LightBlue" Content="ButtonRight" RelativePanel.RightOf="textBox1"/>
    <Button x:Name="orangeButton" Margin="5" Background="Orange" Content="ButtonBelow" RelativePanel.RightOf="textBox1" RelativePanel.Below="blueButton"/>
</RelativePanel>
```

### 使用視覺狀態觸發程序以建置可以適應可用螢幕空間的 UI

您的 UI 可能需要適應視窗大小的變更。 彈性的視覺狀態可讓您變更視覺狀態，以回應視窗的大小變更。

StateTriggers 會定義啟動視覺狀態的閾值，然後針對會觸發狀態變更的視窗大小設定適當的配置屬性。

在下列範例中，當視窗大小為 720 像素或寬度更大時，會觸發名為 **wideView** 的視覺狀態，然後排列**最佳評選遊戲**面板以顯示在右側，並且對齊**熱門免費遊戲**面板的頂端。

![視覺狀態觸發程序範例。 寬形檢視](images/relativepanel-wideview.png)

當視窗小於 720 像素時，會觸發 **narrowView** 視覺狀態，因為 **wideView** 觸發程序已不再能滿足，因此不再有效。 **narrowView** 視覺狀態會位於**最佳評選遊戲**面板下方，並且對齊**熱門付費遊戲**面板的左側：

![視覺狀態觸發程序範例。 窄格式檢視](images/relativepanel-narrowview.png)

以下是上述的視覺狀態觸發程序的 XAML。 由下方的「`...`」提到的面板定義已經移除以求簡潔。

```XML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideView">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="720" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="best.(RelativePanel.RightOf)" Value="free"/>
                    <Setter Target="best.(RelativePanel.AlignTopWidth)" Value="free"/>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="narrowView">
                <VisualState.Setters>
                    <Setter Target="best.(RelativePanel.Below)" Value="paid"/>
                    <Setter Target="best.(RelativePanel.AlignLeftWithPanel)" Value="true"/>
                </VisualState.Setters>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>
```

### 工具

根據預設，您可能想要盡量以最大的裝置系列為目標。 當您準備好查看您的 app 在特定裝置上的外觀以及配置時，使用 Visual Studio 中的裝置預覽工具列，預覽您的 UI 在小型或中型行動裝置、電腦或大型電視螢幕上的外觀。 如此您便能量身打造和測試彈性視覺狀態：

![Visual Studio 2015 裝置預覽工具列](images/vs2015-device-preview-toolbar.png)

您不需要提前決定您將支援的每個裝置類型。 您可以稍後將額外的裝置大小新增至您的專案。

### 彈性縮放比例

Windows 10 引進現有縮放比例模型的進化。 除了縮放向量內容之外，還有統一的比例因素集合，跨各種不同的螢幕大小和顯示器解析度針對 UI 元素提供一致的大小。 比例因素也相容於 iOS 與 Android 等其他作業系統的比例因素。 如此就可以輕鬆地在這些平台之間共用資產。

市集會根據裝置的 DPI 部分來選取要下載的資產。 只會下載最符合裝置的資產。

### 通用輸入處理

您可以使用通用控制項建置通用 Windows 應用程式，這些控制項會控制各種輸入，例如滑鼠、鍵盤、觸控、手寫筆以及控制器 (例如 Xbox 控制器)。 傳統上來說，筆跡已經只與手寫筆輸入相關聯，但是使用 Windows 10，您可以在某些裝置上使用觸控和使用任何指標輸入產生筆跡。 筆跡在許多裝置上 (包括行動裝置) 受到支援，只要幾行程式碼即可輕易結合。

下列 API 提供輸入的存取：

-   [**CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/dn298460) 是新的 API，可讓您在主要執行緒或背景執行緒上使用原始輸入。
-   [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) 藉由使用 **CoreInput**，將原始觸控、滑鼠和手寫筆資料統一成單一、一致的介面和事件集合，可以在主要執行緒或背景執行緒上使用。
-   [**PointerDevice**](https://msdn.microsoft.com/library/windows/apps/br225633) 是裝置 API，支援查詢裝置功能，讓您可以判斷哪些輸入形式可用於裝置上。
-   新的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) XAML 控制項和 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) Windows 執行階段 API 可讓您存取筆跡筆觸資料。

## 撰寫程式碼


您適用於 [Visual Studio 中的 Windows 10 專案](https://msdn.microsoft.com/library/windows/apps/dn609832.aspx#target_win10)的程式設計語言選項包含 Visual C++、C#、Visual Basic 和 JavaScript。 對於Visual C++、C# 和 Visual Basic，您可以使用 XAML 以獲得完全不失真的原生 UI 體驗。 對於 Visual C++，您可以選擇使用 DirectX 繪製，或是同樣使用 XAML。 對於 JavaScript，您的展示層將會是 HTML，而 HTML 當然是跨平台 Web 標準。 您的大部分程式碼與 UI 是通用的，會在任何位置以相同的方式執行。 但是對於為特定裝置系列量身打造的程式碼，以及為特定表單係數量身打造的 UI，您可以選擇使用彈性程式碼和彈性 UI。 讓我們看看這些不同的情況。

**呼叫由您的目標裝置系列實作的 API**

每當您想要呼叫 API，您必須知道您的 API 是否由您的 app 的目標裝置系列實作。 如果有疑問，您可以查閱 API 參考文件。 如果您開啟相關主題並查看「需求」區段，您會看到實作裝置系列是什麼。 假設您的 app 是以通用裝置系列版本 10.0.x.0 為目標，且您想要呼叫 [**Windows.UI.Core.SystemNavigationManager**](https://msdn.microsoft.com/library/windows/apps/dn893595) 類別的成員。 在這個範例中，裝置系列是「通用」。 最好進一步確認您想要呼叫的類別成員也會在您的目標中，在此案例中確實如此。 因此這個範例中，您現在知道 API 保證會存在於可以安裝您的 app 的每個裝置上，您可以如往常一般在程式碼中呼叫 API。

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += TestView_BackRequested;
```

另一個範例是假設您的 app 以 Xbox 裝置系列版本 10.0.x.0 為目標，而您想要呼叫的 API 的參考主題指出 API 已在 Xbox 裝置系列版本 10.0.x.0 中引進。 在這種情況下，一樣地 API 保證會存在於可以安裝您的 app 的每個裝置上。 因此您可以如往常一般在程式碼中呼叫該 API。

請注意，Visual Studio 的 IntelliSense 不會辨識 API，除非它們是由您的 app 的目標裝置系列或您參考的任何擴充功能 SDK 實作。 因此，如果您尚未參考任何擴充功能 SDK，可以確定 IntelliSense 中出現的任何 API 一定會在您的目標裝置系列中，您可以任意呼叫。

**呼叫「不」是由您的目標裝置系列實作的 API**

有可能當您想要呼叫 API，但是您的目標裝置系列未列在文件中。 在這種情況下您可以選擇撰寫彈性程式碼以呼叫該 API。

**使用 ApiInformation 類別撰寫彈性程式碼**

撰寫彈性程式碼有兩個步驟。 第一步是讓您想要存取的 API 可供您的專案使用。 若要這樣做，請將參考新增至擴充功能 SDK，它代表擁有您要有條件呼叫之 API 的裝置系列。 請參閱[擴充功能 SDK](../porting/w8x-to-uwp-porting-to-a-uwp-project.md#extension-sdks)。

第二個步驟是使用程式碼條件式中的 [**Windows.Foundation.Metadata.ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 類別，以測試您要呼叫的 API 是否存在。 每當您的 app 執行時都將會評估此條件，但只會將有 API 存在的裝置評估為 true，也才可供呼叫。

如果您只想要呼叫少數幾個 API，您可以使用像是這樣的 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) 方法。

```csharp
    // Note: Cache the value instead of querying it more than once.
    bool isHardwareButtonsAPIPresent =
        Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Phone.UI.Input.HardwareButtons");

    if (isHardwareButtonsAPIPresent)
    {
        Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
            HardwareButtons_CameraPressed;
    }
```

在這個案例中，我們可以確信 [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 類別的存在暗示 [**CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805) 事件的存在，因為類別和成員具有相同的需求資訊。 但是隨著時間經過，新成員會新增至已經引進的類別，這些成員會具有更新的「引進」版本號碼。 在這種情況下，不是使用 **IsTypePresent**，您可以使用 **IsEventPresent**、**IsMethodPresent**、**IsPropertyPresent** 和類似方法，測試個別成員是否存在。 這裡提供一個範例。

```csharp
    bool isHardwareButtons_CameraPressedAPIPresent =
        Windows.Foundation.Metadata.ApiInformation.IsEventPresent
            ("Windows.Phone.UI.Input.HardwareButtons", "CameraPressed");
```

裝置系列內的 API 組合可進一步細分成小部分，稱為 API 協定。 您可以使用 **ApiInformation.IsApiContractPresent** 方法以測試 API 協定是否存在。 如果您要測試相同版本的 API 協定中是否有大量 API 存在，這是相當有用的方法。

```csharp
    bool isWindows_Devices_Scanners_ScannerDeviceContract_1_0Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1, 0);
```

**UWP 中的 Win32 API**

C++/CX 中撰寫的 UWP app 或 Windows 執行階段元件都有屬於 UWP 之 Win32 API 的存取權。 這些 Win32 API 是由所有 Windows 10 裝置系列實作。 連結您的 app 與 Windowsapp.lib。 Windowsapp.lib 是「雨傘」 lib，提供 UWP API 的匯出。 連結到 Windowsapp.lib 會將 Windows 10 裝置系列上存在的 dll 的相依性新增至您的 app。

如需可用於 UWP app 之 Win32 API 的完整清單，請參閱[適用於 UWP app 的 API 集合](https://msdn.microsoft.com/library/windows/desktop/mt186421)和[適用於 UWP app 的 Dll](https://msdn.microsoft.com/library/windows/desktop/mt186422)。

## 使用者經驗

通用 Windows App 可讓您充分利用執行該 App 之裝置的獨特功能。 您的 App 可以利用傳統型裝置的所有功能、平板電腦上直接操作的自然互動 (包括觸控和手寫筆輸入)、行動裝置的可攜性與便利性、[Surface Hub](http://go.microsoft.com/fwlink/?LinkId=526365) 的共同作業功能，以及其他支援 UWP App 的裝置。

良好的[設計](http://go.microsoft.com/fwlink/?LinkId=258848)是決定您 App 與使用者的互動方式、外觀，以及功能的程序。 使用者經驗在判斷使用者使用您的 app 時有多愉快佔有舉足輕重的地位，因此請不要跳過這個步驟。 [設計基本知識](https://dev.windows.com/design)會為您介紹如何設計通用 Windows app。 請參閱[適用於設計人員的通用 Windows 平台 (UWP) app 簡介](https://msdn.microsoft.com/library/windows/apps/dn958439)，以取得設計能讓使用者滿意的 UWP app 的詳細資訊。 開始撰寫程式碼之前，請參閱[裝置入門](../input-and-devices/device-primer.md)，協助您思考在您要做為目標的所有不同表單係數上使用您的 app 的互動體驗。

![執行 Windows 的裝置](images/1894834-hig-device-primer-01-500.png)

除了在不同裝置上的互動之外，[計劃您的 app](https://msdn.microsoft.com/library/windows/apps/hh465427) 以納入跨多個裝置工作的好處。 例如：

-   使用[雲端服務](http://go.microsoft.com/fwlink/?LinkId=526377)跨裝置同步。 了解如何[連線到 Web 服務](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)以支援您的 app 體驗。

-   請考慮如何支援使用者從一部裝置移到另一部裝置，帶領他們離開原來的位置。 在您的計劃中包含[通知](https://msdn.microsoft.com/library/windows/apps/mt187203)和[在 App 內購買](https://msdn.microsoft.com/library/windows/apps/mt219684)。 這些功能應該可以跨裝置運作。

-   使用 [UWP app 瀏覽設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)設計您的工作流程，以容納行動、小螢幕與大螢幕裝置。 [配置您的使用者介面](https://msdn.microsoft.com/library/windows/apps/dn958435)以回應不同的螢幕大小與解析度。

-   請考慮您的 app 是否有不適用於小型行動裝置螢幕的功能。 也可能有不適用於靜止桌上型電腦的區域，需要運用行動裝置。 例如，[位置](https://msdn.microsoft.com/library/windows/apps/mt219698)周圍的大部分案例暗示行動裝置。

-   請考慮容納多個輸入形式的方式。 請參閱[互動的指導方針](https://msdn.microsoft.com/library/windows/apps/dn611861)以了解使用者如何使用 [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)、[語音](https://msdn.microsoft.com/library/windows/apps/dn596121)、[觸控互動](https://msdn.microsoft.com/library/windows/apps/hh465370)、[觸控式鍵盤](https://msdn.microsoft.com/library/windows/apps/hh972345)等等方式與您的 app 互動。

    請參閱[文字和文字輸入的指導方針](https://msdn.microsoft.com/library/windows/apps/dn611864)以取得更多傳統互動經驗。

## 透過您的儀表板提交通用 Windows 應用程式


全新整合的 Windows 開發人員中心儀表板，可讓您集中管理和提交您為所有 Windows 裝置開發的 app。 新功能不只簡化程序，同時還讓您更好控制。 您在這裡還能找到結合[支付詳細資料](https://msdn.microsoft.com/library/windows/apps/dn986925)的詳細[分析報告](https://msdn.microsoft.com/library/windows/apps/mt148522)、[促銷應用程式和吸引客戶](https://msdn.microsoft.com/library/windows/apps/mt148526)的方式，以及更多好用功能。

請參閱[使用整合的 Windows 開發人員中心儀表板](../publish/using-the-windows-dev-center-dashboard.md)以了解如何提交您的 app 在 Windows 市集中發行。

 

 



<!--HONumber=Jul16_HO2-->


