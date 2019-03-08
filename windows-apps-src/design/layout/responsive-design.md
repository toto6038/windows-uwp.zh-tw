---
Description: 了解回應式設計技術，來調整您的應用程式特定的裝置
label: Responsive design techniques
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9e06131da5d7dd56354e1871867f9956ad13aa2c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624813"
---
# <a name="responsive-design-techniques"></a>回應式設計技術

UWP 應用程式會使用有效的像素為單位，以確保您的 UI 會易於閱讀和用於所有 Windows 架構的裝置上。 那麼，為什麼您還是希望針對特定裝置系列自訂您的應用程式 UI？

- **若要最有效利用空間，並減少瀏覽**

    如果您設計要看起來沒有問題已在小型螢幕裝置上的應用程式時，例如平板電腦應用程式將會使用具有更大的顯示，在電腦上，但可能會有一些浪費掉的空間。 您可以自訂應用程式，在螢幕超過特定大小時顯示更多內容。 比方說，購物的應用程式可能的平板電腦上的一次顯示一個商品類別，但在電腦或膝上型電腦上同時顯示多個類別和產品。

    在螢幕上放置更多內容，會減少使用者需要執行的瀏覽數。

- **若要利用裝置的功能**

    某些裝置可能有特定的裝置功能。 例如，膝上型電腦很可能有位置感應器和相機，而電視可能沒有其中一個。 您的應用程式可以偵測到哪些功能可以使用，並啟用使用這些功能的功能。

- **若要為輸入進行最佳化**

    通用控制項程式庫可搭配所有輸入類型 (觸控、手寫筆、鍵盤、滑鼠)，但是您仍然可以透過重新安排 UI 元素，最佳化特定輸入類型。 例如，如果您在螢幕底部放置瀏覽元素，手機使用者就能輕鬆存取—但是大部分的電腦使用者則希望在螢幕頂端看到瀏覽元素。

當您針對特定螢幕寬度自訂應用程式的 UI 時，我們假設您要建立回應式設計。 以下是六種您可以用來自訂應用程式 UI 的回應式設計技術。

>[!TIP]
> 許多 UWP 控制項自動實作這些回應式的行為。 若要建立回應式 UI，我們建議簽出[UWP 控制項](../controls-and-patterns/index.md)。

## <a name="reposition"></a>調整位置

您可以變更的位置和位置來執行大部分的視窗大小的 UI 項目。 在此範例中，較小的視窗垂直堆疊項目。 當應用程式會轉譯為較大的視窗中時，項目可以利用更多的視窗寬度。

![調整位置](images/rsp-design/rspd-reposition2.gif)

在這個相片應用程式的設計範例中，相片應用程式在較大的螢幕上會重新調整它的內容。

## <a name="resize"></a>重新調整大小

您可以藉由調整的邊界和 UI 項目大小的視窗大小最佳化。 比方說，這可能只成長的內容框架，藉此強化較大的螢幕上的閱讀體驗。

![調整大小設計元素](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>自動重排

依據裝置和方向來變更 UI 元素的排列，應用程式能夠提供最佳的顯示內容。 比方說，較大的螢幕時，它可能會讓合理地加入資料行中，使用較大的容器，或以不同方式來產生清單項目。

這個範例示範如何以垂直方式捲動內容可以在較大螢幕以顯示兩個資料行的文字 reflowed 的較小螢幕上的單一資料行。

![自動重排設計元素](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>顯示/隱藏

您可以根據螢幕空間顯示或隱藏 UI 元素，或者當裝置支援其他功能、特定情況或適合的螢幕方向時顯示。

![隱藏設計元素](images/rsp-design/rspd-revealhide.gif)

比方說，media player 控制項減少較小的螢幕上的按鈕集合，並在較大的畫面上展開。 在較大的視窗上的媒體播放器可以處理更螢幕比它的功能可以在較小的視窗。

顯示或隱藏技術的一部分，包含了選擇何時顯示更多中繼資料。 對於較小的 windows，最好是顯示最少量的中繼資料。 對於較大的 windows，可以顯示一段很長的中繼資料。 顯示或隱藏中繼資料的時機的一些範例包括：

- 在電子郵件應用程式中，您可以顯示使用者的虛擬人偶。
- 在音樂應用程式中，您可以顯示更多專輯或藝人的相關資訊。
- 在影片應用程式中，您可以顯示更多電影或節目的相關資訊 (例如顯示演員名單和劇組的詳細資料)。
- 在任何應用程式中，您可以分割欄位顯示更多詳細資料。
- 在任何應用程式中，您可以將垂直堆疊的內容改成以水平方向排列。 從手機或平版手機移至較大的裝置時，堆疊的清單項目可以改成以清單項目為列，並以中繼資料為欄來顯示。

## <a name="replace"></a>替換

這項技術可讓您切換為特定的中斷點的使用者介面。 在此範例中，瀏覽窗格和其壓縮，暫時性 UI 很適合較小的畫面中，但在較大的畫面上，索引標籤可能會更好的選擇。

![替換設計元素](images/rsp-design/rspd-replace.gif)

[NavigationView](../controls-and-patterns/navigationview.md)控制項支援此回應的技術，讓使用者將窗格位置設定上方或左方。

## <a name="re-architect"></a>重新設計

您可以摺疊或分支應用程式的結構，以便符合特定裝置。 在此範例中，展開視窗會顯示整個主版/詳細資料模式。

![重新架構使用者介面的範例](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>相關主題

- [螢幕大小與中斷點](screen-sizes-and-breakpoints-for-responsive-design.md)
- [使用 XAML 的回應式版面配置](layouts-with-xaml.md)
- [UWP 控制項和模式](../controls-and-patterns/index.md)
