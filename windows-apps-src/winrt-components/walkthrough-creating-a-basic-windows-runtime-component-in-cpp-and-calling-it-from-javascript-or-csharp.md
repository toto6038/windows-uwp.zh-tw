---
author: msatranjr
title: 在 C++/CX 中建立基本 Windows 執行階段元件，然後從 JavaScript 或 C\# 呼叫該元件
description: 本逐步解說示範如何建立可從 JavaScript、C# 或 Visual Basic 呼叫的基本 Windows 執行階段元件 DLL。
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.author: misatran
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8d56e220a65d0de261ab0cc64b8ac417e1f480eb
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6839310"
---
# <a name="walkthrough-creating-a-windows-runtime-component-in-ccx-and-calling-it-from-javascript-or-c"></a>逐步解說：在 C++/CX 中建立 Windows 執行階段元件，然後從 JavaScript 或 C# 呼叫該元件
> [!NOTE]
> 本主題是為協助您維護您 C++/CX 應用程式。 但我們建議您將 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 用於新的應用程式。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 若要了解如何建立 Windows 執行階段元件，使用 C + + /winrt，請參閱[中撰寫事件在 C + + WinRT](../cpp-and-winrt-apis/author-events.md)。

本逐步解說示範如何建立可從 JavaScript、C# 或 Visual Basic 呼叫的基本 Windows 執行階段元件 DLL。 開始本逐步解說之前，請確定您了解一些概念，例如：抽象二進位介面 (ABI)、ref 類別，以及讓 ref 類別更容易使用的 Visual C++ 元件擴充功能。 如需詳細資訊，請參閱[在 C++ 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 語言參考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)。

## <a name="creating-the-c-component-dll"></a>建立 C++ 元件 DLL
此範例會先建立元件專案，但您也可以先建立 JavaScript 專案。 順序並不重要。

請注意，元件的主要類別包含屬性和方法定義的範例，以及事件宣告。 提供這些項目只是為了示範它的進行方式。 這些並不是必要項目，而且在此範例中，我們會以自己的程式碼來取代所有產生的程式碼。

## **<a name="to-create-the-c-component-project"></a>建立 C++ 元件專案**
在 Visual Studio 功能表列上，依序選擇 **\[檔案\]、\[新增\] 及 \[專案\]**。

在 [**新增專案**] 對話方塊的左窗格中，展開 [**Visual C++**]，然後選取通用 Windows app 的節點。

在中央窗格，選取 [**Windows 執行階段元件**]，然後將專案命名為 WinRT\_CPP。

選擇 **\[確定\]** 按鈕。

## **<a name="to-add-an-activatable-class-to-the-component"></a>將可啟用的類別加入至元件**
可啟用的類別是用戶端程式碼可以使用 **new** 運算式 (在 Visual Basic 中是 **New**，在 C++ 中則是 **ref new**) 建立的類別。 在您的元件中，您會將它宣告為 **public ref class sealed**。 事實上，Class1.h 和 .cpp 檔案已經有 ref 類別。 您可以變更名稱，但在這個範例中，我們會使用預設名稱 -- Class1。 如有必要，您可以在元件中定義其他 ref 類別或一般類別。 如需 ref 類別的詳細資訊，請參閱[類型系統 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx)。

將下列 \#include 指示詞加入至 Class1.h：

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

collection.h 是 C++ 具象類別 (例如 Platform::Collections::Vector 類別和 Platform::Collections::Map 類別，這會實作 Windows 執行階段所定義之非語言相關的介面) 的標頭檔。 amp 標頭是用來在 GPU 上執行計算。 它們沒有 Windows 執行階段的對等用法，不過沒關係，因為它們是私用的。 一般而言，基於效能考量，您應該在元件內部使用 ISO C++ 程式碼和標準程式庫，因為它只是必須以 Windows 執行階段類型表示的 Windows 執行階段介面。

