---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: 圖示
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 61157ad23eb55447137531922ea23fa0120e2b98
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653917"
---
# <a name="icons-for-uwp-apps"></a>適用於 UWP 應用程式的圖示



理想的圖示達到印刷格式與其餘設計語言的平衡。 它們不會混合使用隱喻，而且只會盡可能快速並簡單地溝通所需的內容。 

## <a name="linear-scaling-size-ramps"></a>線性縮放大小手法 

<table>
    <tr> 
        <td>16px x 16px</td>
        <td>24px x 24px</td>
        <td>32px x 32px</td>
        <td>48px x 48px</td>
    </tr>
    <tr> 
        <td><img src="images/icons-16x16.png" alt="Icons at 16x16 effective pixels" /></td>
        <td><img src="images/icons-24x24.png" alt="Icons at 24x24 effective pixels" /></td>
        <td><img src="images/icons-32x32.png" alt="Icons at 32x32 effective pixels" /></td>
        <td><img src="images/icons-48x48.png" alt="Icons at 48x48 effective pixels" /></td>
    </tr>
</table>

## <a name="common-shapes"></a>通用圖形

圖示通常應該最大化其指定空間，且具有最小邊框間距。 這些圖形提供可調整基本圖形大小的起點。 

![32px x 32px 格線](images/icons-common-shapes.png)

使用對應到圖示方向的圖形，並透過這些基本參數撰寫。 圖示不一定需要填滿或完全放入圖形的內部，而且可以視需要進行調整，確保最佳的平衡。 

<table class="uwpd-noborder">
    <tr>
        <td>圓形<td>
        <td>方形</td>
        <td>三角形</td>
    </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-1.png" alt="A circle" /><td>
        <td><img src="images/icons-common-shapes-examples-2.png" alt="A square" /></td>
        <td><img src="images/icons-common-shapes-examples-3.png" alt="A triangle " /></td>
    </tr>
        <tr>
        <td>水平矩形<td>
        <td colspan="2">垂直矩形</td>        
        </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-4.png" alt="A horizontal rectangle" /><td>
        <td colspan="2"><img src="images/icons-common-shapes-examples-5.png" alt="A vertical rectangle" /></td>
         
    </tr>

</table>

## <a name="angles"></a>角度

除了使用相同的格線和線條粗細之外，還會使用常用的元素來建構圖示。 

僅使用這些角度來建置圖形，可建立所有圖示的一致性，並確保正確呈現圖示。 

建立圖示時，可以合併、聯結、旋轉和反映這些線條。 

<table>
    <tr>
        <td><b>1:1</b><br/>45°</td>
        <td><b>1:2</b><br />26.57° (垂直)<br/>63.43° (水平)</td>
        <td><b>1:3</b><br/>18.43° (垂直)<br/>71.57° (水平)</td>
        <td><b>1:4</b><br/>14.04° (垂直)<br/>75.96° (水平)</td>
    </tr>
    <tr>
        
        <td><img src="images/icons-grid-1-1.png" alt="1:1" /></td>
        <td><img src="images/icons-grid-1-2.png" alt="1:2" /></td>
        <td><img src="images/icons-grid-1-3.png" alt="1:3" /></td>
        <td><img src="images/icons-grid-1-4.png" alt="1:4" /></td>
    </tr>  
</table>

<p>以下是一些範例：</p>

<table>
    <tr>
        <td><img src="images/icons-angles-examples-1.png" alt="A 1:1 angle example" /></td>
        <td><img src="images/icons-angles-examples-2.png" alt="A 1:2 angle example" /></td>
        <td><img src="images/icons-angles-examples-3.png" alt="A 1:3 angle example" /></td>
        <td><img src="images/icons-angles-examples-4.png" alt="A 1:4 angle example" /></td>
    </tr>
</table>

## <a name="curves"></a>曲線

曲線是透過整個圓形的各個區段建構而成，而且除非需要貼齊像素格線，否則不應該扭曲。 

<table>
    <tr>
        <td>1/4 圓</td>
        <td>1/8 圓</td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-14circle.png" alt="1/4 circle" /></td>
        <td><img src="images/icons-curves-18circle.png" alt="1/8 circle" /></td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-examples-1.png" alt="1/4 cirlce example" /></td>
        <td><img src="images/icons-curves-examples-2.png" alt="1/8 circle example" /></td>
    </tr>    
</table>

## <a name="geometric-construction"></a>幾何建構

建議在建構圖示時僅使用單純幾何形狀。

![具有幾何重疊的吉他圖示 ](images/icons-geometric-construction.png)

## <a name="filled-shapes"></a>填滿的形狀 

圖示可以視需要包含填滿的形狀，但在 32px × 32px 時不應該超過 4px。 填滿的圓形不應該大於 6px × 6px。 

![5px x 8px 填滿 ](images/icons-filled-shapes.png)

## <a name="badges"></a>徽章

「徽章」是一個泛型詞彙，用來描述新增到圖示的元素，而圖示不是用來與基本圖示元素整合。 這些通常會傳達狀態或動作這類圖示的相關資訊的其他部分。 其他常見詞彙包括︰重疊、註釋或修飾詞。 

![狀態徽章 ](images/icons-badge-status.png)

![動作徽章 ](images/icons-badge-action.png)

狀態徽章利用圖示上方的填滿彩色物件，而動作徽章整合到單色樣式且線條粗細相同的圖示。

<table>
<tr>
    <td>常見的狀態徽章</td>
    <td>常見的動作徽章</td>
</tr>
<tr>
    <td><img src="images/icons-badge-common-states-1.png" alt="Status badge " /></td>
    <td><img src="images/icons-badge-common-states-2.png" alt="Action badge " /></td>
</tr>
</table>
<p></p>

### <a name="badge-color"></a>徽章色彩 

色彩徽章只應該用來傳達圖示的狀態。 狀態徽章中所使用的色彩可將特定感情訊息傳達給使用者。 

<table>
<tr><td>綠色 - #128B44</td><td>藍色 - #2C71B9</td><td>黃色 - #FDC214</td></tr>
<tr><td>正向：作業結束、已完成 </td><td>中性：協助、通知 </td><td>注意：警示、警告 </td></tr>
<tr><td><img src="images/icons-color-inbadging-1.png" alt="Green status" /></td><td><img src="images/icons-color-inbadging-2.png" alt="Blue status" /></td>
<td><img src="images/icons-color-inbadging-3.png" alt="Yellow status" /></td></tr>
</table>
<p></p>

### <a name="badge-position"></a>徽章位置

任何狀態或動作的預設位置都是右下方。 只有在設計不允許時，才使用其他位置。 

### <a name="badge-sizing"></a>徽章大小

在 32 px × 32 px 格線上，徽章的大小應該為 10–18 px。 

## <a name="related-articles"></a>相關文章

* [磚和圖示資產的指導方針](../shell/tiles-and-notifications/app-assets.md)
