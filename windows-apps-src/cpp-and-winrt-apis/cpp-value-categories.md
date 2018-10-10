---
author: stevewhims
description: 本主題描述各種不同的值存在於 c + + 類別。 您相信有聽值和右，但有也會其他類型。
title: 值類別，以及它們的參考
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 移動、 轉送、 值類別、 移動語意、 完美轉送、 左、 右值、 glvalue、 prvalue，xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "4503703"
---
# <a name="value-categories-and-references-to-them"></a>值類別，以及它們的參考
本主題說明存在於 c + + 的各種不同類別的值 （和值的參考）。 您將會相信聽*值*和*右*，但您可能不想要它們在本主題提供的條款。 而且有太一些其他類型的值。

在 c + + 中的每個運算式會產生屬於本主題中討論的類別的其中一個值。 有層面的 c + + 語言、 facilies 和規則，需要有適當的了解這些值類別和它們的參考。 例如，進行複製值，移動一個值，且轉送值給另一個函式的值的位址。 本主題不會進入所有這些方面，深入了解，但它提供純色了解它們的基礎資訊。

本主題中的資訊付出的值類別 Stroustrup 的分析，由兩個獨立的身分識別和 movability [Stroustrup 2013] 屬性。

## <a name="an-lvalue-has-identity"></a>左值有身分識別
它代表擁有*身分識別*為值什麼？ 如果您有 （或您可以採取） 值的記憶體位址，並安全地，使用它，則值擁有身分識別。 如此一來，您可以比較超過內容的值： 您可以比較或身分識別來區分。

*左值*有身分識別。 現已上 「 左值 」 中的 「 l 」 是 「 左 」 （如所示，左左手邊工作分派的） 的縮寫只歷史感興趣的偏好。 在 c + +、 左值可以出現在的左邊*或*右邊的作業。 「 L 」 的 「 值 」，然後，不會實際可協助您理解，也不定義它們的是。 您只需要了解，我們稱之為左值是有身分識別的值。

值的運算式的範例包括： 具名的變數或常數。或函式，傳回的參照。 *沒有*值的運算式的範例包括： 暫存;或函式所傳回的值。

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

現在，雖然陳述式，則為 true 的值都有身分識別，因此請勿 xvalues。 我們將會進入更*xvalue*什麼是本主題稍後的。 現在，只是請注意針對 「 一般化左值 」 呼叫 glvalue，值類別。 Glvalues 的超集包含的值 （也稱為*傳統的值*） 和 xvalues。 因此，而 「 左值所擁有的身分識別 」 是，則為 true，一組完整的身分識別的項目是 glvalues，一組，在此圖例所示。