## <a name="to-add-a-delegate-at-namespace-scope"></a>在命名空間範圍內加入委派
委派是可定義方法的參數和傳回類型的建構。 事件是特定委派類型的執行個體，而且訂閱事件的所有事件處理常式方法都必須包含委派中所指定的簽章。 下列程式碼會定義使用 int 並傳回 void 的委派類型。 接著，程式碼會宣告此類型的公用事件，這讓用戶端程式碼能夠在事件引發時提供所叫用的方法。

在 Class1 宣告之前，於 Class1.h 的命名空間範圍內加入下列委派宣告。

```cpp
public delegate void PrimeFoundHandler(int result);
```

如果在 Visual Studio 中貼上程式碼時，程式碼並未正確對齊，請按 Ctrl+K+D 來修正整個檔案的縮排格式。

## <a name="to-add-the-public-members"></a>加入公用成員
這個類別會公開三個公用方法和一個公用事件。 第一個方法是同步方法，因為它一律會以飛快的速度來執行。 另外兩個方法是非同步方法，因為可能需要一些時間才能執行完畢，如此才不會阻擋 UI 執行緒。 這些方法會傳回 IAsyncOperationWithProgress 和 IAsyncActionWithProgress。 前者會定義傳回結果的非同步方法，而後者則會定義傳回 void 的非同步方法。 這些介面也可讓用戶端程式碼接收作業進度的更新。

```cpp
public:

        // Synchronous method.
        Windows::Foundation::Collections::IVector<double>^  ComputeResult(double input);

        // Asynchronous methods
        Windows::Foundation::IAsyncOperationWithProgress<Windows::Foundation::Collections::IVector<int>^, double>^
            GetPrimesOrdered(int first, int last);
        Windows::Foundation::IAsyncActionWithProgress<double>^ GetPrimesUnordered(int first, int last);

        // Event whose type is a delegate "class"
        event PrimeFoundHandler^ primeFoundEvent;

```
## <a name="to-add-the-private-members"></a>加入私用成員
此類別包含三個私用成員：兩個用於數值計算的 Helper 方法以及一個 CoreDispatcher 物件，後者是用來從背景工作執行緒將事件叫用封送處理回 UI 執行緒。

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

## <a name="to-add-the-header-and-namespace-directives"></a>加入標題和命名空間指示詞
在 Class1.cpp 中，加入下列 #include 指示詞：

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

現在，加入下列 using 陳述式來引進所需的命名空間：

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>加入 ComputeResult 的實作
在 Class1.cpp 中，加入下列方法實作。 這個方法會在呼叫執行緒上同步執行，因為它會使用 C++ AMP 在 GPU 上平行處理計算，因此執行速度非常快。 如需詳細資訊，請參閱＜C++ AMP 概觀＞。 結果會附加至 Platform::Collections::Vector<T> 具象類型，它會在傳回時隱含轉換為 Windows::Foundation::Collections::IVector<T>。

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/en-us/library/hh305254.aspx
    parallel_for_each(
        logs.extent,
        [=] (index<1> idx) restrict(amp)
    {
        logs[idx] = concurrency::fast_math::log10(logs[idx]);
    }
    );

    // Return a Windows Runtime-compatible type across the ABI
    auto res = ref new Vector<double>();
    int len = safe_cast<int>(logs.extent.size());
    for(int i = 0; i < len; i++)
    {      
        res->Append(logs[i]);
    }

    // res is implicitly cast to IVector<double>
    return res;
}
```
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>加入 GetPrimesOrdered 及其 Helper 方法的實作
在 Class1.cpp 中，加入 GetPrimesOrdered 及 is_prime Helper 方法的實作。 GetPrimesOrdered 會使用 concurrent_vector 類別和 parallel_for 函式迴圈來劃分工作，並利用程式執行所在電腦上的最大資源來產生結果。 經過計算、儲存及排序之後的結果會加入 Platform::Collections::Vector<T>，並以 Windows::Foundation::Collections::IVector<T> 的形式傳回至用戶端程式碼。

請注意進度報告程式的程式碼，它可讓用戶端連接進度列或其他 UI，讓使用者得知作業還要多久才完成。 使用進度報告也必須付出代價。 您必須在元件端引發事件，並在 UI 執行緒上處理該事件，而且每個反覆項目都必須儲存進度值。 將這個代價降到最低的其中一種方式，就是限制引發進度事件的頻率。 如果不願意付出這個代價，或者您無法評估作業要多久才能完成，請考慮使用進度環來表示作業正在進行中，但直到完成為止都不會顯示剩餘的時間。

```cpp
// Determines whether the input value is prime.
bool Class1::is_prime(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
    {
        if ((n % i) == 0)
            return false;
    }
    return true;
}

