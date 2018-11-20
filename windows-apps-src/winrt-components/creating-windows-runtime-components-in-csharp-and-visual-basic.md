---
author: msatranjr
title: 在 C# 和 Visual Basic 中建立 Windows 執行階段元件
description: 從 .NET Framework 4.5 開始，您可以使用 Managed 程式碼自行建立封裝在 Windows 執行階段元件中的 Windows 執行階段類型。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4e3b9ed2d256fb9ea8d38690a703baf7fbd3e7f0
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7291539"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>在 C# 和 Visual Basic 中建立 Windows 執行階段元件
從 .NET Framework 4.5 開始，您可以使用 Managed 程式碼自行建立封裝在 Windows 執行階段元件中的 Windows 執行階段類型。 您可以在通用 Windows 平台 (UWP) app 中，將元件與 C++、JavaScript、Visual Basic 或 C# 搭配使用。 本主題概述建立元件，規則，並討論.NET Framework 支援的 Windows 執行階段中的某些層面。 一般而言，該支援依設計應可讓 .NET Framework 程式設計人員清楚理解。 但是，當您建立要與 JavaScript 或 C++ 搭配使用的元件時，必須了解這些語言對於 Windows 執行階段的支援方式有何差異。

如果您要建立僅在 UWP app 中與 Visual Basic 或 C# 搭配使用的元件，而且元件中不含 UWP 控制項，請考慮使用**類別庫**範本，而不使用 **Windows 執行階段元件**範本。 簡單類別庫的限制比較少。

本主題包含下列各節：

## <a name="declaring-types-in-windows-runtime-components"></a>在 Windows 執行階段元件中宣告類型
您元件中的 Windows 執行階段類型可以在內部使用通用 Windows app 允許的任何 .NET Framework 功能 (如需詳細資訊，請參閱[適用於 UWP App 的 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx) 概觀)。您的類型成員在外部只能公開其參數和傳回值的 Windows 執行階段類型。 下列清單將說明從 Windows 執行階段元件公開的 .NET Framework 類型有何限制。

-   在您的元件中，所有公用類型和成員的欄位、參數及傳回值都必須是 Windows 執行階段類型。

    此限制也適用於您建立的 Windows 執行階段類型以及 Windows 執行階段本身提供的類型。 其中也包括多種 .NET Framework 類型。 納入這些類型，是 .NET Framework 為了方便您在 Managed 程式碼中使用 Windows 執行階段而提供的支援：您的程式碼能夠使用熟悉的 .NET Framework 類型，而非基礎的 Windows 執行階段類型。 您可以使用 .NET Framework 基本類型 (例如 Int32 和 Double)、特定的基礎類型 (例如 DateTimeOffset 和 Uri)，以及一些常用的泛型介面類型，例如 IEnumerable&lt;T&gt; (在 Visual Basic 中為 IEnumerable(Of T)) 和 IDictionary&lt;TKey,TValue&gt;。 （請注意，這些泛型類型的類型引數必須是 Windows 執行階段類型）。這 Windows 執行階段類型傳遞至 managed 程式碼的各節中討論和 managed 傳遞到 Windows 執行階段中，本主題中稍後的類型。

-   公用類別與介面可包含方法、屬性與事件。 您可以為事件宣告委派，或使用 EventHandler&lt;T&gt; 委派。 公用類別或介面不能：

    -   屬於泛型。
    -   實作不是 Windows 執行階段介面的介面 (不過，您可以建立自己的 Windows 執行階段介面並加以實作)。
    -   從不在 Windows 執行階段中的類型 (例如 System.Exception 與 System.EventArgs) 衍生。
-   所有的公用類型都必須具有符合組件名稱的根命名空間，且組件名稱的開頭不可以是 "Windows"。

    > **提示：** 根據預設，Visual Studio 專案具有符合組件名稱的命名空間名稱。 在 Visual Basic 中，此預設命名空間的 Namespace 陳述式不會顯示在您的程式碼中。

-   公用結構不可以有公用欄位以外的任何成員，而且這些欄位必須是實值類型或字串。
-   公用類別必須是 **sealed** (在 Visual Basic 中為 **NotInheritable**)。 如果您的程式設計模型需要使用多型，您可以建立公用介面，然後在必須是多型的類別上實作該介面。

## <a name="debugging-your-component"></a>偵錯您的元件
如果您的通用 Windows app 與元件都是使用 Managed 程式碼所建置，則可同時針對這兩者進行偵錯。

