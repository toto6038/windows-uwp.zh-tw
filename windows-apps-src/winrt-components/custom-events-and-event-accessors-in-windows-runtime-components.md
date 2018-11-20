---
author: msatranjr
title: Windows 執行階段元件中的自訂事件和事件存取子
description: 適用於 Windows 執行階段元件的 .NET Framework 支援可隱藏通用 Windows 平台 (UWP) 事件模式和 .NET Framework 事件模式之間的差異，以便輕易地宣告事件元件。
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 99e215f382bbfe409ac72d021540a471294634ca
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7291195"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Windows 執行階段元件中的自訂事件和事件存取子



適用於 Windows 執行階段元件的 .NET Framework 支援可隱藏通用 Windows 平台 (UWP) 事件模式和 .NET Framework 事件模式之間的差異，以便輕易地宣告事件元件。 但是，當您在 Windows 執行階段元件中宣告自訂事件存取子時，必須遵循 UWP 中使用的模式。

## <a name="registering-events"></a>註冊事件


當您註冊以在 UWP 中處理事件時，add 存取子會傳回語彙基元。 若要取消註冊，您可以將這個語彙基元傳遞至 remove 存取子。 這表示 UWP 事件的 add 和 remove 存取子與您使用的存取子具有不同的簽章。

幸運的是，Visual Basic 和 C# 編譯器會簡化這個處理序：當您在 Windows 執行階段元件中宣告具有自訂存取子的事件時，編譯器會自動使用 UWP 模式。 例如，若 add 存取子未傳回語彙基元，就會發生編譯器錯誤。 .NET Framework 提供兩個類型來支援實作：

-   [EventRegistrationToken](https://msdn.microsoft.com/library/windows/apps/windows.foundation.eventregistrationtoken.aspx) 結構代表語彙基元。
-   [EventRegistrationTokenTable&lt;T&gt;](https://msdn.microsoft.com/library/hh138412.aspx) 類別會建立語彙基元，並維護語彙基元和事件處理常式之間的對應。 泛型類型引數是事件引數類型。 您為每個事件建立此類別的執行個體時，第一次為該事件註冊事件處理常式。

下列適用於 NumberChanged 事件的程式碼會顯示 UWP 事件的基本模式。 在這個範例中，事件引數物件的建構函式 (NumberChangedEventArgs) 會採用表示已變更數值的單一整數參數。

> **注意：** 這就是編譯器用於您在 Windows 執行階段元件中宣告之一般事件的相同模式。

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

static (在 Visual Basic 中為 Shared) GetOrCreateEventRegistrationTokenTable 方法會延遲建立事件的 EventRegistrationTokenTable&lt;T&gt; 物件執行個體。 將保存語彙基元資料表執行個體的類別層級欄位傳遞給此方法。 如果此欄位空白，這個方法會建立資料表、在欄位中儲存資料表的參考，然後傳回該資料表的參考。 如果欄位已經包含語彙基元資料表參考，這個方法就只會傳回該參考。

> **重要**為了確保執行緒安全，保存事件之 EventRegistrationTokenTable 的執行個體的欄位&lt;T&gt;必須是類別層級的欄位。 如果是類別層級的欄位，則 GetOrCreateEventRegistrationTokenTable 方法會確保當多個執行緒嘗試建立語彙基元資料表時，所有執行緒都會取得此資料表的相同執行個體。 對於指定的事件，GetOrCreateEventRegistrationTokenTable 方法的所有呼叫都必須使用相同的類別層級欄位。

在 remove 存取子和 [RaiseEvent](https://msdn.microsoft.com/library/fwd3bwed.aspx) 方法 (在 C# 中為 OnRaiseEvent 方法) 中呼叫 GetOrCreateEventRegistrationTokenTable 方法，可確保在加入任何事件處理常式委派之前呼叫這些方法，不會發生例外狀況。

在 UWP 事件模式中使用的其他 EventRegistrationTokenTable&lt;T&gt; 類別成員包括下列各項：

-   [AddEventHandler](https://msdn.microsoft.com/library/hh138458.aspx) 方法會產生事件處理常式委派的語彙基元、在資料表中儲存委派、將它加入至叫用清單，然後傳回語彙基元。
-   [RemoveEventHandler(EventRegistrationToken)](https://msdn.microsoft.com/library/hh138425.aspx) 方法多載會從資料表和叫用清單中移除此委派。

    >**注意：** AddEventHandler 和 removeeventhandler （eventregistrationtoken） 方法會鎖定資料表，協助確保執行緒安全。

-   [InvocationList](https://msdn.microsoft.com/library/hh138465.aspx) 屬性傳回的委派包含目前已註冊來處理事件的所有事件處理常式。 使用此委派來引發事件，或使用 Delegate 類別的方法個別叫用處理常式。

    >**注意：** 我們建議您遵循本文中，稍早所提供的範例中顯示的模式，並將委派複製到暫存變數，再予以叫用它。 這可避免某一個執行緒移除最後一個處理常式的競爭情形，進而讓委派在另一個執行緒嘗試叫用委派之前減少至 null。 委派是不可變的，因此複本仍然有效。

視情況將自己的程式碼放在存取子中。 如果執行緒安全有問題，您就必須為程式碼提供自己的鎖定。

C# 使用者：當您在 UWP 事件模式中寫入自訂事件存取子時，編譯器不會提供一般語法快速鍵。 如果您在程式碼中使用事件名稱，則會產生錯誤。

Visual Basic 使用者：在 .NET Framework 中，事件只是表示所有已註冊事件處理常式的多點傳送委派。 引發事件只是表示叫用委派。 Visual Basic 語法通常會隱藏與委派的互動，而且編譯器會在叫用委派之前先行複製 (如執行緒安全相關注意事項所述)。 當您在 Windows 執行階段元件中建立自訂事件時，您必須直接處理委派。 這也表示您可以使用 [MulticastDelegate.GetInvocationList](https://msdn.microsoft.com/library/system.multicastdelegate.getinvocationlist.aspx) 方法來取得一個陣列，如果您想要個別叫用處理常式，該陣列會包含每個事件處理常式的個別委派。

## <a name="related-topics"></a>相關主題

* [事件 (Visual Basic)](https://msdn.microsoft.com/library/ms172877.aspx)
* [事件 (C# 程式設計指南)](https://msdn.microsoft.com/library/awbftdfh.aspx)
* [.NET for UWP 應用程式的概觀](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [適用於 UWP App 的 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [逐步解說：建立簡單的 Windows 執行階段元件，並從 JavaScript 呼叫該元件](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