// This method computes all primes, orders them, then returns the ordered results.
IAsyncOperationWithProgress<IVector<int>^, double>^ Class1::GetPrimesOrdered(int first, int last)
{
    return create_async([this, first, last]
    (progress_reporter<double> reporter) -> IVector<int>^ {
        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }
        // Perform the computation in parallel.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        parallel_for(first, last + 1, [this, &primes, &operation,
            range, &lastPercent, reporter](int n) {

                // Increment and store the number of times the parallel
                // loop has been called on all threads combined. There
                // is a performance cost to maintaining a count, and
                // passing the delegate back to the UI thread, but it's
                // necessary if we want to display a determinate progress
                // bar that goes from 0 to 100%. We can avoid the cost by
                // setting the ProgressBar IsDeterminate property to false
                // or by using a ProgressRing.
                if(InterlockedIncrement(&operation) % 100 == 0)
                {
                    reporter.report(100.0 * operation / range);
                }

                // If the value is prime, add it to the local vector.
                if (is_prime(n)) {
                    primes.push_back(n);
                }
        });

        // Sort the results.
        std::sort(begin(primes), end(primes), std::less<int>());        
        reporter.report(100.0);

        // Copy the results to a Vector object, which is
        // implicitly converted to the IVector return type. IVector
        // makes collections of data available to other
        // Windows Runtime components.
        return ref new Vector<int>(primes.begin(), primes.end());
    });
}
```

## <a name="to-add-the-implementation-for-getprimesunordered"></a>加入 GetPrimesUnordered 的實作
建立 C++ 元件的最後一個步驟是在 Class1.cpp 中加入 GetPrimesUnordered 的實作。 這個方法會傳回找到的每個結果，而不會等待直到找到所有結果為止。 事件處理常式中的每個結果都會傳回並即時顯示於 UI 上。 同樣地，請注意這裡也使用了進度報告程式。 這個方法也會使用 is_prime Helper 方法。

```cpp
// This method returns no value. Instead, it fires an event each time a
// prime is found, and passes the prime through the event.
// It also passes progress info.
IAsyncActionWithProgress<double>^ Class1::GetPrimesUnordered(int first, int last)
{

    auto window = Windows::UI::Core::CoreWindow::GetForCurrentThread();
    m_dispatcher = window->Dispatcher;


    return create_async([this, first, last](progress_reporter<double> reporter) {

        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }

        // In this particular example, we don't actually use this to store
        // results since we pass results one at a time directly back to
        // UI as they are found. However, we have to provide this variable
        // as a parameter to parallel_for.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        // Perform the computation in parallel.
        parallel_for(first, last + 1,
            [this, &primes, &operation, range, &lastPercent, reporter](int n)
        {
            // Store the number of times the parallel loop has been called  
            // on all threads combined. See comment in previous method.
            if(InterlockedIncrement(&operation) % 100 == 0)
            {
                reporter.report(100.0 * operation / range);
            }

            // If the value is prime, pass it immediately to the UI thread.
            if (is_prime(n))
            {                
                // Since this code is probably running on a worker
                // thread, and we are passing the data back to the
                // UI thread, we have to use a CoreDispatcher object.
                m_dispatcher->RunAsync( CoreDispatcherPriority::Normal,
                    ref new DispatchedHandler([this, n, operation, range]()
                {
                    this->primeFoundEvent(n);

                }, Platform::CallbackContext::Any));

            }
        });
        reporter.report(100.0);
    });
}
```

## <a name="creating-a-javascript-client-app"></a>建立 JavaScript 用戶端 App
如果您只想建立 C# 用戶端，則可略過本節。

## <a name="to-create-a-javascript-project"></a>建立 JavaScript 專案
在 [方案總管] 中，開啟 [方案] 節點的捷徑功能表，然後依序選擇 **\[加入\] 和 \[新增專案\]**。

展開 \[JavaScript\] (它可能是以巢狀方式置於 **\[其他語言\]** 底下)，然後選擇 **\[空白應用程式 (通用 Windows)\]**。

選擇 **\[確定\]** 按鈕，接受 App1 這個預設名稱。

開啟 App1 專案節點的捷徑功能表，然後選擇 **\[設定為啟始專案\]**。

將專案參考加入至 WinRT_CPP：

開啟 [參考] 節點的捷徑功能表，然後選擇 **\[加入參考\]**。

在 [參考管理員] 對話方塊的左窗格中，依序選取 **\[專案\]** 和 **\[方案\]**。

在中央窗格，選取 [WinRT_CPP]，然後選擇 **\[確定\]** 按鈕。

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>加入可叫用 JavaScript 事件處理常式的 HTML
將此 HTML 貼到 default.html 頁面的 <body> 節點中：

```HTML
<div id="LogButtonDiv">
     <button id="logButton">Logarithms using AMP</button>
 </div>
 <div id="LogResultDiv">
     <p id="logResult"></p>
 </div>
 <div id="OrderedPrimeButtonDiv">
     <button id="orderedPrimeButton">Primes using parallel_for with sort</button>
 </div>
 <div id="OrderedPrimeProgress">
     <progress id="OrderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="OrderedPrimeResultDiv">
     <p id="orderedPrimes">
         Primes found (ordered):
     </p>
 </div>
 <div id="UnorderedPrimeButtonDiv">
     <button id="ButtonUnordered">Primes returned as they are produced.</button>
 </div>
 <div id="UnorderedPrimeDiv">
     <progress id="UnorderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="UnorderedPrime">
     <p id="unorderedPrimes">
         Primes found (unordered):
     </p>
 </div>
 <div id="ClearDiv">
     <button id="Button_Clear">Clear</button>
 </div>