當您使用 C++ 在通用 Windows app 中測試您的元件時，可以同時對 Managed 程式碼和機器碼進行偵錯。 預設值為僅限機器碼。

## **<a name="to-debug-both-native-c-code-and-managed-code"></a>同時對 C++ 機器碼與 Managed 程式碼進行偵錯**
1.  為您的 Visual C++ 專案開啟捷徑功能表，然後選擇 **\[屬性\]**。
2.  在屬性頁中的 **\[組態屬性\]** 下方，選擇 **\[偵錯\]**。
3.  選擇 **\[偵錯工具類型\]**，然後在下拉式清單方塊中，將 **\[僅限機器碼\]** 變更為 **\[混合 (Managed 和機器碼)\]**。 選擇 **\[確定\]**。
4.  在機器碼與 Managed 程式碼中設定中斷點。

當您使用 JavaScript 在通用 Windows app 中測試元件時，方案依預設會處於 JavaScript 偵錯模式。 在 Visual Studio 中，您無法同時偵錯 JavaScript 和 Managed 程式碼。

## **<a name="to-debug-managed-code-instead-of-javascript"></a>偵錯 Managed 程式碼，而不偵錯 JavaScript**
1.  為您的 JavaScript 專案開啟捷徑功能表，然後選擇 **\[屬性\]**。
2.  在屬性頁中的 **\[組態屬性\]** 下方，選擇 **\[偵錯\]**。
3.  選擇 **\[偵錯工具類型\]**，然後在下拉式清單方塊中，將 **\[僅限指令碼\]** 變更為 **\[僅限 Managed\]**。 選擇 **\[確定\]**。
4.  在 Managed 程式碼中設定中斷點，然後如常進行偵錯。

## <a name="passing-windows-runtime-types-to-managed-code"></a>將 Windows 執行階段類型傳遞至 Managed 程式碼
如先前在＜在 Windows 執行階段元件中宣告類型＞一節中所述，特定的 .NET Framework 類型可以出現在公用類別成員的簽章中。 這是 .NET Framework 為了方便您在 Managed 程式碼中使用 Windows 執行階段而提供的支援。 其中包括基本類型以及一些類別和介面。 從 JavaScript 或 C++ 程式碼使用您的元件時，請務必了解您的 .NET Framework 類型如何對呼叫端顯示的方式。 如需 JavaScript 的範例，請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。 本節將討論常用的類型。

在 .NET Framework 中，基本類型 (例如 Int32 結構) 具有許多有用的屬性和方法，例如 TryParse 方法。 相對地，Windows 執行階段中的基本類型和結構就只有欄位而已。 當您將這些類型傳遞至 Managed 程式碼時，它們將顯示為 .NET Framework 類型，而您可以如常使用 .NET Framework 類型的屬性和方法。 下列清單將摘要說明在 IDE 中自動執行的替代：

-   對於 Windows 執行階段基本 Int32、Int64、Single、Double、Boolean、String (Unicode 字元的不可變集合)、Enum、UInt32、UInt64 與 Guid，請在 System 命名空間中使用相同名稱的類型。
-   對於 UInt8，使用 System.Byte。
-   對於 Char16，使用 System.Char。
-   對於 IInspectable 介面，使用 System.Object。

如果 C# 或 Visual Basic 為其中任何類型提供了語言關鍵字，您就能改用該語言關鍵字。

除了基本類型以外，有些基本且常用的 Windows 執行階段類型在 Managed 程式碼中也會顯示為其 .NET Framework 對等類型。 例如，假設您的 JavaScript 程式碼使用 Windows.Foundation.Uri 類別，而您要將其傳遞至 C# 或 Visual Basic 方法。 在 Managed 程式碼中的對等類型是 .NET Framework System.Uri 類別，而這就是要用於方法參數的類型。 由於 Visual Studio 中的 IntelliSense 在您撰寫 Managed 程式碼時會隱藏 Windows 執行階段類型，並顯示對等的 .NET Framework 類型，因此您知道 Windows 執行階段類型何時會呈現為 .NET Framework 類型。 (這兩個類型通常會具備相同的名稱。 但請注意，Windows.Foundation.DateTime 結構在 Managed 程式碼中會顯示為 System.DateTimeOffset，而不是 System.DateTime。)

