---
title: 啟動 app 以取得結果
description: 了解如何從某個應用程式啟動另一個應用程式，以及在這兩者間交換資料的方式。 這稱為「啟動應用程式以取得結果」。
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f627cf2a897de32aea0e35faf66f5ea70695efd5
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8690682"
---
# <a name="launch-an-app-for-results"></a>啟動應用程式以取得結果




**重要 API**

-   [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
-   [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

了解如何從某個 app 啟動另一個 app，以及在這兩者間交換資料的方式。 這稱為 *「啟動 App 以取得結果」*。 下列範例示範如何使用 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) 來啟動 App 以取得結果。

新的應用程式-應用程式通訊 windows 10 中的 Api 讓 Windows 應用程式 （及 Windows Web 應用程式） 來啟動某個 app 並交換資料和檔案。 這讓您能夠從多個 App 建置混搭式解決方案。 使用這些新的 API，就能流暢地立即處理需要使用者使用多個 App 的複雜工作。 例如，您的 App 可以啟動社交網路 App 來選擇連絡人，或啟動結帳 App 來完成付款程序。

您將啟動以取得結果的 App 將稱為啟動的 App。 啟動該 App 的 App 將稱為呼叫的 App。 您將針對此範例撰寫呼叫的 app 和啟動的 app。

## <a name="step-1-register-the-protocol-to-be-handled-in-the-app-that-youll-launch-for-results"></a>步驟 1：在您將啟動以取得結果的 App 中，登錄要處理的通訊協定


在已啟動 App 的 Package.appxmanifest 檔案中，將通訊協定延伸模組新增到 **&lt;Application&gt;** 區段。 下列範例會使用名為 **test-app2app** 的虛構通訊協定。

通訊協定延伸模組中的 **ReturnResults** 屬性會接受下列其中一個值：

-   **optional**—您可以使用 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) 方法來啟動 app 以取得結果，或者使用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 來啟動 app 而不取得結果。 當您使用 **optional** 時，啟動的 app 必須判斷是否要啟動它來取得結果。 您可以藉由檢查 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件引數來執行這個動作。 如果引數的 [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 屬性傳回 [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693)，或者事件引數的類型是 [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)，則 app 會透過 **LaunchUriForResultsAsync** 來啟動。
-   **always**—可以只為了取得結果來啟動 app，也就是它只會回應 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)。
-   **none**—無法啟動 app 來取得結果；它只會回應 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)。

在這個通訊協定延伸模組範例中，可以只為了取得結果來啟動 app。 這會簡化 **OnActivated** 方法內部的邏輯 (如下所述)，因為我們只需處理「啟動以取得結果」的案例，而且沒有其他方法可用以啟動 app。

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## <a name="step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results"></a>步驟 2：在您將啟動以取得結果的 app 中覆寫 Application.OnActivated


如果這個方法尚未存在於啟動的 app 中，請在 App.xaml.cs 中定義的 `App` 類別內建立它。

在讓您能夠於社交網路中挑選朋友的 app 中，此函式也是您開啟人員選擇器頁面的所在之處。 在下列範例中，名為 **LaunchedForResultsPage** 的頁面會在啟動 app 以取得結果時顯示。 確保 **using** 陳述式已包含於檔案頂端。

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

針對此 app，由於 Package.appxmanifest 檔案中的通訊協定延伸模組已將 **ReturnResults** 指定為 **always**，因此，上述程式碼可放心地將 `args` 直接轉換為 [**ProtocolForResultsActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn906905)，只有 **ProtocolForResultsActivatedEventArgs** 會傳送到 **OnActivated**。 如果您的 app 是利用啟動以取得結果以外的方式來啟動，則您可以檢查 [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 屬性是否傳回 [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693)，以了解是否要啟動 app 來取得結果。

## <a name="step-3-add-a-protocolforresultsoperation-field-to-the-app-you-launch-for-results"></a>步驟 3：在您啟動以取得結果的 app 中新增 ProtocolForResultsOperation 欄位


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

您將使用 [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913) 欄位，在啟動的 app 已準備好將結果傳回給呼叫的 app 時發出訊號。 在這個範例中，已將欄位新增到 **LaunchedForResultsPage** 類別，因為您將從該頁面完成「啟動以取得結果」作業，而且需要存取它。

## <a name="step-4-override-onnavigatedto-in-the-app-you-launch-for-results"></a>步驟 4：在您啟動以取得結果的 app 中覆寫 OnNavigatedTo()


在啟動 App 以取得結果時顯示的頁面上，覆寫 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 方法。 如果這個方法尚未存在，請在 &lt;pagename&gt;.xaml.cs 中定義的頁面類別內建立它。 確保下列 **using** 陳述式已包含於檔案頂端：

```cs
using Windows.ApplicationModel.Activation
```

[**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 方法中的 [**NavigationEventArgs**](https://msdn.microsoft.com/library/windows/apps/br243285) 物件包含呼叫的 app 所傳送的資料。 資料不能超過 100 KB 並儲存於 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 物件中。

在下列範例程式碼中，啟動的 app 預期呼叫的 app 所傳送的資料會在名為 **TestData** 之機碼下方的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 中，而這就是撰寫呼叫的 app 範例程式碼來傳送的原因。

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## <a name="step-5-write-code-to-return-data-to-the-calling-app"></a>步驟 5：撰寫程式碼以將資料傳回呼叫的 app


在啟動的 app 中，使用 [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913) 來將資料傳回呼叫的 app。 下列範例程式碼會建立 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 物件，其中包含要傳回呼叫之 app 的值。 接著，使用 **ProtocolForResultsOperation** 欄位，將值傳送給呼叫的 app。

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## <a name="step-6-write-code-to-launch-the-app-for-results-and-get-the-returned-data"></a>步驟 6：撰寫程式碼來啟動 app 以取得結果，並取得傳回的資料


在呼叫之 app 中的非同步方法內啟動 app，如下列範例程式碼所示。 請注意 **using** 陳述式，這是程式碼編譯所需的陳述式：

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

這個範例會將包含機碼 **TestData** 的 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 傳送到啟動的 app。 啟動的 app 會建立包含機碼名稱為 **ReturnedData** 的 **ValueSet**，其中包含傳回給呼叫者的結果。

您必須先建置並部署將啟動來取得結果的 app，才能執行呼叫的 app。 否則，[**LaunchUriResult.Status**](https://msdn.microsoft.com/library/windows/apps/dn906892) 將會報告 **LaunchUriStatus.AppUnavailable**。

當您設定 [**TargetApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/dn893511) 時，需要啟動的 app 系列名稱 。 取得系列名稱的一種方式是在啟動的 app 內進行下列呼叫：

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## <a name="remarks"></a>備註


此做法中的範例提供可用來啟動 app 以取得結果的 "hello world" 簡介。 要注意的重點是新的 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) API 讓您能夠以非同步方式啟動 app，並透過 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 類別進行通訊。 透過 **ValueSet** 傳送的資料大小上限為 100KB。 如果需要傳送更大量的資料，可使用 [**SharedStorageAccessManager**](https://msdn.microsoft.com/library/windows/apps/dn889985) 類別來共用檔案，以建立可在 app 之間傳送的檔案權杖。 例如，假設有一個名為 `inputData` 的 **ValueSet**，您可以將權杖儲存到想要與啟動的 app 共用的檔案中：

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

然後透過 **LaunchUriForResultsAsync** 將它傳送給啟動的 app。

## <a name="related-topics"></a>相關主題


* [**LaunchUri**](https://msdn.microsoft.com/library/windows/apps/hh701476)
* [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
* [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

 

 
