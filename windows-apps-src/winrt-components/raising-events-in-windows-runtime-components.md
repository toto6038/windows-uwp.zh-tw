---
author: msatranjr
title: 在 Windows 執行階段元件中引發事件
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 如何引發使用者定義的委派類型，在背景執行緒上的事件，如此 JavaScript 能夠接收該事件。
ms.author: misatran
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 89c021bb2c094aafc9b534acef9b009817669461
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2018
ms.locfileid: "4060414"
---
# <a name="raising-events-in-windows-runtime-components"></a>在 Windows 執行階段元件中引發事件
> [!NOTE]
> 了解如何以引發事件中的[C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Windows 執行階段元件，請參閱[中撰寫事件在 C + + /winrt](../cpp-and-winrt-apis/author-events.md)。

如果您的 Windows 執行階段元件會在背景執行緒 (背景工作執行緒) 上引發使用者定義之委派類型的事件，而您希望 JavaScript 可以接收該事件，則可透過下列其中一種方式來實作和 (或) 引發此事件：

-   (選項 1) 透過 [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) 引發事件，將事件封送處理至 JavaScript 執行緒內容。 雖然這通常是最佳選擇，但在某些情況下卻無法提供最佳效能。
-   (選項 2) 使用 [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt;，不過會遺失類型資訊 (但會遺失事件類型資訊)。 如果選項 1 不可行或其效能不足，而且您可以接受遺失類型資訊的缺點，那麼選項 2 也是不錯的選擇。
-   (選項 3) 針對元件建立您自己的 Proxy 和虛設常式。 這是最難實作的選項，但對於有需要的案例來說，與選項 1 相比，這個選項不僅可以保留類型資訊，可能還會提供更佳的效能。

如果您僅在背景執行緒上引發事件，卻沒有使用上述任何選項，JavaScript 用戶端就不會接收事件。

## <a name="background"></a>背景

所有 Windows 執行階段元件和 app 基本上都是 COM 物件，無論您使用哪種語言來建立它們都一樣。 在 Windows API 中，大部分元件都是敏捷式 COM 物件，這些元件與在背景執行緒上及 UI 執行緒上的物件通訊的能力都相同。 如果 COM 物件無法成為敏捷式物件，則它需要稱為 Proxy 和虛設常式的協助程式物件，協助跨越 UI 執行緒背景的執行緒界限，才能與其他 COM 物件通訊 (從 COM 方面來說，這稱為執行緒 Apartment 之間的通訊)。

Windows API 中的大部分物件都是敏捷式物件或內建 Proxy 和虛設常式。 不過，您無法為泛型類型 (例如 Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx)) 建立 Proxy 和虛設常式，因為除非提供類型引數，否則它們都不是完整類型。 JavaScript 用戶端只有在缺少 Proxy 或虛設常式時才會有問題，但若您希望自己的元件在 JavaScript 以及 C++ 或 .NET 語言中都能使用，就必須使用下列其中一個選項。

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(選項 1) 透過 CoreDispatcher 引發事件

您可以使用 [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx)，傳送具有任何使用者定義之委派類型的事件，如此 JavaScript 就可以接收這些事件。 如果不確定要使用哪個選項，請先嘗試這一個。 如果事件引發和事件處理之間的延遲情況造成問題，請嘗試其他兩個選項。

下列範例示範如何使用 CoreDispatcher 來引發強類型事件。 請注意，類型引數是 Toast 而非 Object。

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(選項 2) 使用 EventHandler&lt;Object&gt;，但會遺失類型資訊

另一種從背景執行緒傳送事件的方式，是使用 [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; 做為事件的類型。 Windows 會提供這個泛型類型的具象安裝，並為其提供 Proxy 和虛設常式。 缺點是事件引數的類型資訊和傳送者都會遺失。 C++ 和 .NET 用戶端必須透過文件來得知收到事件時要轉換回的類型。 JavaScript 用戶端不需要原始類型資訊， 而是會根據其在中繼資料內的名稱來尋找 arg 屬性。

下列範例示範如何在 C# 中使用 Windows.Foundation.EventHandler&lt;Object&gt;：

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

在 JavaScript 端取用這個事件的方式如下所示：

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(選項 3) 建立您自己的 Proxy 和虛設常式。

如需激發使用者定義之事件類型的潛在效能並完整保留類型資訊，您必須建立自己的 Proxy 和虛設常式物件，並將其內嵌於 app 封裝中。 通常，只有另外兩個選項不適用時才必須使用這個選項，但這是極少見的情況。 此外，這個選項也不保證能提供比其他兩個選項更佳的效能。 實際效能取決於諸多因素。 請使用 Visual Studio 分析工具或其他程式碼剖析工具來測量應用程式中的實際效能，並判斷事件是否確實為效能瓶頸。

本文的剩餘部分說明如何使用 C# 來建立基本 Windows 執行階段元件，然後使用 C++ 來為 Proxy 和虛設常式建立 DLL，讓 JavaScript 能夠取用非同步作業中元件所引發的 Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; 事件。 (您也可以使用 C++ 或 Visual Basic 來建立元件。 建立 Proxy 和虛設常式的相關步驟都一樣)。這個逐步解說會以＜建立 Windows 執行階段同處理序元件範例 (C++/CX)＞為基礎，協助說明其用途。

這個逐步解說包含下列部分：

-   您將在此處建立兩個基本的 Windows 執行階段類別。 其中一個類別會公開類型為 [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) 的事件，另一個類別則是傳回 JavaScript 做為 TValue 之引數的類型。 除非您完成後續步驟，否則這些類別都無法與 JavaScript 通訊。
-   這個 app 會啟用主要類別物件、呼叫方法，然後處理 Windows 執行階段元件所引發的事件。
-   用來產生 Proxy 和虛設常式類別的工具需要這些項目。
-   然後您會使用 IDL 檔案來產生 Proxy 和虛設常式的 C 原始程式碼。
-   註冊 Proxy 虛設常式物件，讓 COM 執行階段能找到這些物件，並在 app 專案中參考 Proxy 虛設常式 DLL。

## <a name="to-create-the-windows-runtime-component"></a>建立 Windows 執行階段元件

在 Visual Studio 的功能表列上，依序選擇 **\[檔案\] &gt; \[新增專案\]**。 在 **\[新增專案\]** 對話方塊中，展開 **\[JavaScript\] &gt; \[通用 Windows\]**，然後選取 **\[空白應用程式\]**。 將專案命名為 ToasterApplication，然後選擇 **\[確定\]** 按鈕。

將 C# Windows 執行階段元件加入至方案：在 [方案總管] 中開啟方案的捷徑功能表，然後選擇 **\[加入\] &gt; \[新增專案\]**。 展開**Visual C# &gt; Microsoft Store** ，然後選取 [ **Windows 執行階段元件**。 將專案命名為 ToasterComponent，然後選擇 **\[確定\]** 按鈕。 ToasterComponent 將是您在後續步驟中建立之元件的根命名空間。

在 [方案總管] 中，開啟方案的捷徑功能表，然後選擇 **\[屬性\]**。 在 **\[屬性頁\]** 對話方塊的左窗格中選取 **\[組態屬性\]**，然後將對話方塊頂端的 **\[組態\]** 設定為 **\[偵錯\]**，並將 **\[平台\]** 設定為 [x86]、[x64] 或 [ARM]。 選擇 **\[確定\]** 按鈕。

**注意：** 將 [平台] 設定為 [任何 CPU] 將無法運作，因為它並不適用於您稍後加入至方案的機器碼架構 Win32 DLL。

在 [方案總管] 中，將 class1.cs 重新命名為 ToasterComponent.cs，使其符合專案的名稱。 Visual Studio 會自動重新命名檔案中的類別，以符合新的檔案名稱。

在 .cs 檔案中，為 Windows.Foundation 命名空間加上 using 指示詞，以便將 TypedEventHandler 納入範圍。

需要 Proxy 和虛設常式時，您的元件必須使用介面來公開它的公用成員。 在 ToasterComponent.cs 中，分別為快顯通知程式及其產生的 Toast 定義介面。

**注意：** 在 C# 中，您可以略過此步驟， 改為先建立類別，然後開啟其捷徑功能表並選擇 **\[重構\] &gt; \[擷取介面\]**。 在產生的程式碼中，手動為介面提供公用存取範圍。

```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
```

IToast 介面包含可擷取來描述快顯通知類型的字串。 IToaster 介面包含建立快顯通知的方法，以及表示已建立該快顯通知的事件。 由於這個事件會傳回快顯通知的特定部分 (也就是類型)，因此也稱為具類型事件。

接著，我們需要類別來實作這些介面，而且必須是公用和密封類別，方便您之後將進行程式設計的 JavaScript app 進行存取。

```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
```

在上述程式碼中，我們建立了快顯通知，然後備妥執行緒集區工作項目來引發通知。 雖然 IDE 可能會建議您將 await 關鍵字套用至非同步呼叫，但在這種情況下不必這麼做，因為方法不會執行任何取決於作業結果的工作。

**注意：** 上述程式碼中的非同步呼叫會單獨使用 ThreadPool.RunAsync，示範以簡單的方式在背景執行緒上引發事件。 您可以撰寫這個特殊的方法 (如下列範例所示)，因為 .NET 工作排程器會自動將 async/await 呼叫封送處理回 UI 執行緒，因此這個方法可正常運作。
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

如果您現在建置專案時，它應該會完全建置。

## <a name="to-program-the-javascript-app"></a>計畫的 JavaScript app

現在我們可以將按鈕新增至 JavaScript 應用程式會使用我們只是定義讓快顯通知的類別。 我們執行此動作之前，我們必須新增到我們剛建立的命名為 ToasterComponent 專案的參考。 在 [方案總管] 中開啟命名為 ToasterApplication 專案的捷徑功能表，選擇**新增&gt;參考**，然後選擇 [**加入新參考]** 按鈕。 在 [加入參考] 對話方塊中，在方案下的左窗格中選取元件專案，然後在中間窗格中，選取命名為 ToasterComponent。 選擇 **\[確定\]** 按鈕。

在方案總管] 中開啟命名為 ToasterApplication 專案的捷徑功能表，然後選擇**設定為啟始專案**。

在 default.js 檔案的結尾，新增命名空間包含可呼叫元件，其回呼叫的函式。 命名空間將會有兩個函式，一個讓快顯通知，另一個處理快顯通知完成事件。 MakeToast 的實作會建立分別物件，註冊事件處理常式中，並讓快顯通知。 到目前為止，事件處理常式不會執行太多，如下所示：

```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
```

MakeToast 函式必須設定好的按鈕。 更新 default.html 来包含按鈕，以及一些空間輸出讓快顯通知的結果：

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

如果我們無法使用 TypedEventHandler，我們現在能夠在本機電腦上執行的應用程式，並按一下按鈕讓快顯通知。 但我們的應用程式中沒有任何作用。 若要了解為何，讓我們來偵錯 ToastCompletedEvent，就會引發的 managed 程式碼。 停止專案，然後在功能表列上，選擇**偵錯&gt;分別應用程式內容**。 將**偵錯工具類型**變更為**僅限管理**。 一次的功能表列上，選擇**偵錯&gt;例外狀況**，然後選取 [**通用語言執行階段例外狀況**。

立即執行應用程式，然後按一下讓快顯通知按鈕。 偵錯工具會攔截例外狀況無效的轉型。 雖然您不能從其訊息，則這個例外狀況的發生原因 proxy 是針對該介面遺失。

![遺失的 proxy](./images/debuggererrormissingproxy.png)

建立 proxy 和虛設常式元件的第一個步驟是要新增的唯一識別碼或 GUID 的介面。 不過，要使用的 GUID 格式不同取決於您正在撰寫在 C#、 Visual Basic 中或另一個.NET 語言，或 c + + 中。

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>若要針對元件的介面 （C# 和其他.NET 語言） 產生 Guid

在功能表列上，選擇 [工具&gt;建立 GUID。 在對話方塊中，選取 5。 \[Guid (」...儲存-xxxx xxxx) \]。 選擇 [新的 GUID] 按鈕，然後選擇 [複製] 按鈕。

![guid 產生器工具](./images/guidgeneratortool.png)

返回介面定義，然後再將新的 GUID 貼之前 IToaster 介面，如下列範例所示。 （不在範例中使用的 GUID。 每個唯一的介面應該擁有它自己的 GUID）。

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

新增 using 指示詞 System.Runtime.InteropServices 命名空間。

IToast 介面重複這些步驟。

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>若要產生的 Guid 元件的介面 （c + +）

在功能表列上，選擇 [工具&gt;建立 GUID。 在對話方塊中，選取 3。 靜態 const 結構 GUID = {...}。 選擇 [新的 GUID] 按鈕，然後選擇 [複製] 按鈕。

貼上之前 IToaster 介面定義的 GUID。 貼上之後，該 GUID 應該類似下列的範例。 （不在範例中使用的 GUID。 每個唯一的介面應該擁有它自己的 GUID）。
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
新增 using 指示詞 Windows.Foundation.Metadata，以便將 GuidAttribute 納入範圍。

現在將手動轉換 const GUID 為 GuidAttribute，讓它格式如下列範例所示。 請注意，大括號括號與括號內，就會取代並移除尾端分號。
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
IToast 介面重複這些步驟。

既然介面的唯一識別碼，我們可以建立 IDL 檔案送入.winmd 檔案到 winmdidl 命令列工具，並提供該 IDL 檔案到 MIDL 命令列工具，以產生 proxy 和虛設常式的 C 原始程式碼。 Visual Studio 這麼做為我們如果我們建立建置後事件，如下列步驟中所示。

## <a name="to-generate-the-proxy-and-stub-source-code"></a>產生 proxy 和虛設常式原始碼

若要新增自訂的建置後事件，在 [方案總管] 中，開啟命名為 ToasterComponent 專案的捷徑功能表，然後選擇 [屬性]。 在左窗格中的屬性頁面，選取 Build Events，，然後選擇 [建置後編輯按鈕。 將下列命令新增到建置後命令列。 （批次檔案必須被稱為第一次設定環境變數，以尋找 winmdidl 工具）。

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**重要** 適用於 ARM 或 x64 專案設定，將 MIDL /env 參數變更為 x64 或 arm32。

若要確保每次.winmd 檔案變更時，會重新產生 IDL 檔案，變更**執行建置後事件**至**當組建更新專案輸出。**
建置事件的屬性頁面上看起來應該像這樣：![建置事件](./images/buildevents.png)

重新建置產生及編譯 IDL 解決方案。

您可以確認 MIDL 正確藉由尋找 ToasterComponent.h、 ToasterComponent_i.c、 ToasterComponent_p.c，以及 dlldata.c 命名為 ToasterComponent 專案目錄中的編譯方案。

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>編譯 proxy 和虛設常式程式碼到 DLL

現在，您擁有必要的檔案，您可以將它們產生的 DLL，也就是 c + + 檔案編譯。 這相當簡單，只要可能，請新增新的專案，以支援建置 proxy。 開啟命名為 ToasterApplication 方案的捷徑功能表，然後選擇**新增 > 新專案**。 在 [**新增專案**] 對話方塊的左窗格中，依序展開**Visual c + + &gt; Windows&gt;通用 Windows**，然後在中間窗格中，選取 [ **DLL （UWP 應用程式）**。 （請注意這不是 c + + Windows 執行階段元件專案）。專案 Proxy 的名稱，然後選擇 [**確定**] 按鈕。 在 C# 類別中的項目變更時的建置後事件會更新這些檔案。

根據預設，Proxy 專案會產生標頭.h 檔案和 c + +.cpp 檔案。 因為建置 DLL 的從 MIDL 從產生的檔案，則不需要.h 和.cpp 檔案。 在 [方案總管] 中，開啟捷徑功能表，選擇**移除**，並確認刪除。

現在，在專案是空的您可以新增返回 MIDL 產生的檔案。 開啟 Proxy 專案的捷徑功能表，然後選擇**新增 > 現有項目。** 在對話方塊中，瀏覽至命名為 ToasterComponent 專案目錄，然後選取這些檔案： ToasterComponent.h、 ToasterComponent_i.c、 ToasterComponent_p.c，以及 dlldata.c 檔案。 選擇 [**加入**] 按鈕。

在 Proxy 專案中，建立.def 檔案，以定義 dlldata.c 中所述的 DLL 匯出。 開啟專案的捷徑功能表，然後選擇**新增 > 新項目**。 在對話方塊的左窗格中，選取的程式碼，然後在中間窗格中，選取 [模組定義檔案。 命名檔案 proxies.def，然後選擇 [**新增**\] 按鈕。 開啟此.def 檔案，並修改它以包含匯出 dlldata.c 中定義：

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

如果您現在建置專案時，它將會失敗。 若要正確編譯這個專案，您必須變更如何在專案是以編譯和連結。 在方案總管] 中開啟 Proxy 專案的捷徑功能表，然後選擇**屬性**。 變更的屬性頁面，如下所示。

在左窗格中，選取**C/c + + > 前置處理器**，並在右窗格中，然後選取**前置處理器定義**、 選擇向下箭號按鈕，以及然後選取 [**編輯**。 在方塊中新增這些定義：

```cpp
WIN32;_WINDOWS
```
在**C/c + + > 先行編譯標頭**，將**先行編譯標頭**變更為**未使用先行編譯標頭檔**，然後選擇 [**套用**] 按鈕。

在**連結器 > 一般**、 **Ar-ye**s、 變更**忽略匯入程式庫**，然後選擇 [**套用**] 按鈕。

在**連結器 > 輸入**，選取**其他相依性**，選擇 [向下鍵] 按鈕，然後選取 [**編輯**。 在方塊中，新增此文字：

```cpp
rpcrt4.lib;runtimeobject.lib
```

不貼上這些程式庫直接將清單列。 使用**編輯**方塊，以確保在 Visual Studio 中的 MSBuild 會維持正確的其他相依性。

當您擁有這些變更，請**屬性頁**] 對話方塊中選擇 [**確定**] 按鈕。

接下來，採取命名為 ToasterComponent 專案的相依性。 這樣可確保分別會建置之前 proxy 專案會建置。 這是必要的因為分別專案負責產生建置 proxy 的檔案。

開啟 Proxy 專案的捷徑功能表，然後選擇 [專案相依性。 選取核取方塊，以指出 Proxy 專案，取決於命名為 ToasterComponent 專案中，以確保，Visual Studio 就會將它們組建以正確的順序。

確認 [方案選擇正確建置**建置 > 重新建置方案**Visual Studio 功能表列上。


## <a name="to-register-the-proxy-and-stub"></a>若要登錄 proxy 和虛設常式

在命名為 ToasterApplication 專案中，開啟 package.appxmanifest 的捷徑功能表，然後選擇 [**開啟方式**。 在 [開啟檔案] 對話方塊中，選取 [ **XML 文字編輯器**，然後選擇 [**確定**] 按鈕。 我們將在 windows.activatableClass.proxyStub 擴充功能註冊，並可根據 proxy 中的 Guid 會提供一些 XML 中貼上。 若要尋找的 Guid，用於.appxmanifest 檔案中，開啟 ToasterComponent_i.c。 尋找類似下列的範例中的項目。 也請注意的 IToast，IToaster，定義和第三個介面，有兩個參數的輸入的事件處理常式： 分別與快顯通知。 這符合分別類別中定義的事件。 請注意 IToast 和 IToaster Guid 符合在 C# 檔案中的介面中定義的 Guid。 因為具類型的事件處理常式介面是自動產生，此介面的 GUID 也是自動產生。

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

現在我們要將複製的 Guid，將它們貼在 package.appxmanifest 中，我們將新增節點和名稱擴充功能，然後再將它們重新格式化。 資訊清單項目類似下列的範例 — 但同樣地，請記得要使用您自己的 Guid。 請注意，在 XML 中 ClassId GUID ITypedEventHandler2 相同。 這是因為該 GUID 是第一個 ToasterComponent_i.c 中所列。 以下的 Guid 不區分大小寫。 而不是 IToast 和 IToaster，手動重新格式化的 Guid，您可以返回介面定義，並取得 GuidAttribute 值，其中有正確的格式。 在 c + +，有在註解是格式正確的 GUID。 在任何情況下，您必須手動重新格式化用於 ClassId 和事件處理常式的 GUID。

```cpp
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
```

貼上的延伸 XML 節點，做為子系的 [套件] 節點中，並為例，資源節點的對等。

然後再移，請務必確定：

-   ProxyStub ClassId ToasterComponent\_i.c 檔案中設定為第一次的 GUID。 使用第一個 classId 這個檔案中定義的 GUID。 （這可能是相同的 GUID，ITypedEventHandler2。）
-   路徑是二進位 proxy 的套件相對路徑。 （在本逐步解說，proxies.dll 是 ToasterApplication.winmd 相同的資料夾中）。
-   Guid 為正確的格式。 （這是容易就能取得錯誤）。
-   在資訊清單中的介面識別碼符合 Iid ToasterComponent\_i.c 檔案中。
-   介面名稱是在資訊清單中唯一的。 由於這些不由系統所用，您可以選擇的值。 最好選擇清楚符合您所定義的介面的介面名稱。 產生的介面，名稱應該要產生的介面表示。 您可以使用 ToasterComponent\_i.c 檔案，可協助您產生介面名稱。

如果您嘗試現在執行方案，您會收到錯誤，proxies.dll 不是裝載的一部分。 命名為 ToasterApplication 專案中開啟 [**參考**] 資料夾的捷徑功能表，然後選擇 [**加入參考**。 選取 Proxy 專案旁邊的核取方塊。 此外，請確定已也選取命名為 ToasterComponent 旁邊的核取方塊。 選擇 **\[確定\]** 按鈕。

現在應該建置專案。 執行專案，並確認您可以讓快顯通知。

## <a name="related-topics"></a>相關主題

* [在 C++ 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md)