對於某些常用的集合類型，會在 Windows 執行階段類型所實作的介面與相對應的 .NET Framework 類型所實作的介面之間進行對應。 如同先前提到的類型，您也可以使用 .NET Framework 類型來宣告參數類型。 這會隱藏不同類型之間的某些差異，以方便撰寫 .NET Framework 程式碼。 下表列出其中最常見的泛型介面類型，以及其他的通用類別與介面對應。 如需 .NET Framework 對應的 Windows 執行階段類型完整清單，請參閱＜Windows 執行階段類型的 .NET Framework 對應＞。

| Windows 執行階段                                  | .NET Framework                                    |
|--------------------------------------------------|---------------------------------------------------|
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

當某個類型實作多個介面時，您可以將它實作的任何介面當做成員的參數類型或傳回類型。 例如，您可以傳遞或傳回字典&lt;int，string&gt; (Dictionary （Of Integer，String） 在 Visual Basic 中的) 為 IDictionary&lt;int，string&gt;、 IReadOnlyDictionary&lt;int，string&gt;，或 IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;&gt;。

**重要**JavaScript 會使用 managed 的類型實作之介面清單中第一個出現的介面。 例如，若您將 Dictionary&lt;int, string&gt; 傳回 JavaScript 程式碼，則無論您將哪個介面指定為傳回類型，其皆會顯示為 IDictionary&lt;int, string&gt;。 這表示，如果第一個介面不包含出現在後續介面上的成員，該成員即不會對 JavaScript 顯示。

在 Windows 執行階段中，IMap&lt;K, V&gt; 與 IMapView&lt;K, V&gt; 可透過 IKeyValuePair 反覆執行。 當您將其傳遞至 Managed 程式碼時，它們會顯示為 IDictionary&lt;TKey, TValue&gt; 與 IReadOnlyDictionary&lt;TKey, TValue&gt;，以便您使用 System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt; 加以列舉。

介面顯示於 Managed 程式碼中的方式會影響實作這些介面之類型的顯示方式。 例如，PropertySet 類別會實作 IMap&lt;K, V&gt;，而這在 Managed 程式碼中會顯示為 IDictionary&lt;TKey, TValue&gt;。 PropertySet 會以實作了 IDictionary&lt;TKey, TValue&gt; (而不是 IMap&lt;K, V&gt;) 的形態出現，因此在 Managed 程式碼中，其看似具有 Add 方法 (此方法的行為類似於 .NET Framework 字典上的 Add 方法)。 它看起來並沒有 Insert 方法。 您可以查看本主題中的範例[逐步解說： 在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>將 Managed 類型傳遞至 Windows 執行階段
如上一節所討論，某些 Windows 執行階段類型在您元件成員的簽章中 (如果在 IDE 中使用這些類型，則是在 Windows 執行階段成員的簽章中) 會顯示為 .NET Framework 類型。 當您將 .NET Framework 類型傳遞給這些成員，或使用這些類型做為元件成員的傳回值時，它們在另一端的程式碼上便會顯示為對應的 Windows 執行階段類型。 若想透過範例了解這在從 JavaScript 呼叫您的元件時將有何效用，請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，並從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)中的＜從您的元件傳回 Managed 類型＞一節。