```

## <a name="to-add-styles"></a>加入樣式
移除 default.css 中的 body 樣式，然後加入下列樣式：

```css
#LogButtonDiv {
border: orange solid 1px;
-ms-grid-row: 1; /* default is 1 */
-ms-grid-column: 1; /* default is 1 */
}
#LogResultDiv {
background: black;
border: red solid 1px;
-ms-grid-row: 1;
-ms-grid-column: 2;
}
#UnorderedPrimeButtonDiv, #OrderedPrimeButtonDiv {
border: orange solid 1px;
-ms-grid-row: 2;   
-ms-grid-column:1;
}
#UnorderedPrimeProgress, #OrderedPrimeProgress {
border: red solid 1px;
-ms-grid-column-span: 2;
height: 40px;
}
#UnorderedPrimeResult, #OrderedPrimeResult {
border: red solid 1px;
font-size:smaller;
-ms-grid-row: 2;
-ms-grid-column: 3;
-ms-overflow-style:scrollbar;
}
```

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>加入可呼叫元件 DLL 的 JavaScript 事件處理常式
在 default.js 檔案的結尾加入下列函式。 選擇主頁面上的按鈕時，就會呼叫這些函式。 請注意 JavaScript 啟用 C++ 類別的方式，然後呼叫其方法並使用傳回值來填入 HTML 標籤。

```JavaScript
var nativeObject = new WinRT_CPP.Class1();

