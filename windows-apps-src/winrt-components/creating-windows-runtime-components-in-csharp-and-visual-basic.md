---
title: Windows 執行階段元件與 C# 和 Visual Basic
description: 從 .NET 4.5 開始，您可以使用 managed 程式碼來建立您自己的 Windows 執行階段類型，封裝在 Windows 執行階段元件中。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
ms.openlocfilehash: a52bca6ecc08016d1cb7633acadd362e2215cd3a
ms.sourcegitcommit: 9beb6cce7375b726ad90ee84b72754268ae2819a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2020
ms.locfileid: "88047775"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>Windows 執行階段元件與 C# 和 Visual Basic

您可以使用 managed 程式碼來建立自己的 Windows 執行階段類型，並將它們封裝在 Windows 執行階段元件中。 您可以在通用 Windows 平臺 (UWP) 以 c + +、JavaScript、Visual Basic 或 c # 撰寫的應用程式中使用您的元件。 本主題將概述建立元件的規則，並就某些層面討論 .NET 對於 Windows 執行階段的支援。 一般而言，該支援依設計應可讓 .NET 程式設計人員清楚理解。 但是，當您建立要與 JavaScript 或 C++ 搭配使用的元件時，必須了解這些語言對於 Windows 執行階段的支援方式有何差異。

如果您要建立只能在以 Visual Basic 或 c # 撰寫的 UWP 應用程式中使用的元件，而元件不包含 UWP 控制項，則請考慮使用 [**類別庫**] 範本，而不是 Microsoft Visual Studio 中的 [ **Windows 執行階段元件**] 專案範本。 簡單類別庫的限制比較少。

## <a name="declaring-types-in-windows-runtime-components"></a>在 Windows 執行階段元件中宣告類型

就內部而言，您元件中的 Windows 執行階段類型可以使用 UWP 應用程式中所允許的任何 .NET 功能。 如需詳細資訊，請參閱適用[于 UWP 應用程式的 .net](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)。

就外部而言，您的型別成員只能公開其參數和傳回值的 Windows 執行階段類型。 下列清單說明從 Windows 執行階段元件公開之 .NET 類型的限制。

