---
author: QuinnRadich
Description: The hub control uses a hierarchical navigation pattern to support apps with a relational information architecture.
title: 中樞控制項
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: Hub
template: detail.hbs
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4fecff4fdd836f271b9fb066c58f159111c37772
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "4506889"
---
# <a name="hub-controlpattern"></a>中樞控制項/模式

 


中樞控制項可讓您將應用程式內容組織成不同但相關的區段或類別。 中樞中區段的目的是在於能夠以慣用的順序進行周遊，並可做為更詳細經驗的起點。

> **重要 API**：[Hub 類別](https://msdn.microsoft.com/library/windows/apps/dn251843)、[HubSection 類別](https://msdn.microsoft.com/library/windows/apps/dn251845)

![中樞範例](images/hub_example_tablet.png)

中樞中的內容可以採用全景檢視方式顯示，讓使用者能夠檢視新功能、可用功能及相關項目。 中樞通常會有頁面標頭，而每個內容區段都會有一個區段標頭。


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

中樞控制項適用於顯示依階層排列的大量內容。 中樞會為新內容的瀏覽與探索排定優先順序，讓它們不論是顯示市集中的項目還是媒體集合中的項目，都相當有用。

Hub 控制項有數個功能，可使它在建置內容瀏覽模式上能夠良好運作。

-   **視覺化瀏覽**

    中樞可讓內容以多種、簡短且容易瀏覽的陣列方式顯示。

-   **分類**

    每個中樞區段都可以使用邏輯順序安排其內容。

-   **混合內容類型**

    有了混合內容類型之後，可變的資產大小和比例就變得很常見。 中樞可讓每種內容類型成為唯一的類型，且可以在每個中樞區段中整齊地排列。

-   **可變的頁面和內容寬度**

    採用全景模型之後，中樞便可允許變更其區段寬度。 這非常適用於具有不同深度或數量的內容。

-   **彈性架構**

    如果您想要讓您的應用程式架構維持淺層狀態，您可以將所有通道內容都納入中樞區段摘要中。

中樞只是您可以使用的數個瀏覽元素其中之一；若要深入了解瀏覽模式及其他瀏覽元素，請參閱[通用 Windows 平台 (UWP) 應用程式的瀏覽設計基本知識](../basics/navigation-basics.md)。

## <a name="hub-architecture"></a>中樞架構

中樞控制項具備階層式瀏覽模式，此模式支援使用關聯式資訊架構的應用程式。 中樞由不同的內容類別組成，每個類別均對應至應用程式的區段頁面。 區段頁面可以使用任何形式顯示，只要能夠呈現出最佳的狀況和區段所包含的內容。

![階層式 Food with Friends 應用程式的線框](images/navigation_diagram_food_with_friends_app_new.png)

## <a name="layouts-and-panningscrolling"></a>移動瀏覽/捲動的配置

有一些方式可用來在中樞中配置及瀏覽內容；但是請確定中樞中列出的內容會一律以和中樞捲動方向垂直的方向移動瀏覽。

**水平移動瀏覽**

![水平移動瀏覽中樞的範例](images/controls_hub_horizontal_pan.png)
**垂直移動瀏覽**

![垂直移動瀏覽中樞的範例](images/controls_hub_vertical_pan.png)
**具備垂直捲動清單/格線的水平移動瀏覽**

![具備垂直捲動清單之水平移動瀏覽中樞的範例](images/controls_hub_horizontal_vertical_scroll.png)
**具備水平捲動清單/格線的垂直移動瀏覽**

![水平移動瀏覽中樞範例](images/controls_hub_vertical_horizontal_scroll.png)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/Hub">開啟應用程式並查看 Hub 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

中樞提供大量的設計彈性。 這可讓您設計具有各式各樣引人注目及豐富視覺體驗的應用程式。 您可以為第一個群組使用主角影像或內容區段；主角的大型影像可以在不失去中心焦點的情況下，從垂直與水平方向裁剪。 以下是單個主角影像以及如何針對橫向、直向及窄格式寬度裁剪影像的範例。

![針對不同視窗大小裁剪主角影像](images/hub_hero_cropped2.png)

在行動裝置上，一次只能看到一個中樞區段。

![小螢幕上的中樞模式範例](images/phone_hub_example.png)

## <a name="recommendations"></a>建議

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

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [Hub 類別](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [瀏覽基本知識](../basics/navigation-basics.md)
- [使用中樞](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)
- [XAML Hub 控制項範例](http://go.microsoft.com/fwlink/p/?LinkID=310072)