function LogButton_Click() {

    var val = nativeObject.computeResult(0);
    var result = "";

    for (i = 0; i < val.length; i++) {
        result += val[i] + "<br/>";
    }

    document.getElementById('logResult').innerHTML = result;
}

function ButtonOrdered_Click() {
    document.getElementById('orderedPrimes').innerHTML = "Primes found (ordered): ";

    nativeObject.getPrimesOrdered(2, 10000).then(
        function (v) {
            for (var i = 0; i < v.length; i++)
                document.getElementById('orderedPrimes').innerHTML += v[i] + " ";
        },
        function (error) {
            document.getElementById('orderedPrimes').innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("OrderedPrimesProgressBar");
            progressBar.value = p;
        });
}

function ButtonUnordered_Click() {
    document.getElementById('unorderedPrimes').innerHTML = "Primes found (unordered): ";
    nativeObject.onprimefoundevent = handler_unordered;

    nativeObject.getPrimesUnordered(2, 10000).then(
        function () { },
        function (error) {
            document.getElementById("unorderedPrimes").innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("UnorderedPrimesProgressBar");
            progressBar.value = p;
        });
}

var handler_unordered = function (n) {
    document.getElementById('unorderedPrimes').innerHTML += n.target.toString() + " ";
};

function ButtonClear_Click() {

    document.getElementById('logResult').innerHTML = "";
    document.getElementById("unorderedPrimes").innerHTML = "";
    document.getElementById('orderedPrimes').innerHTML = "";
    document.getElementById("UnorderedPrimesProgressBar").value = 0;
    document.getElementById("OrderedPrimesProgressBar").value = 0;
}
```

新增程式碼來加入事件接聽程式，方法是利用下列在 then 區塊中實作事件註冊的程式碼，來取代 default.js 中對於 app.onactivated 之 WinJS.UI.processAll 的現有呼叫。 如需此動作的詳細說明，請參閱＜建立 "Hello World" app (JS)＞。

```JavaScript
args.setPromise(WinJS.UI.processAll().then( function completed() {
    var logButton = document.getElementById("logButton");
    logButton.addEventListener("click", LogButton_Click, false);
    var orderedPrimeButton = document.getElementById("orderedPrimeButton");
    orderedPrimeButton.addEventListener("click", ButtonOrdered_Click, false);
    var buttonUnordered = document.getElementById("ButtonUnordered");
    buttonUnordered.addEventListener("click", ButtonUnordered_Click, false);
    var buttonClear = document.getElementById("Button_Clear");
    buttonClear.addEventListener("click", ButtonClear_Click, false);
}));
```

按 F5 來執行 app。

## <a name="creating-a-c-client-app"></a>建立 C# 用戶端 App

## <a name="to-create-a-c-project"></a>建立 C# 專案
在 [方案總管] 中，開啟 [方案] 節點的捷徑功能表，然後依序選擇 **\[加入\] 和 \[新增專案\]**。

展開 \[Visual C#\] (它可能是以巢狀方式置於 **\[其他語言\]** 底下)、選取 **\[Windows\]**，接著選取左窗格中的 **\[通用\]**，然後選取中間窗格的 **\[空白應用程式\]**。

將這個 app 命名為 CS_Client，然後選擇 **\[確定\]** 按鈕。

開啟 CS_Client 專案節點的捷徑功能表，然後選擇 **\[設定為啟始專案\]**。

將專案參考加入至 WinRT_CPP：

開啟 **\[參考\]** 節點的捷徑功能表，然後選擇 **\[加入參考\]**。

在 **\[參考管理員\]** 對話方塊的左窗格中，依序選取 **\[專案\]** 和 **\[方案\]**。

在中央窗格，選取 [WinRT_CPP]，然後選擇 **\[確定\]** 按鈕。

## <a name="to-add-the-xaml-that-defines-the-user-interface"></a>加入定義使用者介面的 XAML
將下列程式碼複製到 MainPage.xaml 中的 Grid 元素。

```xaml
<ScrollViewer>
            <StackPanel Width="1400">

                <Button x:Name="Button1" Width="340" Height="50"  Margin="0,20,20,20" Content="Synchronous Logarithm Calculation" FontSize="16" Click="Button1_Click_1"/>
                <TextBlock x:Name="Result1" Height="100" FontSize="14"></TextBlock>
            <Button x:Name="PrimesOrderedButton" Content="Prime Numbers Ordered" FontSize="16" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesOrderedButton_Click_1"></Button>
            <ProgressBar x:Name="PrimesOrderedProgress" IsIndeterminate="false" Height="40"></ProgressBar>
                <TextBlock x:Name="PrimesOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>
            <Button x:Name="PrimesUnOrderedButton" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesUnOrderedButton_Click_1" Content="Prime Numbers Unordered" FontSize="16"></Button>
            <ProgressBar x:Name="PrimesUnOrderedProgress" IsIndeterminate="false" Height="40" ></ProgressBar>
            <TextBlock x:Name="PrimesUnOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>

            <Button x:Name="Clear_Button" Content="Clear" HorizontalAlignment="Left" Margin="0,20,20,20" VerticalAlignment="Top" Width="341" Click="Clear_Button_Click" FontSize="16"/>
        </StackPanel>
