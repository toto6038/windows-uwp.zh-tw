---
author: stevewhims
description: Windows app 在電腦、行動裝置與任何其他類型的裝置，都有相同的外觀及操作方式。 使用者介面、輸入及互動模式皆非常相似，而在裝置之間移動的使用者會欣然感受到熟悉的體驗。
title: 尺寸與 UX 的移植 WindowsPhone Silverlight 到 UWP
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 809cf2691a2bc7b7c72d4ba031fa4c6b45335dde
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6265412"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-form-factor-and-ux"></a>尺寸與 UX 的移植 WindowsPhone Silverlight 到 UWP


前一個主題是[移植商務與資料層](wpsl-to-uwp-business-and-data.md)。

Windows app 在電腦、行動裝置與任何其他類型的裝置，都有相同的外觀及操作方式。 使用者介面、輸入及互動模式皆非常相似，而在裝置之間移動的使用者會欣然感受到熟悉的體驗。 Windows 10 會決定轉譯裝置，例如實體大小、 預設方向與有效像素解析度到通用 Windows 平台 (UWP) 應用程式的方式有何差異。 好消息是系統會使用智慧型概念 (例如有效像素)，為您處理大部分的繁重工作。

## <a name="different-form-factors-and-user-experience"></a>不同的尺寸與使用者體驗

不同裝置有多種外觀比例不同的直向和橫向解析度。 UWP app 的介面、文字與資產的視覺比例會如何縮放？ 如何除了滑鼠與鍵盤輸入之外，也支援觸控輸入？ 而如果應用程式支援在各種不同大小、各有不同檢視距離的裝置上使用觸控，控制項要如何在不同的像素密度顯示成正確大小的觸控目標，*「同時」* 其內容在不同的距離又能清楚可讀？ 下列各節將說明您需要知道的事項。

## <a name="what-is-the-size-of-a-screen-really"></a>說實在的，什麼是螢幕大小？

簡單說來，這是相當主觀的，因為它不只取決於顯示器的客觀大小，也取決於您與它的距離有多遠。 主觀意謂著我們必須以使用者的角度出發，而這就是優良應用程式的開發人員無論如何都會做到的。

客觀來說，螢幕是以英吋與實體 (原始) 像素的單位來測量。 知道這兩個計量，您就可以知道一英吋能夠容納多少像素。 這就是像素密度，也稱為 DPI (每英吋點數) 或 PPI (每英吋像素數)。 而 DPI 的倒數就是像素的實體大小 (以英吋的分數表示)。 像素密度也稱為 *「解析度」*，雖然此詞彙通常被寬鬆地用來表示像素計數。

