---
title: 在 Windows 執行階段元件中引發事件
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 了解如何在背景執行緒中引發使用者定義委派型別的事件，使 JavaScript 能接收到該事件。
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5b5678ad1a0666e6f008a2ec69ba63c35441edf
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493513"
---
# <a name="raising-events-in-windows-runtime-components"></a>在 Windows 執行階段元件中引發事件

> [!NOTE]
> 如需有關如何在[c + +/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Windows 執行階段元件中引發事件的詳細資訊，請參閱[在 c + + 中撰寫事件/WinRT](/windows/uwp/cpp-and-winrt-apis/author-events)。

如果您的 Windows 執行階段元件會在背景執行緒（工作者執行緒）上引發使用者定義之委派類型的事件，而且您希望 JavaScript 能夠接收事件，則可以使用任何一種方法來執行和/或引發它。

-   （選項1）透過[**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher)引發事件，將事件封送處理至 JavaScript 執行緒內容。 雖然這通常是最佳選擇，但在某些情況下卻無法提供最佳效能。
-   （選項2）請使用** [EventHandler](/uwp/api/windows.foundation.eventhandler-1) \<Object\> ** （但遺失事件種類資訊）。 如果選項1不可行，或其效能不足，則這是很好的第二個選擇，因為可以接受遺失類型資訊。 如果您要撰寫 c # Windows 執行階段元件，則無法使用**EventHandler \<Object\> **類型; 相反地，該類型會投射至[**EventHandler**](/dotnet/api/system.eventhandler)，因此您應該改為使用它。
-   (選項 3) 針對元件建立您自己的 Proxy 和虛設常式。 這是最難實作的選項，但對於有需要的案例來說，與選項 1 相比，這個選項不僅可以保留類型資訊，可能還會提供更佳的效能。

如果您僅在背景執行緒上引發事件，卻沒有使用上述任何選項，JavaScript 用戶端就不會接收事件。

## <a name="background"></a>背景

所有 Windows 執行階段元件和 app 基本上都是 COM 物件，無論您使用哪種語言來建立它們都一樣。 在 Windows API 中，大部分元件都是敏捷式 COM 物件，這些元件與在背景執行緒上及 UI 執行緒上的物件通訊的能力都相同。 如果 COM 物件無法成為敏捷式物件，則它需要稱為 Proxy 和虛設常式的協助程式物件，協助跨越 UI 執行緒背景的執行緒界限，才能與其他 COM 物件通訊 (從 COM 方面來說，這稱為執行緒 Apartment 之間的通訊)。

Windows API 中的大部分物件都是敏捷式物件或內建 Proxy 和虛設常式。 不過，您無法為泛型類型 (例如 Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler)) 建立 Proxy 和虛設常式，因為除非提供類型引數，否則它們都不是完整類型。 JavaScript 用戶端只有在缺少 Proxy 或虛設常式時才會有問題，但若您希望自己的元件在 JavaScript 以及 C++ 或 .NET 語言中都能使用，就必須使用下列其中一個選項。

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(選項 1) 透過 CoreDispatcher 引發事件

您可以使用 [Windows.UI.Core.CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)，傳送具有任何使用者定義之委派類型的事件，如此 JavaScript 就可以接收這些事件。 如果不確定要使用哪個選項，請先嘗試這一個。 如果事件引發和事件處理之間的延遲情況造成問題，請嘗試其他兩個選項。

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

> [!NOTE]
> 如果您要撰寫 c # Windows 執行階段元件，則無法使用**EventHandler \<Object\> **類型; 相反地，該類型會投射至[**EventHandler**](/dotnet/api/system.eventhandler)，因此您應該改為使用它。

另一種從背景執行緒傳送事件的方式，是使用[EventHandler](/uwp/api/windows.foundation.eventhandler) &lt; 物件 &gt; 做為事件的型別。 Windows 會提供這個泛型類型的具象安裝，並為其提供 Proxy 和虛設常式。 缺點是事件引數的類型資訊和傳送者都會遺失。 C++ 和 .NET 用戶端必須透過文件來得知收到事件時要轉換回的類型。 JavaScript 用戶端不需要原始類型資訊， 而是會根據其在中繼資料內的名稱來尋找 arg 屬性。

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

本逐步解說包含這些部分。

