---
author: martinekuan
title: 在 Windows 執行階段元件中引發事件
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 
---

# 在 Windows 執行階段元件中引發事件


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

如果您的 Windows 執行階段元件會在背景執行緒 (背景工作執行緒) 上引發使用者定義之委派類型的事件，而您希望 JavaScript 可以接收該事件，則可透過下列其中一種方式來實作和 (或) 引發此事件：

-   (選項 1) 透過 [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) 引發事件，將事件封送處理至 JavaScript 執行緒內容。 雖然這通常是最佳選擇，但在某些情況下卻無法提供最佳效能。
-   (選項 2) 使用 [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt;，不過會遺失類型資訊 (但會遺失事件類型資訊)。 如果選項 1 不可行或其效能不足，而且您可以接受遺失類型資訊的缺點，那麼選項 2 也是不錯的選擇。
-   (選項 3) 針對元件建立您自己的 Proxy 和虛設常式。 這是最難實作的選項，但對於有需要的案例來說，與選項 1 相比，這個選項不僅可以保留類型資訊，可能還會提供更佳的效能。

如果您僅在背景執行緒上引發事件，卻沒有使用上述任何選項，JavaScript 用戶端就不會接收事件。

## 背景


所有 Windows 執行階段元件和 app 基本上都是 COM 物件，無論您使用哪種語言來建立它們都一樣。 在 Windows API 中，大部分元件都是敏捷式 COM 物件，這些元件與在背景執行緒上及 UI 執行緒上的物件通訊的能力都相同。 如果 COM 物件無法成為敏捷式物件，則它需要稱為 Proxy 和虛設常式的協助程式物件，協助跨越 UI 執行緒背景的執行緒界限，才能與其他 COM 物件通訊 (從 COM 方面來說，這稱為執行緒 Apartment 之間的通訊)。

Windows API 中的大部分物件都是敏捷式物件或內建 Proxy 和虛設常式。 不過，您無法為泛型類型 (例如 Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx)) 建立 Proxy 和虛設常式，因為除非提供類型引數，否則它們都不是完整類型。 JavaScript 用戶端只有在缺少 Proxy 或虛設常式時才會有問題，但若您希望自己的元件在 JavaScript 以及 C++ 或 .NET 語言中都能使用，就必須使用下列其中一個選項。

## (選項 1) 透過 CoreDispatcher 引發事件


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

## (選項 2) 使用 EventHandler&lt;Object&gt;，但會遺失類型資訊


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

## (選項 3) 建立您自己的 Proxy 和虛設常式。


如需激發使用者定義之事件類型的潛在效能並完整保留類型資訊，您必須建立自己的 Proxy 和虛設常式物件，並將其內嵌於 app 封裝中。 通常，只有另外兩個選項不適用時才必須使用這個選項，但這是極少見的情況。 此外，這個選項也不保證能提供比其他兩個選項更佳的效能。 實際效能取決於諸多因素。 請使用 Visual Studio 分析工具或其他程式碼剖析工具來測量應用程式中的實際效能，並判斷事件是否確實為效能瓶頸。

本文的剩餘部分說明如何使用 C# 來建立基本 Windows 執行階段元件，然後使用 C++ 來為 Proxy 和虛設常式建立 DLL，讓 JavaScript 能夠取用非同步作業中元件所引發的 Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; 事件。 (您也可以使用 C++ 或 Visual Basic 來建立元件。 建立 Proxy 和虛設常式的相關步驟都一樣)。這個逐步解說會以＜建立 Windows 執行階段同處理序元件範例 (C++/CX)＞為基礎，協助說明其用途。

這個逐步解說包含下列部分：

-   您將在此處建立兩個基本的 Windows 執行階段類別。 其中一個類別會公開類型為 [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) 的事件，另一個類別則是傳回 JavaScript 做為 TValue 之引數的類型。 除非您完成後續步驟，否則這些類別都無法與 JavaScript 通訊。
-   這個 app 會啟用主要類別物件、呼叫方法，然後處理 Windows 執行階段元件所引發的事件。
-   用來產生 Proxy 和虛設常式類別的工具需要這些項目。
-   然後您會使用 IDL 檔案來產生 Proxy 和虛設常式的 C 原始程式碼。
-   註冊 Proxy 虛設常式物件，讓 COM 執行階段能找到這些物件，並在 app 專案中參考 Proxy 虛設常式 DLL。

## 建立 Windows 執行階段元件

1.  在 Visual Studio 的功能表列上，選擇 [檔案] &gt; [新增專案]****。 在 [新增專案]**** 對話方塊中，展開 [JavaScript] &gt; [通用 Windows]****，然後選取 [空白應用程式]****。 將專案命名為 ToasterApplication，然後選擇 [確定]**** 按鈕。
2.  將 C# Windows 執行階段元件加入至方案：在 [方案總管] 中開啟方案的捷徑功能表，然後選擇 [加入] &gt; [新增專案]****。 展開 [Visual C#] &gt; [Windows 市集]****，然後選取 [Windows 執行階段元件]****。 將專案命名為 ToasterComponent，然後選擇 [確定]**** 按鈕。 ToasterComponent 將是您在後續步驟中建立之元件的根命名空間。

    在 [方案總管] 中，開啟方案的捷徑功能表，然後選擇 [屬性]****。 在 [屬性頁]**** 對話方塊的左窗格中選取 [組態屬性]****，然後將對話方塊頂端的 [組態]**** 設定為 [偵錯]****，並將 [平台]**** 設定為 [x86]、[x64] 或 [ARM]。 選擇 [確定]**** 按鈕。

    > **注意：**將 [平台] 設定為 [任何 CPU] 將無法運作，因為它並不適用於您稍後加入至方案的機器碼架構 Win32 DLL。

