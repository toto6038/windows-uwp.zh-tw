---
author: QuinnRadich
description: 可讓使用者檢視並設定反映其對內容與服務滿意度的評分。
title: 評分控制項
template: detail.hbs
ms.author: quradic
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 242ecdaf128e1e01b1bdeac4cce649504b8efc74
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3388646"
---
# <a name="rating-control"></a>評分控制項

評分控制項可讓使用者檢視並設定反映其對內容與服務滿意度的評分。 使用者可以使用觸控、手寫筆、滑鼠、遊戲台或鍵盤，與評分控制項互動。 下列指導方針示範如何使用評分控制項的功能來提供彈性和自訂。

> **重要 API**：[RatingControl 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.ratingcontrol)

![評分控制項範例](images/rating_rs2_doc_ratings_intro.png)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/RatingControl">開啟應用程式並查看 RatingControl 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>有預留位置值的可編輯評分

評分控制項最常見的使用方式或許是顯示平均評分，但仍允許使用者輸入他們自己的評分值。 在此案例中，評分控制項一開始已設定為反映對特定服務或內容類型 (例如音樂、影片、書籍等) 的平均滿意度評分。 在使用者為了對某個項目個別評分而與控制項互動之前，控制項都會保持在此狀態。 這個互動會變更評分控制項的狀態，以反映使用者的個人滿意度評分。

#### <a name="initial-average-rating-state"></a>初始平均評分狀態
![初始平均評分狀態](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>使用者評分一旦設定後的表示方式

![使用者評分一旦設定後的表示方式](images/rating_rs2_doc_movie_user.png)

```XAML
<RatingControl x:Name="MyRating" ValueChanged="RatingChanged"/>
```

```csharp
private void RatingChanged(RatingControl sender, object args)
{
    if (sender.Value == null)
    {
        MyRating.Caption = "(" + SomeWebService.HowManyPreviousRatings() + ")";
    }

    else
    {
        MyRating.Caption = "Your rating";
    }
}
```

### <a name="read-only-rating-mode"></a>唯讀評分模式

您有時需要顯示次要內容的評分，例如在推薦內容中顯示，或在顯示意見清單及其對應評分時顯示。 在此情況下，不可允許使用者編輯評分，這樣您才能讓控制項變成唯讀。
同時考量 UI 設計與效能而在非常大型虛擬化清單中使用評分控制項時，唯讀模式也是此控制項的建議使用方式。

![唯讀長清單](images/rating_rs2_doc_reviews.png)

為達此目的，您會執行下列動作：

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>其他功能

評分控制項還有許多可用的其他功能。 您可以在 MSDN 參考文件中找到有關如何使用這些功能的詳細資訊。
以下是其他功能的清單，但並非詳盡無遺。
-   優異長清單處理效能
-   精簡調整緊湊 UI 案例
-   連續值填寫和評分
-   間距自訂
-   停用成長動畫
-   自訂星星數

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。