隨著檢視距離增加，所有這些客觀計量也就 *「看起來」* 變得較小，它們會解析成螢幕的「*有效大小」* 與其 *「有效解析度」*。 與您眼睛保持的距離最接近的通常是您的手機，其次是您的平板電腦，接著是您的電腦監視器，而最遠的是 [Surface Hub](http://www.microsoft.com/microsoft-surface-hub) 裝置與電視。 為了做為補償，裝置傾向於隨著檢視距離的增加而在客觀上變得較大。 當您設定 UI 元素大小時，您是使用稱為有效像素 (epx) 的單位設定大小。 與 windows 10 會考慮 DPI 和的一般檢視距離的裝置，來計算您的 UI 元素，以提供最佳檢視體驗的實體像素的最佳大小。 請參閱[檢視/有效像素、檢視距離與縮放比例](wpsl-to-uwp-porting-xaml-and-ui.md)。

即便如此，我們還是建議您使用多種不同的裝置測試您的 app，以親自確認各種體驗。

## <a name="touch-resolution-and-viewing-resolution"></a>觸控解析度與檢視解析度

能供性 (UI 小工具) 必須是適合觸控的大小。 因此，觸控目標需要在各種可能具有不同像素密度的不同裝置上大致維持其實體大小。 針對這方面，有效像素也會協助您，它們會在不同裝置上縮放將像素密度納入考量，以達到對觸控目標而言較為理想，且約略一致的實體大小。

文字必須是適合閱讀的正確大小 (20 英吋的檢視距離搭配 12 點的文字大小是一個不錯的經驗法則)，而影像必須是適合檢視距離的正確大小和有效解析度。 在不同裝置上，該相同有效像素縮放可讓 UI 元素維持適當大小與可讀性。 文字與其他向量型圖形會自動以最佳方式調整。 如果是點陣 (點陣圖) 型圖形，在開發人員有提供單一大尺寸資產的情況下，這些圖形也會自動調整。 但是，最好是開發人員為每個資產提供各種大小，這樣就會自動針對目標裝置的縮放比例載入適當大小的資產。 如需詳細資訊，請參閱[檢視/有效像素、檢視距離與縮放比例](wpsl-to-uwp-porting-xaml-and-ui.md)。

## <a name="layout-and-adaptive-visual-state-manager"></a>版面配置與調適型 Visual State Manager

我們已經說明了解螢幕大小意義所涉及的各種因素。 現在讓我們來思考您應用程式的版面配置，以及如何在有額外螢幕空間時加以利用。 請想想這個來自專為在窄格式行動裝置上執行而設計之簡易應用程式的頁面。 這個頁面在較大的螢幕上應該看起來怎樣？

![移植的 Windows Phone 市集 app](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

行動裝置版本的方向僅限於直向，因為這是書籍清單的最佳外觀比例；我們會針對文字頁面採用相同的版面配置方式，此頁面在行動裝置上最好是保持單欄形式。 但是電腦和平板電腦螢幕在任一方向都較大，因此在大型裝置上，行動裝置的限制似乎就顯得不必要。

將應用程式做視覺縮放，使其看起來就像只是變得較大的行動裝置版本，並無法充分利用裝置與其額外的空間，而這無法為使用者提供良好的使用體驗。 我們應該要考慮顯示更多內容，而不是讓相同的內容變得更大。 即使是在平板手機上，我們也可以多顯示幾列內容。 我們可以使用額外的空間來顯示不同的內容 (例如廣告)，或是將清單方塊變更成清單檢視，並讓它在情況允許時將項目包裝在多欄中，以此方式利用空間。 請參閱[清單與格線檢視控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/mt186889)。

除了新控制項，例如清單檢視與格線檢視，大部分的確立版面配置類型，從 WindowsPhone Silverlight 有對等項目在通用 Windows 平台 (UWP)。 例如 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)、[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 與 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)。 移植使用這些類型的大部分 UI 應該相當簡單，但是請一律尋找可利用這些版面配置面板之動態配置功能的方法，以自動在不同大小的裝置上調整大小和重新配置。

跳脫系統控制項與版面配置面板內建的動態版面配置，我們可以使用新的 windows 10 功能，稱為[調適型 Visual State Manager](wpsl-to-uwp-porting-xaml-and-ui.md)。

## <a name="input-modalities"></a>輸入形式

WindowsPhone Silverlight 介面是觸控專屬介面。 因此，所移植之應用程式的介面也應該理所當然地支援觸控，但是您可以選擇另外支援其他輸入形式，例如滑鼠與鍵盤。 在 UWP 中，滑鼠、手寫筆與觸控輸入已整合為 *「指標輸入」*。 如需詳細資訊，請參閱[處理指標輸入](https://msdn.microsoft.com/library/windows/apps/mt404610)和[鍵盤互動](https://msdn.microsoft.com/library/windows/apps/mt185607)。

## <a name="maximizing-markup-and-code-re-use"></a>將標記和程式碼做最大幅的重複使用

請回頭參考[儘可能重複使用標記與程式碼](wpsl-to-uwp-porting-to-a-uwp-project.md)清單，以了解以各種尺寸規格裝置為目標來共用 UI 的技術。

## <a name="more-info-and-design-guidelines"></a>更多資訊及設計指導方針

-   [設計 UWP App](http://dev.windows.com/design)
-   [字型的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700394)
-   [針對不同的尺寸規格做規劃](https://msdn.microsoft.com/library/windows/apps/dn958435)

## <a name="related-topics"></a>相關主題

* [命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)

