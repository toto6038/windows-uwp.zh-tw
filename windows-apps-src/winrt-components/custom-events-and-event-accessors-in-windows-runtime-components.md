---
title: Windows 執行階段元件中的自訂事件和事件存取子
description: .NET 對 Windows 執行階段元件的支援可讓您藉由隱藏通用 Windows 平臺（UWP）事件模式與 .NET 事件模式之間的差異，輕鬆宣告事件元件。
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1891a89d83ea8a193d5c0fcedf939101323981e6
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340530"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Windows 執行階段元件中的自訂事件和事件存取子

.NET 對 Windows 執行階段元件的支援可讓您藉由隱藏通用 Windows 平臺（UWP）事件模式與 .NET 事件模式之間的差異，輕鬆宣告事件元件。 不過，當您在 Windows 執行階段元件中宣告自訂事件存取子時，必須遵循 UWP 中使用的模式。

## <a name="registering-events"></a>註冊事件

當您註冊以在 UWP 中處理事件時，add 存取子會傳回語彙基元。 若要取消註冊，您可以將這個語彙基元傳遞至 remove 存取子。 這表示 UWP 事件的 add 和 remove 存取子與您使用的存取子具有不同的簽章。

幸運的是，Visual Basic C#和編譯器會簡化此程式：當您在 Windows 執行階段元件中宣告具有自訂存取子的事件時，編譯器會自動使用 UWP 模式。 例如，若 add 存取子未傳回語彙基元，就會發生編譯器錯誤。 .NET 提供兩種類型來支援執行：

-   [EventRegistrationToken](https://docs.microsoft.com/uwp/api/windows.foundation.eventregistrationtoken) 結構代表語彙基元。
-   [EventRegistrationTokenTable&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1) 類別會建立語彙基元，並維護語彙基元和事件處理常式之間的對應。 泛型類型引數是事件引數類型。 您為每個事件建立此類別的執行個體時，第一次為該事件註冊事件處理常式。

下列適用於 NumberChanged 事件的程式碼會顯示 UWP 事件的基本模式。 在這個範例中，事件引數物件的建構函式 (NumberChangedEventArgs) 會採用表示已變更數值的單一整數參數。

> **請注意**  This 是編譯器針對您在 Windows 執行階段元件中宣告的一般事件所使用的相同模式。

 
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

> **重要**  To 確保執行緒安全性，保留事件 EventRegistrationTokenTable @ No__t-2T @ no__t-3 之實例的欄位，必須是類別層級欄位。 如果是類別層級的欄位，則 GetOrCreateEventRegistrationTokenTable 方法會確保當多個執行緒嘗試建立語彙基元資料表時，所有執行緒都會取得此資料表的相同執行個體。 對於指定的事件，GetOrCreateEventRegistrationTokenTable 方法的所有呼叫都必須使用相同的類別層級欄位。

在 remove 存取子和 [RaiseEvent](https://docs.microsoft.com/dotnet/articles/visual-basic/language-reference/statements/raiseevent-statement) 方法 (在 C# 中為 OnRaiseEvent 方法) 中呼叫 GetOrCreateEventRegistrationTokenTable 方法，可確保在加入任何事件處理常式委派之前呼叫這些方法，不會發生例外狀況。

在 UWP 事件模式中使用的其他 EventRegistrationTokenTable&lt;T&gt; 類別成員包括下列各項：

-   [AddEventHandler](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.addeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_AddEventHandler__0_) 方法會產生事件處理常式委派的語彙基元、在資料表中儲存委派、將它加入至叫用清單，然後傳回語彙基元。
-   [RemoveEventHandler(EventRegistrationToken)](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.removeeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_RemoveEventHandler_System_Runtime_InteropServices_WindowsRuntime_EventRegistrationToken_) 方法多載會從資料表和叫用清單中移除此委派。

    >**注意**  The AddEventHandler 和 RemoveEventHandler （EventRegistrationToken）方法會鎖定資料表，以協助確保執行緒安全性。

-   [InvocationList](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.invocationlist#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_InvocationList) 屬性傳回的委派包含目前已註冊來處理事件的所有事件處理常式。 使用此委派來引發事件，或使用 Delegate 類別的方法個別叫用處理常式。

    >**注意**  We 建議您遵循本文稍早所提供的範例中所示的模式，並將委派複製到暫存變數，再叫用它。 這可避免某一個執行緒移除最後一個處理常式的競爭情形，進而讓委派在另一個執行緒嘗試叫用委派之前減少至 null。 委派是不可變的，因此複本仍然有效。

視情況將自己的程式碼放在存取子中。 如果執行緒安全有問題，您就必須為程式碼提供自己的鎖定。

C#使用者當您在 UWP 事件模式中撰寫自訂事件存取子時，編譯器不會提供一般語法快捷方式。 如果您在程式碼中使用事件名稱，則會產生錯誤。

Visual Basic 使用者：在 .NET 中，事件只是一個多播委派，代表所有已註冊的事件處理常式。 引發事件只是表示叫用委派。 Visual Basic 語法通常會隱藏與委派的互動，而且編譯器會在叫用委派之前先行複製 (如執行緒安全相關注意事項所述)。 當您在 Windows 執行階段元件中建立自訂事件時，您必須直接處理委派。 這也表示您可以使用 [MulticastDelegate.GetInvocationList](https://docs.microsoft.com/dotnet/api/system.multicastdelegate.getinvocationlist#System_MulticastDelegate_GetInvocationList) 方法來取得一個陣列，如果您想要個別叫用處理常式，該陣列會包含每個事件處理常式的個別委派。

## <a name="related-topics"></a>相關主題

* [事件（Visual Basic）](https://docs.microsoft.com/dotnet/articles/visual-basic/programming-guide/language-features/events/index)
* [事件（C#程式設計手冊）](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/events/index)
* [適用于 UWP 應用程式的 .NET 總覽](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [適用于 UWP 應用程式的 .NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [建立 C# 或 Visual Basic Windows 執行階段元件，並從 JavaScript 呼叫該元件的逐步解說](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
