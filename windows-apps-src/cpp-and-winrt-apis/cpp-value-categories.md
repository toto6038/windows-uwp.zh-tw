---
description: 本主題說明各種類別的 c + + 中存在的值。 您其實有聽說的左值和右值，但有其他種類的太。
title: 值類別和它們的參考
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10、 uwp、 standard、 c + +、 cpp、 winrt、 投影、 移動、 轉送、 值類別、 移動語意，完美轉送、 左值，右值、 glvalue、 prvalue，xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593013"
---
# <a name="value-categories-and-references-to-them"></a>值類別和它們的參考
本主題描述存在於 c + + 中的各種類別的值 （和值的參考）。 您其實已耳聞過*lvalues*並*右值*，但您可能不認為這些條款，本主題提供中。 還有其他類型的值，太。

C + + 中的每個運算式會產生屬於本主題中討論的類別目錄的其中一個值。 仍有一些層面的 c + + 語言、 facilies 和規則，需要適當的了解值類別，以及它們的參考。 比方說，採用的值，將值複製、 移動的值，也會轉送到另一個函式值的位址。 本主題不討論所有的這些層面，深入了解，但它提供充分的了解它們的基本資訊。

本主題中的資訊付出 Stroustrup 的分析值類別中，身分識別和 movability [Stroustrup 2013] 兩個獨立屬性。

## <a name="an-lvalue-has-identity"></a>左值具有身分識別
代表什麼意義的值具有*識別*？ 如果您有 （或您可以採取） 值的記憶體位址並安全地使用它，則此值有身分識別。 如此一來，您可以執行多個比較值的內容： 您可以比較或區分它們依身分識別。