</ScrollViewer>
```

## <a name="to-add-the-event-handlers-for-the-buttons"></a>為按鈕加入事件處理常式
在 [方案總管] 中，開啟 MainPage.xaml.cs (這個檔案可能是以巢狀方式置於 MainPage.xaml 底下。) 針對 System.Text 加入 using 指示詞，然後在 MainPage 類別中加入事件處理常式以進行 Logarithm 計算。

```csharp
private void Button1_Click_1(object sender, RoutedEventArgs e)
{
    // Create the object
    var nativeObject = new WinRT_CPP.Class1();

    // Call the synchronous method. val is an IList that
    // contains the results.
    var val = nativeObject.ComputeResult(0);
    StringBuilder result = new StringBuilder();
    foreach (var v in val)
    {
        result.Append(v).Append(System.Environment.NewLine);
    }
    this.Result1.Text = result.ToString();
}
```

為已排序的結果加入事件處理常式：

```csharp
async private void PrimesOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (ordered): ");

    PrimesOrderedResult.Text = sb.ToString();

    // Call the asynchronous method
    var asyncOp = nativeObject.GetPrimesOrdered(2, 100000);

    // Before awaiting, provide a lambda or named method
    // to handle the Progress event that is fired at regular
    // intervals by the asyncOp object. This handler updates
    // the progress bar in the UI.
    asyncOp.Progress = (asyncInfo, progress) =>
        {
            PrimesOrderedProgress.Value = progress;
        };

    // Wait for the operation to complete
    var asyncResult = await asyncOp;

    // Convert the results to strings
    foreach (var result in asyncResult)
    {
        sb.Append(result).Append(" ");
    }

    // Display the results
    PrimesOrderedResult.Text = sb.ToString();
}
```

為未排序的結果加入事件處理常式，並為清除結果的按鈕加入事件處理常式，方便您再次執行程式碼。

```csharp
private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (unordered): ");
    PrimesUnOrderedResult.Text = sb.ToString();

    // primeFoundEvent is a user-defined event in nativeObject
    // It passes the results back to this thread as they are produced
    // and the event handler that we define here immediately displays them.
    nativeObject.primeFoundEvent += (n) =>
    {
        sb.Append(n.ToString()).Append(" ");
        PrimesUnOrderedResult.Text = sb.ToString();
    };

    // Call the async method.
    var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

    // Provide a handler for the Progress event that the asyncResult
    // object fires at regular intervals. This handler updates the progress bar.
    asyncResult.Progress += (asyncInfo, progress) =>
        {
            PrimesUnOrderedProgress.Value = progress;
        };
}

