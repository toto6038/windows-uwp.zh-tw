---
Description: 中樞控制項使用階層式瀏覽模式來支援使用關聯式資訊架構的 app。
title: 中樞控制項
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: 中樞
template: detail.hbs
---
# 中樞控制項/模式


中樞控制項可讓您將 app 內容組織成不同但相關的區段或類別。 中樞中區段的目的是在於能夠以慣用的順序進行周遊，並可做為更詳細經驗的起點。

![中樞範例](images/hub_example_tablet.png)

中樞中的內容會以健全的移動瀏覽檢視方式顯示，讓使用者能夠輕鬆檢視新功能、可用功能及相關項目。 中樞通常會有頁面標頭，而多個內容區段的每個區段都會有區段標頭。

中樞控制項有數個功能，使它能夠用於建置內容瀏覽模式。

-   **視覺化瀏覽**

    中樞可讓內容以多種、簡短且容易瀏覽的陣列方式顯示。

-   **分類**

    每個中樞區段都可以使用邏輯順序安排其內容。

-   **混合內容類型**

    有了混合內容類型之後，可變的資產大小和比例就變得很常見。 中樞可讓每種內容類型成為唯一的類型，且可以在每個中樞區段中整齊地排列。

-   **可變的頁面和內容寬度**

    中樞變成全景模型之後，其區段寬度就可以變更。 這很適合用於深度不同的內容，且可以讓由少到多的項目數目同時具備同樣優美的格式。

-   **彈性架構**

    如果您想要讓您的 app 架構維持淺層狀態，您可以在中樞區段摘要中包含所有通道內容。

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**Hub 類別 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn251843)
-   [**HubSection 類別 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn251845)
-   [**Hub 物件 (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn255137)


## 這是正確的控制項嗎？

中樞控制項適用於顯示依階層排列的大量內容。 中樞會排定新內容的瀏覽與探索之優先順序，讓它們適用於在市集或是媒體集合中顯示項目。

中樞只是多個瀏覽元素的其中一項；若要深入了解瀏覽模式與其他瀏覽元素，請參閱[通用 Windows 平台 (UWP) app 的瀏覽設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)。

## 中樞架構

中樞控制項具備階層式瀏覽模式，此模式支援使用關聯式資訊架構的應用程式。 中樞由不同的內容類別組成，每個類別均對應至應用程式的區段頁面。 區段頁面可以使用任何形式顯示，只要能夠呈現出最佳的狀況和區段所包含的內容。

![階層式 Food with Friends 應用程式的線框](images/navigation_diagram_food_with_friends_app_new.png)

## 移動瀏覽/捲動的配置

有一些方式可用來在中樞中配置及瀏覽內容；但是請確定中樞中列出的內容會一律以和中樞捲動方向垂直的方向移動瀏覽。

**水平移動瀏覽**

![水平移動瀏覽中樞範例](images/controls_hub_horizontal_pan.png)
**垂直移動瀏覽**

![垂直移動瀏覽中樞範例](images/controls_hub_vertical_pan.png)
**具備垂直捲動清單/格線的水平移動瀏覽**

![具備垂直捲動清單的水平移動瀏覽中樞範例](images/controls_hub_horizontal_vertical_scroll.png)
**具備水平捲動清單/格線的垂直移動瀏覽**

![水平移動瀏覽中樞範例](images/controls_hub_vertical_horizontal_scroll.png)

## 範例

中樞提供大量的設計彈性。 這可讓您設計具有各式各樣引人注目及豐富視覺體驗的應用程式。 您可以為第一個群組使用主角影像或內容區段；主角的大型影像可以在不失去中心焦點的情況下，從垂直與水平方向裁剪。 以下是單個主角影像以及如何針對橫向、直向及窄格式寬度裁剪影像的範例。

![針對不同視窗大小裁剪主角影像](images/hub_hero_cropped2.png)

在行動裝置上，一次只能看到一個中樞區段。

![小螢幕上的中樞模式範例](images/phone_hub_example.png)

## 建議

-   若要讓使用者知道中樞區段中有更多內容，建議您裁剪內容，以方便使用者一次檢視某一數量的內容。
-   視應用程式的需求而定，您可以新增數個中樞區段到中樞控制項，每一區段都提供獨特的功能用途。 例如，一個區段可能包含一系列連結和控制項，另一個區段則可能是縮圖影像的存放庫。 使用者可以使用中樞控制項內建的手勢支援，在這些區段之間移動瀏覽。
-   動態自動重排群組中的內容是適應不同視窗大小的最佳方式。
-   如果中樞包含很多區段，請考慮新增語意式縮放。 這樣當應用程式調整到窄格式寬度大小時，會更易於找到區段。
-   我們建議您不要在中樞區段中建立導向其他中樞的項目；您可以改為使用互動式標題來瀏覽至其他中樞區段或頁面。
-   中樞是一個開始點，並且可加以自訂以符合您應用程式的需求。 您可以變更中樞的下列層面：
    -   區段數量
    -   每個區段的內容類型
    -   區段的位置及順序
    -   區段大小
    -   區段之間的間距
    -   區段與中樞頂端或底部之間的間距
    -   標頭和內容中文字樣式和大小
    -   背景、區段、區段標頭及區段內容的色彩

\[此文章包含 UWP app 與 Windows 10 專屬的資訊。 如需 Windows 8.1 指導方針，請下載 [Windows 8.1 指導方針 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## 相關文章
-----------------------------------------------

**適用於設計人員**
- [瀏覽基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)

**適用於開發人員 (XAML)**
- [階層式瀏覽，從開始到完成](https://msdn.microsoft.com/library/windows/apps/xaml/dn440585)
- [**Windows.UI.Xaml.Controls Hub 類別**](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [XAML Hub 控制項範例](http://go.microsoft.com/fwlink/p/?LinkID=310072)
- [使用中樞](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)


<!--HONumber=Mar16_HO1-->


