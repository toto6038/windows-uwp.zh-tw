---
author: stevewhims
description: 本主題說明各種類別的 c + + 中存在的值。 您相信有聽到的值和右，但還有其他種類的太。
title: 值類別及給他們參考
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 預測、 移動、 轉接、 值類別、 move 語意、 完美轉接、 左值、 右值、 glvalue、 prvalue、 xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2835030"
---
# <a name="value-categories-and-references-to-them"></a>值類別及給他們參考
本主題說明存在於 c + + 中的各種類別的值 （以及參考以值）。 您相信有聽到*的值*和*右*，但是您可能不認為這些本主題提供的合約。 並有一些太其他種類的值。

在 c + + 中的每個運算式求得屬於其中一個類別本主題中所討論的值。 有方面的 c + + 語言、 facilies、 及需求的適當了解，這些值類別及給他們參考的規則。 例如，進行複製值、 移動值和轉接到另一個函數的值值的地址。 本主題不會移到所有的這些方面的深度，但它可提供深入瞭解這些基礎資訊。

本主題中的資訊是做為外框值類別的 Stroustrup 的分析出現兩個獨立屬性以及該屬性的身分識別及 movability [Stroustrup 2013]。

## <a name="an-lvalue-has-identity"></a>左值有身分識別
是什麼意思具有*identity*值？ 如果您有 （或您可以採取） 值的記憶體地址和使用安全地，則的值有身分識別。 如此一來，您可以執行一個以上的比較的值內容： 您可以比較或區別他們的身分識別。

*左值*具有 identity。 它只是現在的僅限歷史利息"左值"在"l"是"left"（如同、 左左手邊的工作分派） 的縮寫。 在 c + +、 左值可以出現在左邊緣*或*上右邊的工作分派。 "L"在"值"然後不會真的幫助您理解也無法定義及其。 您只需要了解我們呼叫左值是具有 identity 的值。

運算式的值的範例包括： 具名的變數或常數;或函數會傳回的參照。 不*是*值的運算式的範例包括： 是暫時性;或函數會傳回值。

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

現在時的值是 identity，則為 true 陳述式，, 所以不要 xvalues。 我們將更多移到哪些*xvalue*是在本主題後面。 現在，只是注意呼叫 glvalue，"的通用左值"值類別。 Glvalues 超集包含的值 （也稱為*古典的值*） 和 xvalues。 因此，時 「 左值含有 identity 為"是 true，一組完整的 identity 的那 glvalues、 一組本圖所示。

![左值有身分識別](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>右值是可移動;左值不是
但不是 glvalues 的值。 因此，有您*不能*取得記憶體位址 （或您不能依賴它是有效） 的值。 我們看到上述程式碼範例中這類的值。 這一項缺點的聲音。 但事實上值利用像是也就是您可以*移動*它 （這是一般便宜），而不是從它的副本 （這是一般昂貴）。 移動的值表示它不再是設為其所使用的地方。 因此，嘗試存取它是所使用的地方是以避免使用。 何時和*如何*移動值超出範圍本主題討論。 本主題中，我們只需要知道的值是可移動稱為*右值*（或*古典右值*）。

"R"在"右值"是 「 右側 」 （如同、 右左手邊的工作分派） 的縮寫。 但是您可以使用右、 和右、 工作分派以外的參照。 然後"R"在"右"不是以專注於事。 您只需要瞭解我們呼叫右值不是可移動的值。

左值，反之，不是可移動，在此圖所示。 移動左值會對抗的*左值*，定義及是說過非常合理預期能夠持續存取左值的程式碼未預期的問題。

![右值是可移動;左值不是](images/is-movable.png)

您無法將左值。 但是否有** 一種 glvalue （事項與身分識別一組），則可以移&mdash;如果您知道您正在執行的動作 （包括正在小心不要將存取移動後）&mdash;屬於 xvalue。 我們再探討此概念一更多時間下，當我們查看值類別的完整圖片。

## <a name="rvalue-references-and-reference-binding-rules"></a>右值參考及參照繫結規則
本節介紹右值的參考的語法。 我們將必須等待移到明顯的處理方式移動和轉接、 另一個主題，但這些是由右值參照解決的問題。 我們查看右值參照之前，儘管如此，我們首先需要是關於較清晰`T&`&mdash;事我們已以前已呼叫只是"參考 （英文）"。 其確實是"左值 (非 const) 參考 （英文）"，其中參照參照的使用者可寫入的值。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

右值左值，但不是可以將繫結左值參考 （英文）。

然後有左值 const 參照 (`T const&`)，以參照的物件的使用者*不能*參考 （英文) 寫入 （例如常數）。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

左值 const 參考可繫結至左值或右值。

參照類型的右值的語法`T`撰寫成`T&&`。 右值參照參照可移動值&mdash;我們不需要保留之後我們使用它 （例如暫存） 其內容的值。 自整個點時機 （進而修改） 值繫結至右值參考 （英文）、`const`和`volatile`限定詞 （也稱為限定詞 cv） 不適用於右值參照。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

