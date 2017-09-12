---
author: normesta
Description: "使用 Windows UI 和元件擴充您的傳統型應用程式"
Search.Product: eADQiWindows 10XVcnh
title: "使用 Windows UI 和元件擴充您的傳統型應用程式"
ms.author: normesta
ms.date: 07/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: fce6076f07957b50e83cf80e8d12350630b99456
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2017
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>使用現代化 UWP 元件擴充您的傳統型應用程式

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 如果您想要新增這些體驗，請使用 UWP 元件擴充您的傳統型應用程式。

在許多情況下，您可以直接從傳統型應用程式呼叫 UWP API，因此在檢視本指南之前，請先參閱[增強 Windows 10](desktop-to-uwp-enhance.md)。

>[!NOTE]
>本指南假設您已使用傳統型橋接器，為您的傳統型應用程式建立 Windows 應用程式套件。 如果尚未完成此步驟，請參閱[傳統型橋接器](desktop-to-uwp-root.md)。

當您準備好時，我們就可以開始進行操作。

## <a name="show-a-modern-xaml-ui"></a>顯示現代化 XAML UI

做為應用程式流程的一部分，您可以將現代化 XAML 型使用者介面納入您的傳統型應用程式。 這些使用者介面會自然調整至不同的螢幕大小和解析度，並支援現代化互動模型，例如觸控和筆跡。

例如，只需少量的 XAML 標記，您就可以為使用者提供強大的地圖相關視覺效果功能。

下圖顯示一個 VB6 應用程式，開啟包含地圖控制項的 XAML 型現代化 UI。

![調適型設計](images\desktop-to-uwp\extend-xaml-ui.png)

### <a name="have-a-closer-look-at-this-app"></a>更仔細檢視這個應用程式

:heavy_check_mark: [觀賞影片](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Add-a-XAML-UI-and-Toast-Notification-to-a-VB6-Application-OsJHC7WhD_8006218965)

