---
author: mijacobs
Description: "清單動畫可讓您從集合 (如相簿或搜尋結果清單) 中插入或移除單個或多個項目。"
title: "在 UVP app 中新增和刪除動畫"
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: b6c57d1ceadd0785a51c518bc51f93892aa472d5

---

# 新增和刪除動畫




清單動畫可讓您從集合 (如相簿或搜尋結果清單) 中插入或移除單個或多個項目。

**重要 API**

-   [**AddDeleteThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/br243048)


## 可行與禁止事項


-   使用清單動畫，將單一的新項目加入到現有的一組項目中。 例如，收到新的電子郵件時或在現有的一組相片中匯入新相片時予以使用。
-   使用清單動畫，一次將數個項目加入到一組項目中。 例如，在您將一組新相片匯入到現有集合時予以使用。 新增或刪除多個項目應該要同時進行，個別物件的動作之間不應有任何延遲。
-   新增和刪除清單動畫必須同時搭配使用。 當您使用其中一個動畫，就要在相反動作時使用對應的動畫。
-   對可以一次新增或刪除一個元素或一組元素的項目清單使用清單動畫。
-   請勿使用清單動畫來顯示或移除容器。 這些動畫適合已經顯示之集合或一組項目的成員。 使用快顯動畫，在 app 表面顯示或隱藏暫時容器。 使用內容轉換動畫，顯示或取代屬於應用程式表面的容器。
-   不要在整個項目集合上使用清單動畫。 使用內容轉換動畫，新增或移除容器內的整個集合。



## 相關文章


**適用於開發人員 (XAML)**
* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [讓清單新增和刪除產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649430)
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**AddDeleteThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/br243048)

 

 







<!--HONumber=Aug16_HO3-->


