---
description: 'C #/WinRT 撰寫元件的診斷錯誤訊息'
title: '診斷 c #/WinRT 元件錯誤'
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d1e5eda34a8cbce70edc812ba27437090393ca0
ms.sourcegitcommit: 457239a6907198bc2948322a07c484dd8a170b33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230029"
---
# <a name="diagnose-cwinrt-component-errors"></a>診斷 c #/WinRT 元件錯誤

本文提供使用 c # 撰寫 Windows 執行階段元件限制的其他資訊/Winrt 當作者建立元件時，它會展開 c #/WinRT 中的錯誤訊息所提供的資訊。 針對現有 UWP .NET Native 受控元件，c # WinRT 元件的中繼資料是使用 .NET 工具 [Winmdexp.exe](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)來產生的。 既然 Windows 執行階段的支援已從 .NET 分離，c #/WinRT 會提供從元件產生 winmd 檔案的工具。 Windows 執行階段在程式碼上有比 c # 類別庫更多的條件約束，而 c #/WinRT 診斷掃描器會在產生 winmd 檔案之前警告您。

本文涵蓋您在 c # 的組建中報告的錯誤/Winrt 本文可作為使用 Winmdexp.exe 工具之 [現有 UWP .NET Native 受管理元件](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 的限制相關資訊的更新版本。