## <a name="overloaded-methods"></a>多載方法
在 Windows 執行階段中，方法可以是多載的。 不過，如果您宣告多個具有相同參數數量的多載，則只能將 [Windows.Foundation.Metadata.DefaultOverloadAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) 屬性套用至其中一個多載。 這就是您唯一可從 JavaScript 呼叫的多載。 例如，在下列程式碼中，使用 **int** (在 Visual Basic 中為 **Integer**) 的多載是預設多載。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public string OverloadExample(string s)
> {
>     return s;
> }
> [Windows.Foundation.Metadata.DefaultOverload()]
> public int OverloadExample(int x)
> {
>     return x;
> }
> ```
> ```vb
> Public Function OverloadExample(ByVal s As String) As String
>     Return s
> End Function
> <Windows.Foundation.Metadata.DefaultOverload> _
> Public Function OverloadExample(ByVal x As Integer) As Integer
>     Return x
> End Function
> ```

 **注意：** JavaScript 可讓您將任何值傳遞至 OverloadExample，並會強制類型所需的參數值。 您可以使用 "forty-two"、"42" 或 42.3 來呼叫 OverloadExample，但這些值全都會傳遞至預設多載。 前述範例中的預設多載分別會傳回 0、42 和 42。

您無法將 DefaultOverloadAttribute 屬性套用至建構函式。 一個類別中的所有建構函式必須要有不同數量的參數。

## <a name="implementing-istringable"></a>實作 IStringable
從 Windows 8.1 開始，Windows 執行階段會包含 IStringable 介面，此介面只有一個 IStringable.ToString 方法，可提供相當於 Object.ToString 的基本格式支援。 如果您選擇要在從 Windows 執行階段元件匯出的公用 Managed 類型中實作 IStringable，將會有下列限制：

-   您只能在「類別實作」關聯性中定義 IStringable 介面，例如，下列在 C# 中的程式碼：

    ```cs
    public class NewClass : IStringable
    ```

    或是下列 Visual Basic 程式碼：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   您無法針對介面實作 IStringable。
-   您無法將參數宣告為類型 IStringable。
-   IStringable 不能是方法的傳回類型、屬性或欄位。
-   您無法使用以下所示的方法定義，從基底類別隱藏您的 IStringable 實作：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    相反地，IStringable.ToString 實作必須一律覆寫基底類別實作。 您只能藉由針對強類型的類別執行個體叫用 ToString 實作來隱藏該實作。

請注意，在各種情況下，從機器碼傳送至實作 IStringable 或隱藏其 ToString 實作的 Managed 類型可能會導致非預期的行為。

## <a name="asynchronous-operations"></a>非同步作業
若要在元件中實作非同步方法，請在方法名稱結尾處加上 "Async"，並傳回代表非同步動作或作業的 Windows 執行階段介面之一：IAsyncAction、IAsyncActionWithProgress&lt;TProgress&gt;、IAsyncOperation&lt;TResult&gt; 或 IAsyncOperationWithProgress&lt;TResult, TProgress&gt;。

您可以使用 .NET Framework 工作 ([Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) 類別和泛型 [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) 類別) 來實作您的非同步方法。 您必須傳回代表進行中作業的工作，例如，從使用 C# 或 Visual Basic 撰寫的非同步方法傳回的工作，或是從 [Task.Run](https://msdn.microsoft.com/library/system.threading.tasks.task.run.aspx) 方法傳回的工作。 如果您使用建構函式來建立此工作，就必須先呼叫其 [Task.Start](https://msdn.microsoft.com/library/system.threading.tasks.task.start.aspx) 方法，再加以傳回。

使用 await (在 Visual Basic 中為 Await) 的方法，必須要有 **async** 關鍵字 (在 Visual Basic 中為 **Async**)。 如果您從 Windows 執行階段元件公開此類方法，請將 **async** 關鍵字套用至您傳遞給 Run 方法的委派。

對於不支援取消或進度報告的非同步動作與作業，您可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) 或 [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) 擴充方法，將工作包覆在適當的介面中。 例如，下列程式碼會使用 Task.Run&lt;TResult&gt; 方法來啟動工作，以實作非同步方法。 AsAsyncOperation&lt;TResult&gt; 擴充方法會以 Windows 執行階段非同步作業的形式傳回工作。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return Task.Run<IList<string>>(async () =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     }).AsAsyncOperation();
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>      As IAsyncOperation(Of IList(Of String))
>
>     Return Task.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function).AsAsyncOperation()
> End Function
> ```

下列 JavaScript 程式碼示範如何使用 [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) 物件來呼叫方法。 完成非同步呼叫時，便會執行傳遞至 then 方法的函式。 StringList 參數包含 DownloadAsStringAsync 方法傳回的字串清單，而該函式會執行任何必要的處理。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

對於支援取消或進度報告的非同步動作與作業，請使用 [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) 類別來產生啟動的工作，並將該工作的取消和進度報告功能與適當的 Windows 執行階段介面的取消和進度報告功能相連結。 如需支援取消與進度報告的範例，請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

