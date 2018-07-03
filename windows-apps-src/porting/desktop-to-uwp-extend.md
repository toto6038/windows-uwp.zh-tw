---
author: normesta
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: 使用 Windows UI 和元件擴充您的傳統型應用程式
ms.author: normesta
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4e1d808dd2991aa2ffd1e30967d329b3eced9f99
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018564"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>使用現代化 UWP 元件擴充您的傳統型應用程式

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 如果您想要新增這些體驗，請使用 UWP 專案和 Windows 執行階段元件擴充您的傳統型應用程式。

在許多情況下，您可以直接從傳統型應用程式呼叫 UWP API，因此在檢視本指南之前，請先參閱[增強 Windows 10](desktop-to-uwp-enhance.md)。

>[!NOTE]
>本指南假設您已使用傳統型橋接器，為您的傳統型應用程式建立 Windows 應用程式套件。 如果尚未完成此步驟，請參閱[傳統型橋接器](desktop-to-uwp-root.md)。

當您準備好時，我們就可以開始進行操作。

<a id="setup" />

## <a name="first-setup-your-solution"></a>首先，設定您的解決方案

新增一或多個 UWP 專案和執行階段元件至您的解決方案。

從包含 **Windows 應用程式封裝專案**的解決方案開始，其中包含傳統型應用程式的參考。

下圖顯示範例解決方案。

![延伸起始專案](images/desktop-to-uwp/extend-start-project.png)

如果您的解決方案不包含封裝專案，請查看[使用 Visual Studio封裝您的應用程式套件](desktop-to-uwp-packaging-dot-net.md)。

### <a name="add-a-uwp-project"></a>新增 UWP 專案

新增 **\[空白應用程式 (通用 Windows)\]** 到您的解決方案。

您會在此組建現代化 XAML UI 或使用只在 UWP 處理程序中執行的 API。

![UWP 專案](images/desktop-to-uwp/add-uwp-project-to-solution.png)

在封裝專案的 **\[應用程式\]** 節點上按一下滑鼠右鍵，然後按一下 **\[加入參考\]**。

![參考 UWP 專案](images/desktop-to-uwp/add-uwp-project-reference.png)

然後，新增 UWP 專案的參考。

![參考 UWP 專案](images/desktop-to-uwp/choose-uwp-project.png)

您的解決方案看起來會像這樣：

