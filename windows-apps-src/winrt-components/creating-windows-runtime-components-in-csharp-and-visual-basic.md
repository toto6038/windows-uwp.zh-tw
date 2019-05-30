---
title: 在 C# 和 Visual Basic 中建立 Windows 執行階段元件
description: 從 .NET Framework 4.5 開始，您可以使用 Managed 程式碼自行建立封裝在 Windows 執行階段元件中的 Windows 執行階段類型。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a20f8e85927015d6aa69dbbf4381cb2d7daafc36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372034"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>在 C# 和 Visual Basic 中建立 Windows 執行階段元件
從.NET Framework 4.5 開始，您可以建立自己的 Windows 執行階段型別，並封裝它們以在 Windows 執行階段元件中使用 managed 程式碼。 您可以使用您的元件中撰寫的通用 Windows 平台 (UWP) 應用程式C++、 JavaScript、 Visual Basic 或C#。 本主題將概述建立元件的規則，並就某些層面討論 .NET Framework 對於 Windows 執行階段的支援。 一般而言，該支援依設計應可讓 .NET Framework 程式設計人員清楚理解。 但是，當您建立要與 JavaScript 或 C++ 搭配使用的元件時，必須了解這些語言對於 Windows 執行階段的支援方式有何差異。

如果您要建立使用的元件只能在 Visual Basic 中撰寫的 UWP 應用程式或C#，而且此元件不包含 UWP 控制項，則請考慮使用**類別庫**範本，而不要**Windows執行階段元件**Microsoft Visual Studio 中的專案範本。 簡單類別庫的限制比較少。

## <a name="declaring-types-in-windows-runtime-components"></a>在 Windows 執行階段元件中宣告類型

就內部而言，Windows 執行階段類型，在您的元件可以使用 UWP 應用程式中不允許任何.NET Framework 功能。 如需詳細資訊，請參閱 <<c0> [ 適用於 UWP 應用程式的.NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)。

在外部，您的型別成員可以公開 （expose） 只有其參數的 Windows 執行階段型別和傳回值。 下列清單說明在從 Windows 執行階段元件所公開的.NET Framework 型別上的限制。

