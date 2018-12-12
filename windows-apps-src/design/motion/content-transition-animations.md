---
Description: Content transition animations let you change the content of an area of the screen while keeping the container or background constant. New content fades in. If there is existing content to be replaced, that content fades out.
title: 內容轉換動畫的指導方針
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 096cc0aaa9b0580eb6b45328a3243ba75d82f202
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8940567"
---
# <a name="content-transition-animations"></a>內容轉換動畫



內容轉換動畫可讓您變更畫面中區域的內容，同時保持容器或背景不變。 新的內容會淡入。 如果需要取代現有內容，該內容會淡出。

> **重要 API**: [**ContentThemeTransition 類別 (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)

## <a name="dos-and-donts"></a>可行與禁止事項


-   如果要將一組新項目放入空的容器，可使用進入動畫。 例如，初次載入應用程式後，應用程式的內容部分可能無法立即顯示。 等到內容準備好可以顯示後，使用內容轉換動畫，將最新內容帶入檢視中。
-   使用內容轉換動畫，以一組已經存在於檢視內相同容器中的內容來取代另一組內容。
-   帶入新內容時，針對一般頁面流程或閱讀順序，將該內容向上 (從下到上) 滑入至檢視中。
-   以邏輯方式介紹新的內容，例如，最後介紹最重要的內容。
-   如果您要更新一個以上的容器內容，請同時觸發所有轉換動畫，且不要有任何交錯或延遲。
-   變更整個頁面時請勿使用內容轉換動畫。 在這種情況下，請改用頁面轉換動畫。
-   如果只是重新整理內容，請勿使用內容轉換動畫。 內容轉換動畫是為了顯示移動。 如果是重新整理內容，請使用淡入/淡出動畫。



## <a name="related-articles"></a>相關文章

**適用於開發人員 (XAML)**
* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [讓內容轉換產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**ContentThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 