請注意，即使您的非同步方法不支援取消或進度報告，您仍可使用 AsyncInfo 類別的方法。 如果您使用 Visual Basic Lambda 函式或 C# 匿名方法，請不要提供語彙基元和 [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx) 介面的參數。 如果您使用 C# Lambda 函式，請提供語彙基元參數，但加以忽略。 先前的範例中，使用 AsAsyncOperation&lt;TResult&gt;方法中，看起來像這樣，當您使用[AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken，Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)) 方法多載：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return AsyncInfo.Run<IList<string>>(async (token) =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     });
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>     As IAsyncOperation(Of IList(Of String))
>
>     Return AsyncInfo.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function)
> End Function
> ```

如果您建立選擇性支援取消或進度報告的非同步方法，請考慮新增不具備取消語彙基元或 IProgress&lt;T&gt; 介面參數的多載。

## <a name="throwing-exceptions"></a>擲回例外狀況
您可以擲回適用於 Windows app 的 .NET 所包含的任何例外狀況類型。 您無法在 Windows 執行階段元件中宣告自己的公用例外狀況類型，但可宣告和擲回非公用類型。

如果您的元件不會處理例外狀況，呼叫該元件的程式碼中將會引發對應的例外狀況。 例外狀況對呼叫端的顯示方式，取決於呼叫的語言支援 Windows 執行階段的方式。

-   在 JavaScript 中，例外狀況會顯示為物件，其中例外狀況訊息會由堆疊追蹤所取代。 當您在 Visual Studio 中偵錯 app 時，可以在偵錯工具的 [例外狀況] 對話方塊中看到標示為「WinRT 資訊」的原始訊息文字。 您無法從 JavaScript 程式碼存取原始訊息文字。

    > **提示：** 目前的堆疊追蹤包含 managed 例外狀況類型，但不建議透過剖析追蹤的方式來辨識例外狀況類型。 而是改用 HRESULT 值 (本節稍後會加以說明)。

-   在 C++ 中，例外狀況會顯示為平台例外狀況。 如果 Managed 例外狀況的 HResult 屬性可以對應至某個特定平台例外狀況的 HRESULT，就會使用該特定例外狀況，否則會擲回 [Platform::COMException](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx) 例外狀況。 C++ 程式碼無法使用 Managed 例外狀況的訊息文字。 如果是擲回特定平台例外狀況，則會顯示該例外狀況類型的預設訊息文字，否則不會顯示任何訊息文字。 請參閱[例外狀況 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx)。
-   在 C# 或 Visual Basic 中，例外狀況就是一般的 Managed 例外狀況。

當您從自己的元件擲回例外狀況時，為了讓 JavaScript 或 C++ 呼叫端更容易處理此例外狀況，可以擲回非公開的例外狀況類型，並將其 HResult 屬性值設定為該元件專屬的值。 JavaScript 呼叫端可以透過例外狀況物件的 number 屬性來存取 HRESULT，而 C++ 呼叫端可透過 [COMException::HResult](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx) 屬性來存取。

> **注意：** 使用負值針對您的 HRESULT。 正值會被解譯為成功，如此在 JavaScript 或 C++ 呼叫端中，就不會擲回任何例外狀況。

## <a name="declaring-and-raising-events"></a>宣告和引發事件
當您宣告某個類型包含事件的資料時，請從 Object 衍生，而不要從 EventArgs 衍生，因為 EventArgs 不是 Windows 執行階段類型。 使用 [EventHandler&lt;TEventArgs&gt;](https://msdn.microsoft.com/library/db0etb8x.aspx) 做為事件的類型，並以您的事件引數類型做為泛型類型引數。 引發事件的方式與 .NET Framework 應用程式中的一樣。

從 JavaScript 或 C++ 使用您的 Windows 執行階段元件時，事件會依循這些語言所預期的 Windows 執行階段事件模式。 當您從 C# 或 Visual Basic 使用元件時，事件會顯示為一般的 .NET Framework 事件。 請參閱[逐步解說：在 C# 或 Visual Basic 中建立簡單的元件，然後從 JavaScript 呼叫該元件]()中的範例。

如果您實作自訂事件存取子 (在 Visual Basic 中為使用 **Custom** 關鍵字宣告事件)，則您在實作時必須依循 Windows 執行階段事件模式。 請參閱 [Windows 執行階段元件中的自訂事件和事件存取子](custom-events-and-event-accessors-in-windows-runtime-components.md)。 請注意，當您處理來自 C# 或 Visual Basic 程式碼的事件時，該事件仍會顯示為一般的 .NET Framework 事件。

## <a name="next-steps"></a>後續步驟
在您建立要供自己使用的 Windows 執行階段元件之後，可能會發現它封裝的功能對其他開發人員也很有用。 有兩種方式可供您選擇來包裝元件並發佈給其他開發人員。 請參閱[發佈 Managed Windows 執行階段元件](https://msdn.microsoft.com/library/jj614475.aspx)。

如需 Visual Basic 和 C# 語言功能以及 Windows 執行階段之 .NET Framework 支援的詳細資訊，請參閱 [Visual Basic 和 C# 語言參考](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx)。

## <a name="related-topics"></a>相關主題
* [.NET for UWP 應用程式的概觀](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [適用於 UWP App 的 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [逐步解說：建立簡單的 Windows 執行階段元件，並從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
