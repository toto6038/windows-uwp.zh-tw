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
ms.openlocfilehash: b4f5a2de5c3fa5564b4e4389cfc0806fd5d2844f
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8928107"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>在 C# 和 Visual Basic 中建立 Windows 執行階段元件
從.NET Framework 4.5 開始，您可以使用 managed 程式碼來建立您自己的 Windows 執行階段類型，並在 Windows 執行階段元件中包裝它們。 您可以使用您的元件以 c + +、 JavaScript、 Visual Basic 或 C# 撰寫的通用 Windows 平台 (UWP) 應用程式中。 本主題概述建立元件，規則，並討論.NET Framework 支援的 Windows 執行階段中的某些層面。 一般而言，該支援依設計應可讓 .NET Framework 程式設計人員清楚理解。 但是，當您建立要與 JavaScript 或 C++ 搭配使用的元件時，必須了解這些語言對於 Windows 執行階段的支援方式有何差異。

如果您正在建立只能在 Visual Basic 或 C# 撰寫的 UWP app 中的元件，以供使用，而且元件中不含 UWP 控制項，然後使用**類別庫**的範本，而不**Windows 執行階段元件**專案範本的 onsider在 Microsoft Visual Studio 中。 簡單類別庫的限制比較少。

## <a name="declaring-types-in-windows-runtime-components"></a>在 Windows 執行階段元件中宣告類型

