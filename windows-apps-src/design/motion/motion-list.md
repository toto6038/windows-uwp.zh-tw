---
Description: 清單動畫可讓您從集合 (如相簿或搜尋結果清單) 中插入或移除單個或多個項目。
title: 新增和刪除動畫
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4264a9a3a75c076fc033bb98dad45ff8dc99f2ef
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970283"
---
# <a name="add-and-delete-animations"></a>新增和刪除動畫



清單動畫可讓您從集合 (如相簿或搜尋結果清單) 中插入或移除單個或多個項目。

> **重要 API**: [**AddDeleteThemeTransition 類別**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)


## <a name="dos-and-donts"></a>可行與禁止事項


-   使用清單動畫，將單一的新項目加入到現有的一組項目中。 例如，收到新的電子郵件時或在現有的一組相片中匯入新相片時予以使用。
-   使用清單動畫，一次將數個項目加入到一組項目中。 例如，在您將一組新相片匯入到現有集合時予以使用。 新增或刪除多個項目應該要同時進行，個別物件的動作之間不應有任何延遲。
-   新增和刪除清單動畫必須同時搭配使用。 當您使用其中一個動畫，就要在相反動作時使用對應的動畫。
-   對可以一次新增或刪除一個元素或一組元素的項目清單使用清單動畫。
-   請勿使用清單動畫來顯示或移除容器。 這些動畫適合已經顯示之集合或一組項目的成員。 使用快顯動畫，在 app 表面顯示或隱藏暫時容器。 使用內容轉換動畫，顯示或取代屬於應用程式表面的容器。
-   不要在整個項目集合上使用清單動畫。 使用內容轉換動畫，新增或移除容器內的整個集合。



## <a name="related-articles"></a>相關文章

* [動畫概觀](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [讓清單新增和刪除產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/jj649430(v=win.10))
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**AddDeleteThemeTransition 類別**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)

 

 




