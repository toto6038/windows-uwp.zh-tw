---
author: msatranjr
title: 將陣列傳遞到 Windows 執行階段元件
description: Windows 通用平台 (UWP) 中的參數分成輸入和輸出兩種，但不可能兩者皆是。 這表示傳遞到方法以及陣列本身的陣列內容也會分成輸入或輸出。
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e01c9e5698ec1d7a23298b46f6bde9e1bbf36b04
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6855280"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>將陣列傳遞到 Windows 執行階段元件




Windows 通用平台 (UWP) 中的參數分成輸入和輸出兩種，但不可能兩者皆是。 這表示傳遞到方法以及陣列本身的陣列內容也會分成輸入或輸出。 如果陣列的內容是用於輸入，方法就會從陣列讀取，而不會寫入陣列。 如果陣列的內容是用於輸出，方法就會寫入陣列，而不會從陣列讀取。 這會衍生出一個有關陣列參數的問題，因為 .NET Framework 中的陣列屬於參考類型，即便是依值傳遞陣列參考 (在 Visual Basic 中為 **ByVal**)，陣列的內容都是可變動的。 [Windows 執行階段中繼資料匯出工具 (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) 會在無法從內容判斷陣列的預定用法時，要求您對參數套用 ReadOnlyArrayAttribute 屬性或 WriteOnlyArrayAttribute 屬性，以指定其用法。 陣列用法的判斷方式如下：

-   就傳回值或 out 參數 (在 Visual Basic 中為具有 [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx) 屬性的 **ByRef** 參數) 而言，陣列一律僅供輸出。 請不要套用 ReadOnlyArrayAttribute 屬性。 WriteOnlyArrayAttribute 屬性可用於輸出參數，但這是多此一舉。

    > **注意：** Visual Basic 編譯器不會強制執行僅限輸出的規則。 您不應讀取輸出參數，其中可能包含 **Nothing**。 請一律指派新的陣列。
 
-   參數不可以有 **ref** 修飾詞 (在 Visual Basic 中為 **ByRef**)。 Winmdexp.exe 會產生錯誤。
-   對於依值傳遞的參數，您必須套用 [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx) 屬性或 [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx) 屬性，以指定陣列內容是用於輸入或輸出。 同時指定這兩個屬性會產生錯誤。

如果方法必須接受用於輸入的陣列，請修改陣列內容，並將陣列傳回至呼叫端、對輸入使用唯讀參數，並對輸出使用唯寫參數 (或傳回值)。 下列程式碼將說明一個實作此模式的方式：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

建議您立即建立輸入陣列的複本，並妥善管理此複本。 如此可確保無論 .NET Framework 程式碼是否呼叫您的元件，方法的運作方式都相同。

## <a name="using-components-from-managed-and-unmanaged-code"></a>從 Managed 程式碼與 Unmanaged 程式碼使用元件


具有 ReadOnlyArrayAttribute 屬性或 WriteOnlyArrayAttribute 屬性的參數，會因為呼叫端是以機器碼或 Managed 程式碼所撰寫，而有不同的運作方式。 如果呼叫端是機器碼 (JavaScript 或 Visual C++ 元件擴充功能)，則會將陣列內容視為：

-   ReadOnlyArrayAttribute：在呼叫跨越應用程式二進位介面 (ABI) 界限時，會複製陣列。 視需要轉換元素。 因此，方法無意間對僅限輸入的陣列所做的任何變更，呼叫端都看不見。
-   WriteOnlyArrayAttribute：呼叫的方法無法對原始陣列的內容進行任何假設。 例如，方法所接收的陣列可能並未初始化，或者可能包含預設值。 方法依預期應設定陣列中所有元素的值。

如果呼叫端是 Managed 程式碼，則呼叫的方法將可使用原始陣列，因為 .NET Framework 中的任何方法呼叫都會包含此陣列。 陣列內容在 .NET Framework 程式碼中是可變動的，因此，呼叫端可看見方法對陣列所做的任何變更。 請務必記住這一點，因為這會影響到為 Windows 執行階段元件撰寫的單元測試。 如果測試是利用 Managed 程式碼撰寫的，陣列的內容在測試期間就可能是可變動的。

## <a name="related-topics"></a>相關主題

* [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx)
* [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx)
* [在 C# 和 Visual Basic 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