在內部，您的元件中的 Windows 執行階段類型可以使用 UWP app 中允許的任何.NET Framework 功能。 如需詳細資訊，請參閱[適用於 UWP 應用程式的.NET](https://msdn.microsoft.com/library/windows/apps/mt185501)。

外部，您的類型成員可以公開只有 Windows 執行階段類型其參數和傳回值。 下列清單說明從 Windows 執行階段元件公開的.NET Framework 類型上的限制。

- 在您的元件中，所有公用類型和成員的欄位、參數及傳回值都必須是 Windows 執行階段類型。 這項限制包含您所提供的 Windows 執行階段本身的類型以及撰寫 Windows 執行階段類型。 其中也包括多種 .NET Framework 類型。 納入這些類型，是為了方便您在 managed 程式碼 Windows 執行階段使用提供的支援.NET Framework&mdash;您的程式碼看起來會使用熟悉的.NET Framework 類型，而非基礎的 Windows 執行階段類型。 例如，您可以使用.NET Framework 基本類型，例如**Int32**和**Double**，特定的基礎類型，例如**DateTimeOffset**和**Uri**，以及一些常用的泛型介面類型，例如**IEnumerable&lt;T&gt; ** (在 Visual Basic 中的為 IEnumerable (Of T)) 和**IDictionary&lt;TKey，TValue&gt;**。 請注意，這些泛型類型的類型引數必須是 Windows 執行階段類型。 這是本主題稍後的[Windows 執行階段類型傳遞至 managed 程式碼](#passing-windows-runtime-types-to-managed-code)和[managed 類型傳遞至 Windows 執行階段](#passing-managed-types-to-the-windows-runtime)，> 中討論。

- 公用類別與介面可包含方法、屬性與事件。 您可以為您的事件宣告委派，或使用**EventHandler&lt;T&gt;** 委派。 公用類別或介面不能：
    - 屬於泛型。
    - 實作的介面，不是 Windows 執行階段介面 （不過，您可以建立自己的 Windows 執行階段介面並加以執行）。
    - 衍生自不在 Windows 執行階段，例如**System.Exception**與**System.EventArgs**中的類型。

- 所有的公用類型都必須具有符合組件名稱的根命名空間，且組件名稱的開頭不可以是 "Windows"。

    > **祕訣**。 根據預設，Visual Studio 專案具有符合組件名稱的命名空間名稱。 在 Visual Basic 中，此預設命名空間的 Namespace 陳述式不會顯示在您的程式碼中。

- 公用結構不可以有公用欄位以外的任何成員，而且這些欄位必須是實值類型或字串。
- 公用類別必須是 **sealed** (在 Visual Basic 中為 **NotInheritable**)。 如果您的程式設計模型需要使用多型，則您可以建立公用介面，並在必須是多型的類別上實作該介面。

## <a name="debugging-your-component"></a>偵錯您的元件

如果您的 UWP app 和您的元件都使用 managed 程式碼所建置，然後您可以偵錯兩者同時。

當您使用 c + + 的 UWP 應用程式的一部分中測試您的元件時，您可以在同一時間來偵錯 managed 和原生程式碼。 預設值為僅限機器碼。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>同時對 C++ 機器碼與 Managed 程式碼進行偵錯
1.  為您的 Visual C++ 專案開啟捷徑功能表，然後選擇 **\[屬性\]**。
2.  在屬性頁中的 **\[組態屬性\]** 下方，選擇 **\[偵錯\]**。
3.  選擇 **\[偵錯工具類型\]**，然後在下拉式清單方塊中，將 **\[僅限機器碼\]** 變更為 **\[混合 (Managed 和機器碼)\]**。 選擇 **\[確定\]**。
4.  在機器碼與 Managed 程式碼中設定中斷點。

當您要測試您的元件，使用 JavaScript 的 UWP 應用程式的一部分時，預設方案會處於 JavaScript 偵錯模式。 在 Visual Studio 中，您無法同時偵錯 JavaScript 和 Managed 程式碼。

## <a name="to-debug-managed-code-instead-of-javascript"></a>偵錯 Managed 程式碼，而不偵錯 JavaScript
1.  為您的 JavaScript 專案開啟捷徑功能表，然後選擇 **\[屬性\]**。
2.  在屬性頁中的 **\[組態屬性\]** 下方，選擇 **\[偵錯\]**。
3.  選擇 **\[偵錯工具類型\]**，然後在下拉式清單方塊中，將 **\[僅限指令碼\]** 變更為 **\[僅限 Managed\]**。 選擇 **\[確定\]**。
4.  在 Managed 程式碼中設定中斷點，然後如常進行偵錯。

## <a name="passing-windows-runtime-types-to-managed-code"></a>將 Windows 執行階段類型傳遞至 Managed 程式碼
[在 Windows 執行階段元件中的宣告類型](#declaring-types-in-windows-runtime-components)] 區段中先前所述，特定的.NET Framework 類型可以出現在公用類別成員的簽章。 這是 .NET Framework 為了方便您在 Managed 程式碼中使用 Windows 執行階段而提供的支援。 其中包括基本類型以及一些類別和介面。 當從 JavaScript 中，使用您的元件，或從 c + + 程式碼，請務必了解您的.NET Framework 類型如何顯示給呼叫者。 如需 JavaScript 的範例，請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。 本節將討論常用的類型。

在.NET Framework 中，例如**Int32**結構的基本類型會有許多實用的屬性和方法，例如**TryParse**方法。 相對地，Windows 執行階段中的基本類型和結構就只有欄位而已。 當您將這些類型傳遞至 Managed 程式碼時，它們將顯示為 .NET Framework 類型，而您可以如常使用 .NET Framework 類型的屬性和方法。 下列清單將摘要說明在 IDE 中自動執行的替代：

-   適用於 Windows 執行階段基本**Int32**、 **Int64**、**單一**、 **Double**、**布林值**、 **String** （Unicode 字元的不可變集合）、 **Enum**、 **UInt32**、 **UInt64**，以及**Guid**，在 System 命名空間中使用相同名稱的類型。
-   對於**UInt8**，使用**System.Byte**。
-   對於**Char16**，使用**System.Char**。
-   對於**IInspectable**介面，使用**System.Object**。

如果 C# 或 Visual Basic 為提供了語言關鍵字其中任何類型，然後您可以使用該語言關鍵字改為。

除了基本類型以外，有些基本且常用的 Windows 執行階段類型在 Managed 程式碼中也會顯示為其 .NET Framework 對等類型。 例如，假設您的 JavaScript 程式碼使用**Windows.Foundation.Uri**類別，且您想要將它傳遞到 C# 或 Visual Basic 方法。 在 managed 程式碼中的對等類型是.NET Framework **System.Uri**類別，以及這就是要用於方法參數類型。 由於 Visual Studio 中的 IntelliSense 在您撰寫 Managed 程式碼時會隱藏 Windows 執行階段類型，並顯示對等的 .NET Framework 類型，因此您知道 Windows 執行階段類型何時會呈現為 .NET Framework 類型。 (這兩個類型通常會具備相同的名稱。 不過，請注意， **Windows.Foundation.DateTime**結構會出現在 managed 程式碼為**System.DateTimeOffset** ，而**不是 system.datetime**。)

對於某些常用的集合類型，會在 Windows 執行階段類型所實作的介面與相對應的 .NET Framework 類型所實作的介面之間進行對應。 如同先前提到的類型，您也可以使用 .NET Framework 類型來宣告參數類型。 這會隱藏不同類型之間的某些差異，以方便撰寫 .NET Framework 程式碼。

下表列出其中最常見的泛型介面類型，以及其他的通用類別與介面對應。 Windows 執行階段類型的.NET Framework 對應的完整清單，請參閱[Windows 執行階段類型的.NET Framework 對應](net-framework-mappings-of-windows-runtime-types.md)。

| Windows 執行階段                                  | .NET Framework                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | Ilist<&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K，V&gt;                                 | IDictionary&lt;TKey，TValue&gt;                   |
| IMapView&lt;K，V&gt;                             | IReadOnlyDictionary&lt;TKey，TValue&gt;           |
| IKeyValuePair&lt;K，V&gt;                        | KeyValuePair&lt;TKey，TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

當某個類型實作多個介面時，您可以將它實作的任何介面當做成員的參數類型或傳回類型。 例如，您可以傳遞或傳回**字典&lt;int，string&gt; ** （在 Visual Basic 中**Dictionary （Of Integer，String）** ） 做為**IDictionary&lt;int，string&gt;**， **IReadOnlyDictionary&lt;int，string&gt; **，或**IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;**。

> [!IMPORTANT]
> JavaScript 會使用 managed 的類型實作之介面清單中第一個出現的介面。 例如，如果您回傳**字典&lt;int，string&gt; ** JavaScript 程式碼，它會顯示為**IDictionary&lt;int，string&gt;** 無論您將哪個介面指定為傳回類型。 這表示，如果第一個介面不包含出現在後續介面上的成員，該成員即不會對 JavaScript 顯示。

在 Windows 執行階段中， **IMap&lt;K，V&gt;** 並**IMapView&lt;K，V&gt;** 透過 ikeyvaluepair 反覆執行。 當您將其傳遞至 managed 程式碼時，它們會顯示為**IDictionary&lt;TKey，TValue&gt;** 並**IReadOnlyDictionary&lt;TKey，TValue&gt;**，以便您使用**System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;** 加以列舉。

介面顯示於 Managed 程式碼中的方式會影響實作這些介面之類型的顯示方式。 例如， **PropertySet**類別會實作**IMap&lt;K，V&gt;**，而這會顯示在 managed 程式碼做為**IDictionary&lt;TKey，TValue&gt;**。 如同它實作**PropertySet**會出現**IDictionary&lt;TKey，TValue&gt;** 而不是**IMap&lt;K，V&gt;**，因此在 managed 程式碼，其看似具有**Add**方法行為類似**Add**方法在.NET Framework 字典。 它看起來並沒有**Insert**方法。 您可以查看本主題中的範例[逐步解說： 在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>將 Managed 類型傳遞至 Windows 執行階段

如上一節所討論，某些 Windows 執行階段類型在您元件成員的簽章中 (如果在 IDE 中使用這些類型，則是在 Windows 執行階段成員的簽章中) 會顯示為 .NET Framework 類型。 當您將 .NET Framework 類型傳遞給這些成員，或使用這些類型做為元件成員的傳回值時，它們在另一端的程式碼上便會顯示為對應的 Windows 執行階段類型。 若想透過範例了解這在從 JavaScript 呼叫您的元件時將有何效用，請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，並從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)中的＜從您的元件傳回 Managed 類型＞一節。

## <a name="overloaded-methods"></a>多載方法

在 Windows 執行階段中，方法可以是多載的。 不過，如果您宣告多個具有相同數量的參數多載，您必須[**Windows.Foundation.Metadata.DefaultOverloadAttribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)將屬性套用至其中一個多載。 這就是您唯一可從 JavaScript 呼叫的多載。 例如，在下列程式碼中，使用 **int** (在 Visual Basic 中為 **Integer**) 的多載是預設多載。

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

> [重要]JavaScript 可讓您將任何值傳遞至**OverloadExample**，並會強制類型所需的參數值。 您可以呼叫**OverloadExample**使用"forty-two"、"42"或 42.3 來，但這些值全都會傳遞至預設多載。 在上一個範例中的預設多載分別會傳回 0、 42 和 42。

您無法將**DefaultOverloadAttribut**e 屬性套用至建構函式。 一個類別中的所有建構函式必須要有不同數量的參數。

## <a name="implementing-istringable"></a>實作 IStringable

從 Windows 8.1 開始，Windows 執行階段會包含**IStringable**介面，方法**IStringable.ToString**，提供相當於**Object.ToString**的基本格式支援。 如果您選擇要在 Windows 執行階段元件匯出的公用 managed 類型中實作**IStringable** ，就會套用下列限制：

-   您可以只在 「 類別實作 」 關聯性，例如 C# 中，下列程式碼中定義**IStringable**介面：

    ```cs
    public class NewClass : IStringable
    ```

    或是下列 Visual Basic 程式碼：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   您無法針對介面實作**IStringable** 。
-   您無法宣告為類型**IStringable**參數。
-   **IStringable**不能是方法、 屬性或欄位的傳回類型。
-   您無法使用的方法定義，例如下列隱藏基底類別從您的**IStringable**實作：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    相反地， **IStringable.ToString**實作必須一律覆寫基底類別實作。 您可以隱藏**ToString**實作只能由的強型別的類別執行個體叫用。

> [!NOTE]
> 在各種情況下，在原生程式碼的 managed 類型實作**IStringable**或隱藏其**ToString**實作的呼叫可產生非預期的行為。

## <a name="asynchronous-operations"></a>非同步作業

若要在您的元件中實作非同步方法，將"Async"新增至方法名稱的結尾，並傳回其中一個 Windows 執行階段介面表示非同步動作或作業： **IAsyncAction**， **IAsyncActionWithProgress&lt;TProgress&gt;**， **IAsyncOperation&lt;TResult&gt;**，或**IAsyncOperationWithProgress&lt;TResult，TProgress&gt;**。

您可以使用.NET Framework 工作 ( [**Task**](/dotnet/api/system.threading.tasks.task)類別和泛型[**工作&lt;TResult&gt;**](/dotnet/api/system.threading.tasks.task-1)類別) 來實作您的非同步方法。 您必須傳回代表進行中作業，例如，從 C# 或 Visual Basic 撰寫的非同步方法傳回的工作或從[**Task.Run**](/dotnet/api/system.threading.tasks.task.run)方法傳回的工作的工作。 如果您使用建構函式來建立此工作，就必須先呼叫其 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 方法，再加以傳回。

方法，使用`await`(`Await`在 Visual Basic 中) 需要`async`關鍵字 (`Async`在 Visual Basic 中)。 如果您公開此類方法從 Windows 執行階段元件，套用`async`至您傳遞給**Run**方法的委派的關鍵字。

對於不支援取消或進度報告的非同步動作與作業，您可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) 或 [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) 擴充方法，將工作包覆在適當的介面中。 例如，下列程式碼使用實作非同步方法**Task.Run&lt;TResult&gt;** 方法來啟動工作。 **AsAsyncOperation&lt;TResult&gt;** 延伸方法會傳回 Windows 執行階段非同步作業的工作。

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

下列 JavaScript 程式碼示範如何使用[**WinJS.Promise**](https://msdn.microsoft.com/library/windows/apps/br211867.aspx)物件無法呼叫的方法。 完成非同步呼叫時，便會執行傳遞至 then 方法的函式。 StringList 參數包含**DownloadAsStringAsync**方法中，傳回的字串清單並函式會執行任何處理必要。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

非同步動作與作業支援取消或進度報告，請使用[**AsyncInfo**](/dotnet/api/system.runtime.interopservices.windowsruntime)類別來產生啟動的工作，並取消與進度報告功能，該工作的取消和進度的連結適當的 Windows 執行階段介面的報告功能。 如需支援取消與進度報告的範例，請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

請注意，您就可以使用**AsyncInfo**類別的方法，即使您的非同步方法不支援取消或進度報告。 如果您使用 Visual Basic lambda 函式或 C# 匿名方法，不要提供語彙基元參數並[**IProgress&lt;T&gt;**](https://msdn.microsoft.com/library/hh138298.aspx)介面。 如果您使用 C# Lambda 函式，請提供語彙基元參數，但加以忽略。 先前的範例中，使用 AsAsyncOperation&lt;TResult&gt;方法中，看起來像這樣，當您使用[**AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken，Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/hh779740.aspx)) 方法多載。

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

如果您建立選擇性支援取消或進度報告的非同步方法時，請考慮新增不具備取消語彙基元參數多載或有**IProgress&lt;T&gt;** 介面。

## <a name="throwing-exceptions"></a>擲回例外狀況

您可以擲回適用於 Windows app 的 .NET 所包含的任何例外狀況類型。 您無法在 Windows 執行階段元件中宣告自己的公用例外狀況類型，但可宣告和擲回非公用類型。

如果您的元件不會處理例外狀況，呼叫該元件的程式碼中將會引發對應的例外狀況。 例外狀況對呼叫端的顯示方式，取決於呼叫的語言支援 Windows 執行階段的方式。

-   在 JavaScript 中，例外狀況會顯示為物件，其中例外狀況訊息會由堆疊追蹤所取代。 當您在 Visual Studio 中偵錯 app 時，可以在偵錯工具的 [例外狀況] 對話方塊中看到標示為「WinRT 資訊」的原始訊息文字。 您無法從 JavaScript 程式碼存取原始訊息文字。

    > **祕訣**。目前，堆疊追蹤包含 managed 例外狀況類型，但不建議透過剖析追蹤的方式來辨識例外狀況類型。 而是改用 HRESULT 值 (本節稍後會加以說明)。

-   在 C++ 中，例外狀況會顯示為平台例外狀況。 如果 managed 例外狀況的 HResult 屬性可以對應至特定平台例外狀況的 HRESULT，會使用特定的例外狀況;否則，會擲回[**platform:: comexception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx)例外狀況。 C++ 程式碼無法使用 Managed 例外狀況的訊息文字。 如果是擲回特定平台例外狀況，則會顯示該例外狀況類型的預設訊息文字，否則不會顯示任何訊息文字。 請參閱[例外狀況 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx)。
-   在 C# 或 Visual Basic 中，例外狀況就是一般的 Managed 例外狀況。

當您從自己的元件擲回例外狀況時，為了讓 JavaScript 或 C++ 呼叫端更容易處理此例外狀況，可以擲回非公開的例外狀況類型，並將其 HResult 屬性值設定為該元件專屬的值。 使用 JavaScript 呼叫端可以透過例外狀況物件的 number 屬性，而 c + + 呼叫端可透過[**Comexception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx)屬性的 HRESULT。

> [!NOTE]
> 使用負值針對您的 HRESULT。 正值會被解譯為成功，如此在 JavaScript 或 C++ 呼叫端中，就不會擲回任何例外狀況。

## <a name="declaring-and-raising-events"></a>宣告和引發事件

當您宣告某個類型包含事件的資料時，請從 Object 衍生，而不要從 EventArgs 衍生，因為 EventArgs 不是 Windows 執行階段類型。 使用[**EventHandler&lt;TEventArgs&gt;**](https://msdn.microsoft.com/library/db0etb8x.aspx)做為事件，並使用您的事件引數類型做為泛型類型引數的類型。 引發事件的方式與 .NET Framework 應用程式中的一樣。

從 JavaScript 或 C++ 使用您的 Windows 執行階段元件時，事件會依循這些語言所預期的 Windows 執行階段事件模式。 當您從 C# 或 Visual Basic 使用元件時，事件會顯示為一般的 .NET Framework 事件。 請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該元件]()中的範例。

如果您實作自訂事件存取子 (在 Visual Basic 中為使用 **Custom** 關鍵字宣告事件)，則您在實作時必須依循 Windows 執行階段事件模式。 請參閱 [Windows 執行階段元件中的自訂事件和事件存取子](custom-events-and-event-accessors-in-windows-runtime-components.md)。 請注意，當您處理來自 C# 或 Visual Basic 程式碼的事件時，該事件仍會顯示為一般的 .NET Framework 事件。

## <a name="next-steps"></a>後續步驟

在您建立要供自己使用的 Windows 執行階段元件之後，可能會發現它封裝的功能對其他開發人員也很有用。 有兩種方式可供您選擇來包裝元件並發佈給其他開發人員。 請參閱[發佈 Managed Windows 執行階段元件](https://msdn.microsoft.com/library/jj614475.aspx)。

如需 Visual Basic 和 C# 語言功能以及 Windows 執行階段之 .NET Framework 支援的詳細資訊，請參閱 [Visual Basic 和 C# 語言參考](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx)。

## <a name="related-topics"></a>相關主題
* [適用於 UWP App 的 .NET](https://msdn.microsoft.com/library/windows/apps/mt185501)
* [逐步解說：建立簡單的 Windows 執行階段元件，並從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
