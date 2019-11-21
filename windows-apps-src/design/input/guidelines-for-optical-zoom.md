---
Description: 這個主題描述 Windows 縮放和調整元素大小的方式，並提供在應用程式中使用這些互動機制時的使用者經驗指導方針。
title: 視覺化縮放和調整大小的指導方針
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fbcb4510a5b3ecca80b388172fe30028ac511452
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257984"
---
# <a name="optical-zoom-and-resizing"></a>視覺化縮放和調整大小



本文說明 Windows 縮放和調整元素大小的方式，並提供在應用程式中使用這些互動機制時的使用者經驗指導方針。

> **重要 API**：[**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)、[**Input (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

視覺化縮放可以讓使用者將內容區域內的內容檢視放大 (執行對象是內容區域本身)，而調整大小則可以讓使用者變更一或多個物件的相對大小，卻不變更對該內容區域的檢視 (執行對象是內容區域內的物件)。

視覺化縮放與調整大小互動的執行方式是透過捏合和伸展手勢 (將手指分開則會放大，將手指靠攏會縮小)、按住 Ctrl 鍵不放並捲動滑鼠滾輪，或按住 Ctrl 鍵不放 (如果沒有數字鍵台，請搭配 Shift 鍵) 並按下加號 (+) 鍵或減號 (-) 鍵。

下圖示範調整大小和視覺化縮放的差異。

**視覺化縮放**：使用者選取一個區域，然後縮放整個區域。

![將手指靠攏會縮小，將手指分開則會放大。](images/areazoom.png)

**調整大小**：使用者選取區域內的一個物件，然後調整該物件大小。

![將手指靠攏會縮小物件，將手指分開會放大物件。](images/objectresize.png)

**注意**   光學縮放不應與 [[語義縮放](../controls-and-patterns/semantic-zoom.md)] 混淆。 雖然這兩個互動使用相同的手勢，但語意式縮放是指呈現和瀏覽在單一檢視內 (例如，電腦的資料夾結構、文件庫或相簿) 組織的內容。

 

## <a name="dos-and-donts"></a>可行與禁止事項


對於支援調整大小或視覺化縮放的應用程式，請參考下列指導方針：

-   如果定義了最大和最小大小限制或界限，可以在使用者達到或超出這些界限時，使用視覺化回饋作為顯示。
-   使用貼齊點來影響縮放和大小調整行為，方法為提供停止操作的邏輯點，並確保檢視區中顯示特定內容子集。 提供一般縮放比例或邏輯檢視的貼齊點，讓使用者可以比較容易選取這些比例。 例如，相片應用程式會提供 100% 比例的大小調整貼齊點，或如果是地圖應用程式，貼齊點對於城市、省以及國家/地區檢視有可能會相當有用。

    貼齊點可以讓使用者不需要太精準就能達到他們的目標。 如果使用的是 XAML，請參閱 [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 的貼齊點屬性。 若為 JavaScript 和 HTML，請使用 [ **-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895)。

    貼齊點有兩種類型：

    -   鄰近性 - 提起手指後，如果慣性作用讓貼齊點停在距離閾值範圍內，就會選取該貼齊點。 鄰近性貼齊點仍然允許縮放或大小調整在貼齊點之間結束。
    -   強制 - 選取的貼齊點為提起手指之前，在最後一個越過之貼齊點之前或之後的那個貼齊點 (取決於手勢的方向和速度)。 操作必須在強制貼齊點結束。
-   使用慣性物理。 包括以下各項：
    -   減速：在使用者停止捏合或伸展時發生。 這類似於在光滑的表面上滑動到停止的現象。
    -   反彈：超過大小限制或界限時發生的輕微彈回效果。
-   根據[目標預測的指導方針](guidelines-for-targeting.md)來設定控制項之間的空間。
-   提供限制調整大小縮放比例控點。 如果未指定控點，預設是以等體積或等比例的方式調整大小。
-   不要在應用程式使用縮放來瀏覽 UI 或顯示額外的控制項，請使用移動瀏覽區域。 如需移動瀏覽的詳細資訊，請參閱[移動瀏覽的指導方針](guidelines-for-panning.md)。
-   不要將可調整大小的物件放在可調整大小的內容區域中。 例外狀況包括：
    -   繪圖應用程式，可調整大小的項目可以顯示在可調整大小的畫布或製圖板上。
    -   包含內嵌物件 (如地圖) 的網頁。

    **請注意**   在所有情況下，除非所有觸控點都在可調整大小的物件內，否則內容區域會調整大小。

     

## <a name="related-articles"></a>相關文章


**範例**
* [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

**封存範例**
* [輸入： XAML 使用者輸入事件範例](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [輸入：裝置功能範例](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [輸入：觸控點擊測試範例](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML 捲軸、移動流覽和縮放範例](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [輸入：簡化的筆跡範例](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [輸入： Windows 8 手勢範例](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Input：操作和手勢（C++）範例](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX touch 輸入範例](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