![左值有身分識別](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>右值是可移動;左值不是
但有不是 glvalues 的值。 因此，有值，您*無法*取得的記憶體位址 （或您不能依賴它為有效）。 我們所見上述的程式碼範例中的某些這類值。 聽起來像是缺點。 但事實上值，利用種也就是您可以*移動*從其 （也就是通常並不昂貴），而不是從其複本 （也就是通常高度耗費資源）。 移值表示它不會再已中則用的位置。 因此，嘗試在它用來進行的位置中存取它是以避免的項目。 何時及*如何*移動的值是超出範圍，如本主題的討論。 本主題中，我們只需要知道的值是可移動稱為*右值*（或*傳統右值*）。

在右 「 值 」 「 r 」 是 「 權利 」 （如所示，右左手邊工作分派的） 的縮寫。 但是，您可以使用右，以及右，指派以外的參考。 然後，在 「 右 」，「 r 」 不是將焦點放在一件事。 您只需要了解，我們稱之為右值是可移動的值。

左值，相反地，不是可移動，在此圖例所示。 移動左值會對抗的*左值*，定義，它會預期的問題，適用於非常合理預期要能夠繼續存取左值的程式碼。

![右值是可移動;左值不是](images/is-movable.png)

您無法移動左值。 但有** 一種 glvalue （組件事與身分識別），您可以移動&mdash;如果您知道您正在執行的動作 （包括要小心，不要在移動後無法存取它）&mdash;而這就是 xvalue。 我們將會重新瀏覽這個主意一次下方，當我們來看看值類別的完整的分析。

## <a name="rvalue-references-and-reference-binding-rules"></a>右值的參考和參考資料繫結規則
本節介紹右值的參考的語法。 我們必須等候另一個主題，以移至 [更多的處理方式的移動並轉送，但這些是由右值的參考已解決的問題。 我們來看看右值的參考之前，不過，我們必須先是關於清楚`T&`&mdash;一件事我們已經先前已呼叫只是 「 參考 」。 它真的是 「 左值 (非 const) 參考 」，指的是要參考的使用者可以撰寫一個值。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

左值，而非右值，可以繫結左值的參考。

然後有左 const 參考 (`T const&`)，其參考寫入 （例如，常數） 的使用者*無法*參考的物件。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

左 const 參考可以繫結至左或右值。

適用於右值類型的參考資料的語法`T`會寫成`T&&`。 可移動值是指右值參考&mdash;我們不需要保留之後，我們使用它 （例如，暫存） 其內容的值。 因為整個點可從 （因而修改） 值繫結到右值的參考，`const`和`volatile`限定詞 （也稱為限定詞 cv） 不適用於右值的參考。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

右值參考繫結至右值。 事實上，方面的多載解析度，右值*慣用*繫結至左 const 參考比右值參考。 但右值參考無法繫結至左值因為說過，右值參考指的是值，它假設我們不需要保留 （例如，移動建構函式的參數） 其內容。

您也可以傳遞右值其中一個值的引數預期，透過複製建構 （或移動建構，如果左值是 xvalue）。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Glvalue 有身分識別。prvalue 沒有
在這個階段，我們知道什麼有身分識別。 而且我們知道什麼是可移動和項目不是。 但是我們還名為一組值*不*該有的身分識別。 該集合稱為*prvalue*或*單純右值*。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![左值有身分識別。prvalue 沒有](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>值類別的完整的分析
它只會保留來結合成單一、 大圖片上述圖例與資訊。

![值類別的完整的分析](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Glvalue （一般化左） 都有身分識別。

### <a name="lvalue-im"></a>左值 （i\ 與 \!m）
左值 （一種 glvalue） 有識別身分，但不是可移動。 這些是您傳遞周圍參考或 const 參考資料，或值並不昂貴複製是否通常讀寫值。 無法繫結左到右值的參考。

### <a name="xvalue-im"></a>xvalue (i\ & m)
有識別身分，同時也可移動 xvalue （一種 glvalue，但也是一種右值）。 這可能是奔左值，您決定將移動，因為複製耗費大量資源，而您可以小心，不要之後存取它。 以下是如何您可以將左值轉變 xvalue。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

在上述的程式碼範例中，我們還沒有尚未移動任何項目。 我們剛建立了 xvalue 由左到右命名的值的參考值來轉型。 仍然能夠加以識別透過它左值的名稱;但是，做為 xvalue，是，現在*能夠*移動。 如此一來的原因，而哪些移動的實際外觀，必須等待的其他主題。 但您可以將 「 xvalue 」 做為意義 「 專家-僅限 」，可協助如果中的"x"。 將左值轉換成 xvalue （一種右值），該值就會變成可繫結到右值的參考。

以下是其他兩個 xvalues 範例&mdash;呼叫的函式，傳回未命名的右值的參考，並存取 xvalue 的成員。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Prvalue （純右值; 一種右值） 不會有識別身分，但可移動。 這些通常都暫存檔，傳回的值或任何其他不是 glvalue 的運算式的評估結果的呼叫函式的結果

### <a name="rvalue-m"></a>右值 (m)
右值是可移動。 右值*的參考*一律是指右值 （它假設其內容，因此不需要保留的值）。

但是，是右值參考本身右值？ *未命名*的右值參考 （例如 「 上述的 xvalue 程式碼範例所示） 是 xvalue 是的因此，它會是在右值。 它慣用繫結至右值參考函式參數，例如，移動建構函式。 相反地 （和或許 counter-intuitively），如果右值參考有一個名稱，則該名稱所組成的運算式是左值。 因此它*無法*繫結至右值的參考參數。 很容易就能讓您這樣做，但是&mdash;只是將其轉換為未命名的右值的參考 (xvalue) 一次。

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

### <a name="im"></a>\!i\ 與 \!m
不會有身分識別，並不是可移動值的類型是一個組合我們尚未討論。 但我們可以忽略它，因為該類別不是很有用的做法，在 c + + 語言中。

## <a name="reference-collapsing-rules"></a>參考摺疊的規則
運算式 （左值的參考到左值的參考或右值參考右值的參考） 中的多個 like 參考取消一個另一個跨。

- `A& &` 摺疊到`A&`。
- `A&& &&` 摺疊到`A&&`。

不同於在運算式中參考的多個摺疊到左值的參考。

- `A& &&` 摺疊到`A&`。
- `A&& &` 摺疊到`A&`。

## <a name="forwarding-references"></a>轉送參考
本節將最終對照右值參考資料，我們已討論過，使用不同的*轉送參考*的概念。

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 我們已經看過，則是右值參考資料。 Const 和動態不適用於右值的參考。
- `foo` 接受只類型**A**的右。
- 原因右值的參考 (例如`A&&`) 存在於是，這樣您可以撰寫的多載可最適合的暫存 （或其他右值） 傳遞的情況。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` 是*轉送參考*。 視您傳遞給`bar`，輸入 **_Ty**可能是 const/非常考慮揮發性/非揮發性。
- `bar` 接受任何左或右值的類型 **_Ty**。
- 傳遞左值會導致轉送參考，以使它成為`_Ty& &&`，這會摺疊到左值的參考`_Ty&`。
- 傳遞右值會導致轉送參考，以使它成為`_Ty&& &&`，這會摺疊到右值的參考`_Ty&&`。
- 轉送參考的原因 (例如`_Ty&&`) 存在於是*不*進行最佳化，但需要您傳遞給他們和無障礙地且有效率地轉送上。 您很可能會遇到轉送的參考，只有當您撰寫 （或仔細研究） 文件庫的程式碼&mdash;例如，在建構函式引數轉寄 factory 函式。

## <a name="sources"></a>[來源]
* \[Stroustrup，2013\] B Stroustrup: c + + 程式設計語言，第四個版本。 Addison Wesley。 2013。
