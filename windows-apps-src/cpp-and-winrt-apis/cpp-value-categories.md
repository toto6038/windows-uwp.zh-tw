---
author: stevewhims
description: 本主題說明各種類別的 c + + 中存在的值。 您相信有聽到的值和右，但還有其他種類的太。
title: 值類別
ms.author: stwhi
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 預測、 移動、 轉接、 值類別、 move 語意、 完美轉接、 左值、 右值、 glvalue、 prvalue、 xvalue
ms.localizationpriority: medium
ms.openlocfilehash: 75176334909e0e6bcf81763b543a19b70a4cba73
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2018
ms.locfileid: "2739074"
---
# <a name="value-categories"></a>值類別
本主題說明各種類別的 c + + 中存在的值。 您相信有聽到*的值*和*右*，但還有其他種類的太。 在 c + + 中的每個運算式求得屬於其中一個類別本主題中所討論的值。 有方面的 c + + 語言、 facilies、 及需求值類別的適當了解的規則。

## <a name="an-lvalue-has-identity"></a>左值有身分識別
是什麼意思具有*identity*值？ 如果您有] （或您可以採取） 值的記憶體地址的值有身分識別。 如此一來，您可以執行一個以上的比較的值內容： 您可以比較或區別他們的身分識別。

*左值*具有 identity。 它只是現在的僅限歷史利息"左值"在"l"是"left"（如同、 左左手邊的工作分派） 的縮寫。 在 c + +、 左值可以出現在左邊緣*或*上右邊的工作分派。 "L"在"值"然後不會真的幫助您理解也無法定義及其。 您只需要了解我們呼叫左值是具有 identity 的值。

現在時的值是 identity，則為 true 陳述式，, 所以不要 xvalues。 我們將移至何種*xvalue*是本主題稍後 （雖然它們的完整作法未涵蓋以下） 一點點。 現在，只是注意呼叫 glvalue，"的通用左值"值類別。 Glvalues 超集包含的值 （也稱為 「 傳統值"） 及 xvalues。 因此，時 「 左值含有 identity 為"是 true，一組完整的 identity 的那 glvalues、 一組本圖所示。

![左值有身分識別](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>右值是可移動;左值不是
但不是 glvalues 的值。 因此，有您*不能*取得記憶體位址 （或您不能依賴它是有效） 的值。 哪些聲音像是缺點。 但事實上值利用像是也就是可以*移動*它 （這是一般便宜），而不是複本 it （這是一般昂貴）。 移動值表示它不再是設為其所使用的地方。 因此，嘗試存取它是所使用的地方是以避免使用。 何時和*如何*移動值超出範圍本主題討論。 本主題中，我們只需要知道是可移動值稱為將*右值*。

"R"在"右值"是 「 右側 」 （如同、 右左手邊的工作分派） 的縮寫。 但是您可以使用右、 和右、 工作分派以外的參照。 然後"R"在"右"不是以專注於事。 您只需要瞭解我們呼叫右值不是可移動的值。

左值，反之，不是可移動，在此圖所示。 您無法將左值因為如果可能，則它會是不安全 （或即使慘） 繼續認知存取。 請記住，您有其身分識別。

![右值是可移動;左值不是](images/is-movable.png)

您無法將左值。 但是否有** 一種 glvalue （事項與身分識別一組），則可以移&mdash;如果您知道您正在執行的動作&mdash;屬於 xvalue。 我們再探討此概念一更多時間下，當我們查看值類別的完整圖片。

## <a name="an-lvalue-has-identity-a-prvalue-does-not"></a>左值已識別身分 ；prvalue 沒有
在此階段中，我們知道什麼具有 identity。 與我們知道什麼是可移動和項目不是。 但我們還沒有名為一組值*不*具有 identity。 該集就是所謂的*prvalue*或"純粹右值"。

![左值已識別身分 ；prvalue 沒有](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>值類別的完整圖片
它只會維持合併資訊以及上方到單一的大圖片的圖例。

![值類別的完整圖片](images/value-categories.png)

- Glvalue （一般左值） 具有 identity。
- 左值 （一種 glvalue） 身分識別，但不是可移動。 這些是您傳遞周圍參考 （英文） 或 const 參考或值如果複製為便宜通常是可讀寫值。
- Xvalue （種 glvalue，但也是一種右值） 身分識別，而且也是可移動。 這可能是您已決定要移動複製是昂貴，因為奔左值和必須小心不要將認知加以存取。 Xvalue，並移動一個，原因中轉換左值的方式將會有等待另一個主題。 但是您可以將"x"在"xvalue"視為意義"專家"如果的協助。
- Prvalue （純粹右值 ； 種右值） 不具有 identity、 但可移動。 這些通常是常值、 暫存檔，傳回值&mdash;的任何項目是結果的評估運算式 （不是 glvalue），或從函數傳回的值的任何項目。
- 右值是可移動。