*左值*具有身分識別。 現在是指派的關乎只歷程記錄中 「 左值 「"l"是指派的"left"(in、 左手右手邊） 的縮寫的感興趣。 C + + 中的左值可以出現在左邊*或*指派的右邊。 "L"中 「 左值 」，然後，不實際幫助您理解也定義是什麼。 您只需要了解我們所謂的左值而言，具有身分識別的值。

都是左值的運算式的範例包括： 具名的變數或常數;或函式傳回之參考。 運算式所產生的範例*不*左值包括： 暫存檔; 或傳回值的函式。

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

現在，雖然陳述式與左值有身分識別，則為 true，因此請勿 xvalues。 我們會討論多個後續*xvalue*是本主題稍後的。 現在，只是知道呼叫 glvalue，「 一般化左值 」 的值分類。 Glvalues 的超集包含兩個左值 (也稱為*古典 lvalues*) 和 xvalues。 因此，雖然 「 左值會有身分識別 」 為 true 時，一組完整的已識別的項目是 glvalues，一組，此圖中所示。

![左值具有身分識別](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>右值是可移動;不是左值
但有不 glvalues 的值。 因此，有值，您*無法*取得的記憶體位址 （或您不能依賴它是有效的）。 我們發現上述程式碼範例中的某些這類值。 這聽起來像是一項缺點。 但事實上的優點是值，也就是您可以*移動*從它 （這是通常便宜），而不是從它的複本 （也就是一般而言成本較高）。 移動值中表示，它已不存在於要的位置。 因此，嘗試存取它以往是就地是要避免的項目。 當的討論並*如何*移動值已超出本主題的範圍。 本主題中，我們只需要知道為可移動的值即為*rvalue* (或*傳統的右值*)。

「 右值 」 中的"r"是指派的 「 權利 」 (in、 右邊右手邊） 的縮寫。 但是，您可以使用右值和右值，指派之外的參考。 在 「 右值"，"r"，則無法將焦點放在項目。 您只需要了解我們所謂的右值都是可移動的值。

左值，相反地，不是可移動的在此圖中所示。 移動的左值會歸的定義*左值*，而且它會是非常合理預期能夠繼續存取左值的程式碼未預期的問題。

![右值是可移動;不是左值](images/is-movable.png)

您無法移動左值。 但沒有*已*glvalue （身分識別的項目集合），您可以移動一種&mdash;如果您知道您正在做什麼 （包括要小心，不要在移動之後存取它）&mdash;是 xvalue。 我們再探討這個議題一次，當我們查看值類別的完整內容。

## <a name="rvalue-references-and-reference-binding-rules"></a>右值參考和參考繫結規則
本節將介紹的右值參考的語法。 我們必須等候到大量的處理方式移動和轉送，另一個主題，但這些都是右值參考來解決的問題。 我們看看右值參考之前，不過，我們必須更加清楚的相關`T&`&mdash;東西我們已先前已呼叫只是 「 參考 」。 它其實是 「 左值 (非 const) 參考 」，這是指要參考的使用者可以寫入的值。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

左值參考可以繫結至左值，但不是右值。

然後有左值 const 參考 (`T const&`)，其參考的物件參考的使用者*無法*寫入 （例如，常數）。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Const 左值參考可以繫結至左值或右值。

類型的右值參考的語法`T`寫入為`T&&`。 右值參考是指可移動值&mdash;我們不需要保留之後我們利用它 （例如，暫存）, 其內容的值。 因為重點從移動 （藉此修改） 的值繫結至右值參考，`const`和`volatile`限定詞 （也就是 cv 限定詞） 未套用至右值參考。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

右值參考繫結至右值。 事實上，依據多載解析，右值*偏好*繫結至 lvalue const 參考比的右值參考。 但為右值參考無法繫結至左值因為我們已經說過，右值參考參考的值會假設我們不需要保留 （例如，移動建構函式的參數） 的內容。

您也可以傳遞右值的數值引數所預期的位置，透過複製建構 （或移動建構函式，如果右值 xvalue）。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Glvalue 具有身分識別;prvalue 則否
在這個階段，我們知道何者得到身分識別。 而且我們知道什麼是可移動與不支援內容。 我們尚未尚未命名的一組值，但是*不*有身分識別。 設定就所謂*prvalue*，或*純粹的右值*。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![左值具有身分識別;prvalue 則否](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>完整的值類別
它只能維持結合成單一的大圖片上方的圖例與資訊。

![完整的值類別](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Glvalue （一般化的左值） 會有身分識別。

### <a name="lvalue-im"></a>左值 (我\&\!m)
左值 （一種 glvalue） 具有身分識別，但不是可移動。 這些是您傳遞參考或 const 的參考，或值，如果複製時實惠的一般的讀寫值。 左值不能為右值參考繫結。

### <a name="xvalue-im"></a>xvalue (我\&m)
具有身分識別，而且也可移動 xvalue （一種 glvalue，但也是一種右值）。 這可能是您已經決定要移動，因為複製是昂貴，拾左值，您會注意不要之後存取它。 以下是如何您可以在這裡將左值轉換成 xvalue。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

在上述程式碼範例中，我們還沒有尚未移轉的任何項目。 我們剛建立了 xvalue 藉由將具名右值參考將左值轉型。 它仍然左值名稱，來識別但是，您也可以為 xvalue，現在正是*能夠*的移動。 這麼做，原因，而哪些移動實際看起來像，必須等候另一個主題。 但是，您可以將 「 xvalue 」 意義 「 專家-僅限 」 如果這樣可以協助您為中的"x"。 藉由將左值轉型成 xvalue （一種右值），值就能夠為右值參考繫結。

以下是兩個其他範例的 xvalues&mdash;呼叫函式傳回未具名右值參考，並存取 xvalue 的成員。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!我\&m)
Prvalue （單純的右值) 是一種右值沒有身分識別，但是是可移動。 這些通常是暫存，呼叫函式的結果，傳回值或任何其他不的 glvalue 運算式的評估結果

### <a name="rvalue-m"></a>右值 (m)
右值是可移動的。 右值*參考*一律是指右值 （假設我們不需要保留其內容的值）。

但是，您也可以是本身為右值參考右值嗎？ *未命名*右值參考 （例如上述 xvalue 程式碼範例所示的項目） 是 xvalue 沒錯，因此，它會是在右值。 它偏好繫結至右值參考函式參數，例如移動建構函式。 相反地 （並可能 counter-intuitively），為右值參考的名稱，則該名稱所組成的運算式為左值。 因此它*無法*繫結至右值參考參數。 但就得這麼做，您可以輕鬆&mdash;只是將它轉換成未具名右值參考 (xvalue) 一次。

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
值，沒有身分識別，而且不是可移動的類型是一個的組合，我們還沒討論。 但是，我們可以忽略它，因為該類別不是很有用的做法，在 c + + 語言中。

## <a name="reference-collapsing-rules"></a>參考摺疊規則
在運算式 （左值參考至 lvalue 參考或右值參考的右值參考） 中的多個 like 參考取消其中一個其他外。

- `A& &` 摺疊成`A&`。
- `A&& &&` 摺疊成`A&&`。

不同於運算式中參考多個摺疊至 lvalue 參考。

- `A& &&` 摺疊成`A&`。
- `A&& &` 摺疊成`A&`。

## <a name="forwarding-references"></a>轉送的參考
最後一節中會對照右值參考，我們已經討論過，不同的概念*轉送參考*。

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 是右值參考，如我們所見。 Const 和 volatile 不適用於右值參考。
- `foo` 接受的類型的右值**A**。
- 原因右值參考 (例如`A&&`) 存在時，讓您可以撰寫多載，適用於大小寫的暫時密碼 （或其他的右值） 傳遞。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` 已*轉送參考*。 根據您傳遞給`bar`，輸入 **_Ty**可能是 const/非 const volatile/非 volatile 獨立。
- `bar` 接受任何左值或右值的型別 **_Ty**。
- 傳遞左值，會導致轉送參考會變成`_Ty& &&`，這會摺疊到左值參考`_Ty&`。
- 傳遞的右值，會導致轉送參考會變成`_Ty&& &&`，其右值參考摺疊`_Ty&&`。
- 轉送參考的原因 (例如`_Ty&&`) 存在時*不*最佳化，但才會傳遞給它們，並且將它上明確而有效率地。 您很可能會遇到轉送的參考，只有當您撰寫 （或仔細研究） 程式庫程式碼時，才&mdash;轉送上建構函式引數，例如 factory 函式。

## <a name="sources"></a>來源
* \[Stroustrup 2013\] B.Stroustrup:C + + 程式設計語言，第四版。 Addison-Wesley. 2013.