3.  在 [方案總管] 中，將 class1.cs 重新命名為 ToasterComponent.cs，使其符合專案的名稱。 Visual Studio 會自動重新命名檔案中的類別，以符合新的檔案名稱。
4.  在 .cs 檔案中，為 Windows.Foundation 命名空間加上 using 指示詞，以便將 TypedEventHandler 納入範圍。
5.  需要 Proxy 和虛設常式時，您的元件必須使用介面來公開它的公用成員。 在 ToasterComponent.cs 中，分別為快顯通知程式及其產生的 Toast 定義介面。

    > **注意：**在 C# 中，您可以略過此步驟， 改為先建立類別，然後開啟其捷徑功能表並選擇 [重構] &gt; [擷取介面]****。 在產生的程式碼中，手動為介面提供公用存取範圍。

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

6.  接著，我們需要類別來實作這些介面，而且必須是公用和密封類別，方便您之後將進行程式設計的 JavaScript app 進行存取。

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

    > **注意：**上述程式碼中的非同步呼叫會單獨使用 ThreadPool.RunAsync，示範以簡單的方式在背景執行緒上引發事件。 您可以撰寫這個特殊的方法 (如下列範例所示)，因為 .NET 工作排程器會自動將 async/await 呼叫封送處理回 UI 執行緒，因此這個方法可正常運作。
  
    ````csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
    ```

    If you build the project now, it should build cleanly.

## To program the JavaScript app

1.  Now we can add a button to the JavaScript app to cause it to use the class we just defined to make toast. Before we do this, we must add a reference to the ToasterComponent project we just created. In Solution Explorer, open the shortcut menu for the ToasterApplication project, choose **Add &gt; References**, and then choose the **Add New Reference** button. In the Add Reference dialog box, in the left pane under Solution, select the component project, and then in the middle pane, select ToasterComponent. Choose the **OK** button.
2.  In Solution Explorer, open the shortcut menu for the ToasterApplication project and then choose **Set as Startup Project**.
3.  At the end of the default.js file, add a namespace to contain the functions to call the component and be called back by it. The namespace will have two functions, one to make toast and one to handle the toast-complete event. The implementation of makeToast creates a Toaster object, registers the event handler, and makes the toast. So far, the event handler doesn’t do much, as shown here:

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

4.  The makeToast function must be hooked up to a button. Update default.html to include a button and some space to output the result of making toast:

    ```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
    ```

    If we weren’t using a TypedEventHandler, we would now be able to run the app on the local machine and click the button to make toast. But in our app, nothing happens. To find out why, let’s debug the managed code that fires the ToastCompletedEvent. Stop the project, and then on the menu bar, choose **Debug &gt; Toaster Application properties**. Change **Debugger Type** to **Managed Only**. Again on the menu bar, choose **Debug &gt; Exceptions**, and then select **Common Language Runtime Exceptions**.

    Now run the app and click the make-toast button. The debugger catches an invalid cast exception. Although it’s not obvious from its message, this exception is occurring because proxies are missing for that interface.

    ![missing proxy](./images/debuggererrormissingproxy.png)

    The first step in creating a proxy and stub for a component is to add a unique ID or GUID to the interfaces. However, the GUID format to use differs depending on whether you're coding in C#, Visual Basic, or another .NET language, or in C++.

## To generate GUIDs for the component's interfaces (C# and other .NET languages)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 5. \[Guid(“xxxxxxxx-xxxx...xxxx)\]. Choose the New GUID button and then choose the Copy button.

    ![guid generator tool](./images/guidgeneratortool.png)

2.  Go back to the interface definition, and then paste the new GUID just before the IToaster interface, as shown in the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp
    [Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
    ```

3.  Add a using directive for the System.Runtime.InteropServices namespace.
4.  Repeat these steps for the IToast interface.

## To generate GUIDs for the component's interfaces (C++)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 3. static const struct GUID = {...}. Choose the New GUID button and then choose the Copy button.

2.  Paste the GUID just before the IToaster interface definition. After you paste, the GUID should resemble the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

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

    Paste the Extensions XML node as a direct child of the Package node, and a peer of, for example, the Resources node.

2.  Before moving on, it’s important to ensure that:

    -   The ProxyStub ClassId is set to the first GUID in the ToasterComponent\_i.c file. Use the first GUID that's defined in this file for the classId. (This might be the same as the GUID for ITypedEventHandler2.)
    -   The Path is the package relative path of the proxy binary. (In this walkthrough, proxies.dll is in the same folder as ToasterApplication.winmd.)
    -   The GUIDs are in the correct format. (This is easy to get wrong.)
    -   The interface IDs in the manifest match the IIDs in ToasterComponent\_i.c file.
    -   The interface names are unique in the manifest. Because these are not used by the system, you can choose the values. It is a good practice to choose interface names that clearly match interfaces that you have defined. For generated interfaces, the names should be indicative of the generated interfaces. You can use the ToasterComponent\_i.c file to help you generate interface names.

3.  If you try to run the solution now, you will get an error that proxies.dll is not part of the payload. Open the shortcut menu for the **References** folder in the ToasterApplication project and then choose **Add Reference**. Select the check box next to the Proxies project. Also, make sure that the check box next to ToasterComponent is also selected. Choose the **OK** button.

    The project should now build. Run the project and verify that you can make toast.

## Related topics

* [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=May16_HO2-->