- 在您的元件中，所有公用類型和成員的欄位、參數及傳回值都必須是 Windows 執行階段類型。 這項限制包含您撰寫以及類型所提供的 Windows 執行階段本身的 Windows 執行階段類型。 其中也包括多種 .NET Framework 類型。 納入這些型别，是支援的.NET Framework 提供自然地使用 managed 程式碼中的 Windows 執行階段&mdash;使用熟悉的.NET Framework 型別，而不是基礎 Windows 執行階段類型的程式碼會出現。 比方說，您可以使用.NET Framework 基本型別這類**Int32**並**Double**，特定的基礎型別這類**DateTimeOffset**和**Uri**，和一些常用泛型介面型別這類**IEnumerable&lt;T&gt;**  (在 Visual Basic 中的有 IEnumerable (Of T)) 和**IDictionary&lt;TKey，TValue&gt;** 。 請注意，這些泛型型別的型別引數必須是 Windows 執行階段類型。 這幾節會討論[將 Windows 執行階段類型至 managed 程式碼](#passing-windows-runtime-types-to-managed-code)並[managed 類型傳遞給 Windows 執行階段](#passing-managed-types-to-the-windows-runtime)，本主題稍後的。

- 公用類別與介面可包含方法、屬性與事件。 您可以為您的事件宣告委派，或使用**事件處理常式&lt;T&gt;** 委派。 公用類別或介面不可：
    - 屬於泛型。
    - 實作的介面，不是 Windows 執行階段介面 （不過，您可以建立您自己的 Windows 執行階段介面和實作它們）。
    - 衍生自不在 Windows 執行階段中，這類的型別**System.Exception**並**System.EventArgs**。

- 所有的公用類型都必須具有符合組件名稱的根命名空間，且組件名稱的開頭不可以是 "Windows"。

    > **提示**。 根據預設，Visual Studio 專案具有符合組件名稱的命名空間名稱。 在 Visual Basic 中，此預設命名空間的 Namespace 陳述式不會顯示在您的程式碼中。

- 公用結構不可以有公用欄位以外的任何成員，而且這些欄位必須是實值類型或字串。
- 公用類別必須是 **sealed** (在 Visual Basic 中為 **NotInheritable**)。 如果您的程式設計模型需使用多型，然後您可以建立公用介面，並必須是多型的類別上實作該介面。

## <a name="debugging-your-component"></a>偵錯您的元件

如果您的 UWP 應用程式與元件建立的 managed 程式碼，然後您可以偵錯兩者都在相同的時間。

當您測試您的 UWP 應用程式，使用一部分的元件C++，您可以同時偵錯 managed 和原生程式碼。 預設值為僅限機器碼。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>同時對 C++ 機器碼與 Managed 程式碼進行偵錯
1.  為您的 Visual C++ 專案開啟捷徑功能表，然後選擇 [屬性]  。
2.  在屬性頁中的 [組態屬性]  下方，選擇 [偵錯]  。
3.  選擇 [偵錯工具類型]  ，然後在下拉式清單方塊中，將 [僅限機器碼]  變更為 [混合 (Managed 和機器碼)]  。 選擇 [確定]  。
4.  在機器碼與 Managed 程式碼中設定中斷點。

當您要測試您的元件使用 JavaScript 的 UWP 應用程式的一部分時，預設方案會處於 JavaScript 偵錯模式。 在 Visual Studio 中，您無法同時偵錯 JavaScript 和 Managed 程式碼。

## <a name="to-debug-managed-code-instead-of-javascript"></a>偵錯 Managed 程式碼，而不偵錯 JavaScript
1.  為您的 JavaScript 專案開啟捷徑功能表，然後選擇 [屬性]  。
2.  在屬性頁中的 [組態屬性]  下方，選擇 [偵錯]  。
3.  選擇 [偵錯工具類型]  ，然後在下拉式清單方塊中，將 [僅限指令碼]  變更為 [僅限 Managed]  。 選擇 [確定]  。
4.  在 Managed 程式碼中設定中斷點，然後如常進行偵錯。

## <a name="passing-windows-runtime-types-to-managed-code"></a>將 Windows 執行階段類型傳遞至 Managed 程式碼
如先前一節中所述[宣告 Windows 執行階段元件中的型別](#declaring-types-in-windows-runtime-components)，特定.NET Framework 型别可出現在公用類別成員的簽章。 這是 .NET Framework 為了方便您在 Managed 程式碼中使用 Windows 執行階段而提供的支援。 其中包括基本類型以及一些類別和介面。 當您的元件從 JavaScript，或使用從C++程式碼中，務必知道您的.NET Framework 型別顯示呼叫端的方式。 請參閱[逐步解說：建立簡單的元件，在C#或 Visual Basic 和從 JavaScript 呼叫該](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)如需 JavaScript 的範例。 本節將討論常用的類型。

在.NET Framework 中，基本型別例如**Int32**結構可有許多有用的屬性和方法，例如**TryParse**方法。 相對地，Windows 執行階段中的基本類型和結構就只有欄位而已。 當您將這些類型傳遞至 Managed 程式碼時，它們將顯示為 .NET Framework 類型，而您可以如常使用 .NET Framework 類型的屬性和方法。 下列清單將摘要說明在 IDE 中自動執行的替代：

-   Windows 執行階段基本類型的**Int32**， **Int64**，**單一**， **Double**，**布林**， **字串**（Unicode 字元的不可變集合）， **Enum**， **UInt32**， **UInt64**，以及**Guid**，在 System 命名空間中使用相同名稱的型別。
-   針對**UInt8**，使用**System.Byte**。
-   針對**Char16**，使用**System.Char**。
-   針對**IInspectable**介面，請使用**System.Object**。

如果C#或 Visual Basic 為任何這些型別，提供了語言關鍵字，則您可以改為使用該語言關鍵字。

除了基本類型以外，有些基本且常用的 Windows 執行階段類型在 Managed 程式碼中也會顯示為其 .NET Framework 對等類型。 例如，假設您的 JavaScript 程式碼使用**Windows.Foundation.Uri**類別，而您想要將它傳遞給C#或 Visual Basic 方法。 Managed 程式碼中的對等型別是.NET Framework **System.Uri**類別，是要用於方法參數的型別。 由於 Visual Studio 中的 IntelliSense 在您撰寫 Managed 程式碼時會隱藏 Windows 執行階段類型，並顯示對等的 .NET Framework 類型，因此您知道 Windows 執行階段類型何時會呈現為 .NET Framework 類型。 (這兩個類型通常會具備相同的名稱。 但請注意， **Windows.Foundation.DateTime**結構會出現在 managed 程式碼當做**System.DateTimeOffset**而非**System.DateTime**。)

對於某些常用的集合類型，會在 Windows 執行階段類型所實作的介面與相對應的 .NET Framework 類型所實作的介面之間進行對應。 如同先前提到的類型，您也可以使用 .NET Framework 類型來宣告參數類型。 這會隱藏不同類型之間的某些差異，以方便撰寫 .NET Framework 程式碼。

下表列出其中最常見的泛型介面類型，以及其他的通用類別與介面對應。 如需.NET Framework 對應的 Windows 執行階段類型的完整清單，請參閱 < [Windows 執行階段類型的.NET Framework 對應](net-framework-mappings-of-windows-runtime-types.md)。

| Windows 執行階段                                  | .NET Framework                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K, V&gt;                                 | IDictionary&lt;Dictionary<tkey，Tvalue>&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary&lt;TKey, TValue&gt;           |
| IKeyValuePair&lt;K, V&gt;                        | KeyValuePair&lt;TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

當某個類型實作多個介面時，您可以將它實作的任何介面當做成員的參數類型或傳回類型。 例如，您可以傳遞或傳回**字典&lt;字串為 int&gt;**  (**Dictionary （Of Integer，字串）** 在 Visual Basic 中) 做為**IDictionary&lt;int，字串&gt;** ， **Ireadonlydictionary<string&lt;int、 字串&gt;** ，或**IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;&gt;** 。

> [!IMPORTANT]
> JavaScript 會使用 managed 的類型實作的介面清單中第一個出現的介面。 比方說，如果您返回**字典&lt;字串為 int&gt;**  JavaScript 程式碼，它會顯示為**IDictionary&lt;int、 字串&gt;** 不論使用哪些您指定的傳回型別為介面。 這表示，如果第一個介面不包含出現在後續介面上的成員，該成員即不會對 JavaScript 顯示。

在 Windows 執行階段**IMap&lt;K，V&gt;** 並**Imapview<k&lt;K，V&gt;** 使用 Inputiterator<ikeyvaluepair<k 重複處理。 當您將其傳遞至 managed 程式碼時，它們會顯示成**IDictionary&lt;TKey，TValue&gt;** 並**Ireadonlydictionary<string&lt;TKey，TValue&gt;** ，因此您使用自然**System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;** 加以列舉。

介面顯示於 Managed 程式碼中的方式會影響實作這些介面之類型的顯示方式。 例如， **PropertySet**類別會實作**IMap&lt;K，V&gt;** ，會出現在 managed 程式碼當做**IDictionary&lt;TKey，TValue&gt;** . **PropertySet**如同實作**IDictionary&lt;TKey，TValue&gt;** 而非**IMap&lt;K，V&gt;** ，因此在受管理程式碼，它似乎有**新增**方法，其運作方式如同**新增**.NET Framework 字典上的方法。 它看起來並沒有**插入**方法。 您可以看到此主題中的範例[逐步解說：建立簡單的元件，在C#或 Visual Basic 和從 JavaScript 呼叫該](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>將 Managed 類型傳遞至 Windows 執行階段

如上一節所討論，某些 Windows 執行階段類型在您元件成員的簽章中 (如果在 IDE 中使用這些類型，則是在 Windows 執行階段成員的簽章中) 會顯示為 .NET Framework 類型。 當您將 .NET Framework 類型傳遞給這些成員，或使用這些類型做為元件成員的傳回值時，它們在另一端的程式碼上便會顯示為對應的 Windows 執行階段類型。 如這可能會有從 JavaScript 呼叫您的元件時效果的範例，請參閱中的 「 傳回 managed 從您的元件類型 」 一節[逐步解說：建立簡單的元件，在C#或 Visual Basic 和從 JavaScript 呼叫該](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="overloaded-methods"></a>多載方法

在 Windows 執行階段中，方法可以是多載的。 不過，如果您宣告具有相同數量參數的多個多載時，您必須套用[ **Windows.Foundation.Metadata.DefaultOverloadAttribute** ](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)屬性設定為其中一個多載。 這就是您唯一可從 JavaScript 呼叫的多載。 例如，在下列程式碼中，使用 **int** (在 Visual Basic 中為 **Integer**) 的多載是預設多載。

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> [重要]JavaScript 可讓您將任何值傳遞至**OverloadExample**，強制轉型為型別所需的參數值。 您可以呼叫**OverloadExample**使用"forty-"，"42"或 42.3，但所有這些值會傳遞至預設多載。 在上述範例中的預設多載會分別傳回 0、 42 和 42。

您不能套用**DefaultOverloadAttribut**e 屬性建構函式。 一個類別中的所有建構函式必須要有不同數量的參數。

## <a name="implementing-istringable"></a>實作 IStringable

從 Windows 8.1 開始，Windows 執行階段包含**IStringable**介面方法**IStringable.ToString**，提供相當於提供基本格式支援**Object.ToString**。 如果您選擇實作**IStringable**公用 managed 類型中的 Windows 執行階段元件中匯出，適用下列限制：

-   您可以定義**IStringable**只能在 「 類別實作 」 的關聯性，如下列程式碼中的介面C#:

    ```cs
    public class NewClass : IStringable
    ```

    或是下列 Visual Basic 程式碼：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   您不能實作**IStringable**介面上。
-   您不能宣告為類型參數**IStringable**。
-   **IStringable**不可為方法、 屬性或欄位的傳回型別。
-   您無法隱藏您**IStringable**實作基底類別使用的方法定義，如下所示：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    相反地， **IStringable.ToString**實作一律必須覆寫基底類別實作。 您可以隱藏**ToString**只能由強型別的類別執行個體上叫用的實作。

> [!NOTE]
> 各種情況下呼叫原生程式碼到 managed 類型可實作**IStringable**或隱藏其**ToString**實作可能會產生非預期的行為。

## <a name="asynchronous-operations"></a>非同步作業

若要在元件中實作非同步方法，方法名稱的結尾加上"Async"，並傳回代表非同步動作或作業的 Windows 執行階段介面的其中一個：**IAsyncAction**， **IAsyncActionWithProgress&lt;Tprogress>&gt;** ， **IAsyncOperation&lt;TResult&gt;** ，或**IAsyncOperationWithProgress&lt;Iasyncoperationwithprogress<tresult，Tprogress>&gt;** 。

您可以使用.NET Framework 工作 ( [**任務**](/dotnet/api/system.threading.tasks.task)類別和泛型[**工作&lt;TResult&gt;**  ](/dotnet/api/system.threading.tasks.task-1)類別) 至實作您的非同步方法。 您必須傳回代表進行中作業，例如撰寫的非同步方法會傳回工作的工作C#或 Visual Basic 中或從傳回的工作[ **Task.Run** ](/dotnet/api/system.threading.tasks.task.run)方法。 如果您使用建構函式來建立此工作，就必須先呼叫其 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 方法，再加以傳回。

可使用的方法`await`(`Await` Visual Basic 中) 需要`async`關鍵字 (`Async` Visual Basic 中)。 如果您公開這種方法從 Windows 執行階段元件時，套用`async`關鍵字，您將傳遞至委派**執行**方法。

對於不支援取消或進度報告的非同步動作與作業，您可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) 或 [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) 擴充方法，將工作包覆在適當的介面中。 例如，下列程式碼會實作非同步方法使用**Task.Run&lt;TResult&gt;** 啟動工作的方法。 **AsAsyncOperation&lt;TResult&gt;** 擴充方法會傳回與 Windows 執行階段非同步作業的工作。

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

下列 JavaScript 程式碼顯示如何藉由呼叫方法[ **WinJS.Promise** ](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10))物件。 完成非同步呼叫時，便會執行傳遞至 then 方法的函式。 StringList 參數包含所傳回的字串清單**DownloadAsStringAsync**方法和函式會執行任何處理所需。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

非同步動作和作業支援取消或進度報告，請使用[ **AsyncInfo** ](/dotnet/api/system.runtime.interopservices.windowsruntime)類別來產生啟動的工作，並且將連結的取消和進度報告在工作的取消和進度報告功能的適當的 Windows 執行階段介面的功能。 如需支援取消與進度報告的範例，請參閱[逐步解說：建立簡單的元件，在C#或 Visual Basic 和從 JavaScript 呼叫該](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

請注意，您可以使用的方法**AsyncInfo**類別，即使您的非同步方法不支援取消或進度報告。 如果您使用 Visual Basic lambda 函式或C#匿名方法中，不提供權杖的參數和[ **IProgress&lt;T&gt;**  ](https://docs.microsoft.com/dotnet/api/system.iprogress-1?redirectedfrom=MSDN)介面。 如果您使用 C# Lambda 函式，請提供語彙基元參數，但加以忽略。 上述的範例中，使用 AsAsyncOperation&lt;TResult&gt;方法，看起來像這樣，當您使用[ **AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken，工作&lt;TResult&gt;&gt;** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime?redirectedfrom=MSDN)) 改為方法多載。

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

如果您建立選擇性支援取消或進度報告的非同步方法時，請考慮將沒有參數的取消語彙基元的多載或**IProgress&lt;T&gt;** 介面。

## <a name="throwing-exceptions"></a>擲回例外狀況

您可以擲回適用於 Windows app 的 .NET 所包含的任何例外狀況類型。 您無法在 Windows 執行階段元件中宣告自己的公用例外狀況類型，但可宣告和擲回非公用類型。

如果您的元件不會處理例外狀況，呼叫該元件的程式碼中將會引發對應的例外狀況。 例外狀況對呼叫端的顯示方式，取決於呼叫的語言支援 Windows 執行階段的方式。

-   在 JavaScript 中，例外狀況會顯示為物件，其中例外狀況訊息會由堆疊追蹤所取代。 當您在 Visual Studio 中偵錯 app 時，可以在偵錯工具的 [例外狀況] 對話方塊中看到標示為「WinRT 資訊」的原始訊息文字。 您無法從 JavaScript 程式碼存取原始訊息文字。

    > **提示**。 目前的堆疊追蹤包含 managed 例外狀況型別，但是不建議透過剖析追蹤以識別例外狀況類型。 而是改用 HRESULT 值 (本節稍後會加以說明)。

-   在 C++ 中，例外狀況會顯示為平台例外狀況。 如果 managed 例外狀況的 HResult 屬性可以對應至特定平台例外狀況的 HRESULT，則使用特定的例外狀況否則，請[ **platform:: comexception** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class)擲回例外狀況。 C++ 程式碼無法使用 Managed 例外狀況的訊息文字。 如果是擲回特定平台例外狀況，則會顯示該例外狀況類型的預設訊息文字，否則不會顯示任何訊息文字。 請參閱[例外狀況 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx)。
-   在 C# 或 Visual Basic 中，例外狀況就是一般的 Managed 例外狀況。

當您從自己的元件擲回例外狀況時，為了讓 JavaScript 或 C++ 呼叫端更容易處理此例外狀況，可以擲回非公開的例外狀況類型，並將其 HResult 屬性值設定為該元件專屬的值。 HRESULT 可供 JavaScript 呼叫者可以透過例外狀況物件的數字的屬性，以及C++呼叫者可以透過[ **comexception:: Hresult** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult)屬性。

> [!NOTE]
> 使用您的 HRESULT 為負數值。 正值會被解譯為成功，如此在 JavaScript 或 C++ 呼叫端中，就不會擲回任何例外狀況。

## <a name="declaring-and-raising-events"></a>宣告和引發事件

當您宣告某個類型包含事件的資料時，請從 Object 衍生，而不要從 EventArgs 衍生，因為 EventArgs 不是 Windows 執行階段類型。 使用  [**事件處理常式&lt;TEventArgs&gt;**  ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1?redirectedfrom=MSDN)做為事件，並以您的事件引數型別作為泛型型別引數的類型。 引發事件的方式與 .NET Framework 應用程式中的一樣。

從 JavaScript 或 C++ 使用您的 Windows 執行階段元件時，事件會依循這些語言所預期的 Windows 執行階段事件模式。 當您從 C# 或 Visual Basic 使用元件時，事件會顯示為一般的 .NET Framework 事件。 中提供範例[逐步解說：建立簡單的元件，在C#或 Visual Basic 和從 JavaScript 呼叫該](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)。

如果您實作自訂事件存取子 (在 Visual Basic 中為使用 **Custom** 關鍵字宣告事件)，則您在實作時必須依循 Windows 執行階段事件模式。 請參閱 [Windows 執行階段元件中的自訂事件和事件存取子](custom-events-and-event-accessors-in-windows-runtime-components.md)。 請注意，當您處理來自 C# 或 Visual Basic 程式碼的事件時，該事件仍會顯示為一般的 .NET Framework 事件。

## <a name="next-steps"></a>後續步驟

在您建立要供自己使用的 Windows 執行階段元件之後，可能會發現它封裝的功能對其他開發人員也很有用。 有兩種方式可供您選擇來包裝元件並發佈給其他開發人員。 請參閱[發佈 Managed Windows 執行階段元件](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140))。

如需 Visual Basic 和 C# 語言功能以及 Windows 執行階段之 .NET Framework 支援的詳細資訊，請參閱 [Visual Basic 和 C# 語言參考](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)。

## <a name="related-topics"></a>相關主題
* [適用於 UWP 應用程式的.NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [逐步解說：建立簡單的 Windows 執行階段元件，然後從 JavaScript 呼叫該](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