右值參考 （英文） 繫結至右值。 事實上，就右值*偏好*繫結至左值 const 參考比右值參照的多載解析度。 但右值參考 （英文） 無法繫結至左值因為說過、 右值 reference 參照一個值則假定它是我們不需要保留 (say、 move 建構函式的參數) 其內容。

您也可以傳遞右值所在的數值引數預期，透過副本情況下 （或透過移動情況下，如果右值為 xvalue）。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Glvalue 已識別身分 ；prvalue 沒有
在此階段中，我們知道什麼具有 identity。 與我們知道什麼是可移動和項目不是。 但我們還沒有名為一組值*不*具有 identity。 該集就是所謂的*prvalue*或*完全右值*。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![左值已識別身分 ；prvalue 沒有](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>值類別的完整圖片
它只會維持合併資訊以及上方到單一的大圖片的圖例。

![值類別的完整圖片](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Glvalue （一般左值） 具有 identity。

### <a name="lvalue-im"></a>左值 (i\ & \!m)
左值 （一種 glvalue） 身分識別，但不是可移動。 這些是您傳遞周圍參考 （英文） 或 const 參考或值如果複製為便宜通常是可讀寫值。 左值不能繫結至右值參考 （英文）。

### <a name="xvalue-im"></a>xvalue (i\ & m)
Xvalue （種 glvalue，但也是一種右值） 身分識別，而且也是可移動。 這可能是您已決定要移動複製是昂貴，因為奔左值和必須小心不要將認知加以存取。 以下是如何您可轉換為左值 xvalue。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

在上述程式碼範例中，我們尚未尚未移動任何項目。 我們已建立 xvalue 由轉換左值至未命名右值參考 （英文）。 仍然可以識別以其左值名稱 ；不過，為 xvalue，現在*可使用*的正在移動。 能力、 原因及何種移動實際上起來，必須以等候另一個主題。 但是您可以設想"xvalue"做為意義"專家僅"如果這有助於在"x"。 將左值轉換成 xvalue （一種右值），此值會變成雖能夠所繫結至右值參考 （英文）。

以下是 xvalues 的兩個其他範例&mdash;呼叫會傳回未命名右值參考 （英文）、 函數及存取 xvalue 的成員。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Prvalue （純粹右值 ； 右值的類型） 都不會有 identity、 但可移動。 這些通常是暫存檔、 結果的呼叫該函數會傳回值，或不是 glvalue 的任何其他運算式的評估結果

### <a name="rvalue-m"></a>右值 (m)
右值是可移動。 右值*參照*一律參照右值 （假設我們不需要保留其內容的值）。

不過，右值參照本身右值吗？ *未命名*右值參考 （英文） （例如過上述 xvalue 程式碼範例所示） 是 xvalue 是的如此，它會是在右值。 其偏好繫結至右值參照函數參數，例如移動建構函式。 相反地 （與或許 counter-intuitively） 如果右值參考 （英文） 有名稱，則該名稱所組成的運算式是左值。 讓它*不能*繫結至右值參照參數。 但是很容易讓這麼&mdash;只是將它轉換為未命名右值參照 (xvalue) 一次。

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

### <a name="im"></a>\!i\ & \!m
值都不會有身分識別並不是可移動類型是我們尚未尚未討論一個組合。 但是我們可以略過，因為該類別無法在 c + + 語言是有用的做法。

## <a name="reference-collapsing-rules"></a>參考 （英文） 摺疊的規則
運算式 （左值參考 （英文）、 左值參照或右值參照右值參照） 中的多個 like 參考取消一個其他 （英文）。

- `A& &` 摺疊成`A&`。
- `A&& &&` 摺疊成`A&&`。

向左值參照摺疊不同運算式中參照多。

- `A& &&` 摺疊成`A&`。
- `A&& &` 摺疊成`A&`。

## <a name="forwarding-references"></a>轉寄參考
最終本節比較右值參照，我們已經討論過，具有不同的*轉接參考 （英文）* 的概念。

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 是右值參照，如我們所見。 Const 和動態不適用於右值參照。
- `foo` 接受只有**A**類型的右。
- 變更理由右值參照 (例如`A&&`) 存在於會使您可以撰寫是暫時性 （或其他右值） 所傳遞的大小寫的功能已最佳化多載。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` 已*轉接參考 （英文）*。 這取決於您傳遞到`bar`、 類型 **_Ty**可能是 const/非-const 獨立動態/非動態。
- `bar` 可接受任何左值或右值的類型 **_Ty**。
- 傳遞給左值會導致轉接參照成為`_Ty& &&`，其摺疊的左值參照`_Ty&`。
- 傳遞給右值會導致轉接參照成為`_Ty&& &&`，其摺疊的右值參照`_Ty&&`。
- 轉寄參照的原因 (例如`_Ty&&`) 存在於是*無法*進行最佳化，但才會傳遞給他們和轉寄上透明和有效率地。 您是可能遇到的轉接參考只有在您撰寫 （或密切研究） 文件庫的程式碼&mdash;例如轉寄建構函式引數的原廠函數。

## <a name="sources"></a>[來源]
* \[Stroustrup，2013\] B.Stroustrup: c + + 程式設計語言、 第四個版本。 Addison Wesley。 2013。