![解決方案與 UWP 專案](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(選用) 新增 Windows 執行階段元件

若要完成一些案例，您必須將程式碼新增到 Windows 執行階段元件。

![執行階段元件應用程式服務](images/desktop-to-uwp/add-runtime-component.png)

然後，從您的 UWP 專案，新增執行階段元件的參考。 您的解決方案看起來會像這樣：

![執行階段參考](images/desktop-to-uwp/runtime-component-reference.png)

讓我們看看您可以使用 UWP 專案和執行階段元件做幾件事。

## <a name="show-a-modern-xaml-ui"></a>顯示現代化 XAML UI

做為應用程式流程的一部分，您可以將現代化 XAML 型使用者介面納入您的傳統型應用程式。 這些使用者介面會自然調整至不同的螢幕大小和解析度，並支援現代化互動模型，例如觸控和筆跡。

例如，只需少量的 XAML 標記，您就可以為使用者提供強大的地圖相關視覺效果功能。

此圖顯示一個 Windows Forms 應用程式，開啟包含地圖控制項的 XAML 型現代化 UI。

![調適型設計](images/desktop-to-uwp/extend-xaml-ui.png)

### <a name="the-design-pattern"></a>設計模式

若要顯示 XAML 型 UI，請執行下列動作：

:one: [設定您的解決方案](#solution-setup)

:two: [建立 XAML UI](#xaml-UI)

:three: [新增通訊協定延伸模組至 UWP 專案](#protocol)

:four: [從您的傳統型應用程式啟動 UWP app](#start)

:five: [在 UWP 專案中，顯示您要的頁面](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>設定您的解決方案

如需如何設定您的解決方案的一般指導方針，請參閱本指開頭南的[首先，設定您的解決方案](#setup)一節。

您的解決方案看起來會像這樣：

![XAML UI 解決方案](images/desktop-to-uwp/xaml-ui-solution.png)

在此範例中，Windows Forms 專案名為**地標**和包含 XAML U 的 UWP 專案名為 **MapUI** XAML UI。

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>建立 XAML UI

新增 XAML UI 至您的 UWP 專案 以下是 XAML 的基本對應。

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"    
                     HorizontalAlignment="Left"               
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>新增通訊協定延伸模組

在 **\[方案總管\]** 中，開啟解決方案中 UWP 專案的 **package.appxmanifest** 檔案，並新增此延伸模組。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>    
```

為通訊協定命名，提供由 UWP 專案產生的可執行檔名稱，以及進入點類別的名稱。

您也可以在設計工具中開啟 **package.appxmanifest**、選擇 **\[宣告\]** 索引標籤，然後在那裡新增延伸模組。

![宣告索引標籤](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> 地圖控制項會從網際網路下載資料，所以如果您使用地圖控制項，則也必須將「網際網路用戶端」功能新增至您的資訊清單。

<a id="start" />

### <a name="start-the-uwp-app"></a>啟動 UWP 應用程式。

首先，從您的傳統型應用程式建立 [Uri](https://msdn.microsoft.com/library/system.uri.aspx)，包括通訊協定名稱以及您想要傳遞到 UWP 應用程式的任何參數。 然後，呼叫 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 方法。

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>剖析參數並顯示頁面

在 UWP 專案的 **App** 類別中，覆寫 **OnActivated** 事件處理常式。 如果應用程式是由您的通訊協定啟動，請剖析參數然後開啟您想要的頁面。

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

覆寫 ``OnNavigatedTo`` 方法以使用傳遞到頁面的參數。 在這種情形下，我們會使用傳遞到此頁面的緯度和經度，以在地圖中顯示位置。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

### <a name="similar-samples"></a>類似範例

[新增 UWP XAML 使用者體驗到 VB6 應用程式](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

[Northwind 範例：UWA UI 和 Win32 舊版程式碼的端對端範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Northwind 範例：連接至 SQL Server 的 UWP 應用程式](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>提供服務給其他應用程式

您新增一種服務，可供其他應用程式使用。 例如，您可以新增一種服務，提供其他應用程式對於您應用程式後面資料庫的受控制存取權。 藉由執行背景工作，即使您的傳統型應用程式未執行，其他應用程式仍可連線到服務。

以下是執行此程序的範例。

![調適型設計](images/desktop-to-uwp/winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>更仔細檢視這個應用程式

:heavy_check_mark: [取得應用程式](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [瀏覽程式碼](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>設計模式

若要顯示提供服務，請執行下列動作：

:one: [實作應用程式服務](#appservice)

:two: [新增應用程式服務擴充功能](#extension)

:three: [測試應用程式服務](#test)

<a id="appservice" />

### <a name="implement-the-app-service"></a>實作應用程式服務

您在此處驗證並處理來自其他應用程式的要求。 新增此程式碼至解決方案中的 Windows 執行階段元件。

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

<a id="extension" />

### <a name="add-an-app-service-extension-to-the-uwp-project"></a>將應用程式服務延伸模組新增到 UWP 專案

開啟 UWP 專案的 **package.appxmanifest** 檔案，並將應用程式服務延伸模組新增到 ``<Application>`` 元素。

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="AppServiceComponent.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```
命名應用程式服務，並提供進入點類別的名稱。 這是您實作服務的類別。

<a id="test" />

### <a name="test-the-app-service"></a>測試應用程式服務

從另一個應用程式呼叫服務，測試您的服務。 此程式碼可以是傳統型應用程式 (例如 Windows Forms 應用程式) 或其他 UWP app。

> [!NOTE]
> 此程式碼只有當您正確設定 ``AppServiceConnection`` 類別的 ``PackageFamilyName`` 屬性時適用。 您可以藉由在 UWP 專案的內容中呼叫打 ``Windows.ApplicationModel.Package.Current.Id.FamilyName`` 取得該名稱。 請參閱[建立和取用 App 服務](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

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

![分享目標](images/desktop-to-uwp/share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>更仔細檢視這個應用程式

:heavy_check_mark: [取得應用程式](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [瀏覽程式碼](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>設計模式

若要讓您的應用程式成為分享目標，請進行下列項目：

:one: [新增分享目標擴充功能](#share-extension)

:two: [覆寫 OnNavigatedTo 事件處理常式](#override)

<a id="share-extension" />

### <a name="add-a-share-target-extension"></a>新增分享目標擴充功能

在 **\[方案總管\]** 中，開啟解決方案中 UWP 專案的 **package.appxmanifest** 檔案，並新增此延伸模組。

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

<a id="override" />

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

## <a name="create-a-background-task"></a>建立背景工作

新增背景工作，即使 app 已暫停時執行程式碼。 背景工作非常適合不需要使用者互動的小工作。 例如，您的工作可以下載電子郵件、顯示收到聊天訊息的快顯通知，或回應系統條件的變更。

以下是註冊背景工作的 WPF 範例應用程式。

![背景工作](images/desktop-to-uwp/sample-background-task.png)

工作提供 http 要求並測量傳回要求回應所需的時間。 您的工作可能更有趣，但此範例非常適合學習背景工作的入門技巧。

### <a name="have-a-closer-look-at-this-app"></a>更仔細檢視這個應用程式

:heavy_check_mark: [瀏覽程式碼](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)

### <a name="the-design-pattern"></a>設計模式

若要建立背景服務，請執行下列動作：

:one: [實作背景工作](#implement-task)

:two: [設定背景工作](#configure-background-task)

:three: [註冊背景工作](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>實作背景工作

將程式碼新增至加入 Windows 執行階段元件專案，實作背景工作。

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>設定背景工作

在資訊清單設計工具中，開啟解決方案中 UWP 專案的 **package.appxmanifest** 檔案。

在**\[宣告\]** 索引標籤，新增**\[背景工作\]** 宣告。

![背景工作選項](images/desktop-to-uwp/background-task-option.png)

然後，選擇您想要的屬性。 我們的範例使用**\[計時器\]** 屬性。

![計時器屬性](images/desktop-to-uwp/timer-property.png)

在實作背景工作的 Windows 執行階段元件中提供類別的完整名稱。

![計時器屬性](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>註冊背景工作

在註冊背景工作的傳統型應用程式專案中加入程式碼。

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```
## <a name="support-and-feedback"></a>支援與意見反應

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
