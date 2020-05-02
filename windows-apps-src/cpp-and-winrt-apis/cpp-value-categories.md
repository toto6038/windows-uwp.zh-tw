---
description: 本主題說明 C++ 中存在的各類值。 您必定聽說左值和右值，但還有其他種類的值。
title: 值類別，以及其參考
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 移動, 轉送, 值類別, 移動語意, 完美轉送, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1312b84ded26859cd4b83ffbe3e8a75bfdef6950
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "77037878"
---
# <a name="value-categories-and-references-to-them"></a>值類別，以及其參考
本主題說明 C++ 中存在的各種值類別 (以及值參考)。 您一定曾聽過 *lvalues* 和 *rvalues*，但您可能不認為它們如本主題所呈現。 而且還有其他種類的值。

採用 C++ 的每個運算式都會產生一個值，該值屬於本主題所討論的其中一個類別。 C++ 語言有一些層面、其設備和規則，都需要您適當了解這些值類別，以及它們的參考。 比方說，取得值的位址、複製值、移動值，以及將值轉送到另一個函式。 本主題不會深入討論上述所有層面，但會提供充分了解它們的基本資訊。

本主題中的資訊是根據 Stroustrup 的值類別分析 (依照身分識別和可移動性的兩個獨立屬性分析) 來構想 [Stroustrup 2013]。

## <a name="an-lvalue-has-identity"></a>lvalue 具有身分識別
值具有「身分識別」  是什麼意思？ 如果您有 (或可取得) 值的記憶體位址並安全地使用它，則此值就有身分識別。 如此一來，您不只可以比較值的內容：還可以依照身分識別比較或區分它們。

lvalue  具有身分識別。 現在只關乎歷史價值："lvalue" 中的 "l" 是 "left" 的縮寫 (就像，在指派的左手邊)。 在C++，lvalue 可以出現在指派的左邊「或」  右邊。 然後，"lvalues" 中的 "L" 實際上不會幫助您理解或定義其意義。 您只需要了解我們所謂的 lvalue 是具有身分識別的值。

lvalue 的運算式範例包括：具名的變數或常數；或可傳回參考的函式。 「不是」  lvalue 的運算式範例包括：暫存項目；或可依照值傳回資料的函式。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

現在，lvalue 確實有身分識別，然而 xvalue 也有身分識別。 我們稍後會在本主題中近一步討論什麼是 xvalue  。 現在，只要知道有一個名為 glvalue 的值類別即可，其代表「廣義 lvalue」。 glvalue 的超集同時包含 lvalue (也稱為「傳統 lvalue」  ) 和 xvalue。 因此，「lvalue 具有身分識別」是真的，而具有身分識別的完整項目集合就是 glvalue 集合，如下圖所示。

