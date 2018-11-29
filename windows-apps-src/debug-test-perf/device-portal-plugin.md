---
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: 撰寫 Device Portal 的自訂外掛程式
description: 了解如何撰寫 UWP app 以使用 Windows Device Portal 來裝載網頁，以及提供診斷資訊。
ms.date: 03/24/2017
ms.topic: article
keywords: windows 10，uwp，裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: d9e11445d77434320c8842608bf8183a078c0660
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "7989037"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>撰寫 Device Portal 的自訂外掛程式

了解如何撰寫 UWP app 以使用 Windows Device Portal 來裝載網頁，以及提供診斷資訊。

從 Creators Update 開始，您可以使用 Device Portal 來管理應用程式的診斷介面。 本文涵蓋建立應用程式之 DevicePortalProvider 所需的三個部分 – appxmanifest 變更、設定應用程式的 Device Portal 服務連線，以及處理連入要求。 也會提供範例應用程式，讓您開始使用 (即將推出)。 

## <a name="create-a-new-uwp-app-project"></a>建立新的 UWP app 專案
在本手冊中，為了簡單起見，我們將在一個方案中建立所有項目。

在 Microsoft Visual Studio 2017 中，建立新的 UWP app 專案。 移至 [檔案] > [新增專案]，並選取 [範本] > [Visual C#] > [Windows 通用] > [空白應用程式 (Windows 通用)]。 將它命名為 "DevicePortalProvider"。 這會是包含應用程式服務的應用程式。 確認您選擇要支援的 Creators Update SDK。  您可能需要更新 Visual Studio，或安裝新的 SDK - 如需詳細資料，請參閱[這裡](https://blogs.windows.com/buildingapps/2017/04/05/updating-tooling-windows-10-creators-update/)。 

## <a name="add-the-deviceportalprovider-extension-to-your-packageappxmanifest-file"></a>將 devicePortalProvider 延伸模組新增至 package.appxmanifest 檔案
您需要將某個程式碼新增至*package.appxmanifest*檔案，以讓應用程式作用為 Device Portal 外掛程式。 首先，請在檔案頂端新增下列命名空間定義。 也將它們新增至`IgnorableNamespaces`屬性。

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

若要宣告您的應用程式是「Device Portal 提供者」，您需要建立應用程式服務，以及使用它的新「Device Portal 提供者」延伸模組。 在`Application`下方的 `Extensions` 元素中，新增 windows.appService 延伸模組和 windows.devicePortalProvider 延伸模組。 請確定每個延伸模組中的`AppServiceName`屬性相符。 這表示可啟動此應用程式服務來處理處理常式命名空間上要求的 Device Portal 服務。 

```xml
...   
<Application 
    Id="App" 
    Executable="$targetnametoken$.exe"
    EntryPoint="DevicePortalProvider.App">
    ...
    <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MySampleProvider.SampleProvider">
            <uap:AppService Name="com.sampleProvider.wdp" />
        </uap:Extension>
        <uap4:Extension Category="windows.devicePortalProvider">
            <uap4:DevicePortalProvider 
                DisplayName="My Device Portal Provider Sample App" 
                AppServiceName="com.sampleProvider.wdp" 
                HandlerRoute="/MyNamespace/api/" />
        </uap4:Extension>
    </Extensions>
</Application>
...
```

`HandlerRoute`屬性參考您應用程式所宣告的 REST 命名空間。 該命名空間上 Device Portal 服務所收到的任何 HTTP 要求 (後面隱含地接著萬用字元)，將會傳送至您的應用程式進行處理。 在此情況下，`<ip_address>/MyNamespace/api/*`的任何驗證成功的 HTTP 要求將會傳送至您的應用程式。 透過「獲勝最久」檢查來解決處理常式路徑間的衝突︰即選取符合多個要求的路徑，表示 "/MyNamespace/api/foo" 的要求會與具有 "/MyNamespace/api" 的提供者進行比對，而非具有 "/MyNamespace" 的提供者。  

這項功能需要兩個新的功能。 它們也必須新增至您的*package.appxmanifest*檔案。

```xml
...
<Capabilities>
    ...
    <Capability Name="privateNetworkClientServer" />
    <rescap:Capability Name="devicePortalProvider" />
</Capabilities>
...
```

> [!NOTE]
> "devicePortalProvider" 功能會受限 ("rescap")，這表示您必須事先取得市集的核准，才能在該處發行應用程式。 不過，這不會讓您無法透過側載在本機測試應用程式。 如需受限功能的詳細資訊，請參閱[應用程式功能宣告](https://msdn.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities)。

## <a name="set-up-your-background-task-and-winrt-component"></a>設定背景工作和 WinRT 元件
若要設定 Device Portal 連線，您的應用程式必須接通 Device Portal 服務中的應用程式服務連線與您應用程式內執行之 Device Portal 的執行個體。 若要這樣做，請使用實作[**IBackgroundTask**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtask)的類別，將新的 WinRT 元件新增至應用程式。

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

請確定其名稱符合 AppService EntryPoint ("MySampleProvider.SampleProvider") 所設定的命名空間和類別名稱。 第一次對 Device Portal 提供者提出要求時，Device Portal 會隱藏要求、啟動應用程式的背景工作、呼叫其**Run**方法，並傳入 [**IBackgroundTaskInstance**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance)。 應用程式接著會使用它來設定[**DevicePortalConnection**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection)執行個體。

```csharp
// Implement background task handler with a DevicePortalConnection
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take a deferral to allow the background task to continue executing 
    this.taskDeferral = taskInstance.GetDeferral();
    taskInstance.Canceled += TaskInstance_Canceled;

    // Create a DevicePortal client from an AppServiceConnection 
    var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    var appServiceConnection = details.AppServiceConnection;
    this.devicePortalConnection = DevicePortalConnection.GetForAppServiceConnection(appServiceConnection);

    // Add Closed, RequestReceived handlers 
    devicePortalConnection.Closed += DevicePortalConnection_Closed;
    devicePortalConnection.RequestReceived += DevicePortalConnection_RequestReceived;
}
```

有兩個必須由應用程式以完成要求處理迴圈處理的事件：**已關閉**，每當適用於 Device Portal 服務關閉時，並[**RequestReceived**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs)，呈現連入 HTTP 要求，並提供主要Device Portal 提供者的功能。 

## <a name="handle-the-requestreceived-event"></a>處理 RequestReceived 事件
針對在外掛程式的指定「處理常式路徑」上提出的每個要求，將會引發**RequestReceived**事件一次。 Device Portal 提供者的要求處理迴圈與 NodeJS Express 中的要求處理迴圈類似︰要求和回應物件會與事件一起提供，而且處理常式透過填入回應物件來回應。 在 Device Portal 提供者中，**RequestReceived**事件和其處理常式使用[**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httprequestmessage)和[**HttpResponseMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpresponsemessage)物件。   

```csharp
// Sample RequestReceived echo handler: respond with an HTML page including the query and some additional process information. 
private void DevicePortalConnection_RequestReceived(DevicePortalConnection sender, DevicePortalConnectionRequestReceivedEventArgs args)
{
    var req = args.RequestMessage;
    var res = args.ResponseMessage;

    if (req.RequestUri.AbsolutePath.EndsWith("/echo"))
    {
        // construct an html response message
        string con = "<h1>" + req.RequestUri.AbsoluteUri + "</h1><br/>";
        var proc = Windows.System.Diagnostics.ProcessDiagnosticInfo.GetForCurrentProcess();
        con += String.Format("This process is consuming {0} bytes (Working Set)<br/>", proc.MemoryUsage.GetReport().WorkingSetSizeInBytes);
        con += String.Format("The process PID is {0}<br/>", proc.ProcessId);
        con += String.Format("The executable filename is {0}", proc.ExecutableFileName);
        res.Content = new HttpStringContent(con);
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
        res.StatusCode = HttpStatusCode.Ok;            
    }
    //...
}
```

在此範例要求處理常式中，我們會先從*args*參數中提取要求和回應物件，接著建立含有要求 URL 和某些其他 HTML 格式的字串。 這會新增至作為[**HttpStringContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpstringcontent)執行個體的回應物件。 也允許使用其他[**IHttpContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.ihttpcontent)類別，如「字串」和「緩衝區」的類別。

回應接著會設定為 HTTP 回應，並指定 200 (確定) 狀態碼。 它應該如預期呈現在進行原始呼叫的瀏覽器中。 請注意，傳回**RequestReceived**事件處理常式時，會自動將回應訊息傳回給使用者代理程式︰不需要任何其他 "send" 方法。

![Device Portal 回應訊息](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>提供靜態內容
可以直接從您套件內的資料夾提供靜態內容，而將 UI 新增至提供者極為簡單。  提供靜態內容的最簡單方式是在您的專案中建立可對應至 URL 的內容資料夾。

![Device Portal 靜態內容資料夾](images/device-portal/plugin-static-content.png)
 
接著，在偵測靜態內容路徑的**RequestReceived**事件處理常式中新增路徑處理常式，並適當地對應要求。  

```csharp
if (req.RequestUri.LocalPath.ToLower().Contains("/www/")) {
    var filePath = req.RequestUri.AbsolutePath.Replace('/', '\\').ToLower();
    filePath = filePath.Replace("\\backgroundprovider", "")
    try {
        var fileStream = Windows.ApplicationModel.Package.Current.InstalledLocation.OpenStreamForReadAsync(filePath).GetAwaiter().GetResult();
        res.StatusCode = HttpStatusCode.Ok;
        res.Content = new HttpStreamContent(fileStream.AsInputStream());
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    } catch(FileNotFoundException e) {
        string con = String.Format("<h1>{0} - not found</h1>\r\n", filePath);
        con += "Exception: " + e.ToString();
        res.Content = new HttpStringContent(con);
        res.StatusCode = HttpStatusCode.NotFound;
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    }
}
```
請確定內容資料夾內的所有檔案都是標示為 "Content"，並在 Visual Studio 的 [屬性] 功能表中設定為 [有更新時才複製] 或 [一律複製]。  這樣可確保在您部署檔案時，檔案會在您的 AppX 套件內。  

![設定靜態內容檔案複製](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>使用現有的 Device Portal 資源和 API
Device Portal 提供者所提供的靜態內容，是在與核心 Device Portal 服務相同的連接埠上提供。  這表示您可以在 HTML 中使用簡單 `<link>` 和 `<script>` 標記，來參考 Device Portal 隨附的現有 JS 和 CSS。 一般而言，建議使用*rest.js*將所有核心 Device Portal REST API 包裝到方便使用的 webbRest 物件中，以及使用*common.css*檔案讓您設定內容樣式以符合 Device Portal UI 的其他部分。 您可以在範例所含的*index.html*頁面中查看這項作業的範例。 它會使用*rest.js*，從 Device Portal 擷取裝置名稱和執行中程序。 

![Device Portal 外掛程式輸出](images/device-portal/plugin-output.png)
 
重要的是，在 webbRest 上使用 HttpPost/DeleteExpect200 方法，將會自動執行[CSRF 處理](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks)(英文)，如此可讓您的網頁呼叫狀態變更 REST API。  

> [!NOTE] 
> Device Portal 隨附的靜態內容並未保證具有重大變更。 雖然不預期 API 會經常變更，但可能會，尤其是在提供者不應該使用的*common.js*和*controls.js*檔案中。 

## <a name="debugging-the-device-portal-connection"></a>偵錯 Device Portal 連線
若要針對背景工作進行偵錯，您必須變更 Visual Studio 執行程式碼的方法。 請遵循下列步驟，對應用程式服務連線進行偵錯，以檢查您的提供者如何處理 HTTP 要求︰

1.  從 [偵錯] 功能表中，選取 [DevicePortalProvider 屬性]。 
2.  在 [偵錯] 索引標籤下方，於 [起始動作] 區段中選取 [不啟動，但在我的程式碼啟動時進行偵錯]。  
![讓外掛程式進入偵錯模式](images/device-portal/plugin-debug-mode.png)
3.  在 RequestReceived 處理常式函式中設定中斷點。
![requestreceived 處理常式中的中斷點](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> 請確定組建架構完全符合目標的架構。 如果您使用的是 64 位元電腦，就必須使用 AMD64 組建來部署。 
4.  按下 F5 鍵部署應用程式
5.  將 Device Portal 關閉後再重新開啟，讓它找到您的應用程式 (只有當您變更應用程式資訊清單時才需要 – 其他時間，您只需要重新部署和跳過此步驟)。 
6.  在您的瀏覽器中，存取提供者的命名空間，並且應該會叫用中斷點。

## <a name="related-topics"></a>相關主題
* [Windows Device Portal 概觀](device-portal.md)
* [建立和取用應用程式服務](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


