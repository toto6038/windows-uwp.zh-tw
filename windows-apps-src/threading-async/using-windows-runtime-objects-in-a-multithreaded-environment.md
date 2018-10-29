---
title: 在多執行緒環境中使用 Windows 執行階段物件 | Microsoft Docs
description: 本文討論 .NET Framework 如何處理從 C# 與 Visual Basic 程式碼對 Windows 執行階段或 Windows 執行階段元件所提供物件的呼叫。
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
author: normesta
ms.author: normesta
keywords: windows 10, uwp, timer, threads, Windows 10, uwp, 計時器, 執行緒
ms.localizationpriority: medium
ms.openlocfilehash: 9f4b8249a81cb7d71ba1f4775fd858ae87779c85
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5748212"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>在多執行緒環境中使用 Windows 執行階段物件
本文討論 .NET Framework 如何處理從 C# 與 Visual Basic 程式碼對 Windows 執行階段或 Windows 執行階段元件所提供物件的呼叫。

在 .NET Framework 中，您預設可從多執行緒中存取任何物件，無需特殊處理。 您只需要物件的參考即可。 在 Windows 執行階段中，這類物件稱做 *agile*。 大多數的 Windows 執行階段類別為 agile，但一些類別不是，agile 類別甚至可能需要特殊處理。

如果可行，Common Language Runtime (CLR) 會將其他來源 (例如 Windows 執行階段) 的物件視為 .NET Framework 物件：

- 如果物件實作 [IAgileObject](http://msdn.microsoft.com/library/Hh802476.aspx) 介面，或有具有 [MarshalingType.Agile](http://go.microsoft.com/fwlink/p/?LinkId=256023) 的 [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) 屬性，則 CLR 會將該物件視為 agile。

- 如果 CLR 會封送處理來自執行緒的呼叫，且該執行緒是對目標物件的執行緒內容進行此呼叫之處，則系統會以透明的方式進行。

- 如果物件有具有 [MarshalingType.None](http://go.microsoft.com/fwlink/p/?LinkId=256023) 的 [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) 屬性，則該類別不會提供封送處理資訊。 CLR 無法封送處理呼叫，因此會擲回 [InvalidCastException](/dotnet/api/system.invalidcastexception) 例外狀況，並顯示訊息指出該物件僅用於其建立所在的執行緒內容中。

下列章節說明此行為對各種來源中物件的效果。

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>來自以 C# 或 Visual Basic 語言所撰寫 Windows 執行階段元件的物件
可啟動之元件中的所有類型皆預設為 agile。

> [!NOTE]
>  敏捷性並不代表執行緒是安全的。 在 Windows 執行階段與 .NET Framework 中，大多數的類別都不是安全執行緒，因為執行緒安全性具有效能成本，且大多數的物件從未由多個執行緒存取過。 只有在需要時才同步處理對個別物件的存取 (或使用安全執行緒類別)，這種做法會更有效率。

當您撰寫 Windows 執行階段元件時，您可以覆寫預設值。 查看 [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) 介面與 [IAgileObject](http://msdn.microsoft.com/library/Hh802476.aspx) 介面。

## <a name="objects-from-the-windows-runtime"></a>來自 Windows 執行階段的物件
Windows 執行階段中大多數的類別是 agile，CLR 會將這些類別視為 agile。 這些類別的文件會列出類別屬性之間的 "MarshalingBehaviorAttribute(Agile)"。 但是，如果未在 UI 執行緒上呼叫這些 agile 類別中的成員，例如 XAML 控制項，則這些成員會擲回例外狀況。 例如，下列程式碼嘗試使用背景執行緒，來設定已按下按鈕的屬性。 按鈕的 [Content](http://go.microsoft.com/fwlink/p/?LinkId=256025) 屬性擲回例外狀況。

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

您可以使用按鈕的 [Dispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256026) 屬性，或任何存在於 UI 執行緒 (例如按鈕為開啟的頁面) 其內容中物件的 `Dispatcher` 屬性，來安全地存取按鈕。 下列程式碼使用 [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) 物件的 [RunAsync](http://go.microsoft.com/fwlink/p/?LinkId=256030) 方法在 UI 執行緒上發送呼叫。

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  當從另一個執行緒呼叫 `Dispatcher` 屬性時，該屬性不會擲回例外狀況。

在 UI 執行緒上建立的 Windows 執行階段物件其存留期受執行緒存留期的約束。 在視窗關閉後，不要嘗試存取物件 UI 執行緒上的物件。

如果您透過繼承 XAML 控制項或撰寫一組 XAML 控制項來建立自己的控制項，您的控制項是 agile，因為它是 .NET Framework 物件。 但如果它呼叫其基底類別或組成類別的成員，或如果您呼叫繼承的成員，則當從 UI 執行緒以外的任何執行緒呼叫這些成員時，這些成員將擲回例外狀況。

### <a name="classes-that-cant-be-marshaled"></a>無法封送處理的類別
未提供封送處理資訊的 Windows 執行階段類別有具有 [MarshalingType.None](http://go.microsoft.com/fwlink/p/?LinkId=256023) 的 [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) 屬性。 這類類別的文件會在其屬性之間列出 "MarshalingBehaviorAttribute(None)"。

下列程式碼會在 UI 執行緒中建立 [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) 物件，然後嘗試從執行緒集區執行緒設定該物件的屬性。 CLR 無法封送處理呼叫，且會擲回 [System.InvalidCastException](/dotnet/api/system.invalidcastexception) 例外狀況，並顯示訊息指出該物件僅用於其建立所在的執行緒內容中。

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

[CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) 的文件也列出類別屬性之間的 "ThreadingAttribute(STA)"，因為它必須在如 UI 執行緒的單一執行緒內容中建立。

如果您想要存取另一個執行緒的 [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) 物件，您可以快取 UI 執行緒的 [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) 物件，並稍後使用此物件在該執行緒上發送呼叫。 或者您可以從如頁面等 XAML 物件取得發送器，如以下程式碼所示。

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>來自以 C++ 語言所撰寫 Windows 執行階段元件的物件
依預設，在可啟用元件中的類別為 agile。 然而，C++ 允許大量的控制項控制執行緒模型與封送處理行為。 如本文稍早所述，CLR 會辨識 agile 類別、在類別非 agile 時嘗試封送處理呼叫，及在類別沒有封送處理資訊時擲回 [System.InvalidCastException](/dotnet/api/system.invalidcastexception) 例外狀況。

若物件在 UI 執行緒上執行，且當從 UI 執行緒之外的執行緒呼叫該物件時，您可以對該物件使用 UI 執行緒的 [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) 物件來發送呼叫。

## <a name="see-also"></a>另請參閱
[C# 指南](/dotnet/articles/csharp/)

[Visual Basic 指南](/dotnet/articles/visual-basic/)
