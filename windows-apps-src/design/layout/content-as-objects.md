---
description: ''
title: 做為物件的內容
template: detail.hbs
ms.localizationpriority: medium
ms.openlocfilehash: 37ba5093f2d7cfe268be40413b889801daf00967
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8689028"
---
# <a name="content-as-objects"></a>做為物件的內容

 

您可以操控元素的深度，或圖層順序，建立視覺階層，其協助有助於您更容易使用應用程式。  

> 注意：本文是針對 Windows 10 RS2 的新功能所撰寫的早期草稿。 功能名稱、詞彙和功能並非最終版本。 

## <a name="why-visual-hierarchy-is-important"></a>視覺階層很重要的原因

使用者持續受到他們注意的請求轟炸。 請求查看螢幕上的每個元素，讀取每個文字的字串，按一下每個按鈕。 隨著視覺環境變得更加擁擠和混亂，必須花更多心力剖析場景，找出問題所在。  

這就是為什麼要務必小心選取使用者介面的元素，以及為什麼建立能在 UI 元素中建立清楚視覺階層的版面配置如此重要的原因。 <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

清楚的視覺階層顯示使用者哪些元素最重要，並在元素間建立關聯。 有了最佳視覺階層，使用者一目瞭然頁面的版面配置，且可以專注於他們想要完成的工作。 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>因此，該如何建立清楚的視覺階層呢？ 使用較舊的 Windows 10 版本，您可以使用空格、位置及印刷樣式來定義視覺階層。 </p>
  </div>
  <div class="side-by-side-content-right">
    ![一般版面配置](images/content-as-objects/flat-layout.png)
    
  </div>
</div>
</div>

使用 Windows 10 RS2，我們實際上新增另一個維度：深度。 

![版面配置的深度](images/content-as-objects/depth-in-layout2.png)


## <a name="use-depth-to-establish-a-hierarchy"></a>使用深度建立階層 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>您可以使用深度 (圖層順序)，以及其他設計工具 (空格、位置、印刷樣式) 建立階層。 將您最重要元素的權限提升至最上層；使用較低層顯示較不重要的 UI。 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    ![版面配置的深度](images/content-as-objects/elements-forward-backward.png) 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>它如何運作？
> TODO：簡短描述您如何控制元素的圖層順序。 您明確的硬式編碼圖層順序，或是否有語意排名系統？ 項目如何從一層移動到另一層？ 系統會自動執行什麼，以及設計人員/開發人員需要擔心什麼？ 

## <a name="the-four-layers-of-a-typical-app-layers"></a>一般應用程式層級的四個層級

<p>一般的應用程式有 4 層級。</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **背景以外** <br/>
這個層級在應用程式的後面。  當元素移到這個層級時，我們建議讓它們成為非互動式。 在這個層級的元素有最慢的視差且裁剪至應用程式視窗。 TODO：這個層級會縮放嗎？ 

<p>範例背景元素包括內容後面的影像，TODO：範例、TODO：範例。</p>
  </div>
  <div class="side-by-side-content-right">
    ![應用程式的背景以外層級](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **被動式層** <br/>
這是應用程式的基本層，預設情況下 UI 元素存在於其中。  元素在這個層級即時移動 (沒有視差) 、裁剪至應用程式視窗，且以 100% 縮放比例轉譯。 

<p>範例元素：應用程式背景、文字、次要 UI，例如應用程式瀏覽 UI。</p>
  </div>
  <div class="side-by-side-content-right">
    ![應用程式的被動式層](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **呼叫行動** <br/>
這個層級適用於上述被動式層的優先順序互動式項目。 這個層級上的元素有中度視差且裁剪至應用程式視窗。 TODO：在這個層級縮放中執行元素，或會有陰影嗎？

<p>範例元素：清單、格線、主要命令 (TODO：例如...)。</p> 
  </div>
  <div class="side-by-side-content-right">
    ![應用程式的呼叫動作層級](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **主角層** <br/>
這個層級適用於此時螢幕上最高優先順序的元素。  這個層級上的元素可能會中斷應用程式視窗的界限，其可縮放並自動取得陰影。

<p>範例元素：攝影元素，目前所選的項目。</p>  
  </div>
  <div class="side-by-side-content-right">
    ![應用程式的主角層](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>範例：TBD
> TODO：示範如何調整常見 UI 模式來使用圖層順序。 我們應該顯示圖例和程式碼。 

## <a name="download-the-code-samples"></a>下載程式碼範例
>TODO：連結至示範此功能的範例。 


## <a name="related-articles"></a>相關文章
* [內容基本知識](../basics/content-basics.md)
