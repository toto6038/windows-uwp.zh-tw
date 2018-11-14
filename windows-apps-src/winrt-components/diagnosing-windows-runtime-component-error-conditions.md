---
author: msatranjr
title: 診斷 Windows 執行階段元件錯誤狀況
description: 本文提供關於以 Managed 程式碼撰寫的 Windows 執行階段元件有何限制的其他資訊。
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 833dd0a6447e9d0bb49c21a18d17bd7b0dc3455d
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6648429"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>診斷 Windows 執行階段元件錯誤狀況




本文提供關於以 Managed 程式碼撰寫的 Windows 執行階段元件有何限制的其他資訊。 其中詳述 [Winmdexp.exe (Windows 執行階段中繼資料匯出工具)](https://msdn.microsoft.com/library/hh925576.aspx) 中錯誤訊息所提供的資訊，並補充[在 C# 和 Visual Basic 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)中提供的限制相關資訊。

本文並未涵蓋所有錯誤。 本文討論的錯誤會依照一般類別分門別類，而每個類別都有一個相關錯誤訊息的表格。 請搜尋訊息文字 (省略預留位置的特定值) 或訊息編號。 如果您在此處找不到所需的資訊，請使用本文結尾處的意見反應按鈕來協助我們改善本文。 請納入錯誤訊息。 或者，您也可以將 Bug 歸檔在 Microsoft Connect 網站上。

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>實作非同步介面提供不正確類型的錯誤訊息


Managed Windows 執行階段元件無法實作表示非同步動作或作業 ([IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx)、[IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx)、[IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br206598.aspx) 或 [IAsyncOperationWithProgress&lt;TResult、TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)) 的通用 Windows 平台 (UWP) 介面。 相反地，.NET Framework 會提供 [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) 類別，用來在 Windows 執行階段元件中產生非同步作業。 當您嘗試以不正確的方式實作非同步介面時，Winmdexp.exe 顯示的錯誤訊息會根據這個類別的舊名稱 AsyncInfoFactory 指出這個類別， 因為 .NET Framework 不再包含 AsyncInfoFactory 類別。

| 錯誤代碼 | 訊息文字|       
|--------------|-------------|
| WME1084      | 類型 '{0}'實作了 Windows 執行階段非同步介面'{1}'。 Windows 執行階段類型不能實作非同步介面。 請使用 System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory 類別產生非同步作業，以匯出到 Windows 執行階段。 |

> **注意：** 參考 Windows 執行階段的錯誤訊息使用了舊的詞彙。 這現在稱為通用 Windows 平台 (UWP)。 例如，Windows 執行階段類型現在稱為 UWP 類型。

 

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>遺失 mscorlib.dll 或 System.Runtime.dll 的參考


僅在從命令列使用 Winmdexp.exe 時，才會發生此問題。 建議您使用 /reference 選項來包含對於來自 .NET Framework 核心參考組件之 mscorlib.dll 和 System.Runtime.dll 的參考，其位於 "%ProgramFiles(x86)%\\Reference Assemblies\\Microsoft\\Framework\\.NETCore\\v4.5" (在 32 位元電腦上為 "%ProgramFiles%\\...") 中。

| 錯誤號碼 | 訊息文字                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | 未參考 mscorlib.dll。 必須有這個中繼資料檔的參考，才能正確匯出。                               |
| WME1090      | 無法判斷核心參考組件。 請確定已使用 /reference 參數參考 mscorlib.dll 和 System.Runtime.dll。 |

 

## <a name="operator-overloading-is-not-allowed"></a>不允許運算子多載


在使用 Managed 程式碼撰寫的 Windows 執行階段元件中，您無法公開公用類型的多載運算子。

> **注意：** 在錯誤訊息中，中繼資料名稱，例如 op\_Addition、 op\_Multiply、 op\_ExclusiveOr、 op\_Implicit （隱含轉換），以及等等來識別電信業者。

 

| 錯誤代碼 | 訊息文字                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | '{0}' 是運算子多載。 Managed 類型無法在 Windows 執行階段中公開運算子多載。 |

 

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>一個類別的建構函式具有相同數量的參數


在 UWP 中，一個類別只能有一個具有指定數量參數的建構函式，例如，您不能有一個建構函式具有類型 **String** 的單一參數，同時又有一個建構函式具有類型 **int** (在 Visual Basic 中為 **Integer**) 的單一參數。 唯一的解決方法是對每個建構函式使用不同數量的參數。

| 錯誤代碼 | 訊息文字                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | 類型 '{0}'有多個建構函式具有'{1}' 個引數。 Windows 執行階段類型不能有多個引數數目相同的建構函式。 |

 

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>對於具有相同參數數量的多載，必須指定預設值


在 UWP 中，只有將某個多載指定為預設多載後，多載方法才可以有相同數量的參數。 如需詳細資訊，請參閱[在 C# 和 Visual Basic 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)中的＜多載方法＞。

| 錯誤代碼 | 訊息文字                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | 多個{0}-參數多載 '{1}。{2}' 使用 Windows.Foundation.Metadata.DefaultOverloadAttribute 裝飾。                                                            |
| WME1085      | {0}-參數多載{1}。{2}必須指定為預設多載，藉由使用 Windows.Foundation.Metadata.DefaultOverloadAttribute 裝飾它的單一方法。 |

 

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>命名空間錯誤且輸出檔的名稱無效


在通用 Windows 平台中，Windows 中繼資料 (.winmd) 檔案中的所有公用類型必須位於共用 .winmd 檔案名稱的命名空間中，或位於該檔案名稱的子命名空間中。 例如，如果您的 Visual Studio 專案名稱為 A.B (也就是說，您的 Windows 執行階段元件為 A.B.winmd)，則此專案可包含公用類別 A.B.Class1 與 A.B.C.Class2，但不可包含 A.Class3 (WME0006) 或 D.Class4 (WME1044)。

> **注意：** 這些限制適用於公用類型，而不適用於您實作所使用的私用類型。

 

以 A.Class3 為例，您可以將 Class3 移至其他命名空間，或將 Windows 執行階段元件的名稱變更為 A.winmd。 雖然 WME0006 是警告，但您應將其視為錯誤。 在上述範例中，呼叫 A.B.winmd 的程式碼將找不到 A.Class3。

以 D.Class4 為例，沒有任何檔案名稱可同時包含 D.Class4 與 A.B 命名空間中的類別，因此變更 Windows 執行階段元件的名稱是不可行的。 您可以將 D.Class4 移至另一個命名空間，或將其放在另一個 Windows 執行階段元件中。

檔案系統無法區分大小寫，因此不允許只有大小寫不同的命名空間 (WME1067)。

您的元件至少必須包含一個 **public sealed** 類型 (在 Visual Basic 中為 **Public NotInheritable**)。 否則，根據您的元件是否包含私用類型而定，將會出現 WME1042 或 WME1043。

Windows 執行階段元件中的類型不可與命名空間同名 (WME1068)。

> **注意：** 如果您直接呼叫 Winmdexp.exe，而未使用 /out 選項來指定您的 Windows 執行階段元件的名稱，Winmdexp.exe 會嘗試產生包含元件中所有命名空間的名稱。 為命名空間重新命名，可變更元件的名稱。

 

| 錯誤代碼 | 訊息文字                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | '{0}' 不是這個組件的有效 winmd 檔案名稱。 Windows 中繼資料檔中的所有類型都必須存在檔名所隱含之命名空間的子命名空間內。 執行階段找不到不存在於這種子命名空間的類型。 在這個組件，可做為檔名的最小通用命名空間是 '{1}'。 |
| WME1042      | 輸入模組必須至少包含一個位在命名空間內的公用類型。                                                                                                                                                                                                                                                                   |
| WME1043      | 輸入模組必須至少包含一個位在命名空間內的公用類型。 在命名空間內只發現私用類型。                                                                                                                                                                                                               |
| WME1044      | 公用類型有命名空間 ('{1}') 的共用通的前置詞與其他命名空間 ('{0}')。 Windows 中繼資料檔中的所有類型都必須存在檔名所隱含之命名空間的子命名空間內。                                                                                                                              |
| WME1067      | 命名空間名稱不能只有大小寫不同: '{0}'、'{1}'。                                                                                                                                                                                                                                                                                                |
| WME1068      | 類型 '{0}'不能有相同的命名空間名稱'{1}'。                                                                                                                                                                                                                                                                                                 |

 

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>匯出的類型不是有效的通用 Windows 平台類型


元件的公用介面必須只公開 UWP 類型。 但是，.NET Framework 針對許多在 .NET Framework 與 UWP 中只有些許差異的通用類型提供了對應。 這可讓 .NET Framework 開發人員使用熟悉的類型，而不用學習新的類型。 您可以在元件的公用介面中使用這些對應的 .NET Framework 類型。 請參閱[在 C# 和 Visual Basic 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)與 [Windows 執行階段類型的 .NET Framework 對應](net-framework-mappings-of-windows-runtime-types.md)中的＜在 Windows 執行階段元件中宣告類型＞與＜將 Windows 執行階段類型傳遞至 Managed 程式碼＞。

其中有許多對應都是介面。 例如，[IList&lt;T&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) 會對應至 UWP 介面 [IVector&lt;T&gt;](https://msdn.microsoft.com/library/windows/apps/br206631.aspx)。 若您使用 List&lt;string&gt; (在 Visual Basic 中為 `List(Of String)`)，而非使用 IList &lt;string&gt; 做為參數類型，則 Winmdexp.exe 會提供包含所有由 List&lt;T&gt; 實作之對應介面的替代項目清單。 若您使用 List&lt;Dictionary&lt;int, string&gt;&gt; 之類的巢狀泛型類型 (在 Visual Basic 中為 List(Of Dictionary(Of Integer, String)))，則 Winmdexp.exe 會提供適用於各個巢狀層級的選項。 這些清單可能會變得相當長。

一般而言，最接近類型的介面就是最好的選擇。 以 Dictionary&lt;int, string&gt; 為例，IDictionary&lt;int, string&gt; 最有可能是最佳選擇。

> **重要**JavaScript 會使用 managed 的類型實作之介面清單中第一個出現的介面。 例如，若您將 Dictionary&lt;int, string&gt; 傳回 JavaScript 程式碼，則無論您將哪個介面指定為傳回類型，其皆會顯示為 IDictionary&lt;int, string&gt;。 這表示，如果第一個介面不包含出現在後續介面上的成員，該成員即不會對 JavaScript 顯示。

> **注意：** 避免使用非泛型[IList](https://msdn.microsoft.com/library/system.collections.ilist.aspx)和[IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)介面，如果您的元件會使用的 JavaScript。 這兩個介面會分別對應至 [IBindableVector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindablevector.aspx) 和 [IBindableIterator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindableiterator.aspx)。 它們支援 XAML 控制項的繫結，且不會對 JavaScript 顯示。 JavaScript 會發出執行階段錯誤「函式 'X' 具有無效簽章，無法呼叫」。

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">錯誤代碼</th>
<th align="left">訊息文字</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">方法 '{0}'有參數'{1}'的類型'{2}'。 '{2}' 不是有效的 Windows 執行階段參數類型。</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">方法 '{0}'有類型的參數'{1}' 簽章中。 雖然這種類型不是有效的 Windows 執行階段類型，但它實作了有效 Windows 執行階段類型的介面。 請考慮將變更將方法簽章改為使用其中一個下列類型: '{2}'。</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>方法 '{0}'有類型的參數'{1}' 簽章中。 雖然這種泛型類型不是有效的 Windows 執行階段類型，但此類型或其泛型參數實作了有效 Windows 執行階段類型的介面。 {2}</p>
> **注意**適用於{2}，Winmdexp.exe 會附加替代項目，清單例如 「 請考慮類型 'System.Collections.Generic.List&lt;T&gt;' 其中一項將方法簽章中類型: 'System.Collections.Generic.IList&lt;T&gt;，System.Collections.Generic.IReadOnlyList&lt;T&gt;，System.Collections.Generic.IEnumerable&lt;T&gt;'。 」
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">方法 '{0}'有類型的參數'{1}' 簽章中。 請勿使用 Managed 工作類型，應改用 Windows.Foundation.IAsyncAction、Windows.Foundation.IAsyncOperation，或其他 Windows 執行階段非同步介面其中之一。 標準的 .NET await 模式也適用於這些介面。 如需將 Managed 工作物件轉換成 Windows 執行階段非同步介面的詳細資訊，請參閱 System.Runtime.InteropServices.WindowsRuntime.AsyncInfo。</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>結構中包含不允許的欄位類型


在 UWP 中，結構只能包含欄位，而且只有結構可以包含欄位。 這些欄位必須是公用的。 有效的欄位類型包括列舉、結構及基本類型。

| 錯誤代碼 | 訊息文字                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | 結構 '{0}'有欄位'{1}'的類型'{2}'。 '{2}' 不是有效的 Windows 執行階段欄位類型。 Windows 執行階段結構中的每個欄位只能是 UInt8、Int16、UInt16、Int32、UInt32、Int64、UInt64、Single、Double、Boolean、String、Enum，或本身是結構。 |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>成員簽章中的陣列限制


在 UWP 中，成員簽章中的陣列必須是下限為 0 (零) 的一維陣列。 不允許 `myArray[][]` 之類的巢狀陣列類型 (在 Visual Basic 中為 `myArray()()`)。

> **注意：** 這項限制不適用於您的實作中內部使用的陣列。

 

| 錯誤代碼 | 訊息文字                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | 方法 '{0}'有陣列的類型'{1}' 的簽章中的非零下限。 Windows 執行階段方法簽章中的陣列的下限必須是零。 |
| WME1035      | 方法 '{0}'有類型的多維陣列'{1}' 簽章中。 Windows 執行階段方法簽章中的陣列必須是一維陣列。                  |
| WME1036      | 方法 '{0}'有類型的巢狀的陣列'{1}' 簽章中。 Windows 執行階段方法簽章中的陣列不能是巢狀陣列。                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>陣列參數必須指定陣列的內容是否可讀取或寫入


在 UWP 中，參數必須是唯讀或唯寫的。 參數不可標示 **ref** (在 Visual Basic 中為不含 [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx) 屬性的 **ByRef**)。 這適用於陣列的內容，因此陣列參數必須指出陣列內容是否為唯讀或唯寫。 這項指示對於 **out** 參數是明確的 (在 Visual Basic 中為具有 OutAttribute 屬性的 **ByRef** 參數)，但是以傳值方式傳遞的陣列參數 (在 Visual Basic 中為 ByVal) 則必須標示。 請參閱[將陣列傳遞到 Windows 執行階段元件](passing-arrays-to-a-windows-runtime-component.md)。

| 錯誤代碼 | 訊息文字         |
|--------------|----------------------|
| WME1101      | 方法 '{0}'有參數'{1}' 陣列，並且兩者都具有{2}和{3}。 在 Windows 執行階段中，內容陣列參數必須是可讀取或可寫入的。 請移除其中一個屬性從 '{1}'。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | 方法 '{0}'有輸出參數'{1}' 陣列，但該有{2}。 在 Windows 執行階段中，輸出陣列的內容是可寫入的。 請移除屬性從 '{1}'。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | 方法 '{0}'有參數'{1}' 陣列，並且具有 System.Runtime.InteropServices.InAttribute 或 System.Runtime.InteropServices.OutAttribute。 在 Windows 執行階段中，陣列參數必須具有{2}或{3}。 請移除這些屬性，或視需要以適當的 Windows 執行階段屬性取代。                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | 方法 '{0}'有參數'{1}' 不是陣列，並且具有任一{2}或{3}。 Windows 執行階段不支援標記非陣列參數與{2}或{3}。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | 方法 '{0}'有參數'{1}' 具有 System.Runtime.InteropServices.InAttribute 或 System.Runtime.InteropServices.OutAttribute。 Windows 執行階段不支援以 System.Runtime.InteropServices.InAttribute 或 System.Runtime.InteropServices.OutAttribute 標記參數。 請考慮移除 System.Runtime.InteropServices.InAttribute，並改以 'out' 修飾詞取代 System.Runtime.InteropServices.OutAttribute。 方法 '{0}'有參數'{1}' 具有 System.Runtime.InteropServices.InAttribute 或 System.Runtime.InteropServices.OutAttribute。 Windows 執行階段只支援以 System.Runtime.InteropServices.OutAttribute 標記 ByRef 參數，不支援這些屬性的其他用法。 |
| WME1106      | 方法 '{0}'有參數'{1}' 陣列。 在 Windows 執行階段中，陣列參數的內容必須是可讀取或可寫入的。 請套用{2}或{3}到 '{1}'。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## <a name="member-with-a-parameter-named-value"></a>參數名稱為 "value" 的成員


在 UWP 中，傳回值會視為輸出參數，且參數名稱必須是唯一的。 根據預設，Winmdexp.exe 會爲傳回值指定名稱 "value"。 如果您的方法有名稱為 "value" 的參數，就會發生 WME1092 錯誤。 有兩種方式可以修正這種情形：

-   使用 "value" 以外的名稱命名您的參數 (在屬性存取子中，則為 "returnValue" 以外的名稱)。
-   使用 ReturnValueNameAttribute 屬性變更傳回值的名稱，如下所示：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > using System.Runtime.InteropServices;
    > using System.Runtime.InteropServices.WindowsRuntime;
    >
    > [return: ReturnValueName("average")]
    > public int GetAverage(out int lowValue, out int highValue)
    > ```
    > ```vb
    > Imports System.Runtime.InteropServices
    > Imports System.Runtime.InteropServices.WindowsRuntime
    >
    > Public Function GetAverage(<Out> ByRef lowValue As Integer, _
    > <Out> ByRef highValue As Integer) As <ReturnValueName("average")> String
    > ```

> **注意：** 如果您變更傳回值的名稱，而新的名稱與另一個參數的名稱，您會收到錯誤 WME1091。

JavaScript 程式碼可依名稱存取方法的輸出參數，包括傳回值在內。 如需範例，請參閱 [ReturnValueNameAttribute](https://msdn.microsoft.com/library/windows/apps/system.runtime.interopservices.windowsruntime.returnvaluenameattribute.aspx) 屬性。

| 錯誤代碼 | 訊息文字 |
|--------------|--------------|
| WME1091 | 方法 '\{0}' 的傳回值是名為 '\{1}' 做為參數名稱相同。 Windows 執行階段方法參數和傳回值必須具有唯一的名稱。 |
| WME1092 | 方法 '\{0}' 有一個名為參數 '\{1}' 這等同於預設傳回值的名稱。 請考慮使用不同的參數名稱，或使用 System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute 明確指定傳回值的名稱。 |

**注意：** 的預設名稱為"returnValue 屬性存取子，而其他所有方法的 「 值 」。


## <a name="related-topics"></a>相關主題

* [在 C# 和 Visual Basic 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp.exe (Windows 執行階段中繼資料匯出工具)](https://msdn.microsoft.com/library/hh925576.aspx)
