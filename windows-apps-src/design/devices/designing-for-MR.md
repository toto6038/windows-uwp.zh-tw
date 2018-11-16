---
author: jken
Description: Design your app so that it looks good and functions well in Mixed Reality.
title: 設計混合實境
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: 混合實境、Hololens、擴增實境、注視、語音、控制器
ms.author: jken
ms.date: 2/5/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: e6bba6c22b7f0055a93bfd1826bc3a2acc0f2164
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6968382"
---
# <a name="designing-for-mixed-reality"></a>設計混合實境

在混合實境中將您的應用程式設計美觀，並充分利用新的輸入法。

## <a name="overview"></a>概觀

[混合實境](https://developer.microsoft.com/windows/mixed-reality/mixed_reality)是實際環境與數位世界混合的結果。 混合實境體驗的頻譜包含一個極端的裝置，例如 HoloLens (混合真實世界與電腦產生的內容的裝置)，以及其他虛擬實境的完全沈浸式檢視 (如同使用 Windows Mixed Reality 頭戴式裝置檢視)。 請查閱 [混合實境應用程式的類型](https://developer.microsoft.com/en-us/windows/mixed-reality/types_of_mixed_reality_apps)，了解體驗的範例有什麼不同。

雖然可以依照本主題中下列指導方針改進使用者的體驗，幾乎所有現有的 UWP app 會在混合實境的環境中執行，如同 2D app 沒有變更。

![混合實境檢視](images/MR-01.png)

HoloLens 和 Windows Mixed Reality 頭戴式裝置皆支援 UWP 平台上執行的應用程式，且皆支援兩種不同的體驗。 

### <a name="2d-vs-immersive-experience"></a>2D 與沈浸式體驗

沉浸式 app 取代了使用者可見的整體顯示器，將其置於 app 建立的顯示方式中心。 例如，沈浸式遊戲可能會將使用者置於外星球的表面，或旅遊導覽應用程式可能將使用者置於南美洲的村莊。 建立沈浸式應用程式需要 3D 圖形或擷取立體的影片。 通常使用第三方遊戲引擎，例如 Unity，或 DirectX 來開發沈浸式應用程式。

如果您要建立沈浸式應用程式，您應該瀏覽 [Windows Mixed Reality 開發人員中心](https://developer.microsoft.com/windows/mixed-reality)了解詳細資訊。

2D 應用程式在使用者檢視中，如同傳統平面視窗執行。 在 HoloLens 上，這表示釘選檢視於專頁，或在使用者真實世界客廳或辦公室空間裡的一點。 Windows Mixed Reality 頭戴式裝置中，應用程式釘選到在 [混合實境首頁](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home)(有時稱為*懸崖之屋*) 的專頁。

![混合實境中執行多個應用程式](images/MR-multiple.png)


這些 2D 應用程式不會取代整個檢視：放置它們於其中。 多個 2D 應用程式可以一次存在環境中。

本主題中其餘的部分討論 2D 體驗的設計考量。

## <a name="launching-2d-apps"></a>啟動 2D 應用程式

![混合實境 [開始] 功能表](images/MR-start-options.png)

所有應用程式皆從 [開始] 功能表啟動，但也可以建立一個 3D 物件做為應用程式啟動程式。 請參閱 [此影片](https://www.youtube.com/watch?v=TxIslHsEXno) 了解詳細資訊。

## <a name="the-2d-app-input-overview"></a>2D 應用程式輸入概觀

在 HoloLens 和混合實境平台上皆支援鍵盤和滑鼠。 您可以透過藍牙直接與 HoloLens 配對鍵盤和滑鼠。 混合實境應用程式支援連接至主機電腦的滑鼠與鍵盤。 需要控制項的細緻層級時，這兩項在此情況下非常實用。

也支援其他更自然的輸入法，且使用者沒有坐在桌子旁使用眼前的實際鍵盤，或需要細微控制項時，這些輸入法可能特別實用。

不需要任何額外的硬體或程式設計，使用 2D 應用程式時，應用程式會使用注視 (您的使用者正在尋找的向量) 做為滑鼠指標。 實作時如同滑鼠指標暫留在虛擬場景中的項目上。

在一般互動中，您的使用者會查看您應用程式中的控制項，使其反白顯示。 使用者會在觸發動作時，使用手勢 (在 HoloLens 上) 或控制器或提供語音命令。 如果使用者選取文字欄位輸入時，會出現軟體鍵盤。 


![在混合實境中的快顯鍵盤](images/MR-keyboard.png)

請務必注意所有這類互動會自動發生，在您的部分不須額外撰寫程式碼，如同 UWP 平台上執行的結果。 HoloLens 和混合實境頭帶式裝置的輸入，會像 2D 應用程式的觸控輸入一樣顯示。 這表示許多 UWP 應用程式會執行，且預設情況下，在混合實境中可使用。 

也就是說，有一些額外的工作，可大幅改善體驗。 例如，[語音控制](https://developer.microsoft.com/windows/mixed-reality/voice_design)可以非常有效。 HoloLens 和混合實境環境都支援語音命令，來啟動以及與應用程式互動，且包括語音支援會像這種方法的自然擴充功能一樣顯示。 請參閱[語音互動]( https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)了解將語音支援新增至您的 UWP 應用程式的詳細資訊。 


### <a name="selecting-the-right-controller"></a>選取正確的控制器

![混合實境運動控制器](images/MR-controllers.png)

設計了幾個全新的輸入法，專門給混合實境使用，尤其是：

* [交付手勢](https://developer.microsoft.com/windows/mixed-reality/gestures)(僅限 HoloLens，但僅限用於啟動 2D 應用程式)
* [遊戲台支援](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (兩個環境皆可) 
* [Clicker 裝置](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories)(僅限 HoloLens)
* [運動控制器](https://developer.microsoft.com/windows/mixed-reality/motion_controllers) (僅限混合實境裝置，上述所示。) 

這些控制器與虛擬物件進行互動，看起來自然且精確。 您可取得部分免費的互動。 例如，HoloLens 選取手勢，或按一下運動控制器的 Windows 鍵或觸發程序會產生您預期，同樣地，在您的部分未編碼的輸入的回應。

其他時候，您會想要新增程式碼，善加利用額外的資訊及可用的輸入。 例如，運動控制器可用於執行具有精細層級控制的物件，如果您撰寫程式碼，考慮其位置和按鈕的按壓動作。

> [!NOTE]
> 總結來說：指導主體應盡可能永遠為使用者提供自然且無障礙的輸入法。


## <a name="2d-app-design-considerations-functionality"></a>2D 應用程式設計注意事項：功能

建立可能會用於混合實境平台上的 UWP 應用程式時，有幾件事，請牢記。

* 使用運動控制器、遊戲台或手勢時，拖放功能可能無法正常運作。 如果您的應用程式仰賴拖放功能，則您將需要提供另一個方法支援此動作，例如呈現一個對話方塊確認是否將物件移動到新的位置。

* 要知道如何變更音效。 如果您的應用程式產生音效，則聲音的來源會顯示為虛擬世界中您應用程式釘選的位置。 當使用者從應用程式移動離開，將會降低音效。 請參閱 [空間音效](https://developer.microsoft.com/windows/mixed-reality/spatial_sound)了解更多資訊。

* 請考慮檢視欄位，並提供能供性。 並非每部裝置都會提供像電腦螢幕一樣大的檢視欄位。 請參閱[全像攝影框架](https://developer.microsoft.com/windows/mixed-reality/holographic_frame)了解完成詳細資料。 此外，使用者可能會某些原位執行中應用程式的距離。 也就是說，應用程式在世界中 (真實或虛擬) 不同的位置裡可能會出現已釘選的專頁。 您的應用程式可能需要吸引使用者的注意力，或考量整個檢視無法隨時看到。 快顯通知可供使用，但吸引使用者注意力的另一個方法是產生音效或[語音](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs)警示。

* 2D 應用程式會自動獲得[應用程式列](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box) 讓使用者在虛擬環境中移動及調整其大小。 檢視可以垂直重新調整大小，或重新調整大小維持相同的外觀比例。


## <a name="2d-app-design-considerations-uiux"></a>2D 應用程式設計注意事項：UI/UX

* XAML 控制項實作 [Fluent Design 系統](https://docs.microsoft.com/windows/uwp/design/fluent-design-system/) 例如[瀏覽檢視](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview)，以及效果，例如[壓克力](https://docs.microsoft.com/windows/uwp/design/style/acrylic)所有作業尤其適用於 2D 混合實境應用程式。

* 在混合實境裝置中，或至少在混合實境模擬器中，測試您應用程式的文字和視窗大小。 您的應用程式會有 853x480 有效像素的預設視窗大小。 使用大字型， (建議使用的點大小大約是 32 )，以及閱讀 [更新您現有的 Hololens 跨平台應用程式](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)。 文章 [印刷樣式](https://developer.microsoft.com/windows/mixed-reality/typography)涵蓋本主題的詳細資料。 在 Visual Studio 中工作時，有 57" HoloLens 2D 應用程式的 XAML 設計編輯器設定，其供應有正確縮放及尺寸的檢視。 

![混合實境應用程式中顯示的文字應該要大。](images/MR-text.png)

* [您的注視是滑鼠](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting)。 當使用者查看項目時，做為 **touch hover** 事件，只需查看物件可能會觸發非故意的快顯或其他不想要的互動。 如果應用程式目前在混合實境中執行與變更此行為，您可能需要偵測。 請查閱 **執行階段支援**，於下方。 

* 當使用者注視項目或點指向運動控制器，會發生 **touch hover** 事件。 這包含**PointerPoint**，其中 **PointerType** 是 **Touch**，但 **IsInContact** 是 **\ [false\]**。 發生某種認可的形式時 (例如，按下遊戲台 A 按鈕、按下 clicker 裝置、按下運動控制器觸發程序，或語音辨識出現「選取」)，發生 **touch press**，**PointerPoint** 有 **IsInContact** 變成 ** true**。 請參閱 [觸控互動](https://docs.microsoft.com/windows/uwp/design/input/touch-interactions)，了解這些輸入事件的詳細資訊。

* 請記住，注視不像滑鼠指向那麼準確。 較小的滑鼠目標或按鈕可能會造成使用者的挫折，因此請適當的重新調整控制項。 如果是為觸控而設計，可在混合實境中工作，但您會決定在執行階段放大一些按鈕。 請參閱 [更新您現有的 Hololens 跨平台應用程式](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)。

* 當缺少光線時，HoloLens 定義顏色為黑色。 它不會經過轉譯，直接讓「真實世界」顯示。 如果這會造成混淆，則您的應用程式不應該使用黑色。 在混合實境頭戴式裝置中，黑色便是黑色。

* HoloLens 不支援應用程式中的色彩佈景主題，且預設值為藍色，確保使用者的最佳體驗。 如需更多有關選取色彩的建議，請洽詢 [本主題](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials)其討論混合實境設計中使用的色彩和資料。


## <a name="other-points-to-consider"></a>其他考慮的點

* 雖然 [傳統型橋接器](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)可協助您將現有 (Win32) 的傳統型應用程式帶到 Windows 10 和「Microsoft Store，但無法建立這次在 HoloLens 上或在混合實境中執行的應用程式。




## <a name="runtime-support"></a>執行階段支援

可讓您的應用程式判斷是否在執行階段中在混合實境裝置上執行，且使用其做為重新調整控制器的機會，或以其他方式在頭戴式裝置上最佳化應用程式的使用。

以下是程式碼的片段，只有當在混合實境裝置上使用應用程式時，可重新調整 XAML TextBlock 控制項中的文字大小。

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 16;
            }

```





## <a name="related-articles"></a>相關文章


* [從殼層使用 API 的應用程式的目前限制](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [建置 2D 應用程式](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens：為 Microsoft HoloLens 建置 UWP 2D 應用程式](https://channel9.msdn.com/Events/Build/2016/B854)
* [條件式 XAML](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml)


