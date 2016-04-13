---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
視覺層
Windows.UI.Composition API 可讓您存取架構層 (XAML) 與圖形層 (DirectX) 之間的組合層。
---
# 視覺層

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 Windows 10 中，已完成重要的工作來為所有 Windows 應用程式 (不論是傳統型或行動式) 建立新的整合式撰寫器和轉譯引擎。 該工作的結果即是稱為 Windows.UI.Composition 的「組合 WinRT API」，此 API 可讓您存取新的輕量型 Composition 物件，連同新的 Compositor 導向 Animations 與 Effects。

Windows.UI.Composition 是一個宣告式的[保留模式](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) API，您可以從任何「通用 Windows 平台」(UWP) 應用程式呼叫它以直接在應用程式中建立組合物件、動畫及效果。 API 是一個對現有架構 (例如 XAML) 的強大補充，可為 UWP 應用程式的開發人員提供 一個熟悉的 C# 表面來新增到其應用程式。 這些 API 可用來建立無 DX 樣式架構的應用程式。

XAML 開發人員可以使用 C#「下拉」到組合層以在組合層進行自訂工作，也就是使用 WinRT 在其 XAML 應用程式中建立物件的「組合島」，而不是一路下拉到圖形層並使用 DirectX 和 C++ 來進行任何自訂 UI 工作。

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"> </span> <span id="composition_objects_and_the_compositor"> </span> <span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"> </span>組合物件與撰寫器

組合物件是由 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 所建立，用來做為組合物件的處理站。 撰寫器可以建立 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 物件，這些物件可用來建立 API 中所有其他功能與 「組合」物件所使用並倚賴的視覺化樹狀結構。

API 可讓開發人員定義及建立一或多個 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 物件，每個物件各代表視覺化樹狀結構中的單一節點。

「視覺效果」可以是其他「視覺效果」的容器，也可以裝載內容「視覺效果」。 API 藉由為存在於階層中的特定工作 提供一組明確的 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 物件，讓使用更為便利：

-   [
            **Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – 基底物件。 大多數的屬性都在這裡，並且會被其他視覺物件繼承。
-   [
            **ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – 衍生自 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)，並且會新增插入子系視覺效果的能力。
-   [
            **SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – 衍生自 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810)，並且包含影像、效果及交換鏈結形式的內容。
-   [
            **Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) – 管理應用程式與系統撰寫器處理程序之間關係的物件處理站。

撰寫器除了是一組豐富的動畫和效果之外，也是一些其他用來裁剪或轉換樹狀結構中視覺效果之組合物件的處理站。

## <span id="Effects_System"> </span> <span id="effects_system"> </span> <span id="EFFECTS_SYSTEM"> </span>效果系統

Windows.UI.Composition 支援可以產生動畫效果、自訂及鏈結的即時效果。 效果包含 2D 仿射轉換、算術複合、混合、色彩來源、組合、 對比、曝光、灰階、色差補正移轉、色調旋轉、反轉、飽和、復古，色溫及濃淡。

如需詳細資訊，請參閱[組合效果](composition-effects.md)概觀。

## <span id="Animation_System"> </span> <span id="animation_system"> </span> <span id="ANIMATION_SYSTEM"> </span>動畫系統

Windows.UI.Composition 包含一個豐富生動、與架構無關的動畫系統，可讓您設定兩種類型的動畫：主要畫面格動畫和運算式動畫。 它們可用來移動視覺物件、 驅動轉換或裁剪，或是以動畫表達效果。 藉由直接在撰寫器處理程序中執行，這確保了流暢性與縮放比例，讓您能夠執行大量同時、獨特的動畫。

如需詳細資訊，請參閱[組合動畫](composition-animation.md)概觀。

## <span id="XAML_Interoperation"> </span> <span id="xaml_interoperation"> </span> <span id="XAML_INTEROPERATION"> </span>XAML 交互操作

除了從頭開始建立視覺化樹狀結構之外，「組合 API」也可以使用 [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908) 中的 [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) 類別與現有的 XAML UI 交互操作。


**注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## <span id="Additional_Resources_"> </span> <span id="additional_resources_"> </span> <span id="ADDITIONAL_RESOURCES_"> </span>其他資源：

-   閱讀 Kenny Kerr 針對這個 API 撰寫的 MSDN 文章：[圖形與動畫 - Windows 組合邁向 10](https://msdn.microsoft.com/magazine/mt590968) (英文)
-   [Composition GitHub](https://github.com/Microsoft/composition) 中的組合範例 (英文)。
-   [
            **API 的完整參考文件**](https://msdn.microsoft.com/library/windows/apps/Dn706878)。
-   已知問題：[已知問題](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues)。

 

 






<!--HONumber=Mar16_HO1-->