- 在您的元件中，所有公用類型和成員的欄位、參數及傳回值都必須是 Windows 執行階段類型。 這項限制包括您所撰寫的 Windows 執行階段類型，以及 Windows 執行階段本身提供的類型。 它也包含許多 .NET 類型。 包含這些類型是 .NET 提供的支援的一部分，可讓您在 managed 程式碼中自然使用 Windows 執行階段您的 &mdash; 程式碼會使用熟悉的 .net 類型，而不是基礎 Windows 執行階段類型。 例如，您可以使用 .NET 基本類型（例如**Int32**和**Double**）、某些基本類型（例如**DateTimeOffset**和**Uri**）和一些常用的泛型介面類別型（例如**ienumerable &lt; (&gt; T** ），Visual Basic) 和**IDictionary &lt; TKey，TValue &gt; **中的 T) 的 ienumerable (。 請注意，這些泛型型別的類型引數必須是 Windows 執行階段類型。 這會在本主題稍後的將[Windows 執行階段類型傳遞至 managed 程式碼](#passing-windows-runtime-types-to-managed-code)和將[managed 類型傳遞至 Windows 執行階段](#passing-managed-types-to-the-windows-runtime)一節中討論。

- 公用類別與介面可包含方法、屬性與事件。 您可以為您的事件宣告委派，或使用**EventHandler &lt; T &gt; **委派。 公用類別或介面無法：
    - 屬於泛型。
    - 執行不是 Windows 執行階段介面的介面 (不過，您可以建立自己的 Windows 執行階段介面，並) 加以執行。
    - 衍生自不在 Windows 執行階段中的類型 **，例如 system.string 和** **system.object**。

- 所有的公用類型都必須具有符合組件名稱的根命名空間，且組件名稱的開頭不可以是 "Windows"。

    > **秘訣**。 根據預設，Visual Studio 專案具有符合組件名稱的命名空間名稱。 在 Visual Basic 中，此預設命名空間的 Namespace 陳述式不會顯示在您的程式碼中。

- 公用結構不可以有公用欄位以外的任何成員，而且這些欄位必須是實值類型或字串。
- 公用類別必須是 **sealed** (在 Visual Basic 中為 **NotInheritable**)。 如果您的程式設計模型需要多型，您可以建立公用介面，然後在必須是多型的類別上執行該介面。

## <a name="debugging-your-component"></a>偵錯您的元件

如果您的 UWP 應用程式和元件都是以受控碼建立的，則您可以同時對兩者進行一次調試。

當您使用 c + + 在 UWP 應用程式中測試您的元件時，您可以同時對 managed 和機器碼進行程式碼的偵錯工具。 預設值為僅限機器碼。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>同時對 C++ 機器碼與 Managed 程式碼進行偵錯
1.  為您的 Visual C++ 專案開啟捷徑功能表，然後選擇 **\[屬性\]**。
2.  在屬性頁中的 **\[組態屬性\]** 下方，選擇 **\[偵錯\]**。
3.  選擇 **\[偵錯工具類型\]**，然後在下拉式清單方塊中，將 **\[僅限機器碼\]** 變更為 **\[混合 (Managed 和機器碼)\]**。 選擇 [確定]。
4.  在機器碼與 Managed 程式碼中設定中斷點。

當您使用 JavaScript 將元件測試為 UWP 應用程式的一部分時，解決方案預設會處於 JavaScript 的「偵測模式」。 在 Visual Studio 中，您無法同時偵錯 JavaScript 和 Managed 程式碼。

## <a name="to-debug-managed-code-instead-of-javascript"></a>偵錯 Managed 程式碼，而不偵錯 JavaScript
1.  為您的 JavaScript 專案開啟捷徑功能表，然後選擇 **\[屬性\]**。
2.  在屬性頁中的 **\[組態屬性\]** 下方，選擇 **\[偵錯\]**。
3.  選擇 **\[偵錯工具類型\]**，然後在下拉式清單方塊中，將 **\[僅限指令碼\]** 變更為 **\[僅限 Managed\]**。 選擇 [確定]。
4.  在 Managed 程式碼中設定中斷點，然後如常進行偵錯。

## <a name="passing-windows-runtime-types-to-managed-code"></a>將 Windows 執行階段類型傳遞至 Managed 程式碼
如先前在宣告[Windows 執行階段元件中的類型](#declaring-types-in-windows-runtime-components)一節中所述，某些 .net 類型可能會出現在公用類別成員的簽章中。 這是 .NET 提供的支援的一部分，可讓您在 managed 程式碼中自然使用 Windows 執行階段。 其中包括基本類型以及一些類別和介面。 當您從 JavaScript 或從 c + + 程式碼使用您的元件時，請務必瞭解 .NET 類型對呼叫端的顯示方式。 如需 JavaScript 的範例，請參閱[建立 c # 或 Visual Basic Windows 執行階段元件的逐步解說，並從 javascript 呼叫它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。 本節將討論常用的類型。

在 .NET 中，例如**Int32**結構的基本型別有許多有用的屬性和方法，例如**TryParse**方法。 相對地，Windows 執行階段中的基本類型和結構就只有欄位而已。 當您將這些型別傳遞至 managed 程式碼時，它們看起來就像是 .NET 型別，而您可以像平常一樣使用 .NET 型別的屬性和方法。 下列清單將摘要說明在 IDE 中自動執行的替代：

-   針對 Windows 執行階段基本類型**Int32**、 **Int64**、 **Single**、 **Double**、 **Boolean**、 **String** (不變的 Unicode 字元集合) 、 **Enum**、 **UInt32**、 **UInt64**和**Guid**，請在系統命名空間中使用相同名稱的型別。
-   針對**UInt8**，請使用**system.object**。
-   針對**Char16**，請使用**system.object**。
-   針對**IInspectable**介面，請使用**system.object**。

如果 c # 或 Visual Basic 為其中任何一種類型提供 language 關鍵字，則您可以改用 language 關鍵字。

除了基本型別以外，某些基本、常用的 Windows 執行階段型別會顯示在 managed 程式碼中作為其 .NET 對等專案。 例如，假設您的 JavaScript 程式碼使用**Windows. Foundation. Uri**類別，而您想要將它傳遞給 c # 或 Visual Basic 方法。 Managed 程式碼中的對等類型是 **.net system.string**類別，而這是要用於方法參數的類型。 您可以告訴 Windows 執行階段型別何時會顯示為 .NET 型別，因為 Visual Studio 中的 IntelliSense 會在您撰寫 managed 程式碼時隱藏 Windows 執行階段型別，並顯示對等的 .NET 型別。 (這兩個類型通常會具備相同的名稱。 不過，請注意，在 managed 程式碼中， **Windows. Foundation. datetime**結構會顯示為**system.object** ，而不是**system.string。 ) **

對於一些常用的集合型別，對應是介於由 Windows 執行階段型別所實做的介面，以及相對應的 .NET 型別所執行的介面之間。 如同上面所述的類型，您可以使用 .NET 類型宣告參數類型。 這會隱藏類型之間的一些差異，並讓撰寫 .NET 程式碼更自然。

下表列出其中最常見的泛型介面類型，以及其他的通用類別與介面對應。 如需 .NET 對應之 Windows 執行階段類型的完整清單，請參閱[Windows 執行階段類型的 .net](net-framework-mappings-of-windows-runtime-types.md)對應。

| Windows 執行階段                                  | .NET                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable &lt; T&gt;                              |
| IVector&lt;T&gt;                                 | IList &lt; T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList &lt; T&gt;                            |
| IMap &lt; K、V&gt;                                 | IDictionary &lt; TKey，TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | ..Ireadonlydictionary<string &lt; TKey，TValue&gt;           |
| Inputiterator<ikeyvaluepair<k &lt; K，V&gt;                        | KeyValuePair &lt; TKey，TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

當某個類型實作多個介面時，您可以將它實作的任何介面當做成員的參數類型或傳回類型。 例如，您可以傳遞或傳回**字典 &lt; int、 &gt; 字串** (**字典 (的整數、字串) **在 Visual Basic 中) 為**IDictionary &lt; int、string &gt; **、 **..ireadonlydictionary<string &lt; int、string &gt; **或**IEnumerable &lt; KeyValuePair &lt; TKey &gt; &gt; ，TValue**。

> [!IMPORTANT]
> JavaScript 會使用 Managed 型別所實作的介面清單中第一個出現的介面。 例如，如果您將**Dictionary &lt; int、 &gt; String**傳回至 JavaScript 程式碼，它就會顯示為**IDictionary &lt; int，字串 &gt; ** ，無論您指定哪一個介面做為傳回型別。 這表示，如果第一個介面不包含出現在後續介面上的成員，該成員即不會對 JavaScript 顯示。

在 Windows 執行階段中，會使用 Inputiterator<ikeyvaluepair<k 來逐一查看**IMap &lt; k、 &gt; v**和**IMapView &lt; K，v &gt; ** 。 當您將它們傳遞至 managed 程式碼時，它們會顯示為**IDictionary &lt; TKey、 &gt; TValue**和 **..ireadonlydictionary<string &lt; TKey、TValue &gt; **，因此，您自然會使用 system.servicemodel. **KeyValuePair &lt; TKey &gt; ，TValue**來列舉它們。

介面在 Managed 程式碼中的顯示方式，會影響到實作這些介面之型别的顯示方式。 例如， **PropertySet**類別會執行**IMap &lt; K，V &gt; **，這會在 managed 程式碼中以**IDictionary &lt; TKey， &gt; TValue**的形式出現。 **PropertySet**看起來就像它實作為**IDictionary &lt; TKey &gt; ，TValue** ，而不是**IMap &lt; K， &gt; V**，因此在 managed 程式碼中，它似乎具有**Add**方法，其行為類似于 .net 字典上的**add**方法。 它似乎沒有**Insert**方法。 您可以在[建立 c # 或 Visual Basic Windows 執行階段元件的逐步解說主題中看到此範例，並從 JavaScript 呼叫它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>將 Managed 類型傳遞至 Windows 執行階段

如上一節所討論，有些 Windows 執行階段型別在您的元件成員的簽章中可能會顯示為 .NET 型別，或是當您在 IDE 中使用這些型別時，在 Windows 執行階段成員的簽章中出現。 當您將 .NET 型別傳遞給這些成員，或使用它們做為元件成員的傳回值時，它們就會顯示在另一端的程式碼，做為對應的 Windows 執行階段型別。 如需從 JavaScript 呼叫您的元件時，這可能會有的效果範例，請參閱[建立 c # 或 Visual Basic Windows 執行階段元件的逐步](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)解說中的「從您的元件傳回 managed 類型」一節，然後從 javascript 呼叫它。

## <a name="overloaded-methods"></a>多載方法

在 Windows 執行階段中，方法可以是多載的。 不過，如果您宣告多個具有相同參數數目的多載，就必須將[**windows.foundation.metadata.defaultoverloadattribute 裝飾**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)屬性只套用至其中一個多載。 這就是您唯一可從 JavaScript 呼叫的多載。 例如，在下列程式碼中，使用 **int** (在 Visual Basic 中為 **Integer**) 的多載是預設多載。

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

> 重大JavaScript 可讓您將任何值傳遞至**OverloadExample**，並將值強制轉型為參數所需的類型。 您可以使用 "42"、"42" 或42.3 呼叫**OverloadExample** ，但這些值全都會傳遞至預設多載。 上一個範例中的預設多載分別會傳回0、42和42。

您無法將**DefaultOverloadAttribut**e 屬性套用至函數。 一個類別中的所有建構函式必須要有不同數量的參數。

## <a name="implementing-istringable"></a>實作 IStringable

從 Windows 8.1 開始，Windows 執行階段包含**IStringable**介面，其單一方法**IStringable ToString**提供基本格式支援，可與**物件 ToString**所提供的相容。 如果您選擇在 Windows 執行階段元件中匯出的公用 managed 類型中執行**IStringable** ，則適用下列限制：

-   您只能在「類別實行」關聯性中定義**IStringable**介面，例如 c # 中的下列程式碼：

    ```cs
    public class NewClass : IStringable
    ```

    或是下列 Visual Basic 程式碼：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   您無法在介面上執行**IStringable** 。
-   您不能將參數宣告為**IStringable**類型。
-   **IStringable**不能是方法、屬性或欄位的傳回型別。
-   您無法使用如下所示的方法定義，從基類隱藏您的**IStringable**執行：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    取而代之的是， **IStringable**必須一律覆寫基類的實作為。 您只能在強型別類別實例上叫用**ToString**執行，藉以隱藏它。

> [!NOTE]
> 在各種情況下，從機器碼呼叫至執行**IStringable**或隱藏其**ToString**執行的 managed 型別，可能會產生非預期的行為。

## <a name="asynchronous-operations"></a>非同步作業

若要在您的元件中執行非同步方法，請將 "Async" 加入至方法名稱的結尾，並傳回代表非同步動作或作業的其中一個 Windows 執行階段介面： **IAsyncAction**、 **iasyncactionwithprogress<tprogress> &lt; TProgress &gt; **、 **iasyncoperation<tresult> &lt; TResult &gt; **或**IAsyncOperationWithProgress &lt; TResult、 &gt; TProgress**。

您可以使用 ([**工作類別和一般工作**](/dotnet/api/system.threading.tasks.task) [** &lt; TResult &gt; **](/dotnet/api/system.threading.tasks.task-1)類別的 .net 工作，) 來執行您的非同步方法。 您必須傳回代表進行中作業的工作，例如，從使用 c # 或 Visual Basic 所撰寫的非同步方法傳回的工作，或是從工作[**. Run**](/dotnet/api/system.threading.tasks.task.run)方法傳回的工作。 如果您使用建構函式來建立此工作，就必須先呼叫其 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 方法，再加以傳回。

在 `await` Visual Basic) 中使用 (的方法 `Await` 需要 `async` `Async` Visual Basic) 中的關鍵字 (。 如果您從 Windows 執行階段元件公開這類方法，請將 `async` 關鍵字套用至您傳遞給**Run**方法的委派。

對於不支援取消或進度報告的非同步動作與作業，您可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system) 或 [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system) 擴充方法，將工作包覆在適當的介面中。 例如，下列程式碼會使用工作來實行非同步方法 **。請執行 &lt; TResult &gt; **方法來啟動工作。 **AsAsyncOperation &lt; TResult &gt; **擴充方法會以 Windows 執行階段非同步作業的形式傳回工作。

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

下列 JavaScript 程式碼顯示如何使用[**WinJS**](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10))的物件來呼叫方法。 完成非同步呼叫時，便會執行傳遞至 then 方法的函式。 StringList 參數包含**DownloadAsStringAsync**方法所傳回的字串清單，而函式會執行任何必要的處理。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

對於支援取消或進度報告的非同步動作和作業，請使用[**system.runtime.interopservices.windowsruntime.asyncinfo**](/dotnet/api/system.runtime.interopservices.windowsruntime)類別來產生已啟動的工作，並將工作的取消和進度報告功能與適當 Windows 執行階段介面的取消和進度報告功能連結在一起。 如需同時支援取消和進度報告的範例，請參閱[建立 c # 或 Visual Basic Windows 執行階段元件，然後從 JavaScript 呼叫它的逐步](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)解說。

請注意，即使您的非同步方法不支援取消或進度報告，您仍然可以使用**system.runtime.interopservices.windowsruntime.asyncinfo**類別的方法。 如果您使用 Visual Basic lambda 函數或 c # 匿名方法，請不要提供 token 和[**iprogress<t> &lt; t &gt; **](https://docs.microsoft.com/dotnet/api/system.iprogress-1)介面的參數。 如果您使用 C# Lambda 函式，請提供語彙基元參數，但加以忽略。 先前的範例（使用 AsAsyncOperation &lt; TResult &gt; 方法）在使用 system.runtime.interopservices.windowsruntime.asyncinfo 時看起來像這樣。請改為[**執行 &lt; tresult &gt; (Func &lt; CancellationToken、 &lt; Task &gt; &gt; TResult**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)) 方法超載。

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

如果您建立選擇性支援取消或進度報告的非同步方法，請考慮新增不具有解除標記或**iprogress<t> &lt; t &gt; **介面參數的多載。

## <a name="throwing-exceptions"></a>擲回例外狀況

您可以擲回適用於 Windows app 的 .NET 所包含的任何例外狀況類型。 您無法在 Windows 執行階段元件中宣告自己的公用例外狀況類型，但可宣告和擲回非公用類型。

如果您的元件不會處理例外狀況，呼叫該元件的程式碼中將會引發對應的例外狀況。 例外狀況對呼叫端的顯示方式，取決於呼叫的語言支援 Windows 執行階段的方式。

-   在 JavaScript 中，例外狀況會顯示為物件，其中例外狀況訊息會由堆疊追蹤所取代。 當您在 Visual Studio 中偵錯 app 時，可以在偵錯工具的 [例外狀況] 對話方塊中看到標示為「WinRT 資訊」的原始訊息文字。 您無法從 JavaScript 程式碼存取原始訊息文字。

    > **秘訣**。雖然堆疊追蹤目前包含 Managed 例外狀況型別，但是不建議透過剖析追蹤的方式來辨識例外狀況型別， 而是改用 HRESULT 值 (本節稍後會加以說明)。

-   在 C++ 中，例外狀況會顯示為平台例外狀況。 如果 managed 例外狀況的 HResult 屬性可以對應至特定平臺例外狀況的 HRESULT，則會使用特定的例外狀況。否則，會擲回[**Platform：： COMException**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class)例外狀況。 C++ 程式碼無法使用 Managed 例外狀況的訊息文字。 如果是擲回特定平台例外狀況，則會顯示該例外狀況類型的預設訊息文字，否則不會顯示任何訊息文字。 請參閱[例外狀況 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx)。
-   在 C# 或 Visual Basic 中，例外狀況就是一般的 Managed 例外狀況。

當您從自己的元件擲回例外狀況時，為了讓 JavaScript 或 C++ 呼叫端更容易處理此例外狀況，可以擲回非公開的例外狀況類型，並將其 HResult 屬性值設定為該元件專屬的值。 您可以透過例外狀況物件的 number 屬性，將 HRESULT 提供給 JavaScript 呼叫端，並透過[**COMException：： HRESULT**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult)屬性取得 c + + 呼叫端。

> [!NOTE]
> 針對您的 HRESULT 使用負值值。 正值會被解譯為成功，如此在 JavaScript 或 C++ 呼叫端中，就不會擲回任何例外狀況。

## <a name="declaring-and-raising-events"></a>宣告和引發事件

當您宣告某個類型包含事件的資料時，請從 Object 衍生，而不要從 EventArgs 衍生，因為 EventArgs 不是 Windows 執行階段類型。 使用[**EventHandler &lt; TEventArgs &gt; **](https://docs.microsoft.com/dotnet/api/system.eventhandler-1)做為事件的類型，並使用您的事件引數類型做為泛型型別引數。 引發事件，就像在 .NET 應用程式中一樣。

從 JavaScript 或 C++ 使用您的 Windows 執行階段元件時，事件會依循這些語言所預期的 Windows 執行階段事件模式。 當您使用 c # 或 Visual Basic 中的元件時，事件會顯示為一般的 .NET 事件。 [建立 c # 或 Visual Basic Windows 執行階段元件，並從 JavaScript 呼叫它的逐步](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)解說中提供了一個範例。

如果您實作自訂事件存取子 (在 Visual Basic 中為使用 **Custom** 關鍵字宣告事件)，則您在實作時必須依循 Windows 執行階段事件模式。 請參閱[Windows 執行階段元件中的自訂事件和事件存取](custom-events-and-event-accessors-in-windows-runtime-components.md)子。 請注意，當您處理來自 c # 或 Visual Basic 程式碼的事件時，它仍會顯示為一般的 .NET 事件。

## <a name="next-steps"></a>後續步驟

在您建立要供自己使用的 Windows 執行階段元件之後，可能會發現它封裝的功能對其他開發人員也很有用。 有兩種方式可供您選擇來包裝元件並發佈給其他開發人員。 請參閱[發佈 Managed Windows 執行階段元件](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140))。

如需 Visual Basic 和 c # 語言功能以及 Windows 執行階段的 .NET 支援的詳細資訊，請參閱[Visual Basic 和 c # 語言參考](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)。

## <a name="related-topics"></a>相關主題
* [適用於 UWP app 的 .NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [建立 C# 或 Visual Basic Windows 執行階段元件，並從 JavaScript 呼叫該元件的逐步解說](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