:heavy_check_mark: [取得應用程式](https://www.microsoft.com/en-us/store/p/vb6-app-with-xaml-sample/9n191ncxf2f6)

:heavy_check_mark: [瀏覽程式碼](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

### <a name="the-design-pattern"></a>設計模式

若要顯示 XAML 型 UI，請執行下列動作：

:one: [新增 UWP 專案至您的方案](#project)

:two: [新增通訊協定延伸模組至該專案](#protocol)

:three: [從您的傳統型應用程式啟動 UWP app](#start)

:four: [在 UWP 專案中，顯示您要的頁面](#parse)

<span id="project" />
### <a name="add-a-uwp-project"></a>新增 UWP 專案

新增 **[空白應用程式 (通用 Windows)\]** 專案到您的方案。

<span id="protocol" />
### <a name="add-a-protocol-extension"></a>新增通訊協定延伸模組

在 **\[方案總管\]** 中，開啟專案的 **package.appxmanifest** 檔案，並新增延伸模組。

```xml
<Extensions>
      <uap:Extension
          Category="windows.protocol"
          Executable="MapUI.exe"
          EntryPoint=" MapUI.App">
        <uap:Protocol Name="desktopbridgemapsample" />
      </uap:Extension>
    </Extensions>     
```

為通訊協定命名，提供由 UWP 專案產生的可執行檔名稱，以及進入點類別的名稱。

您也可以在設計工具中開啟 **package.appxmanifest**、選擇 **\[宣告\]** 索引標籤，然後在那裡新增延伸模組。

![宣告索引標籤](images\desktop-to-uwp\protocol-properties.png)



> [!NOTE]
> 地圖控制項會從網際網路下載資料，所以如果您使用地圖控制項，則也必須將「網際網路用戶端」功能新增至您的資訊清單。

<span id="start" />
### <a name="start-the-uwp-app"></a>啟動 UWP 應用程式。

首先，建立 [Uri](https://msdn.microsoft.com/library/system.uri.aspx)，包括通訊協定名稱以及您想要傳遞到 UWP 應用程式的任何參數。 然後，呼叫 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 方法。

以下是 C# 的基本範例。

```csharp

private async void showMap(double lat, double lon)
{
    string str = "desktopbridgemapsample://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

    if (success)
    {
        // URI launched
    }
    else
    {
        // URI launch failed
    }
}
```
在我們的範例中，進行的方式比較間接。 我們已將呼叫包裝在可呼叫 VB6 的 interop 函式中，名為 ``LaunchMap``。 該函式是使用 C++ 撰寫。

以下是 VB 區塊：

```VB
Private Declare Function LaunchMap Lib "UWPWrappers.dll" _
  (ByVal lat As Double, ByVal lon As Double) As Boolean
 
Private Sub EiffelTower_Click()
    LaunchMap 48.858222, 2.2945
End Sub
```

以下是 C++ 函式：

```C++

DllExport bool __stdcall LaunchMap(double lat, double lon)
{
  try
  {
    String ^str = ref new String(L"desktopbridgemapsample://");
    Uri ^uri = ref new Uri(
      str + L"location?lat=" + lat.ToString() + L"&?lon=" + lon.ToString());
 
    // now launch the UWP component
    Launcher::LaunchUriAsync(uri);
  }
  catch (Exception^ ex) { return false; }
  return true;
}

```

<span id="parse" />
### <a name="parse-parameters-and-show-a-page"></a>剖析參數並顯示頁面

在 UWP 專案的 **App** 類別中，覆寫 **OnActivated** 事件處理常式。 如果應用程式是由您的通訊協定啟動，請剖析參數然後開啟您想要的頁面。

```C++
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ e)
{
  if (e->Kind == ActivationKind::Protocol)
  {
    ProtocolActivatedEventArgs^ protocolArgs = (ProtocolActivatedEventArgs^)e;
    Uri ^uri = protocolArgs->Uri;
    if (uri->SchemeName == "desktopbridgemapsample")
    {
      Frame ^rootFrame = ref new Frame();
      Window::Current->Content = rootFrame;
      rootFrame->Navigate(TypeName(MainPage::typeid), uri->Query);
      Window::Current->Activate();
    }
  }
}
```

### <a name="similar-samples"></a>類似範例

[Northwind 範例：UWA UI 和 Win32 舊版程式碼的端對端範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Northwind 範例：連接至 SQL Server 的 UWP 應用程式](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>提供服務給其他應用程式

您新增一種服務，可供其他應用程式使用。 例如，您可以新增一種服務，提供其他應用程式對於您應用程式後面資料庫的受控制存取權。 藉由執行背景工作，即使您的傳統型應用程式未執行，其他應用程式仍可連線到服務。

以下是執行此程序的範例。

![調適型設計](images\desktop-to-uwp\winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>更仔細檢視這個應用程式

:heavy_check_mark: [觀賞影片](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Expose-an-AppService-from-a-Windows-Forms-Data-Application-GiqNS7WhD_706218965)

:heavy_check_mark: [取得應用程式](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [瀏覽程式碼](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>設計模式

若要顯示提供服務，請執行下列動作：

:one: [新增 Windows 執行階段元件](#component)

:two: [新增應用程式服務擴充功能](#extension)

:three: [實作應用程式服務](#appservice)

:four: [測試應用程式服務](#test)

<span id="component" />

### <a name="add-a-windows-runtime-component"></a>新增 Windows 執行階段元件

新增 **\[Windows 執行階段元件 (通用 Windows)\]** 專案至您的方案。

然後，從您的 UWP 封裝專案參考該執行階段元件的專案。

<span id="extension" />
### <a name="add-an-app-service-extension"></a>新增應用程式服務擴充功能

在 **\[方案總管\]** 中，開啟封裝專案的 **package.appxmanifest** 檔案，並新增應用程式服務延伸模組。

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="MyAppService.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```

命名應用程式服務，並提供進入點類別的名稱。 這是您將用來實作應用程式服務的類別。

<span id="appservice" />
### <a name="implement-the-app-service"></a>實作應用程式服務

您在此處驗證並處理來自其他應用程式的要求。

```csharp
public sealed class AppServiceTask : IBackgroundTask
{
    private BackgroundTaskDeferral backgroundTaskDeferral;
 
    public void Run(IBackgroundTaskInstance taskInstance)
    {
        this.backgroundTaskDeferral = taskInstance.GetDeferral();
        taskInstance.Canceled += OnTaskCanceled;
        var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        details.AppServiceConnection.RequestReceived += OnRequestReceived;
    }
 
    private async void OnRequestReceived(AppServiceConnection sender,
                                         AppServiceRequestReceivedEventArgs args)
    {
        var messageDeferral = args.GetDeferral();
        ValueSet message = args.Request.Message;
        string id = message["ID"] as string;
        ValueSet returnData = DataBase.GetData(id);
        await args.Request.SendResponseAsync(returnData);
        messageDeferral.Complete();
    }
 
 
    private void OnTaskCanceled(IBackgroundTaskInstance sender,
                                BackgroundTaskCancellationReason reason)
    {
        if (this.backgroundTaskDeferral != null)
        {
            this.backgroundTaskDeferral.Complete();
        }
    }
}
```

<span id="test" />
### <a name="test-the-app-service"></a>測試應用程式服務

從另一個應用程式呼叫服務，測試您的服務。

```csharp
private async void button_Click(object sender, RoutedEventArgs e)
{
    AppServiceConnection dataService = new AppServiceConnection();
    dataService.AppServiceName = "com.microsoft.samples.winforms";
    dataService.PackageFamilyName = "Microsoft.SDKSamples.WinformWithAppService";
 
    var status = await dataService.OpenAsync();
    if (status == AppServiceConnectionStatus.Success)
    {
        string id = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("ID", id);
        AppServiceResponse response = await dataService.SendMessageAsync(message);
        string result = "";
 
        if (response.Status == AppServiceResponseStatus.Success)
        {
            if (response.Message["Status"] as string == "OK")
            {
                DisplayResult(response.Message["Result"]);
            }
        }
    }
}
```

請至此處深入了解應用程式服務：[建立和使用應用程式服務](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

### <a name="similar-samples"></a>類似範例

[應用程式服務橋接器範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[使用 C++ win32 應用程式的應用程式服務橋接器範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[接收推播通知的 MFC 應用程式](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>讓您的傳統型應用程式成為分享目標

您可以讓您的傳統型應用程式成為分享目標，讓使用者可以輕鬆地共用資料，例如來自支援共用的其他應用程式的圖片。

例如，使用者可以選擇您的應用程式來從 Microsoft Edge 的 [相片] App 分享圖片。 以下是具有該功能的 WPF 範例應用程式。

![分享目標](images\desktop-to-uwp\share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>更仔細檢視這個應用程式

:heavy_check_mark: [觀賞影片](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Make-a-WPF-Application-a-Share-Target-xd6Fu6WhD_8406218965)

:heavy_check_mark: [取得應用程式](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [瀏覽程式碼](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>設計模式

若要讓您的應用程式成為分享目標，請進行下列項目：

:one: [新增 UWP 專案至您的方案](#project2)

:two: [新增分享目標擴充功能](#share-extension)

:three: [覆寫 OnNavigatedTo 事件處理常式](#override)

<span id="project2" />
### <a name="add-a-uwp-project-to-your-solution"></a>新增 UWP 專案至您的方案

新增 **[空白應用程式 (通用 Windows)\]** 專案到您的方案。

<span id="share-extension" />
### <a name="add-a-share-target-extension"></a>新增分享目標擴充功能

在 **\[方案總管\]** 中，開啟專案的 **package.appxmanifest** 檔案，並新增延伸模組。

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="ShareTarget.App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

提供由 UWP 專案產生的可執行檔名稱，以及進入點類別的名稱。 您也需要指定您的應用程式可以分享哪些檔案類型。

<span id="override" />
### <a name="override-the-onnavigatedto-event-handler"></a>覆寫 OnNavigatedTo 事件處理常式

覆寫 UWP 專案 **App** 類別中的 **OnNavigatedTo** 事件處理常式。

當使用者選擇您的應用程式來共用檔案時，就會呼叫這個事件處理常式。

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
  this.shareOperation = (ShareOperation)e.Parameter;
  if (this.shareOperation.Data.Contains(StandardDataFormats.StorageItems))
  {
      this.sharedStorageItems =
        await this.shareOperation.Data.GetStorageItemsAsync();
       
      foreach (StorageFile item in this.sharedStorageItems)
      {
          ProcessSharedFile(item);
      }
  }
}
```

## <a name="support-and-feedback"></a>支援與意見反應

**尋找特定問題的解答**

我們的團隊會監視這些 [StackOverflow 標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供意見反應或功能建議**

請參閱[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**提供有關本文的意見反應**

使用下方的留言區塊。
