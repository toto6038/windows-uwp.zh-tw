---
author: mijacobs
Description: "使用淡入/淡出動畫將項目帶入檢視或帶出檢視。 兩個常見的淡入/淡出動畫會淡入和淡出。"
title: "UWP app 中的淡入/淡出動畫"
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.openlocfilehash: cbe1a4ca77f0d6e15b3d12b2b3a0f2f8363530ae
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="fade-animations"></a>淡化動畫

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

使用淡化動畫將項目帶入檢視或帶出檢視。 兩個常見的淡入/淡出動畫會淡入和淡出。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**FadeInThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210298)</li>
<li>[**FadeOutThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210302)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>可行與禁止事項


-   當您的應用程式在不相關或包含大量文字元素之間轉換時，請先使用淡出再接著使用淡入。 這樣能讓傳出物件完全消失，然後才看到傳入物件。
-   如果元素的大小維持一致，而且您希望使用者感覺它們看起來是相同的項目時，請在傳出元素上淡入一或多個傳入元素。 一旦傳入完成後，就可以移除傳出項目。 這個選項是在傳入項目可以完全覆蓋傳出項目時才可行。
-   請避免使用淡入/淡出動畫在清單中新增或刪除項目。 請改用專為該目的建立的清單動畫。
-   請避免使用淡入/淡出動畫變更頁面的全部內容。 請改用專為該目的建立的頁面轉換動畫。
-   淡出是移除元素的一種微妙方式。
## <a name="related-articles"></a>相關文章

* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [讓淡入/淡出產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**FadeInThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**FadeOutThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 