private void Clear_Button_Click(object sender, RoutedEventArgs e)
{
    PrimesOrderedProgress.Value = 0;
    PrimesUnOrderedProgress.Value = 0;
    PrimesUnOrderedResult.Text = "";
    PrimesOrderedResult.Text = "";
    Result1.Text = "";
}
```

## <a name="running-the-app"></a>執行 App
在 [方案總管] 中開啟專案節點的捷徑功能表，然後選擇 **\[設定為啟始專案\]**，選取 C# 專案或 JavaScript 專案做為啟始專案。 然後，按 F5 開始執行並偵錯，或是按 Ctrl+F5 開始執行而不偵錯。

## <a name="inspecting-your-component-in-object-browser-optional"></a>在物件瀏覽器中檢查您的元件 (選擇性)
在 [物件瀏覽器] 中，您可以檢查 .winmd 檔案中定義的所有 Windows 執行階段類型。 這包含 Platform 命名空間和預設命名空間中的類型。 不過，由於 Platform::Collections 命名空間中的類型是定義於標頭檔 collections.h (而非 winmd 檔案) 中，因此它們不會出現在 [物件瀏覽器] 中。

## **<a name="to-inspect-a-component"></a>檢查元件**
在功能表列上，依序選擇 **[檢視] 和 [物件瀏覽器]** (Ctrl+Alt+J)。

在 [物件瀏覽器] 的左窗格中，展開 [WinRT\_CPP] 節點，以顯示在您的元件中定義的類型和方法。

## <a name="debugging-tips"></a>偵錯提示
若想獲得較佳的偵錯經驗，請從公用 Microsoft 符號伺服器下載偵錯符號：

## **<a name="to-download-debugging-symbols"></a>下載偵錯符號**
在功能表列上，依序選擇 **\[工具\] 和 \[選項\]**。

在 **\[選項\]** 對話方塊中，展開 **\[偵錯\]**，然後選取 **\[符號\]**。

選取 **\[Microsoft 符號伺服器\]**，然後選擇 **\[確定\]** 按鈕。

第一次下載這些符號時可能需要一些時間。 若要縮短下載時間，當您下次按 F5 時，請指定用來快取符號的本機目錄。

對含有元件 DLL 的 JavaScript 方案進行偵錯時，您可以設定偵錯工具以啟用逐步執行指令碼或逐步執行元件中的機器碼，但不可兩者同時啟用。 若要變更設定，請在 [方案總管] 中開啟 JavaScript 專案節點的捷徑功能表，然後依序選擇 **\[屬性\]、\[偵錯\] 及 \[偵錯工具類型\]**。

請務必在封裝設計工具中選取適當的功能。 您可以藉由開啟 Package.appxmanifest 檔案來開啟封裝設計工具。 例如，如果您嘗試以程式設計方式存取 [圖片] 資料夾中的檔案，請務必在封裝設計工具中的 **\[功能\]** 窗格中選取 **\[圖片庫\]** 核取方塊 。

如果 JavaScript 程式碼無法辨識元件中的公用屬性或方法，請確定您在 JavaScript 中使用 Camel 命名法的大小寫慣例。 例如，`ComputeResult` C++ 方法必須當做 JavaScript 中的 `computeResult` 來參考。

如果從方案中移除 C++ Windows 執行階段元件專案，也必須手動從 JavaScript 專案中移除該專案的參考。 若未執行此動作，後續的偵錯或建置作業將無法執行。 之後如有必要，您可以加入 DLL 的組件參考。

## <a name="related-topics"></a>相關主題
* [在 C++/CX 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md)