![lvalue 具有身分識別](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>rvalue 可移動；lvalue 不可移動
但有不是 glvalue 的值。 因此，有一些您「無法」  取得其記憶體位址 (或您無法依賴它生效) 的值。 我們在上述程式碼範例中看見一些這類值。 這聽起來像是一項缺點。 但事實上，這類值的優點就是您可以從它「移動」  (通常成本低廉)，而不是從它複製 (通常成本昂貴)。 從值移動表示它已不存在於以往的位置。 因此，應避免嘗試在其以往的位置存取它。 討論何時及「如何」  移動值已超出本主題的範圍。 在本主題中，我們只需要知道可移動的值即為 *rvalue* (或「傳統 rvalue」  )。

"rvalue" 中的 "r" 是 "right" 的縮寫 (就像，在指派的右手邊)。 但您可以在指派之外使用 rvalue，以及 rvalue 的參考。 然而，"rvalues" 中的 "r" 不是要聚焦的項目。 您只需要了解我們所謂的 rvalue 是可移動的值。

相反地，lvalue 不可移動，如此圖所示。 已移動的 lvalue 會蔑視 *lvalue* 的定義，而對於非常合理預期能夠繼續存取 lvalue 的程式碼而言會是非預期的問題。

![rvalue 可移動；lvalue 不可移動](images/is-movable.png)

您無法移動 lvalue。 但有  一種您可移動的 glvalue (具有身分識別的項目集合)&mdash;如果您知道您正在做什麼 (包括小心不要在移動後存取它)&mdash;也就是 xvalue。 當我們研究值類別的全貌時，我們將會再次探討這個概念。

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue 參考和參考繫結規則
本節將介紹 rvalue 參考的語法。 我們必須等待另一個主題討論移動和轉送的實質處理方式，但足以說明右值參考是解決這些問題的必要項目。 不過，在我們研究 rvalue 參考之前，我們必須先更清楚了解 `T&`&mdash;我們之前一直稱為「參考」的項目。 它其實是「lvalue (非 const) 參考」，會參照參考的使用者可以寫入其中的值。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

lvalue 參考可以繫結至 lvalue，但不可繫結至 rvalue。

而後有 lvalue const 參考 (`T const&`)，其參照參考的使用者「無法」  寫入其中的物件 (例如，常數)。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

lvalue const 參考可以繫結至 lvalue 或 rvalue。

類型 `T` 的 rvalue 參考語法會撰寫為 `T&&`。 rvalue 參考會參照可移動的值&mdash;我們在使用後不需要保留其內容的值 (例如，暫存項目)。 由於重點在於從繫結至 rvalue 參考的值移動 (從而修改)，因此 `const` 和 `volatile` 限定詞 (也稱為 cv 限定詞) 不會套用至 rvalue 參考。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

rvalue 參考會繫結至 rvalue。 事實上，就多載解析而論，rvalue「偏好」  繫結至 rvlue 參考 (相較於 lvalue const 參考)。 但是 rvlue 參考無法繫結至 lvalue，因為我們已經說過，rvlue 參考會參照假設我們不需要保留其內容的值 (例如，移動建構函式的參數)。

您也可以透過複製建構 (如果 rvalue 是 xvalue，則透過移動建構)，傳遞預期有 by-value 的 rvalue。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>glvalue 具有身分識別；prvalue 則不具備
在這個階段，我們知道何者具有身分識別。 而且我們知道何者可移動，何者不可移動。 但我們尚未命名「沒有」  身分識別的值集。 該值集就是所謂的 prvalue  或「單純 rvalue」  。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![glvalue 具有身分識別；prvalue 則不具備](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>值類別的全貌
仍然只是將上面的資訊與圖解結合成一個大圖片。

![值類別的全貌](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
glvalue (廣義 lvalue) 具有身分識別。

### <a name="lvalue-im"></a>lvalue (i\&\!m)
lvalue (一種 glvalue) 具有身分識別，但不是可移動。 這些通常是您依照參考或依照 const 參考，或依照值 (如果複製成本低廉) 傳遞的讀寫值。 lvalue 無法繫結至 rvalue 參考。

### <a name="xvalue-im"></a>xvalue (i\&m)
xvalue (一種 glvalue，但也是一種 rvalue) 具有身分識別，也可移動。 這可能是您先前因為複製成本昂貴而決定要移動的 lvalue，您得小心之後不要存取它。 以下顯示如何將 lvalue 轉換成 xvalue。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

在上述程式碼範例中，我們尚未移轉任何項目。 我們剛才將 lvalue 轉換為未具名的 rvalue 參考，而建立了 xvalue。 它仍可依照其 lvalue 名稱識別，但作為 xvalue，現在就「能夠」  移動。 這麼做的原因以及實際的移動情況，就必須等待另一個主題說明。 但您可以將 "xvalue" 中的 "x" 認為是「只有專家才懂」(如果這有幫助的話)。 將 lvalue 轉換為 xvalue (一種 rvalue)，而後此值就能夠繫結至 rvalue 參考。

以下是 xvalue 的兩個其他範例&mdash;呼叫可傳回未具名 rvalue 參考的函式，並存取 xvalue 的成員。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\&m)
Prvalue (單純；一種 rvalue) 沒有身分識別，但可移動。 這些通常都是暫存項目、呼叫函式 (其可依照值傳回資料) 的結果，或是評估任何其他非 glvalue 運算式的結果。

### <a name="rvalue-m"></a>rvalue (m)
rvalue 是可移動。 rvalue「參考」  一律會參照 rvalue (假設我們不需要保留其內容的值)。

但是，rvalue 參考本身是 rvalue 嗎？ 「未具名」  的 rvalue 參考 (如同上述 xvalue 程式碼範例所示的項目) 是 xvalue，所以它就是 rvalue。 它偏好繫結至 rvalue 參考函式參數，例如移動建構函式的參數。 相反地 (或許反直覺)，如果 rvalue 參考具有名稱，則該名稱所組成的運算式為 lvalue。 因此它「無法」  繫結至 rvalue 參考參數。 但這麼做很輕鬆&mdash;只要將它再次轉換為未具名的 rvalue 參考 (xvalue)。

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\&\!m
沒有身分識別且不可移動的值類型是我們還沒討論的一個組合。 但我們可以忽略它，因為該類別不是 C++ 語言中的實用概念。

## <a name="reference-collapsing-rules"></a>參考摺疊規則
運算式中的多個相似參考 (lvalue 參考對 lvalue 參考，或 rvalue 參考對 rvalue 參考) 會相互抵消。

- `A& &` 會摺疊成 `A&`。
- `A&& &&` 會摺疊成 `A&&`。

運算式中的多個相異參考會摺疊成 lvalue 參考。

- `A& &&` 會摺疊成 `A&`。
- `A&& &` 會摺疊成 `A&`。

## <a name="forwarding-references"></a>轉送參考
最後這一節會對比 rvalue 參考 (我們已經討論過) 與「轉送參考」  的不同概念。

```cppwinrt
void foo(A&& a) { ... }
```

- 如我們所見，`A&&` 是 rvalue 參考。 Const 和 volatile 不適用於 rvalue 參考。
- `foo` 只接受類型 **A** 的 rvalue。
- rvalue 參考 (例如 `A&&`) 存在的原因，是方便您撰寫針對傳遞暫存項目 (或其他 rvalue) 的情況最佳化的多載。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` 是「轉送參考」  。 根據您傳遞至 `bar` 的項目，類型 **_Ty** 可能是與 volatile/非 volatile 無關的 const/非 const。
- `bar` 接受任何類型 **_Ty** 的 lvalue 或 rvalue。
- 傳遞 lvalue 會導致轉送參考變成 `_Ty& &&`，這會摺疊成 lvalue 參考`_Ty&`。
- 傳遞 rvalue 會導致轉送參考變成 `_Ty&& &&`，這會摺疊成 rvalue 參考`_Ty&&`。
- 轉送參考 (例如 `_Ty&&`) 存在的原因「不是」  為了最佳化，而是為了取得您傳遞給它們的項目，以及透明且有效地進行轉送。 只有撰寫 (或仔細研究) 程式庫程式碼，才可能遇到轉送參考&mdash;例如，在建構函式引數上轉送的 factory 函式。

## <a name="sources"></a>來源
* \[Stroustrup, 2013\] B. Stroustrup:The C++ Programming Language, Fourth Edition. Addison-Wesley. 2013.