-   您將在此處建立兩個基本的 Windows 執行階段類別。 其中一個類別會公開類型為 [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler-2) 的事件，另一個類別則是傳回 JavaScript 做為 TValue 之引數的類型。 除非您完成後續步驟，否則這些類別都無法與 JavaScript 通訊。
-   這個 app 會啟用主要類別物件、呼叫方法，然後處理 Windows 執行階段元件所引發的事件。
-   用來產生 Proxy 和虛設常式類別的工具需要這些項目。
-   然後您會使用 IDL 檔案來產生 Proxy 和虛設常式的 C 原始程式碼。
-   註冊 Proxy 虛設常式物件，讓 COM 執行階段能找到這些物件，並在 app 專案中參考 Proxy 虛設常式 DLL。

## <a name="to-create-the-windows-runtime-component"></a>建立 Windows 執行階段元件

在 Visual Studio 的功能表列上，選擇 [檔案] [新增] [ ** &gt; 專案**]。 在 [新增專案]**** 對話方塊中，展開 [JavaScript] &gt; [通用 Windows]****，然後選取 [空白應用程式]****。 將專案命名為 ToasterApplication，然後選擇 [確定]**** 按鈕。

將 C# Windows 執行階段元件加入至方案：在 [方案總管] 中開啟方案的捷徑功能表，然後選擇 [加入] &gt; [新增專案]****。 展開 [ **Visual c #] &gt; Microsoft Store**然後選取 [ **Windows 執行階段元件**]。 將專案命名為 ToasterComponent，然後選擇 [確定]**** 按鈕。 ToasterComponent 將是您在後續步驟中建立之元件的根命名空間。

在 [方案總管] 中，開啟方案的捷徑功能表，然後選擇 [屬性]****。 在 [屬性頁]**** 對話方塊的左窗格中選取 [組態屬性]****，然後將對話方塊頂端的 [組態]**** 設定為 [偵錯]****，並將 [平台]**** 設定為 [x86]、[x64] 或 [ARM]。 選擇 [確定] **** 按鈕。

**重要事項**  平臺 = 任何 CPU 都無法使用，因為它對您稍後將加入至方案的機器碼 Win32 DLL 無效。

在 [方案總管] 中，將 class1.cs 重新命名為 ToasterComponent.cs，使其符合專案的名稱。 Visual Studio 會自動重新命名檔案中的類別，以符合新的檔案名稱。

在 .cs 檔案中，為 Windows.Foundation 命名空間加上 using 指示詞，以便將 TypedEventHandler 納入範圍。

需要 Proxy 和虛設常式時，您的元件必須使用介面來公開它的公用成員。 在 ToasterComponent.cs 中，分別為快顯通知程式及其產生的 Toast 定義介面。

**注意**  在 c # 中，您可以略過此步驟。 改為先建立類別，然後開啟其捷徑功能表並選擇 [重構] &gt; [擷取介面]****。 在產生的程式碼中，手動為介面提供公用存取範圍。

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

**注意**  上述程式碼中的非同步呼叫會使用 RunAsync，只是為了示範在背景執行緒上引發事件的簡單方式。 您可以撰寫這個特殊的方法 (如下列範例所示)，因為 .NET 工作排程器會自動將 async/await 呼叫封送處理回 UI 執行緒，因此這個方法可正常運作。
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

若您現在建置專案，專案應該會完全建置。

## <a name="to-program-the-javascript-app"></a>撰寫 JavaScript 應用程式

現在，我們可以將按鈕新增至 JavaScript 應用程式，使其利用我們方才定義的類別建立快顯通知。 在我們繼續之前，我們必須先新增一個我們建立的 ToasterComponent 專案參考。 在 \[方案總管\] 中，開啟 ToasterApplication 專案的捷徑功能表，選擇 **\[新增\] &gt; \[參考\]**，然後選擇 **\[加入參考\]** 按鈕。 在 \[加入參考\] 對話方塊中，在左側窗格的 \[方案\] 底下，選取元件專案，然後在中央窗格中選取 \[ToasterComponent\]。 選擇 [確定] **** 按鈕。

在 \[方案總管\] 中，開啟 ToasterApplication 專案捷徑功能表，然後選擇 **\[設定為啟始專案\]**。

在 default.js 檔案的結尾，新增一個命名空間，以包含呼叫元件，並且可讓元件回呼的函式。 命名空間會有兩個函式：一個負責建立快顯通知，另一個負責處理快顯通知完成 (toast-complete) 事件。 實作 makeToast 會建立一個 Toaster 物件，註冊事件處理常式，並建立快顯通知。 到目前為止，事件處理常式並沒有完成很多工作，如下所示︰

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

