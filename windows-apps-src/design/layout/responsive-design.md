---
Description: 學習回應式設計技術, 為特定裝置量身打造您的應用程式
title: 回應式設計技術
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867417"
---
# <a name="responsive-design-techniques"></a>回應式設計技術

UWP 應用程式會使用有效的圖元來確保您的 UI 在所有 Windows 裝置上都能清楚且可供使用。 那麼，為什麼您還是希望針對特定裝置系列自訂您的應用程式 UI？

- **若要充分發揮空間的使用, 並減少流覽的需求**

    如果您將應用程式設計成在具有小型螢幕 (例如平板電腦) 的裝置上看起來良好, 應用程式將可在具有更大顯示的電腦上使用, 但可能會有一些浪費的空間。 您可以自訂應用程式，在螢幕超過特定大小時顯示更多內容。 例如, 購物應用程式可能會在平板電腦上一次顯示一個商品類別, 但是在一部 PC 或膝上型電腦上同時顯示多個類別和產品。

    在螢幕上放置更多內容，會減少使用者需要執行的瀏覽數。

- **利用裝置的功能**

    某些裝置可能有特定的裝置功能。 例如, 膝上型電腦可能會有定位感應器和相機, 而電視可能不會有任何一種。 您的應用程式可以偵測到哪些功能可以使用，並啟用使用這些功能的功能。

- **若要優化輸入**

    通用控制項程式庫可搭配所有輸入類型 (觸控、手寫筆、鍵盤、滑鼠)，但是您仍然可以透過重新安排 UI 元素，最佳化特定輸入類型。 例如，如果您在螢幕底部放置瀏覽元素，手機使用者就能輕鬆存取—但是大部分的電腦使用者則希望在螢幕頂端看到瀏覽元素。

當您針對特定螢幕寬度自訂應用程式的 UI 時，我們假設您要建立回應式設計。 以下是六種您可以用來自訂應用程式 UI 的回應式設計技術。

>[!TIP]
> 許多 UWP 控制項都會自動執行這些回應式行為。 若要建立回應式 UI, 建議您查看[UWP 控制項](../controls-and-patterns/index.md)。

## <a name="reposition"></a>調整位置

您可以改變 UI 元素的位置和位置, 以充分發揮視窗大小。 在此範例中, 較小的視窗會垂直堆疊元素。 當應用程式轉譯為較大的視窗時, 元素可以利用更寬的視窗寬度。

![調整位置](images/rsp-design/rspd-reposition2.gif)

在這個相片應用程式的設計範例中，相片應用程式在較大的螢幕上會重新調整它的內容。

## <a name="resize"></a>重新調整大小

您可以藉由調整 UI 元素的邊界和大小, 優化視窗大小。 例如, 這可以藉由只擴大內容框架來增加較大螢幕上的閱讀體驗。

![調整大小設計元素](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>自動重排

依據裝置和方向來變更 UI 元素的排列，應用程式能夠提供最佳的顯示內容。 例如, 前往較大的螢幕時, 新增資料行、使用較大的容器, 或以不同的方式產生清單專案可能是合理的。

這個範例會示範如何在較大的螢幕上, 垂直捲動內容的單一資料行, 以顯示兩個文字資料行。

![自動重排設計元素](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>顯示/隱藏

您可以根據螢幕空間顯示或隱藏 UI 元素，或者當裝置支援其他功能、特定情況或適合的螢幕方向時顯示。

![隱藏設計元素](images/rsp-design/rspd-revealhide.gif)

例如, media player 控制項可減少在較小螢幕上設定的按鈕, 並在較大的螢幕上展開。 較大的視窗上的 media player 可以處理比在較小視窗上更多的螢幕功能。

顯示或隱藏技術的一部分，包含了選擇何時顯示更多中繼資料。 使用較小的視窗, 最好只顯示最少量的中繼資料。 使用較大的視窗, 可以呈現大量的中繼資料。 顯示或隱藏中繼資料的一些範例包括:

- 在電子郵件應用程式中，您可以顯示使用者的虛擬人偶。
- 在音樂應用程式中，您可以顯示更多專輯或藝人的相關資訊。
- 在影片應用程式中，您可以顯示更多電影或節目的相關資訊 (例如顯示演員名單和劇組的詳細資料)。
- 在任何應用程式中，您可以分割欄位顯示更多詳細資料。
- 在任何應用程式中，您可以將垂直堆疊的內容改成以水平方向排列。 從手機或平版手機移至較大的裝置時，堆疊的清單項目可以改成以清單項目為列，並以中繼資料為欄來顯示。

## <a name="replace"></a>替換

這項技術可讓您切換特定中斷點的使用者介面。 在此範例中, 流覽窗格及其精簡的暫時性 UI 適用于較小的螢幕, 但在較大的螢幕上, 索引標籤可能是較佳的選擇。

![替換設計元素](images/rsp-design/rspd-replace.gif)

[NavigationView](../controls-and-patterns/navigationview.md)控制項可讓使用者將窗格位置設定為 [上] 或 [左], 以支援這種回應式技術。

## <a name="re-architect"></a>重新設計

您可以摺疊或分支應用程式的結構，以便符合特定裝置。 在此範例中, 展開視窗會顯示整個主要/詳細資料模式。

![重新架構使用者介面的範例](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>相關主題

- [螢幕大小與中斷點](screen-sizes-and-breakpoints-for-responsive-design.md)
- [使用 XAML 的回應式版面配置](layouts-with-xaml.md)
- [UWP 控制項和模式](../controls-and-patterns/index.md)