搜尋錯誤訊息正文， (省略預留位置) 或訊息編號的特定值。 如果您在這裡找不到所需的資訊，可以使用本文結尾的 [意見反應] 按鈕來協助我們改善檔。 在您的意見反應中，請包含錯誤訊息。 或者，您可以在 [c #/WinRT](https://github.com/microsoft/CsWinRT)存放庫中提出錯誤（bug）。

本文依案例組織錯誤訊息。

## <a name="implementing-interfaces-that-arent-valid-windows-runtime-interfaces"></a>執行介面 Windows 執行階段介面無效

C #/WinRT 元件無法執行某些 Windows 執行階段介面，例如代表非同步動作或作業的 Windows 執行階段介面 (**IAsyncAction**、 **\<TProgress\> iasyncactionwithprogress<tprogress> iasyncoperation<tresult>**、 **iasyncoperation<tresult>HTTP \<TResult\>** 或 **IAsyncOperationWithProgress \<TResult,TProgress\>**) 。 相反地，請使用 **system.runtime.interopservices.windowsruntime.asyncinfo** 類別，在 Windows 執行階段元件中產生非同步作業。 注意：這些不是唯一的無效介面，例如類別無法實作為 **例外** 狀況。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1008</p></td>
<td><p>Windows 執行階段元件類型 {0} 無法實 {1} 作為介面，因為介面不是有效的 Windows 執行階段介面</p></td>
</tr>
</tbody>
</table>

## <a name="attribute-related-errors"></a>屬性相關的錯誤

在 Windows 執行階段中，只有當某個多載指定為預設多載時，多載方法才可以有相同數目的參數。 使用 **DefaultOverload** (CsWinRT1015，1016) 的屬性。

當陣列當做函數或屬性中的輸入或輸出使用時，它們必須是唯讀或僅限寫入 (CsWinRT 1025) 。 系統會提供 **system.runtime.interopservices.outattribute. system.runtime.interopservices.windowsruntime.asyncinfo. ReadOnlyArray** 和 **system.runtime.interopservices.outattribute. system.runtime.interopservices.windowsruntime.asyncinfo.** WriteOnlyArray 的屬性供您使用。 提供的屬性僅適用于陣列類型 (CsWinRT1026) 的參數，而且每個參數只能套用一個 (CsWinRT1023) 。

您不需要將任何屬性套用至標示為 **out** 的陣列參數，因為它會假設為僅限寫入。 如果您在此情況下將它裝飾為唯讀，則會出現錯誤訊息 (CsWinRT1024) 。

**System.runtime.interopservices.outattribute. InAttribute** 和 **system.runtime.interopservices.outattribute. OutAttribute** 不能用在任何類型的參數上， (CsWinRT1021，1022) 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1015</p></td>
<td><p>在類別中 {2} ： {0} ' ' 的多重參數多載 {1} 是以 windows.foundation.metadata.defaultoverloadattribute 裝飾裝飾。 屬性只可以套用至方法的一個多載。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1016</p></td>
<td><p>在類別中 {2} ：的 {0} -參數多載 {1} 必須有一個指定為預設多載的方法，方法是使用 windows.foundation.metadata.defaultoverloadattribute 裝飾的屬性進行裝飾。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1021</p></td>
<td><p>方法 ' {0} ' 具有屬於陣列的參數 ' ' {1} ，其具有 System.runtime.interopservices.outattribute. InAttribute 或 System.runtime.interopservices.outattribute. OutAttribute 的參數。 在 Windows 執行階段中，陣列參數必須是 ReadOnlyArray 或 WriteOnlyArray。 請移除這些屬性，或視需要以適當的 Windows 執行階段屬性取代。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1022</p></td>
<td><p>方法 ' {0} ' 的參數 ' ' {1} 具有 System.runtime.interopservices.outattribute. InAttribute 或 system.runtime.interopservices.outattribute. Windows 執行階段 OutAttribute，但不支援將參數標記為 system.runtime.interopservices.outattribute. InAttribute 或 system.runtime.interopservices.outattribute. OutAttribute...............。 請考慮移除 System.Runtime.InteropServices.InAttribute，並改以 'out' 修飾詞取代 System.Runtime.InteropServices.OutAttribute。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1023</p></td>
<td><p>方法 ' {0} ' 具有屬於陣列的參數 ' ' {1} ，且其中同時具有 ReadOnlyArray 和 WriteOnlyArray。 在 Windows 執行階段中，內容陣列參數必須是可讀取或可寫入的。 請從 '{1}' 移除其中一個屬性。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1024</p></td>
<td><p>方法 ' {0} ' 具有具有 ReadOnlyArray 屬性的輸出參數 ' {1} '，其為數組。 在 Windows 執行階段中，輸出陣列的內容是可寫入的。  請從 '{1}' 移除屬性。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1025</p></td>
<td><p>方法 ' {0} ' 具有屬於陣列的參數 ' ' {1} 。 在 Windows 執行階段中，陣列參數的內容必須是可讀取或可寫入的。 請將 ReadOnlyArray 或 WriteOnlyArray 套用至 ' {1} '。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1026</p></td>
<td><p>方法 ' {0} ' 有不是陣列的參數 ' ' {1} ，且具有 ReadOnlyArray 屬性或 WriteOnlyArray 屬性。 Windows 執行階段不支援使用 ReadOnlyArray 或 WriteOnlyArray 標記非陣列參數。」</p></td>
</tr>
</tbody>
</table>

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>命名空間錯誤且輸出檔的名稱無效

在 Windows 執行階段中，Windows 中繼資料 ( winmd) 檔案中的所有公用類型必須位於共用 winmd 檔案名的命名空間中，或位於檔案名的子命名空間中。 例如，如果您的 Visual Studio 專案的名稱為 b. (亦即，您的 Windows 執行階段元件為 b. winmd) ，它可以包含公用類別 A. Class2，但不能包含 A.class3 為例或 d. D.class4 與 a.b。

> [!NOTE]
> 這些限制僅適用於公用類型，而不適用於您的實作所使用的私用類型。

在 A.class3 為例的案例中，您可以將 A.class3 為例移至另一個命名空間，或將 Windows 執行階段元件的名稱變更為 winmd。 在上述範例中，呼叫 A.B.winmd 的程式碼將找不到 A.Class3。

在 D.class4 與 a.b 的案例中，沒有檔案名可以同時包含. B 命名空間中的 D.class4 與 a.b 和類別，因此無法選擇變更 Windows 執行階段元件的名稱。 您可以將 d. D.class4 與 a.b 移至另一個命名空間，或將它放在另一個 Windows 執行階段元件中。

檔案系統無法區分大小寫，因此不允許使用大小寫不同的命名空間 (CsWinRT1002) 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1001</p></td>
<td><p>公用類型具有命名空間 ( ' {1} ' ) ，此命名空間與其他命名空間 ( ' ' ) 不共用任何一般前置詞 {0} 。 Windows 中繼資料檔中的所有類型都必須存在檔名所隱含之命名空間的子命名空間內。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1002</p></td>
<td><p>找到多個名稱為 ' ' 的命名空間 {0} ; 命名空間名稱不能只有 Windows 執行階段中的大小寫不同。</p></td>
</tr>
</tbody>
</table>

## <a name="exporting-types-that-arent-valid-windows-runtime-types"></a>匯出的類型不是有效的 Windows 執行階段類型

元件的公用介面必須只公開 Windows 執行階段類型。 不過，.NET 提供幾個常用類型的對應，這些類型在 .NET 和 Windows 執行階段中稍有不同。 這可讓 .NET 開發人員使用熟悉的類型，而不是學習新的型別。 您可以在元件的公用介面中使用這些對應的 .NET Framework 類型。 如需詳細資訊，請參閱 [在 Windows 執行階段元件中宣告類型](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#declaring-types-in-windows-runtime-components)、 [將 Windows 執行階段型別傳遞至 managed 程式碼](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#passing-windows-runtime-types-to-managed-code)，以及 [Windows 執行階段類型的 .net](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)對應。

其中有許多對應都是介面。 例如，IList \<T\> 對應至 Windows 執行階段介面 IVector \<T\> 。 如果您使用 List \<string\> 而非 IList \<string\> 做為參數類型，c #/WinRT 會提供替代專案清單，其中包含所有由 List 所執行的對應介面 \<T\> 。 如果您使用內嵌的泛型型別（例如 List \<Dictionary\<int, string\> \> ），c #/WinRT 會提供每個嵌套層級的選項。 這些清單可能會變得相當長。

一般而言，最接近類型的介面就是最好的選擇。 例如，針對字典 \<int, string\> ，最好的選擇是最可能的 IDictionary \<int, string\> 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1006</p></td>
<td><p>成員 ' {0} ' 在其簽章中具有類型 ' ' {1} 。 類型 ' {1} ' 不是有效的 Windows 執行階段類型。 但是，類型 (或其泛型參數) 實作為有效 Windows 執行階段類型的介面。 請考慮將成員簽章中的類型 ' {1} ，變更為下列其中一種類型： Generic： {2} 。</p></td>
</tr>
</tbody>
</table>

在 Windows 執行階段中，成員簽章中的陣列必須是下限為 0 (零) 的一維。 不允許嵌套的陣列類型 `myArray[][]` ，例如 (CsWinRT1017) 和 `myArray[,]` (CsWinRT1018) 。

> [!NOTE]
> 這項限制不適用於您在自己的實作內部使用的陣列。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1017</p></td>
<td><p>方法 {0} 在其簽章中具有類型的嵌套陣列 {1} 。 Windows 執行階段方法簽章中的陣列無法進行嵌套。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1018</p></td>
<td><p>方法 ' {0} ' 在其簽章中具有類型 ' ' 的多維度陣列 {1} 。 Windows 執行階段方法簽章中的陣列必須是一維陣列。</p></td>
</tr>
</tbody>
</table>

## <a name="structures-that-contain-fields-of-disallowed-types"></a>結構中包含不允許的欄位類型

在 Windows 執行階段中，結構只能包含欄位，且只有結構可包含欄位。 這些欄位必須是公用的。 有效的欄位類型包括列舉、結構及基本類型。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1007</p></td>
<td><p>結構 {0} 未包含公用欄位。 Windows 執行階段結構必須至少包含一個公用欄位。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1011</p></td>
<td><p>結構 {0} 具有非公用欄位。 針對 Windows 執行階段結構，所有欄位都必須是公用的。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1012</p></td>
<td><p>結構 {0} 有 const 欄位。 常數只能出現在 Windows 執行階段列舉上。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1013</p></td>
<td><p>結構 {0} 具有類型的欄位 {1} ; {1} 不是有效的 Windows 執行階段欄位類型。 Windows 執行階段結構中的每個欄位只能是 UInt8、Int16、UInt16、Int32、UInt32、Int64、UInt64、Single、Double、Boolean、String、Enum，或本身是結構。</p></td>
</tr>
</tbody>
</table>

## <a name="parameter-name-conflicts-with-generated-code"></a>參數名稱與產生的程式碼衝突

在 Windows 執行階段中，傳回值會被視為輸出參數，而參數的名稱必須是唯一的。 根據預設，c #/WinRT 會為傳回值提供名稱 `__retval` 。 如果您的方法具有名為的參數 `__retval` ，您將會收到錯誤 CsWinRT1010。 若要更正這個錯誤，請為您的參數指定以外的名稱 `__retvalue` 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1010</p></td>
<td><p>方法中的參數名稱與 {1} {0} 產生的 c #/WinRT interop 中使用的傳回值參數名稱相同; 請使用不同的參數名稱。</p></td>
</tr>
</tbody>
</table>

## <a name="miscellaneous"></a>其他

C #/WinRT 撰寫元件的其他限制包括：

- 您無法公開公用類型的多載運算子。
- 類別和介面不能是泛型。
- 類別必須是密封的。
- 無法以傳址方式傳遞參數，例如使用 **ref** 關鍵字。
- 屬性必須具有公用 get 方法。
- 元件的命名空間中必須有至少一個公用類型 (類別或介面) 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1014</p></td>
<td><p>' {0} ' 是運算子多載。 Managed 類型無法在 Windows 執行階段中公開運算子多載。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1004</p></td>
<td><p>類型 {0} 為泛型。 Windows 執行階段類型不能是泛型。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1005</p></td>
<td><p>CsWinRT 不支援匯出非密封類型。 請將類型標示 {0} 為密封。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1020</p></td>
<td><p>方法 ' {0} ' 已標記參數 ' ' {1} `ref` 。 Windows 執行階段中不允許參考參數。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1000</p></td>
<td><p>屬性 ' {0} ' 沒有公用 getter 方法。 Windows 執行階段不支援僅限 setter 屬性。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1003</p></td>
<td><p>Windows 執行階段元件至少必須有一個公用類型</p></td>
</tr>
</tbody>
</table>

在 Windows 執行階段中，一個類別只能有一個具有指定參數數目的函式。 例如，您不能有一個具有 **字串** 類型之單一參數的函式，以及另一個具有 **int** 類型之單一參數的函式。唯一的解決方法是針對每個函式使用不同數目的參數。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>錯誤號碼</p></th>
<th><p>訊息文字</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1009</p></td>
<td><p>類別在 Windows 執行階段中不能有多個相同 arity 的函式，而類別 {0} 具有多個 arity 的函 {1} 式。</p></td>
</tr>
</tbody>
</table>