makeToast 函式必須連結至一個按鈕。 更新 default.html 以增加一個按鈕和輸出快顯通知的空間：

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

若我們沒有使用 TypedEventHandler，我們現在應該就可以在本機電腦上執行應用程式，並按一下按鈕以建立快顯通知。 但在我們的應用程式中，卻沒有發生任何事情。 若要找出原因，我們必須針對引發 ToastCompletedEvent 的 Managed 程式碼進行偵錯。 停止專案，然後在功能表列中，選擇 **\[偵錯\] &gt; Toaster 應用程式屬性**。 將 **\[偵錯工具類型\]** 變更為 **\[只針對 Managed 程式碼\]**。 再次在功能表列中，選擇 **\[偵錯\] &gt; \[例外狀況\]**，然後選取 **\[通用語言執行平台例外狀況\]**。

現在執行應用程式，然後按一下 make-toast 按鈕。 偵錯工具會捕捉到類型轉換例外狀況。 雖然從訊息中看起來並不明顯，但此例外狀況是由於該介面遺漏所需要的 Proxy 而導致的。

![遺漏 Proxy](./images/debuggererrormissingproxy.png)

為元件建立 Proxy 和虛設常式的第一步，就是為介面新增一個唯一的識別碼或 GUID。 不過，隨著您用來撰寫程式之語言的不同 (例如：C#、Visual Basic、其他 .NET 程式語言，或是 C++)，其所使用的 GUID 格式也會有所不同。

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>為元件的介面產生 GUID (C# 及其他 .NET 程式語言)

在功能表列上，依序選擇 \[工具\] &gt; \[建立 GUID\]。 在對話方塊中，選取 5. \[Guid （"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx .。。xxxx "） \] 。 選擇 \[新的 GUID\] 按鈕，然後選擇 \[複製\] 按鈕。

![GUID 產生器工具](./images/guidgeneratortool.png)

返回介面定義，並在 IToaster 介面之前貼上新的 GUID，如以下範例所示。 (請不要使用範例中的 GUID。 每個唯一的介面都應該要有其專屬的 GUID。)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

為 System.Runtime.InteropServices 命名空間添加 using 指示詞。

針對 IToast 介面重複這些步驟。

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>為元件的介面產生 GUID (C++)

在功能表列上，依序選擇 \[工具\] &gt; \[建立 GUID\]。 在對話方塊中，選取 3. static const struct GUID = {...}。 選擇 \[新的 GUID\] 按鈕，然後選擇 \[複製\] 按鈕。

在 IToaster 介面定義之前貼上 GUID。 貼上之後，GUID 應該會與以下範例相似。 (請不要使用範例中的 GUID。 每個唯一的介面都應該要有其專屬的 GUID。)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
為 Windows.Foundation.Metadata 添加 using 指示詞，已將 GuidAttribute 新增至範圍之中。

以手動方式將 const GUID 轉換為 GuidAttribute 並使其格式符合以下範例。 請注意大括弧已由中括弧和括弧取代，並且其後置的分號已移除。
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
針對 IToast 介面重複這些步驟。

現在介面都已有唯一的識別碼了，我們就可以藉由提供 .winmd 檔案給 winmdidl 命令列工具產生 IDL 檔案，然後再藉由提供該 IDL 檔案給 MIDL 命令列工具為 Proxy 和虛設常式產生 C 原始程式碼。 若我們依照下列範例建立 post-build 事件，則 Visual Studio 就會為我們完成這項工作。

## <a name="to-generate-the-proxy-and-stub-source-code"></a>產生 Proxy 及虛設常式原始程式碼

若要新增自訂 post-build 事件，請在 \[方案總管\] 中，開啟 ToasterComponent 專案的捷徑功能表，然後選擇 \[屬性\]。 在屬性頁面的左側窗格中，選取 \[建置事件\]，然後選擇 \[編輯 Post-build\] 按鈕。 將下列命令新增至 post-build 命令列。 (您必須先呼叫批次檔案設定環境變數，才能找到 winmdidl 工具。)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**重要事項**   若為 ARM 或 x64 專案設定，請將 MIDL/env 參數變更為 x64 或 arm32。

若要確保每次 .winmd 檔案變更時都會重新產生 IDL 檔案，請將 **\[執行 post-build 事件\]** 變更為 **\[當組建更新專案輸出時\]**
[組建事件] 屬性頁應該與下列類似： ![ 組建事件](./images/buildevents.png)

重建方案以產生並編譯 IDL。

您可以藉由在 ToasterComponent 專案目錄中尋找 ToasterComponent.h、ToasterComponent_i.c、ToasterComponent_p.c，以及 dlldata.c 來驗證 MIDL 已正確編譯方案。

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>將 Proxy 和虛設常式程式碼編譯為 DLL

您現在已經擁有所需的所有檔案，接著您就可以將他們編譯為一個由 C++ 構成的 DLL 檔案。 若要以最簡單的方式完成這項工作，請新增一個專案以支援建置 Proxy。 開啟 ToasterApplication 方案的捷徑功能表，然後依序選擇 **\[加入\] > \[新增專案\]**。 在 [**新增專案**] 對話方塊的左窗格中，展開 [ **Visual C++ &gt; windows &gt; 通用視窗**]，然後在中間窗格中選取 **[DLL （UWP 應用程式）**]。 (請注意這不是 C++ Windows 執行階段元件專案。) 將專案名稱命名為「Proxies」，然後選擇 **\[確定\]** 按鈕。 當 C# 類別發生變更時，這些檔案會由 post-build 事件進行更新。

根據預設值，Proxy 專案會產生標頭 .h 檔案以及 C++ .cpp 檔案。 由於 DLL 是利用 MIDL 產生的檔案進行建置的，因此不需要 .h 和 .cpp 檔案。 在 \[方案總管\] 中，開啟他們的捷徑功能表，選擇 **\[移除\]**，然後確認刪除。

專案清空後，現在您就可以將 MIDL 產生的檔案加回。 開啟 Proxy 專案的捷徑功能表，然後選擇 **\[新增\] > \[現有的項目\]。** 在對話方塊中，移至 ToasterComponent 專案目錄並選取這些檔案︰ToasterComponent.h、ToasterComponent_i.c、ToasterComponent_p.c，以及 dlldata.c 檔案。 選擇 [新增] 按鈕。

在 Proxy 專案中，建立一個 .def 檔案來定義 dlldata.c 中所描述的 DLL 匯出檔。 開啟專案的捷徑功能表，然後依序選擇 **\[加入\] > \[新增項目\]**。 在對話方塊的左側窗格中，選取 \[程式碼\]，然後在中央窗格中，選取 \[模組定義檔案\]。 將檔案命名為 proxies.def，然後選擇 **\[新增\]** 按鈕。 開啟此 .def 檔案，並修改為包含了 dlldata.c 中所定義的 EXPORTS︰

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

若您現在就建置專案，建置工作將會失敗。 若要正確編譯此專案，您必須變更專案編譯和連結的方式。 在 \[方案總管\] 中，開啟 Proxies 方案的捷徑功能表，然後選擇 **\[屬性\]**。 將屬性頁面變更如下。

在左側窗格中，選取  **\[C/C++\] > \[前置處理器\]**，然後在右側窗格中，選取 **\[前置處理器定義\]**，選擇向下箭號按鈕，然後再次選取 **\[編輯\]**。 在方塊中新增這些定義：

```cpp
WIN32;_WINDOWS
```
在 **\[C/C++\] > \[先行編譯標頭檔\]** 底下，將 **\[先行編譯標頭檔\]** 變更為 **\[不使用先行編譯標頭檔\]**，然後選擇 **\[套用\]** 按鈕。

在 **\[連結器\] > \[一般\]** 底下，將 **\[忽略匯入程式庫\]** 變更為 **\[是\]**，然後選擇 **\[套用\]** 按鈕。

在 **\[連結器\] > \[輸入\]** 底下，選取** \[其他相依性\]**，選擇向下箭號按鈕，然後選取 **\[編輯\]**。 在方塊中加入此文字︰

```cpp
rpcrt4.lib;runtimeobject.lib
```

請不要直接在清單資料列中貼上這些程式庫。 使用 **\[編輯\]** 對話方塊確保 Visual Studio 中的 MSBuild 保持其他相依性的正確。

當您完成這些變更之後，請在 **\[屬性頁面\]** 對話方塊中選擇 **\[確定\]** 按鈕。

接下來，在 ToasterComponent 專案中選擇一個相依性。 這樣可確保 Toaster 在 Proxy 專案建置之前就會進行建置。 這是必要的一個步驟，因為 Toaster 專案會負責產生建置 Proxy 所需要的檔案。

開啟 Proxy 專案的捷徑功能表，然後選擇 \[專案相依性\]。 選取核取方塊，表示 Proxy 專案依存於 ToasterComponent 專案，以確保 Visual Studio 會以正確的順序建置他們。

透過選擇 Visual Studio 功能表列上的 **\[組建\] > \[重建方案\]** 以驗證方案可正確建置。


## <a name="to-register-the-proxy-and-stub"></a>註冊 Proxy 和虛設常式

在 ToasterApplication 專案中，請開啟 package.appxmanifest 的捷徑功能表，然後選擇 **\[以...開啟\]**。 在 \[開啟\] 對話方塊中，選取 **\[XML 文字編輯器\]**，然後選擇 **\[確定\]** 按鈕。 我們將會貼上一些 XML，以提供 windows.activatableClass.proxyStub 延伸模組註冊和以 Proxy 中的 GUID 為基礎的東西。 若要尋找在 .appxmanifest 檔案中使用的 GUID，請開啟 ToasterComponent_i.c。 尋找與下列範例相似的項目。 請注意 IToast、IToaster，和第三個介面 (一個具類型，並包含兩個參數︰Toaster 和 Toast 的事件處理常式) 的定義。 這會與 Toaster 類別中所定義的事件相符。 請注意 IToast 和 IToaster 所使用的 GUID 皆符合 C# 檔案中介面上定義的 GUID。 由於具類別的事件處理常式是自動產生的，因此該介面的 GUID 也會自動產生。

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

現在我們可以複製 GUID，在 package.appxmanifest 中我們新增和命名延伸模組的節點中將他們貼上，再把他們重新格式化。 資訊清單項目會與下列範例相似，但請記得：務必要使用您自己的 GUID。 請注意在 XML 中的 ClassId GUID 與 ITypedEventHandler2 中的是一樣的。 這是因為該 GUID 是在 ToasterComponent_i.c 中第一個列出的 GUID。 以下 GUID 不會區分大小寫。 相較於手動將 IToast 和 IToaster 的 GUID 重新格式化，您可以返回介面定義，並取得已經是正確格式的 GuidAttribute 值。 在 C++ 中，註解裡面會有一個已正確格式化的 GUID。 無論如何，您都必須手動將 ClassId 和事件處理常式使用的 GUID 重新格式化。

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

將 Extensions XML 節點貼上並作為 Package 節點的直接子項和其他節點的同儕節點 (例如 Resources 節點等)。

在繼續下一步前，請務必確定︰

-   ProxyStub 的 ClassId 會設定為 ToasterComponent i. c 檔案中的第一個 GUID \_ 。 使用這個檔案中定義的第一個 GUID 作為 classId 使用。 (這可能會跟 ITypedEventHandler2 的 GUID 一樣。)
-   Path 已設定為 Proxy 二進位檔案的相對路徑。 (在此逐步解說中，proxies.dll 位於與 ToasterApplication.winmd 相同的資料夾中。)
-   GUID 已設定為正確的格式。 (這部分很容易發生錯誤。)
-   資訊清單中的介面識別碼符合 ToasterComponent i. c 檔案中的 Iid \_ 。
-   資訊清單中的介面名稱都是唯一的。 由於系統並不會使用到這些，您可以選擇使用數值來命名。 通常選擇與您定義的介面明確相符的介面名稱是一個很好的作法。 針對產生的介面，名稱應該要能夠指示產生的介面。 您可以使用 ToasterComponent \_ i. c 檔案來協助您產生介面名稱。

若您嘗試現在就執行方案，您將會收到一則錯誤，告知您 proxies.dll 並非承載的一部分。 在 ToasterApplication 專案中開啟 **\[參考\]** 資料夾的捷徑功能表，然後選擇 **\[加入參考\]**。 選取 Proxies 專案旁的核取方塊。 同時，請確定 ToasterComponent 旁邊的核取方塊也已選取。 選擇 [確定] **** 按鈕。

現在您應該已可以正常建置專案。 執行專案並驗證您確實可以建立快顯通知。

## <a name="related-topics"></a>相關主題

* [Windows 執行階段元件與 C++/CX](creating-windows-runtime-components-in-cpp.md)
